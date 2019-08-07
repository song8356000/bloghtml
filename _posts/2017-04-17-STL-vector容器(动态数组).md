---
title: STL-vector容器(动态数组)
date: 2019-03-13 17:38:14
tags: CSDN迁移
---
   简介：  
 vector是将元素置于一个动态数组中进行管理的容器  
 vector可以随机存取元素，支持索引值直接存取，用[]或者at()方法  
 vector下尾部添加或者删除元素非常快，但在中间或头部插入或者删除元素比较耗时

 头文件：  
 #include<vector>

 vector基本操作：  
 vector<int> v;  
 v.push_back(1); // 从尾部插入元素  
 int b = v.front(); // 获取头部元素  
 int a = v.back(); // 获取尾部元素  
 a = v.pop_back(); //弹出并删除尾部元素  
 v.size(); //元素长度  
 v.front() = 11; //修改头部元素的值  
 v.back() = 41; //修改尾部元素的值

 vector初始化：  
 vector<int> v;  
 v.push_back(1);   
 v.push_back(3);  
   
 vector<int> v2 = v;  
 vector<int> v3(v.begin(), v.end());  
 vector<int> v4(3);//事先分配3个元素空间，默认初始化为0  
 vector<int> v4(3, 9); // 三个元素都是9

 vector遍历：  
 vector<int> v(10); //当用数组的方式遍历并且是给vector赋值的时候，必须先分配好空间

 // push_back强化  
 v.push_back(1); //此时在第11个位置插入，因为push_back是在尾部插入的  
 v.push_back(3);//此时在第12个位置插入，因为push_back是在尾部插入的

 //数组方式  
 for(int i = 0; i < v.size(); i++) {  
 v[i] = i + 1;   
 }

 //迭代器方式-正向迭代  
 for(vector<int>::iterator it = v.begin(); it != v.end(); it++) {  
 cout << *it << " ";  
 }  
 //迭代器方式-逆向迭代  
 for(vector<int>::reverse_iterator rit = v.rbegin(); rit != v.rend(); rit++) {  
 cout << *rit << " ";  
 }

 迭代器强化：  
 1. v.begin()指向v的第一个元素的位置，v.end()指向的是v最后一个元素的下一个位置  
 2. 迭代器种类：正向迭代器、逆向迭代器、双向迭代器、只读迭代器、、、、

 vector删除和插入：  
 vector<int> v(10);  
 for(int i = 0; i < v.size(); i++) {  
 v[i] = i + 1;   
 }

 //根据元素的位置删除  
 v.erase(v.begin(), v.begin() + 4); //区间删除  
 v.erase(v.begin() + 4); //删除指定位置单个元素

 //根据值删除  
 for(vector<int>::iterator it = v.begin(); it != v.end(); ) {  
 if( *it == 2) {  
 it = v.erase(it); //erase函数会让迭代器自动下移  
 }else {  
 it++;  
 }  
 }  
 //插入  
 vector<int> v1(2, 7);  
 v.insert(v.begin()+3, 100);//在某个位置插入100  
 v.insert(v.begin()+3, 3, 100);//在某个位置插入3个100  
 v.insert(v.begin()+2, v1.begin(), v1.end()); //在某个位置插入一段数据  
  
 原文：https://blog.csdn.net/tangwei2014/article/details/49311969   
 

   
 