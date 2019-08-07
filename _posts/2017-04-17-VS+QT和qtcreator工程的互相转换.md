---
title: VS+QT和qtcreator工程的互相转换
date: 2019-02-26 11:08:00
tags: CSDN迁移
---
   QT Creator的project转成Visual Studio的project

 在windows下，运行Qt Command Prompt。

 输入命令行：

 qmake -tp vc XXX.pro

 会生成文件XXX.vcxproj

 Visual Studio的project转成QT Creator的project

 第一步，因为原Visual Studio工程就是基于QT Template的工程，所以我们可以用VS里的QT插件里的"Create basic .pro file..."菜单选项来自动生成一个.pro文件，实际上，不仅仅生成了.pro 文件，还生成了.pri文件。但是目前的.pro文件还是不能用的，因为其中还有大量的配置需要修改，比如INCLUDEPATH和LIBS. 

 第二步，修改.pro文件使其包含正确而必要的配置信息。  
 总结笔者对.pro文件的具体修改如下：  
 1. TEMPLATE = lib  
 2. 将DESTDIR = xxx改成：  
 Release:DESTDIR = xxx/Release  
 Debug:DESTDIR = xxx/Debug  
 3. CONFIG += debug_and_release  
 4. 完善INCLUDEPATH  
 5. 完善DEPENDPATH （这里又有故事了：事后笔者发现，只要在LIBS里写了-L"<LIBPATH>"，就没必要写DEPENDPATH了。）  
 6. 加上 Release:DEPENDPATH 和 Debug:DEPENDPATH  
 7. 类似的，完善LIBS，添加Release:LIBS和Debug:LIBS

 第三步，双击改好的.pro文件，QT Creator打开此工程，选中合适的Kit，就可以build了。因为是64位机器，Kit选的是QT 5.5.1的msvc2013_64.   
 最后千万要注意的是，在系统环境变量%PATH%里，对于Visual Studio的编译器cl.exe和链接器link.exe, 要选对路径。比如，对于64位的机器，路径 C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\bin\amd64 应该被加到%PATH%中，而不是 C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\bin 这一条在前一篇博文中已有详细阐述。

 以上步骤做完后，就可以顺利地在QT Creator中进行build了。  
 最后还要提醒的一点是，如果更改了.pro文件，想让更改生效，必须手动删除qmake生成的Makefile, Makefile.Debug和Makefile.Release文件。

 下面是一个.pro文件的实例：  
 [plain] view plain copy  
 TEMPLATE = lib   
 TARGET = MyProject   
 Release:DESTDIR = ../../../Output/x64/Release   
 Debug:DESTDIR = ../../../Output/x64/Debug   
 QT += core qml   
 CONFIG += debug_and_release   
 DEFINES += WIN64 QT_DLL QT_QML_LIB MyProject_LIB   
   
 INCLUDEPATH += ./GeneratedFiles \   
 . \   
 ./GeneratedFiles/Release \   
 $$(VC_INCLUDE) \   
 $$(VC_INCLUDE)/../atlmfc/include \   
 $$(WINSDK_INCLUDE)/shared \   
 $$(WINSDK_INCLUDE)/um   
   
 DEPENDPATH += . \   
 $$(WINSDK_LIB)/um/x64 \   
 $$(VC_LIB)/amd64 \   
 $$(QTDIR)/lib \   
 $$(QTDIR)/bin \   
   
 Release:DEPENDPATH += ../../../3rd_library/opencv/libs/Release/x64 \   
 ../../../3rd_library/DirectShow/baseclasses/x64/Release   
   
 Debug:DEPENDPATH += ../../../3rd_library/opencv/libs/Debug/x64 \   
 ../../../3rd_library/DirectShow/baseclasses/x64/Debug   
   
 MOC_DIR += ./GeneratedFiles/release   
 OBJECTS_DIR += release   
 UI_DIR += ./GeneratedFiles   
 RCC_DIR += ./GeneratedFiles   
   
 Release:LIBS += -L"../../../3rd_library/DirectShow/baseclasses/x64/Release" \   
 -lstrmbase \   
 -L"../../../3rd_library/opencv/libs/Release/x64" \   
 -lqtmain \   
 -lQt5Qml \   
 -lQt5Core \   
 -lopencv_calib3d248 \   
 -lopencv_contrib248 \   
 -lopencv_core248 \   
 -lopencv_features2d248 \   
 -lopencv_flann248 \   
 -lopencv_gpu248 \   
 -lopencv_highgui248 \   
 -lopencv_imgproc248 \   
 -lopencv_legacy248 \   
 -lopencv_ml248 \   
 -lopencv_nonfree248 \   
 -lopencv_objdetect248 \   
 -lopencv_ocl248 \   
 -lopencv_photo248 \   
 -lopencv_stitching248 \   
 -lopencv_superres248 \   
 -lopencv_ts248 \   
 -lopencv_video248 \   
 -lopencv_videostab248   
   
 Debug:LIBS += -L"../../../3rd_library/DirectShow/baseclasses/x64/Debug" \   
 -lstrmbasd \   
 -L"../../../3rd_library/opencv/libs/Debug/x64" \   
 -lqtmaind \   
 -lQt5Qmld \   
 -lQt5Cored \   
 -lopencv_calib3d248d \   
 -lopencv_contrib248d \   
 -lopencv_core248d \   
 -lopencv_features2d248d \   
 -lopencv_flann248d \   
 -lopencv_gpu248d \   
 -lopencv_highgui248d \   
 -lopencv_imgproc248d \   
 -lopencv_legacy248d \   
 -lopencv_ml248d \   
 -lopencv_nonfree248d \   
 -lopencv_objdetect248d \   
 -lopencv_ocl248d \   
 -lopencv_photo248d \   
 -lopencv_stitching248d \   
 -lopencv_superres248d \   
 -lopencv_ts248d \   
 -lopencv_video248d \   
 -lopencv_videostab248d   
   
 LIBS += -L"$$(WINSDK_LIB)/um/x64" \   
 -L"$$(VC_LIB)/amd64" \   
 -L"$$(QTDIR)/lib" \   
 -L"$$(QTDIR)/bin" \   
 -lWtsapi32 \   
 -lPathcch \   
 -l3DScanningEngine \   
 -lUserenv \   
 -lwinmm \   
 -lMf \   
 -lMfplat   
   
 include(MyProject.pri)   
 ---------------------   
 转载原文：https://blog.csdn.net/lxj434368832/article/details/79558121   
 

   
 