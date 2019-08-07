---
title: vc中format用法以及c++中Format用法
date: 2017-07-03 19:32:19
tags: CSDN迁移
---
   **总结一下Format的用法:  
  
Format('x=%d',[12]);//'x=12'//最普通  
Format('x==',[12]);//'x=12'//指定宽度  
Format('x=%f',[12.0]);//'x=12.00'//浮点数  
Format('x=%.3f',[12.0]);//'x=12.000'//指定小数  
Format('x=%8.2f'[12.0])//'x=12.00';  
Format('x=%.*f',[5,12.0]);//'x=12.00000'//动态配置  
Format('x=%.5d',[12]);//'x=00012'//前面补充0  
Format('x=%.5x',[12]);//'x=0000C'//十六进制  
Format('x=%1:d%0:d',[12,13]);//'x=1312'//使用索引  
Format('x=%p',[nil]);//'x=00000000'//指针  
Format('x=%1.1e',[12.0]);//'x=1.2E+001'//科学记数法  
Format('x=%%',[]);//'x=%'//得到"%"  
S:=Format('%s%d',[S,I]);//S:=S+StrToInt(I);//连接字符串   
**

 **  
**

 

 **vc中format的用法：**

 以CString的Format举例，第一个参数是格式化字符串，就像printf的第一个参数一样，用%d表示int，%s表示char*，%u表示unsigned int，%hd表示short，%hu表示unsigned short，%hhd表示char，%hhu表示unsigned char，%f表示float等。后面的参数就是与格式化字符串中每个字段对应的类型变量。  
 举例：  
 int a = 10;  
 int b = 100;  
 CString str;  
 str.Format("%d*%d=%d\n", a, b, a * b);  
 那么输出就是10 * 100 = 1000

 该函数就是将CString对象设置为指定的字符串，以便后续处理。

 所以如果想在AfxMessageBox（）显示的消息对话框显示的内容不一定是固定的 可以这样做：

 CString a；

 a.format("%d*%d=%d\n", a, b, a * b);

 AfxMessageBox（a）;

 如果是在unicode环境下：

 CString a；

 a.format(_T("%d*%d=%d\n"),a,b,a*b);

 如果是先定义的char类型的变量，转成CString类型，再用AfxMessageBox（）输出的话，并且是在unicode环境下，应该是

 TCHAR array[]=_T(“AAA”);  
 CString str;  
 str.Format( _T("%s"), array ); //不加_T报错，工程是UNICODE的  
 AfxMessageBox(str);

 **c++中Format的用法**

 首先看它的声明：  
 function Format(const Format: string; const Args: array of const): string; overload;  
  
 事实上Format方法有两个种形式，另外一种是三个参数的，主要区别在于它是线程安全的，  
 但并不多用，所以这里只对第一个介绍：  
 function Format(const Format: string; const Args: array of const): string; overload;  
   
 Format参数是一个格式字符串，用于格式化Args里面的值的。Args又是什么呢，  
 它是一个变体数组，即它里面可以有多个参数，而且每个参数可以不同。  
 如以下例子：  
  
 Format('my name is %6s',['wind']);  
 返回后就是my name is wind  
  
 现在来看Format参数的详细情况：  
 Format里面可以写普通的字符串，比如'my name is',但有些格式指令字符具有特殊意义，比如"%6s"格式指令具有以下的形式：  
 "%" [index ":"] ["-"] [width] ["." prec] type  
 它是以"%"开始,而以type结束，type表示一个具体的类型。中间是用来  
 格式化type类型的指令字符，是可选的。  
  
 先来看看type,type可以是以下字符：  
 d 十制数，表示一个整型值  
 u 和d一样是整型值，但它是无符号的，而如果它对应的值是负的，则返回时是一个2的32次方减去这个绝对值的数,如：  
 Format('this is %u',[－2]);  
 返回的是：this is 4294967294  
  
 f 对应浮点数  
 e 科学表示法，对应整型数和浮点数，比如  
 Format('this is %e',[-2.22]);  
 返回的是：this is -2.22000000000000E+000,等一下再说明如果将数的精度缩小  
  
 g 这个只能对应浮点型，且它会将值中多余的数去掉,比如  
 Format('this is %g',[02.200]);  
 返回的是：this is 2.2  
  
 n 只能对应浮点型，将值转化为号码的形式。看一个例子就明白了  
 Format('this is %n',[4552.2176]);  
 返回的是this is 4,552.22  
  
 注意有两点，一是只表示到小数后两位，等一下说怎么消除这种情况, 二是，即使小数没有被截断，它也不会也像整数部分一样有逗号来分开的  
  
 m钱币类型，但关于货币类型有更好的格式化方法，这里只是简单的格式化,另外它只对应于浮点值  
 Format('this is %m',[9552.21]);  
 返回：this is ￥9,552.21  
  
 p 对应于指针类型，返回的值是指针的地址，以十六进制的形式来表示  
 例如：  
 var X:integer;  
 p:^integer;  
 begin  
 X:=99;  
 p:=@X;  
 Edit1.Text:=Format('this is %p',[p]);  
 end;  
 Edit1的内容是：this is 0012F548  
  
 s 对应字符串类型，不用多说了吧  
 x 必须是一个整形值，以十六进制的形式返回  
 Edit1.Text:=Format('this is %X',[15]);  
 返回是：this is F  
  
 类型讲述完毕，下面介绍格式化Type的指令：  
 [index ":"]这个要怎么表达呢，看一个例子  
 Format('this is %d %d',[12,13]);  
 其中第一个%d的索引是0，第二个%d是1，所以字符显示的时候是这样 this is 12 13  
  
 而如果你这样定义：  
 Format('this is %1:d %0:d',[12,13]);  
 那么返回的字符串就变成了this is 13 12。现在明白了吗，[index ":"] 中的index指示Args中参数显示的顺序还有一种情况，如果这样  
 Format('%d %d %d %0:d %d', [1, 2, 3, 4])  
 将返回1 2 3 1 2。  
  
 如果你想返回的是1 2 3 1 4，必须这样定：  
 Format('%d %d %d %0:d %3:d', [1, 2, 3, 4])  
  
 但用的时候要注意，索引不能超出Args中的个数，不然会引起异常如  
 Format('this is %2:d %0:d',[12,13]);  
 由于Args中只有12 13 两个数，所以Index只能是0或1，这里为2就错了[width] 指定将被格式化的值占的宽度，看一个例子就明白了  
  
 Format('this is M',[12]);  
 输出是：this is 12,这个是比较容易，不过如果Width的值小于参数的长度，则没有效果。  
 如：  
  
 Format('this is ',[12]);  
 输出是：this is 12  
  
 ["-"]这个指定参数向左齐，和[width]合在一起最可以看到效果：  
 Format('this is %-4d,yes',[12]);  
 输出是：this is 12 ,yes  
  
 ["." prec] 指定精度，对于浮点数效果最佳：  
 Format('this is %.2f',['1.1234]);  
 输出 this is 1.12  
 Format('this is %.7f',['1.1234]);  
 输出了 this is 1.1234000  
  
 而对于整型数，如果prec比如整型的位数小，则没有效果反之比整形值的位数大，则会在整型值的前面以0补之  
 Format('this is %.7d',[1234]);  
 输出是：this is 0001234]  
    
 对于字符型，刚好和整型值相反，如果prec比字符串型的长度大则没有效果，反之比字符串型的长度小，则会截断尾部的字符  
 Format('this is %.2s',['1234']);  
 输出是 this is 12,而上面说的这个例子：  
  
 Format('this is %e',[-2.22]);  
 返回的是：this is -2.22000000000000E+000,怎么去掉多余的0呢，这个就行啦  
  
 Format('this is %.2e',[-2.22]);  
   
 好了，第一个总算讲完了，应该对他的应用很熟悉了吧  
  
 ///////////////////////////////////////////////////////////////  
 二 FormatDateTime的用法  
 他的声明为：  
  
 function FormatDateTime(const Format: string; DateTime: TDateTime): string;   
 overload;  
  
 当然和Format一样还有一种，但这里只介绍常用的第一种,Format参数是一个格式化字符串。DateTime是时间类型。返回值是一种格式化后的字符串,重点来看Format参数中的指令字符  
  
 c 以短时间格式显示时间，即全部是数字的表示  
 FormatdateTime('c',now);  
 输出为：2004-8-7 9:55:40  
  
 d 对应于时间中的日期，日期是一位则显示一位，两位则显示两位  
 FormatdateTime('d',now);  
 输出可能为1～31  
  
 dd 和d的意义一样，但它始终是以两位来显示的  
 FormatdateTime('dd',now);  
 输出可能为01～31  
  
 ddd 显示的是星期几  
 FormatdateTime('ddd',now);  
 输出为: 星期六  
  
 dddd 和ddd显示的是一样的。 但上面两个如果在其他国家可能不一样。ddddd 以短时间格式显示年月日   
 FormatdateTime('ddddd',now);  
 输出为：2004-8-7  
  
 dddddd 以长时间格式显示年月日  
 FormatdateTime('dddddd',now);   
 输出为：2004年8月7日  
  
 e/ee/eee/eeee 以相应的位数显示年  
 FormatdateTime('ee',now);   
 输出为：04 （表示04年）  
  
 m/mm/mmm/mmmm 表示月  
 FormatdateTime('m',now);  
 输出为：8  
 FormatdateTime('mm',now);  
 输出为 08  
 FormatdateTime('mmm',now);  
 输出为 八月  
 FormatdateTime('mmmm',now);   
 输出为 八月  
  
 和ddd/dddd 一样，在其他国家可能不同yy/yyyy 表示年  
 FormatdateTime('yy',now);  
 输出为 04  
 FormatdateTime('yyyy',now);  
 输出为 2004,  
  
 h/hh,n/nn,s/ss,z/zzz 分别表示小时，分，秒,毫秒  
  
 t 以短时间格式显示时间  
 FormatdateTime('t',now);  
 输出为 10:17  
  
 tt 以长时间格式显示时间  
 FormatdateTime('tt',now);  
 输出为10:18:46  
  
 ampm 以长时间格式显示上午还是下午  
 FormatdateTime('ttampm',now);  
 输出为：10:22:57上午  
  
 大概如此，如果要在Format中加普通的字符串，可以用双引号隔开那些特定义的字符，这样普通字符串中如果含特殊的字符就不会被显示为时间格式啦：  
 FormatdateTime('"today is" c',now);  
 输出为：today is 2004-8-7 10:26:58  
  
 时间中也可以加"-"或"\"来分开日期：  
 FormatdateTime('"today is" yy-mm-dd',now);  
 FormatdateTime('"today is" yy\mm\dd',now);  
 输出为： today is 04-08-07  
  
 也可以用":"来分开时间   
 FormatdateTime('"today is" hh:nn:ss',now);  
 输出为：today is 10:32:23  
  
 /////////////////////////////////////////////////////////////////  
 三.FormatFloat的用法  
  
 常用的声明：  
 function FormatFloat(const Format: string; Value: Extended): string; overload;  
  
 和上面一样Format参数为格式化指令字符，Value为Extended类型为什么是这个类型，因为它是所有浮点值中表示范围最大的，如果传入该方法的参数比如Double或者其他，则可以保存不会超出范围。  
  
 关键是看Format参数的用法  
 0 这个指定相应的位数的指令。  
 比如：  
 FormatFloat('000.000',22.22);  
 输出的就是022.220  
   
 注意一点，如果整数部分的0的个数小于Value参数中整数的位数，则没有效果如：  
 FormatFloat('0.00',22.22);  
 输出的是：22.22  
  
 但如果小数部分的0小于Value中小数的倍数，则会截去相应的小数和位数如：  
 FormatFloat('0.0',22.22);  
 输出的是：22.2  
   
 也可以在整数0中指定逗号，这个整数位数必须大于3个，才会有逗号出句  
 FormatFloat('0,000.0',2222.22);  
 输出是：2,222.2  
  
 如果这样  
 FormatFloat('000,0.0',2222.22);  
 它的输出还是：2,222.2  
  
 注意它的规律,#和0的用法一样，目前我还没有测出有什么不同。  
  
 FormatFloat('##.##',22.22);  
 输出是：22.00  
  
 E 科学表示法，看几个例子大概就明白了  
 FormatFloat('0.00E+00',2222.22);  
 输出是 2.22E+03  
 FormatFloat('0000.00E+00',2222.22);  
 输出是 2222.22E+00  
 FormatFloat('00.0E+0',2222.22);  
 22.2E+2  
 明白了吗，全靠E右边的0来支配的。  
   
 这个方法并不难，大概就是这样子了。  
  
 上面三个方法是很常用的，没有什么技巧，只要记得这些规范就行了。  
  
  


   
 