---
title: C++中new和delete来创建和释放动态数组
date: 2018-11-30 14:57:54
tags: CSDN迁移
---
   在C++编程中，使用new创建数组然后用delete来释放。

 一、创建并释放一维数组

 
```
 #include<iostream>
using namespace std;
int main()
{
    int n;
    cin>>n;
    //分配动态一维数组 
    int *arr=new int[n];
    
    for(int i=0;i<n;i++)
        cin>>arr[i];
    for(int i=0;i<n;i++)
       cout<<arr[i]<<" ";
    //释放arr数组 
    delete[] arr;
    return 0;
}
```
 注意：delete后面的[]不能少。

 **二**、创建并释放二维数组

 
```
 #include<iostream>
using namespace std;
int main()
{
    int row,col;
    cin>>row>>col;
    //为行指针分配空间 
    int **arr=new int *[row];    
    for(int i=0;i<row;i++)
         arr[i]= new int[col];//为每行分配空间（每行中有col个元素） 
    //输入二维数组的数 
    for(int i=0;i<row;i++)
        for(int j=0;j<col;j++) 
        cin>>arr[i][j];
    cout<<"*******************"<<endl;
     //输出二维数组中的数  
    for(int i=0;i<row;i++)
    {
         for(int j=0;j<col;j++) 
          cout<<arr[i][j]<<" ";
        cout<<endl;
    } 
    //释放二维数组（反过来） 
    for(int i=0;i<row;i++)
        delete[] arr[i]; 
    delete[] arr;
    return 0;
}
```
 三、new创建类

 
```
 ”new”是C++的一个关键字，同时也是操作符。关于new的话题非常多，因为它确实比较复杂，也非常神秘 。 new的过程 当我们使用关键字new在堆上动态创建一个对象时，它实际上做了三件事：1、获得一块内存空间 2、调用构造函 数 3、返回正确的指针。当然，如果我们创建的是简单类型的变量，那么第二步会被省略。假如我们定义了如 下一个类A： class A { int i; public: A(int _i) :i(_i*_i) {} void Say() { printf("i=%dn", i); } }; //调用new： A* pa = new A(3); 那么上述动态创建一个对象的过程大致相当于以下三句话（只是大致上）： A* pa = (A*)malloc(sizeof(A)); pa->A::A(3); return pa; 虽然从效果上看，这三句话也得到了一个有效的指向堆上的A对象的指针pa，但区别在于，当malloc失 败时，它不会调用分配内存失败处理程序new_handler，而使用new的话会的。因此我们还是要尽可能的使用 new，除非有一些特殊的需求。
```
   
 