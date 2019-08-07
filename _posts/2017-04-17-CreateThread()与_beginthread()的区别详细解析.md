---
title: CreateThread()与_beginthread()的区别详细解析
date: 2019-05-22 15:50:32
tags: CSDN迁移
---
   **很多开发者不清楚这两者之间的关系，他们随意选一个函数来用，发现也没有什么大问题，**

 **于是就忙于解决更为紧迫的任务去了。等到有一天忽然发现一个程序运行时间很长的时候会有细微的内存泄露，**

 **开发者绝对不会想到是因为这两套函数用混的结果**

 

 我们知道在Windows下创建一个线程的方法有两种，一种就是调用Windows API CreateThread()来创建线程；另外一种就是调用MSVC CRT的函数_beginthread()或_beginthreadex()来创建线程。相应的退出线程也有两个函数Windows API的ExitThread()和CRT的_endthread()。这两套函数都是用来创建和退出线程的，它们有什么区别呢？

 很多开发者不清楚这两者之间的关系，他们随意选一个函数来用，发现也没有什么大问题，于是就忙于解决更为紧迫的任务去了，而没有对它们进行深究。等到有一天忽然发现一个程序运行时间很长的时候会有细微的内存泄露，开发者绝对不会想到是因为这两套函数用混的结果。

 根据Windows API和MSVC CRT的关系，可以看出来_beginthread()是对CreateThread()的包装，它最终还是调用CreateThread()来创建线程。那么在_beginthread()调用CreateThread()之前做了什么呢？我们可以看一下_beginthread()的源代码，它位于CRT源代码中的thread.c。我们可以发现它在调用CreateThread()之前申请了一个叫_tiddata的结构，然后将这个结构用_initptd()函数初始化之后传递给_beginthread()自己的线程入口函数_threadstart。_threadstart首先把由_beginthread()传过来的_tiddata结构指针保存到线程的显式TLS数组，然后它调用用户的线程入口真正开始线程。在用户线程结束之后，_threadstart()函数调用_endthread()结束线程。并且_threadstart还用__try/__except将用户线程入口函数包起来，用于捕获所有未处理的信号，并且将这些信号交给CRT处理。

 所以除了信号之外，很明显CRT包装Windows API线程接口的最主要目的就是那个_tiddata。这个线程私有的结构里面保存的是什么呢？我们可以从mtdll.h中找到它的定义，它里面保存的是诸如线程ID、线程句柄、erron、strtok()的前一次调用位置、rand()函数的种子、异常处理等与CRT有关的而且是线程私有的信息。可见MSVC CRT并没有使用我们前面所说的__declspec(thread)这种方式来定义线程私有变量，从而防止库函数在多线程下失效，而是采用在堆上申请一个_tiddata结构，把线程私有变量放在结构内部，由显式TLS保存_tiddata的指针。

 了解了这些信息以后，我们应该会想到一个问题，那就是如果我们用CreateThread()创建一个线程然后调用CRT的strtok()函数，按理说应该会出错，因为strtok()所需要的_tiddata并不存在，可是我们好像从来没碰到过这样的问题。查看strtok()函数就会发现，当一开始调用_getptd()去得到线程的_tiddata结构时，这个函数如果发现线程没有申请_tiddata结构，它就会申请这个结构并且负责初始化。于是无论我们调用哪个函数创建线程，都可以安全调用所有需要_tiddata的函数，因为一旦这个结构不存在，它就会被创建出来。

 那么_tiddata在什么时候会被释放呢？ExitThread()肯定不会，因为它根本不知道有_tiddata这样一个结构存在，那么很明显是_endthread()释放的，这也正是CRT的做法。不过我们很多时候会发现，即使使用CreateThread()和ExitThread() （不调用ExitThread()直接退出线程函数的效果相同），也不会发现任何内存泄露，这又是为什么呢？经过仔细检查之后，我们发现原来密码在CRT DLL的入口函数DllMain中。我们知道，当一个进程/线程开始或退出的时候，每个DLL的DllMain都会被调用一次，于是动态链接版的CRT就有机会在DllMain中释放线程的_tiddata。可是DllMain只有当CRT是动态链接版的时候才起作用，静态链接CRT是没有DllMain的！这就是造成使用CreateThread()会导致内存泄露的一种情况，在这种情况下，_tiddata在线程结束时无法释放，造成了泄露。  
  
**我们可以用下面这个小程序来测试：**

 
```
 
#include <Windows.h>
#include <process.h>
void thread(void *a)
{
    char* r = strtok( "aaa", "b" );
    ExitThread(0); // 这个函数是否调用都无所谓
}
int main(int argc, char* argv[])
{
    while(1) {
        CreateThread(  0, 0, (LPTHREAD_START_ROUTINE)thread, 0, 0, 0 );
        Sleep( 5 );
    }
return 0;
}
```
 如果用动态链接的CRT （/MD，/MDd）就不会有问题，但是，如果使用静态链接CRT （/MT，/MTd），运行程序后在进程管理器中观察它就会发现内存用量不停地上升，但是如果我们把thread()函数中的ExitThread()改成_endthread()就不会有问题，因为_endthread()会将_tiddata()释放。

 

 **这个问题可以总结为：**当使用CRT时（基本上所有的程序都使用CRT），请尽量使用_beginthread()/_beginthreadex()/_endthread()/_endthreadex()这组函数来创建线程。在MFC中，还有一组类似的函数是AfxBeginThread()和AfxEndThread()，根据上面的原理类推，它是MFC层面的线程包装函数，它们会维护线程与MFC相关的结构，当我们使用MFC类库时，尽量使用它提供的线程包装函数以保证程序运行正确。

 感谢原作者的分享!

 原文地址：[https://www.jb51.net/article/41459.htm](https://www.jb51.net/article/41459.htm)

   
 