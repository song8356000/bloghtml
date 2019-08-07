---
title: CMakeLists.txt 介绍
date: 2018-09-19 11:04:27
tags: CSDN迁移
---
   ### **CMake 简要内容**

 CMake 是cross platform make的简写，从这里你完全可以看出，CMake是基于Make来实现相关的内容的，换句话说，CMake就是在Make的基础上抽象出来的更高级的框架。

 CMakeLists.txt的编译test.cpp生成test可执行文件的基本例子：

 
```
 cmake_minimum_required(VERSION 2.8.10)
SET(PROJECT_NAME test)
project(${PROJECT_NAME})
add_executable(test test.cpp)  
```
 通过以下Shell Command:

 
```
 mkdir -p build && cd build && cmake .. && make 
```
 通过上文的shell命令，其实你也已经发现了，cmake会生成Makefile，然后我们需要调用make来生成可执行文件。

 
### **[]()CMakeLists.txt 编写要点**

 **常用的cmake指令解释**

 
```
 cmake_minimum_required(VERSION xxx) #cmake最小版本需求，新版本的cmake改了很多东西，提升了便利性，也可能让你自己挖坑了
project(xxx) #设置此项目的名称
add_executable(target target_source_codes) #生成可执行文件target ，后面填写的是生成此可执行文件所依赖的源文件列表。
SET(var_name var_value)# 设置一个名字var_name 的变量，同时给此变量赋值为var_value
MESSAGE("MSG") #类比echo 打印消息
option(var_name "comment" var_value) #给变量var_name赋值为var_value，comment是此变量的注释，和SET 有类似的功效，用于给某变量设置默认值
include_directories(xxx) #添加include路径，也就是 gcc -I xxx 的意思，或者vs ide中添加头文件包含目录
add_subdirectory(xxx) #调用xxx子目录的CMakeLists.txt执行
add_compile_options(xxx) #给编译器添加xxx参数，但是貌似没有什么用，我一般不这样添加参数，不直接
link_directories(xxx) #给编译器添加库目录，也就是 gcc -L xxx 的意思，或者vs ide中添加库的包含目录
add_library(lib_name SHARED or STATIC lib_source_code) #和add_executable类似，生成库文件，SHARED代表动态库，STATIC代表静态库， 最后一个参数代表此库的源文件列表，此指令只有三个参数
target_link_libraries(target_name lib_name ...) #给目标添加依赖库，类似与gcc -l lib_name，此指令有两个用处，一个是给可执行target_name 添加库依赖，二是给库target_name 添加库依赖。
```
 我常见的cmake指令也就是上述的这些，还有部分比较常见的指令这里没有列出，我放到了下面单独讲解如：install()

 **cmake 流控制指令相关**

 条件语句

 
```
 if(xxx)
...
elseif(xx)
...
else()
...
endif()

#常见条件语句用法为:
# if (va)  va为bool型
# if （va MATCHES xxx） va 是string类型，如果va包含了xxx，则此句为真
```
 循环语句

 
```
 foreach(va va_lists)
...
endforeach()
```
 在foreach中，va的值会依次被va_lists的值替换

 **macro 和 function**

 
```
 macro(name arg ...)
...
endmacro()
function(name arg ...)
...
endfunction()
```
 宏和函数效果都类似，唯一区别为function中的变量为局部的。

 **install 指令（主要是生成Makefile中的install target）**

 
```
 install(FILES flie DESTINATION dir_path) #执行make install时，把file拷贝到dir_path
install(PROGRAMS file DESTINATION dir_path) #执行make install时，把file拷贝到dir_path,并给予file可执行权限
INSTALL(TARGETS  ylib ylib_s
    #RUNTIME DESTINATION xxx
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)# 安装libylib.so到lib目录，安装libylib_s.a到lib目录，RUNTIME 是安装可执行文件到xxx目录，注意这个指令有个坑，我后面会说明这个问题。
```
 **configure_file指令**

 
```
 configure_file(fileA fileB @ONLY)
#把fileA 复制并重命名为fileB,此时，fileA中的@var@的值会被替换为cmakelists.txt 中var的值。@ONLY是只转换@va@这种变量
```
 **CMakeLists.txt常用的内置变量**

 
```
 CMAKE_INSTALL_PREFIX  #make install 的安装路径
CMAKE_BUILD_TYPE #生成的目标为debug或者release
CMAKE_C_FLAGS #gcc 的编译参数指定，这个非常好用，一般通过set 修改其值
CMAKE_CXX_FLAGS #g++ 和上面CMAKE_C_FLAGS 类似
CMAKE_CURRENT_SOURCE_DIR # 当前CMakeLists.txt所在的目录，主要用来定位某文件
CMAKE_CURRENT_BINARY_DIR # 当前CMakeLists.txt对应的编译时的目录
```
 **XXXConfig.cmake文件（cmake模块文件）编写以及引用**

 yLibConfig.cmake

 
```
 find_path(yLib_INCLUDE_DIR NAMES ylib.h PATHS @CMAKE_INSTALL_PREFIX@/include) 

find_library(yLib_LIBRARY NAMES ylib PATHS @CMAKE_INSTALL_PREFIX@/lib) 
#find_library 会到@CMAKE_INSTALL_PREFIX@/lib目录查询libylib.so


set(yLib_FOUND TRUE) 
set(yLib_INCLUDE_DIRS ${yLib_INCLUDE_DIR}) 
set(yLib_LIBS ${yLib_LIBRARY}) 


mark_as_advanced(yLib_INCLUDE_DIRS yLib_LIBS )
```
 XXX_INCLUDE_DIR   
 XXX_LIBRARY   
 XXX_FOUND   
 XXX_INCLUDE_DIRS   
 XXX_LIBS   
 以上变量最好都定义了，不然find_package可能会报错   
 .cmake 文件就是定义了相关include变量和lib变量，没有什么其他的东西

 调用：

 
```
 set(yLib_DIR "@CMAKE_INSTALL_PREFIX@/cmake")
#设置.cmake 的目录所在
find_package(yLib REQUIRED)
#find_package会导入.cmake 中的相关变量，完成相关模块的导入
```
 **一个关于install（）指令的深坑**

 
```
 INSTALL(TARGETS  ylib ylib_s
    #RUNTIME DESTINATION xxx
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)
#对于RUNTIME  和 LIBRARY 两种目标，在安装时候，cmake会默认给你移除掉目标文件中的gcc的Wl,rpath的值，导致某些库找不到的错误。
以下变量会影响此坑，更详细的信息去查查别的资料，我这里就不详细说明了。
#set(CMAKE_SKIP_BUILD_RPATH FALSE)                
#set(CMAKE_BUILD_WITH_INSTALL_RPATH FALSE)        
#set(CMAKE_INSTALL_RPATH "")                      
#set(CMAKE_INSTALL_RPATH_USE_LINK_PATH TRUE)    
#set(CMAKE_SKIP_INSTALL_RPATH TRUE)
#set(CMAKE_INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib")

#set(CMAKE_SKIP_RPATH TRUE)
#set(CMAKE_SKIP_INSTALL_RPATH TRUE)
```
 注意：cmake会直接修改你的二进制文件替换掉rpath的相关信息。默认替换的值是一个空值，也就是说移除掉了你设置的rpath的值

 

 转载地址：[https://blog.csdn.net/u011728480/article/details/81480668](https://blog.csdn.net/u011728480/article/details/81480668)

   
 