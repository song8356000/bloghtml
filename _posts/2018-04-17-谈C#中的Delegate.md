---
title: 谈C#中的Delegate
date: 2017-11-08 16:57:40
tags: CSDN迁移
---
   **引言**

 Delegate是Dotnet1.0的时候已经存在的特性了，但由于在实际工作中一直没有机会使用Delegate这个特性，所以一直没有对它作整理。这两天，我再度翻阅了一些关于Delegate的资料，并开始正式整理这个C#中著名的特性。本文将由浅入深的谈一下Delegate这个特性。

 **一.Delegate是什么？**

 Delegate中文翻译为“委托”。[Msdn](http://msdn.microsoft.com/zh-cn/library/aa288459(VS.71).aspx)中对Delegate的解释如下：

 _C#中的委托类似于C或C++中的函数指针。使用委托使程序员可以将方法引用封装在委托对象内。然后可以将该委托对象传递给可调用所引用方法的代码，而不必在编译时知道将调用哪个方法。与C或C++中的函数指针不同，委托是面向对象、类型安全的，并且是安全的。_

 如果你是第一次接触Delegate这个概念，你可能会对上面这段文字感觉不知所云，不过不要紧，你可以先把Delegate认为就是一个函数指针。

 而当你面对一个虚无的概念时，最好的应对方法就是直接看实例。下面一个简单的Delegate使用例子。

 
```
class Program
    {
        static void OtherClassMethod(){
            Console.WriteLine("Delegate an other class's method");
        }

        static void Main(string[] args)
        {
            var test = new TestDelegate();
            test.delegateMethod = new TestDelegate.DelegateMethod(test.NonStaticMethod);
            test.delegateMethod += new TestDelegate.DelegateMethod(TestDelegate.StaticMethod);
            test.delegateMethod += Program.OtherClassMethod;
            test.RunDelegateMethods();
        }
    }

    class TestDelegate
    {
        public delegate void DelegateMethod();  //声明了一个Delegate Type

        public DelegateMethod delegateMethod;   //声明了一个Delegate对象

        public static void StaticMethod()   
        {
            Console.WriteLine("Delegate a static method");
        }

        public void NonStaticMethod()   
        {
            Console.WriteLine("Delegate a non-static method");
        }

        public void RunDelegateMethods()
        {
            if(delegateMethod != null){
                Console.WriteLine("---------");
                delegateMethod.Invoke();    
```
 
```
                   Console.WriteLine("---------");
            }
        }
    }
```
 

 上面是一个Delegate的使用例子，运行看看结果吧。下面我稍微解释一下：

 【1】public delegate void DelegateMethod();这里声明了一个Delegate的类型，名为DelegateMethod，这种Delegate类型可以搭载：返回值为void，无传入参数的函数。

 【2】public DelegateMethod delegateMethod;这里声明了一个DelegateMethod的对象（即，声明了某种Delegate类型的对象）。

 区分：DelegateMethod是类型，delegateMethod是对象。

 【3】为什么上面说Delegate可以看做是函数指针呢？看下面这段代码：

 
```
test.delegateMethod = new TestDelegate.DelegateMethod(test.NonStaticMethod); 
test.delegateMethod += new TestDelegate.DelegateMethod(TestDelegate.StaticMethod); 
test.delegateMethod += Program.OtherClassMethod; 

```
 这里delegateMethod搭载了3个函数，而且可以通过调用delegateMethod.Invoke();运行被搭载的函数。这就是Delegate可以看作为函数指针的原因。上面这段代码中，delegateMethod只能搭载：返回值为void，无传入参数的函数（见：NonStaticMethod，StaticMethod，OtherClassMethod的定义），这和Delegate类型声明有关（见DelegateMethod的声明：public delegate void DelegateMethod()）。

 【4】Delegate在搭载多个方法时，可以通过+=增加搭载的函数，也可以通过-=来去掉Delegate中的某个函数。

 **二.Delegate和C++中函数指针的区别**

 Delegate和C++中的函数指针很像，但如果深入对比，发现其实还是有区别的，区别主要有三个方面（参考[Stanley B. Lippman](http://blogs.msdn.com/slippman/)的一篇文章）

 _1) 一个 delegate对象一次可以搭载多个方法（methods），而不是一次一个。当我们唤起一个搭载了多个方法（methods）的delegate，所有方法以其“被搭载到delegate对象的顺序”被依次唤起。_

 _2) 一个delegate对象所搭载的方法（methods）并不需要属于同一个类别。一个delegate对象所搭载的所有方法（methods）必须具有相同的原型和形式。然而，这些方法（methods）可以即有static也有non-static，可以由一个或多个不同类别的成员组成。_

 _3) 一个delegate type的声明在本质上是创建了一个新的subtype instance，该 subtype 派生自 .NET library framework 的 abstract base classes Delegate 或 MulticastDelegate，它们提供一组public methods用以询访delegate对象或其搭载的方法（methods） ，与函数指针不同，委托是面向对象、类型安全并且安全的。_

 看完上面关于Delegate的介绍，相信大家对它也有所了解了，下面我们将进行更深入地讨论！

 **三.Delegate什么时候该用？**

 看完上面的介绍，你可以会有一些疑问，为什么会有Delegate？实际中什么时候会用到？什么时候应该去用？ 在回答这些问题之前，大家可以先看看下面这段代码：

 
```
    class Program
    {
        static void Main(string[] args)
        {
            var car = new Car(15);
            new Alerter(car);
            car.Run(120);
        }
    }

    class Car
    {
        public delegate void Notify(int value);
        public event Notify notifier;

        private int petrol = 0;
        public int Petrol
        {
            get { return petrol; }
            set
            {
                petrol = value;
                if (petrol < 10)  //当petrol的值小于10时，出发警报
                {
                    if (notifier != null)
                    {
                        notifier.Invoke(Petrol);
                    }
                }
            }
        }

        public Car(int petrol)
        {
            Petrol = petrol;
        }

        public void Run(int speed)
        {
            int distance = 0;
            while (Petrol > 0)
            {
                Thread.Sleep(500);
                Petrol--;
                distance += speed;
                Console.WriteLine("Car is running... Distance is " + distance.ToString());
            }
        }
    }

    class Alerter
    {
        public Alerter(Car car)
        {
            car.notifier += new Car.Notify(NotEnoughPetrol);
        }

        public void NotEnoughPetrol(int value)
        {
            Console.ForegroundColor = ConsoleColor.Red;
            Console.WriteLine("You only have " + value.ToString() + " gallon petrol left!");
            Console.ResetColor();
        }
    }
```
 看完了上面的代码后，你可能会问：为什么不在public int Petrol中直接调用Alerter.NotEnoughPetrol呢？因为Car模块和Alerter模块本身是两个独立的子系统，如果直接调用，耦合性就会增加，这不是我们愿意看到的。

 其实以上的代码是设计模式中的**观察者模式**（观察者模式又称Source/Listener模式）的实现，当汽车在运行中汽油量<10时，警报器便会发出警报。在上面代码中，Delegate相当于一个存放回调函数的函数指针，使用Delegate，我们可以非常方便地实现观察者模式。而其实，在需要使用回调函数时，我们都可以考虑使用Delegate。

 不知道你有没有发现在上面的代码中还有一个问题呢？

 
```
public event Notify notifier;
```
 上面的代码中，我们定义了一个Event，而事实上：

 
```
public Notify notifier;
```
 

 这样写，也完全可以满足我们的需求，这就引出了我们的另一个问题，Delegate和Event!

 **四.Delegate与Event**

 **【1】Delegate和Event的关系**

 看微软的代码时，我们会发现Delegate和Event这两个关键字经常会一起出现！究竟他们是什么关系呢？

 在[Msdn](http://msdn.microsoft.com/zh-cn/library/aa288460(VS.71).aspx)中，有一段话描述Delegate和Event之间的关系，其实很简单：

 _ 声明事件：若要在类内声明事件，首先必须声明该事件的委托类型。_

 **【2】Delegate和Event配合使用的效果**

 看下面几幅图，这是我从一个C#的Application程序截下来的：

 [](http://images.cnblogs.com/cnblogs_com/hyddd/WindowsLiveWriter/baa965523e4a_207B/2_2.jpg)[![](http://pic002.cnblogs.com/img/hyddd/200907/2009072620432252.jpg)](http://images.cnblogs.com/cnblogs_com/hyddd/WindowsLiveWriter/baa965523e4a_207B/3_2.jpg)

 ![](http://pic002.cnblogs.com/img/hyddd/200907/2009072620434951.jpg)

 ![](http://pic002.cnblogs.com/img/hyddd/200907/2009072620440592.jpg)

 从上图看到，在响应图形界面的操作中，我们用到了Event和Delegate，相信这也我们使用Event和Delegate最频繁的地方了。这里我还想罗嗦一下，平时需要我们自己写代码的界面事件响应函数，如：button_Click(…)，其实都是回调函数，在自动生成的文件Form1.Designer.cs中，VS把事件和其对应的回调函数（即：button_Click(…)等）关联起来，当触发某事件时，对应的回调函数便会执行。

 **【3】“public Notify notifier”和“public event Notify notifier”的区别**

 关于这个问题，我们直接ildasm看看IL代码吧:>

 “public Notify notifier”的IL代码，如图：

 [](http://images.cnblogs.com/cnblogs_com/hyddd/WindowsLiveWriter/baa965523e4a_207B/IL1_2.jpg)![](http://pic002.cnblogs.com/img/hyddd/200907/2009072620445364.jpg) 

 “public event Notify notifier”的IL代码，如图：

 [](http://images.cnblogs.com/cnblogs_com/hyddd/WindowsLiveWriter/baa965523e4a_207B/IL2_2.jpg)![](http://pic002.cnblogs.com/img/hyddd/200907/2009072620451815.jpg) 

 差别其实已经很明显了，“public Notify notifier”相当于Class里面的Field，访问级别是public，而“public event Notify notifier”则相当于Property，访问级别是private！由于以上的差别，他们在某些使用上，会稍有不同，详细的可参考shensr写的《[delegate vs. event](http://www.cnblogs.com/shensr/archive/2005/11/24/283653.html)》。

 **五.Delegate中的Invoke与BeginInvoke方法**

 简单说一下，Invoke与BeginInvoke都是执行Delegate里的搭载函数，而不同的是：Invoke是一个同步方法，BeginInvoke是一个异步方法。关于这个，有一篇文章《[Invoke and BeginInvoke](http://www.cnblogs.com/worldreason/archive/2008/06/09/1216127.html)》，对此介绍的比较详细，这里就不多说了。

 **六.小结** 

 回顾一下，到底什么时候我们可能会用到Delegate：

 【1】.当我们在C#中需要类似函数指针这样东西时。

 【2】.当我们需要使用回调函数的时候。

 【3】.需要异步调用的时候。

 【4】.实现观察者模式的时候。

 【5】.处理事件响应的时候。

 以上内容均为个人看法，如果有错漏，请各位及时指出:>

 转载请说明出处，谢谢![hyddd([http://www.cnblogs.com/hyddd/](http://www.cnblogs.com/hyddd/))]

 **参考资料**

 【1】[Msdn委托教程](http://msdn.microsoft.com/zh-cn/library/aa288460(VS.71).aspx)，[Msdn事件教程](http://msdn.microsoft.com/zh-cn/library/aa288460(VS.71).aspx)

 【2】《[深入探索面向对象事件（Delegate）机制](http://www.cnblogs.com/aplo/archive/2007/09/07/886145.html)》

 【3】《[对.net事件的看法](http://www.cnblogs.com/worldreason/archive/2008/05/10/1191575.html)》

 【4】《[Invoke and BeginInvoke](http://www.cnblogs.com/worldreason/archive/2008/06/09/1216127.html)》

 【5】《[delegate vs. event](http://www.cnblogs.com/shensr/archive/2005/11/24/283653.html)》

 【6】《[C#事件(event)解析](http://www.cnblogs.com/michaelxu/archive/2008/04/02/1134217.html)》

 【7】《[C# Delegate 简介](http://www.cnblogs.com/pockey/archive/2006/08/02/465835.html)》

   
 