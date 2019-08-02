---
title: c#的dllimport使用方法详解
date: 2017-11-07 11:19:32
tags: CSDN迁移
---
   DllImport是System.Runtime.InteropServices命名空间下的一个属性类，其功能是提供从非托管DLL导出的函数的必要调用信息

 

 DllImport是System.Runtime.InteropServices命名空间下的一个属性类，其功能是提供从非托管DLL导出的函数的必要调用信息

 DllImport属性应用于方法，要求最少要提供包含入口点的dll的名称。  
 DllImport的定义如下：

 

 代码如下:  
 ```
[AttributeUsage(AttributeTargets.Method)]
public class DllImportAttribute: System.Attribute
{
public DllImportAttribute(string dllName) {…} //定位参数为dllName
public CallingConvention CallingConvention; //入口点调用约定
public CharSet CharSet;                                   //入口点采用的字符接
public string EntryPoint;  //入口点名称
public bool ExactSpelling;   //是否必须与指示的入口点拼写完全一致，默认false
public bool PreserveSig;  //方法的签名是被保留还是被转换
public bool SetLastError;  //FindLastError方法的返回值保存在这里
public string Value { get {…} }
}
```
  
  
 

 用法示例：

 

 代码如下:  
 ```
[DllImport("kernel32")]
private static extern long WritePrivateProfileString(string section,string key,string val,string filePath);
```
  
  
 

 以上是用来写入ini文件的一个win32api。 用此方式调用Win32API的数据类型对应：DWORD=int或uint，BOOL=bool，预定义常量=enum，结构=struct。 

 DllImport会按照顺序自动去寻找的地方： 1、exe所在目录 2、System32目录 3、环境变量目录所以只需要你把引用的DLL 拷贝到这三个目录下 就可以不用写路径了 或者可以这样server.MapPath(.\bin\*.dll)web中的，同时也是应用程序中的 后来发现用[DllImport(@"C:\OJ\Bin\Judge.dll")]这样指定DLL的绝对路径就可以正常装载。 这个问题最常出现在使用第三方非托管DLL组件的时候,我的也同样是这时出的问题,Asp.Net Team的官方解决方案如下: 首先需要确认你引用了哪些组件,那些是托管的,哪些是非托管的.托管的很好办,直接被使用的需要引用,间接使用的需要拷贝到bin目录下.非托管的处理会比较麻烦.实际上,你拷贝到bin没有任何帮助,因为CLR会把文件拷贝到一个临时目录下,然后在那运行web,而CLR只会拷贝托管文件,这就是为什么我们明明把非托管的dll放在了bin下却依然提示不能加载模块了. 具体做法如下: 首先我们在服务器上随便找个地方新建一个目录,假如为C:\DLL 然后,在环境变量中,给Path变量添加这个目录 最后,把所有的非托管文件都拷贝到C:\DLL中. 或者更干脆的把DLL放到system32目录 对于可以自己部署的应用程序，这样未偿不是一个解决办法，然而，如果我们用的是虚拟空间，我们是没办法把注册PATH变量或者把我们自己的DLL拷到system32目录的。同时我们也不一定知道我们的Dll的物理路径。 DllImport里面只能用字符串常量，而不能够用Server.MapPath(@"~/Bin/Judge.dll")来确定物理路径。ASP.NET中要使用DllImport的，必须在先“using System.Runtime.InteropServices;”不过，我发现，调用这种"非托管Dll”相当的慢，可能是因为我的方法需要远程验证吧，但是实在是太慢了。经过一翻研究，终于想到了一个完美的解决办法首先我们用

 

 代码如下:  
 ```
[DllImport("kernel32.dll")]
private extern static IntPtr LoadLibrary(String path);
[DllImport("kernel32.dll")]
private extern static IntPtr GetProcAddress(IntPtr lib, String funcName);
[DllImport("kernel32.dll")]
private extern static bool FreeLibrary(IntPtr lib);
```
  
  


   
 

 分别取得了LoadLibrary和GetProcAddress函数的地址，再通过这两个函数来取得我们的DLL里面的函数。  
 我们可以先用Server.MapPath(@"~/Bin/Judge.dll")来取得我们的DLL的物理路径，然后再用LoadLibrary进行载入，最后用GetProcAddress取得要用的函数地址

 以下自定义类的代码完成LoadLibrary的装载和函数调用

 

 代码如下:  
 ```
public class DllInvoke 
    {            
        [DllImport("kernel32.dll")] 
        private extern static IntPtr LoadLibrary(String path);

        [DllImport("kernel32.dll")]   
        private extern static IntPtr GetProcAddress(IntPtr lib, String funcName); 

        [DllImport("kernel32.dll")]     
        private extern static bool FreeLibrary(IntPtr lib);     

        private IntPtr hLib;  
        public DllInvoke(String DLLPath)   
        {           
            hLib = LoadLibrary(DLLPath);  
        }       

        ~DllInvoke()     
        {        
            FreeLibrary(hLib);  
        }        

        //将要执行的函数转换为委托  
        public Delegate Invoke(String APIName,Type t)     
        {           
            IntPtr api = GetProcAddress(hLib, APIName);   
            return (Delegate)Marshal.GetDelegateForFunctionPointer(api,t);     
        }
    }
```
  
  


   
 

 下面代码进行调用

 

 代码如下:  
 ```
public delegate int Compile(String command, StringBuilder inf);
            //编译
            DllInvoke dll ＝ new DllInvoke(Server.MapPath(@"~/Bin/Judge.dll"));
            Compile compile = (Compile)dll.Invoke("Compile", typeof(Compile));
            StringBuilder inf;
            compile(@“gcc a.c -o a.exe“,inf);//这里就是调用我的DLL里定义的Compile函数
```
  
  
 

 大家在实际工作学习C#的时候，可能会问：为什么我们要为一些已经存在的功能（比如Windows中的一些功能，C++中已经编写好的一些方法）要重新编写代码，C#有没有方法可以直接都用这些原本已经存在的功能呢？答案是肯定的，大家可以通过C#中的DllImport直接调用这些功能。   
 DllImport所在的名字空间 using System.Runtime.InteropServices;   
 MSDN中对DllImportAttribute的解释是这样的：可将该属性应用于方法。DllImportAttribute 属性提供对从非托管 DLL   
 导出的函数进行调用所必需的信息。作为最低要求，必须提供包含入口点的 DLL 的名称。 DllImport 属性定义如下：

 

 代码如下:  
 ```
namespace System.Runtime.InteropServices   
 {   　
     [AttributeUsage(AttributeTargets.Method)]   
     public class DllImportAttribute: System.Attribute  
     {   　 
         public DllImportAttribute(string dllName)
         {...}   　 　

         public CallingConvention CallingConvention;   
         public CharSet CharSet;   　
         public string EntryPoint;   　
         public bool ExactSpelling;   　 
         public bool PreserveSig;   　 　
         public bool SetLastError;   　 
         public string Value { get {...} }   　
     }   
 }
```
  
  
 

 说明：   
 1、DllImport只能放置在方法声明上。   
 2、DllImport具有单个定位参数：指定包含被导入方法的 dll 名称的   
 dllName 参数。   
 3、DllImport具有五个命名参数：   
 a、CallingConvention   
 参数指示入口点的调用约定。如果未指定 CallingConvention，则使用默认值   
 CallingConvention.Winapi。  
 b、CharSet 参数指示用在入口点中的字符集。如果未指定 CharSet，则使用默认值   
 CharSet.Auto。   
 c、EntryPoint 参数给出 dll 中入口点的名称。如果未指定   
 EntryPoint，则使用方法本身的名称。   
 d、ExactSpelling 参数指示 EntryPoint   
 是否必须与指示的入口点的拼写完全匹配。如果未指定 ExactSpelling，则使用默认值 false。   
 e、PreserveSig   
 参数指示方法的签名应当被保留还是被转换。当签名被转换时，它被转换为一个具有 HRESULT返回值和该返回值的一个名为 retval   
 的附加输出参数的签名。如果未指定 PreserveSig，则使用默认值 true。   
 f、SetLastError 参数指示方法是否保留   
 Win32"上一错误"。如果未指定 SetLastError，则使用默认值 false。   
 4、它是一次性属性类。   
   
 5、此外，用 DllImport 属性修饰的方法必须具有 extern 修饰符。

   
   
 