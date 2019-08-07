---
title: Linux命令——chmod
date: 2019-05-30 10:04:12
tags: CSDN迁移
---
   ### 命令描述

 [变更文件](http://baike.baidu.com/edit/id=1229012&amp;subLemmaId=1229012&amp;dl=2#)或目录的权限。在UNIX系统家族里，文件或目录权限的控制分别以读取，写入，执行3种一般权限来区分，另有3种特殊权限可供运用，再搭配拥有者与所属群组管理权限范围。您可以使用chmod指令去[变更文件](http://baike.baidu.com/edit/id=1229012&amp;subLemmaId=1229012&amp;dl=2#)与目录的权限，设置方式采用文字或数字代号皆可。符号连接的权限无法变更，如果您对符号连接修改权限，其改变会作用在被连接的原始文件。权限范围的表示法如下：

 u：User，即文件或目录的拥有者。

 g：Group，即文件或目录的所属群组。

 o：Other，除了文件或目录拥有者或所属群组之外，其他用户皆属于这个范围。

 a：All，即全部的用户，包含拥有者，所属群组以及其他用户。

 有关权限代号的部分，列表于下：

 r：读取权限，数字代号为"4"。

 w：写入权限，数字代号为"2"。

 x：执行或切换权限，数字代号为"1"。

 -：不具任何权限，数字代号为"0"。

 s：特殊?b>功能说明：[变更文件](http://baike.baidu.com/edit/id=1229012&amp;subLemmaId=1229012&amp;dl=2#)或目录的权限。

 
### []()语法

 chmod [-cfRv][--help][--version][<权限范围>+/-/=<权限设置...>][文件或目录...]

 chmod [-cfRv][--help][--version][数字代号][文件或目录...]

 chmod [-cfRv][--help][--reference=<参考文件或目录>][--version][文件或目录...]

 
### []()选项说明

 -c或--changes 效果类似"-v"参数，但仅回报更改的部分。

 -f或--quiet或--silent 不显示[错误信息](http://baike.baidu.com/edit/id=1229012&amp;subLemmaId=1229012&amp;dl=2#)。

 -R或--recursive 递归处理，将指定目录下的所有文件及子目录一并处理。

 -v或--verbose 显示指令执行过程。

 --help 在线帮助。

 --reference=<参考文件或目录> 把指定文件或目录的权限全部设成和参考文件或目录的权限相同

 --version 显示版本信息。

 <权限范围>+<权限设置> 开启权限范围的文件或目录的该项权限设置。

 <权限范围>-<权限设置> 关闭权限范围的文件或目录的该项权限设置。

 <权限范围>=<权限设置> 指定权限范围的文件或目录的该项权限设置。

 
### []()范例

 **范例一** ：将档案 file1.txt 设为所有人皆可读取 :

 chmod ugo+r file1.txt

 将档案 file1.txt 设为所有人皆可读取 :

 chmod a+r file1.txt

 将档案 file1.txt 与 file2.txt 设为该档案拥有者，与其所属同一个群体者可写入，但其他以外的人则不可写入 :

 chmod ug+w,o-w file1.txt file2.txt

 将 ex1.设定为只有该档案拥有者可以执行 :

 chmod u+x ex1

 将目前目录下的所有档案与子目录皆设为任何人可读取 :

 chmod -R a+r *

 当其他用户执行oracle的sqlplus这个程序时，他的身份因这个程序暂时变成oracle

 chmod u+s sqlplus

 此外,chmod也可以用数字来表示权限如 chmod 777 file

 语法为：chmod abc file

 其中a,b,c各为一个数字，分别表示User、Group、及Other的权限。

 r=4，w=2，x=1

 若要rwx属性则4+2+1=7；

 若要rw-属性则4+2=6；

 若要r-x属性则4+1=5。

 **范例二**：

 chmod a=rwx file

 和

 chmod 777 file

 效果相同

 chmod ug=rwx,o=x file

 和

 chmod 771 file

 效果相同

 若用chmod 4755 filename可使此程式具有root的权限

 **范例三**：

 如果在cd /media/amasun/java/develop/array之后执行

 chmod 777 ./

 是将本目录(即/media/amasun/java/develop/array)设为任何人可读,写,执行

 如果是[管理员](http://baike.baidu.com/edit/id=1229012&amp;subLemmaId=1229012&amp;dl=2#)也就是常说的ROOT用户的话，基本上有可以查看所有文件的权力.

 

 感谢原作者的分享！

 原文地址：不详

   
 