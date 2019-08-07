---
title: PowerDesigner执行sql脚本方式建立数据模型
date: 2016-12-24 15:23:36
tags: CSDN迁移
---
 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/53859428   
   PowerDesigner 相信 经常和数据库打交道的都知道这个强大的工具，主要用来建模，在建立模型时以往我都是直接在里面创建一个模型，然后手动建表，触发器，序列等，其实在PowerDesigner中建立数据模型时，如果有相应的sql脚本，则不必通过在物理模型中逐个插入相应的字段方式建立物理模型。可以通过在PowerDesigner中执行sql语句来建立数据模型。

 下面来看一下具体步骤：

 首先 按照下图打开：

 ![](https://img-blog.csdn.net/20161224151517951?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI2NDIyMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 图——1

 

 然后 会弹出 如下界面：

 ![](https://img-blog.csdn.net/20161224151524076?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI2NDIyMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 图——2

 这里为了演示 我新建了一个 物理模型3 ，我以前用的是物理模型2. 出现图——2所示的对画框 我们可以自己在DSMS这一行选择我们使用的数据库版本，这里我用的是oracle11g ，然后点击确定会出现 如 图——3所示的界面:

 ![](https://img-blog.csdn.net/20161224151529529?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMjI2NDIyMzk=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  


 图——3

 在图——3时，注意红色框表示处，选中（使用脚本文件，都懂得。。），然后点击 红色框下的那个带加号的小图标，添加已经准备好的脚本文件，这里是oracle的脚本文件。点击确定 脚本文件就会导入到当前物理模型中，跟以前手动建立的物理模型一样，这样 就不用做重复的工作了。

   
 