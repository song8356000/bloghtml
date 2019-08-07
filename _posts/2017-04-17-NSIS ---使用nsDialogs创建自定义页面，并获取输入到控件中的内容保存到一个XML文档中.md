---
title: NSIS ---使用nsDialogs创建自定义页面，并获取输入到控件中的内容保存到一个XML文档中
date: 2017-03-09 17:54:36
tags: CSDN迁移
---
 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/60963120   
   最近在做安装包的的时候结识了NSIS，只能用神器来形容。通过NSIS可以生成简单的安装包模板，也可以自定义生成各种页面，风格等等。

 下面计入正题，NSIS现在我知道 有两种方法可以定义自定义页面:

 1.通过写INI文件来编写自定义界面。

 2.通过NSDialogs插件来实现。

 第一种方法我以前也经常用，但是最近在编写自定义界面，需要在页面自定义控件（文本编辑控件）为了输入一些安装工具的配置信息，需要获取控件的输入内容，这样的话感觉使用INI这种方式太生硬，需要在INI文件中配置state的值作为输入到控件的内容，这样的话，不能实时的将输入到控件里的内容直接获取，还需要配置INI文件，这样就太麻烦了（有高手有其它用法的话，可以指教一下），使用MFC的话估计大家都很轻松地解决这个问题，不就是一个控件变量的事情么，一点也不复杂，但是使用NSIS的话，我自己感觉稍微麻烦一点（水平有限么。。。）。

 这次我是采用的第二种方法，使用NSDialogs这个插件来完成，因为这个插件里面定义好了许多函数，可以方便的使用。

 下面不废话，直接看一下我的NSIS实现的代码，我为了做一个简单的测试，安装的是一个PEID的工具，这个工具比较简单只有一个可执行文件，没有其它文件，下面是我在生成的模板中加的。

 

 
```
; 该脚本使用 HM VNISEdit 脚本编辑器向导产生

; 安装程序初始定义常量
!define PRODUCT_NAME "My application"
!define PRODUCT_VERSION "1.0"
!define PRODUCT_PUBLISHER "My company, Inc."
!define PRODUCT_WEB_SITE "http://www.mycompany.com"
!define PRODUCT_DIR_REGKEY "Software\Microsoft\Windows\CurrentVersion\App Paths\AppMainExe.exe"
!define PRODUCT_UNINST_KEY "Software\Microsoft\Windows\CurrentVersion\Uninstall\${PRODUCT_NAME}"
!define PRODUCT_UNINST_ROOT_KEY "HKLM"
!define PRODUCT_STARTMENU_REGVAL "NSIS:StartMenuDir"
!include "MUI2.nsh"
!include "nsDialogs.nsh"                   ;注意此处必须要包含nsDialogs.nsh这个头文件，这个文件可以在NSIS的安装目录下的include文件夹下找到，也可自行下载。

SetCompressor lzma

; ------ MUI 现代界面定义 (1.67 版本以上兼容) ------
!include "MUI.nsh"

; MUI 预定义常量
!define MUI_ABORTWARNING
!define MUI_ICON "${NSISDIR}\Contrib\Graphics\Icons\modern-install.ico"
!define MUI_UNICON "${NSISDIR}\Contrib\Graphics\Icons\modern-uninstall.ico"

; 欢迎页面
!insertmacro MUI_PAGE_WELCOME
; 许可协议页面
;!insertmacro MUI_PAGE_LICENSE "c:\path\to\licence\YourSoftwareLicence.txt"
; 组件选择页面
!insertmacro MUI_PAGE_COMPONENTS
Page Custom test                                 ;这里就是自定义页面插入的语句
Var dialog                                       ;下面这几个都是自己定义的变量，这是NSIS自定义变量的格式
Var editcontrol
Var label
Var hwnd 
Var User                                         ;这个变量用来接收输入到控件中的文本内容
; 安装目录选择页面
!insertmacro MUI_PAGE_DIRECTORY
; 开始菜单设置页面
var ICONS_GROUP
!define MUI_STARTMENUPAGE_NODISABLE
!define MUI_STARTMENUPAGE_DEFAULTFOLDER "My application"
!define MUI_STARTMENUPAGE_REGISTRY_ROOT "${PRODUCT_UNINST_ROOT_KEY}"
!define MUI_STARTMENUPAGE_REGISTRY_KEY "${PRODUCT_UNINST_KEY}"
!define MUI_STARTMENUPAGE_REGISTRY_VALUENAME "${PRODUCT_STARTMENU_REGVAL}"
!insertmacro MUI_PAGE_STARTMENU Application $ICONS_GROUP
; 安装过程页面
!insertmacro MUI_PAGE_INSTFILES
; 安装完成页面
!define MUI_FINISHPAGE_RUN "$INSTDIR\AppMainExe.exe"
!insertmacro MUI_PAGE_FINISH

; 安装卸载过程页面
!insertmacro MUI_UNPAGE_INSTFILES

; 安装界面包含的语言设置
!insertmacro MUI_LANGUAGE "SimpChinese"

; 安装预释放文件
;!insertmacro MUI_RESERVEFILE_INSTALLOPTIONS
; ------ MUI 现代界面定义结束 ------

Name "${PRODUCT_NAME} ${PRODUCT_VERSION}"
OutFile "Setup.exe"
InstallDir "$PROGRAMFILES\My application"
InstallDirRegKey HKLM "${PRODUCT_UNINST_KEY}" "UninstallString"
ShowInstDetails show
ShowUnInstDetails show
Section one                                      ;这个区段是用来将从控件获取到的内容转换为XML文件里的某一项，然后保存更改后的XML文件到指定路径。
RegDLL E:\NSIS\Plugins\nsisXML.dll
nsisXML::create
        nsisXML::load C:\Users\sxl\Desktop\Sample.xml       ;这里是为了加载一个现有的XML文件，通过在自定义页面的控件中输入所需文本，并将输入的文本保存到
        StrCpy $2 "add"                                     ;加载的XML文件中。
        nsisXML::select /configuration/appSettings/add[0]   ;nsisxml插件的函数用来选择需要更改的XML文件的节点内容，add[0]就是节点的第一个元素。
        nsisXML::setAttribute "key" $User
        nsisXML::save "11.xml"                           ;nsisxml插件的函数，用来将更改后的XML文件保存到指定路径，不指定路径的话保存到，当前的项目文件下
       ; nsisXML::release $2
SectionEnd

Section "1" SEC01
  SetOutPath "$INSTDIR"
  SetOverwrite ifnewer


; 创建开始菜单快捷方式
  !insertmacro MUI_STARTMENU_WRITE_BEGIN Application
  CreateDirectory "$SMPROGRAMS\$ICONS_GROUP"
  CreateShortCut "$SMPROGRAMS\$ICONS_GROUP\My application.lnk" "$INSTDIR\AppMainExe.exe"
  CreateShortCut "$DESKTOP\My application.lnk" "$INSTDIR\AppMainExe.exe"
  !insertmacro MUI_STARTMENU_WRITE_END
SectionEnd

Section "2" SEC02
  File "PEiD.exe"

; 创建开始菜单快捷方式
  !insertmacro MUI_STARTMENU_WRITE_BEGIN Application
  !insertmacro MUI_STARTMENU_WRITE_END
SectionEnd


Section -AdditionalIcons
  !insertmacro MUI_STARTMENU_WRITE_BEGIN Application
  WriteIniStr "$INSTDIR\${PRODUCT_NAME}.url" "InternetShortcut" "URL" "${PRODUCT_WEB_SITE}"
  CreateShortCut "$SMPROGRAMS\$ICONS_GROUP\Website.lnk" "$INSTDIR\${PRODUCT_NAME}.url"
  CreateShortCut "$SMPROGRAMS\$ICONS_GROUP\Uninstall.lnk" "$INSTDIR\uninst.exe"
  !insertmacro MUI_STARTMENU_WRITE_END
SectionEnd

Section -Post
  WriteUninstaller "$INSTDIR\uninst.exe"
  WriteRegStr HKLM "${PRODUCT_DIR_REGKEY}" "" "$INSTDIR\AppMainExe.exe"
  WriteRegStr ${PRODUCT_UNINST_ROOT_KEY} "${PRODUCT_UNINST_KEY}" "DisplayName" "$(^Name)"
  WriteRegStr ${PRODUCT_UNINST_ROOT_KEY} "${PRODUCT_UNINST_KEY}" "UninstallString" "$INSTDIR\uninst.exe"
  WriteRegStr ${PRODUCT_UNINST_ROOT_KEY} "${PRODUCT_UNINST_KEY}" "DisplayIcon" "$INSTDIR\AppMainExe.exe"
  WriteRegStr ${PRODUCT_UNINST_ROOT_KEY} "${PRODUCT_UNINST_KEY}" "DisplayVersion" "${PRODUCT_VERSION}"
  WriteRegStr ${PRODUCT_UNINST_ROOT_KEY} "${PRODUCT_UNINST_KEY}" "URLInfoAbout" "${PRODUCT_WEB_SITE}"
  WriteRegStr ${PRODUCT_UNINST_ROOT_KEY} "${PRODUCT_UNINST_KEY}" "Publisher" "${PRODUCT_PUBLISHER}"
SectionEnd

#-- 根据 NSIS 脚本编辑规则，所有 Function 区段必须放置在 Section 区段之后编写，以避免安装程序出现未可预知的问题。--#

; 区段组件描述
!insertmacro MUI_FUNCTION_DESCRIPTION_BEGIN
  !insertmacro MUI_DESCRIPTION_TEXT ${SEC01} ""
  !insertmacro MUI_DESCRIPTION_TEXT ${SEC02} ""
!insertmacro MUI_FUNCTION_DESCRIPTION_END

/******************************
 *  以下是安装程序的卸载部分  *
 ******************************/

Section Uninstall
  !insertmacro MUI_STARTMENU_GETFOLDER "Application" $ICONS_GROUP
  Delete "$INSTDIR\${PRODUCT_NAME}.url"
  Delete "$INSTDIR\uninst.exe"
  Delete "$INSTDIR\PEiD.exe"
  Delete "$INSTDIR\Example.file"
  Delete "$INSTDIR\AppMainExe.exe"

  Delete "$SMPROGRAMS\$ICONS_GROUP\Uninstall.lnk"
  Delete "$SMPROGRAMS\$ICONS_GROUP\Website.lnk"
  Delete "$DESKTOP\My application.lnk"
  Delete "$SMPROGRAMS\$ICONS_GROUP\My application.lnk"

  RMDir "$SMPROGRAMS\$ICONS_GROUP"

  RMDir "$INSTDIR"

  DeleteRegKey ${PRODUCT_UNINST_ROOT_KEY} "${PRODUCT_UNINST_KEY}"
  DeleteRegKey HKLM "${PRODUCT_DIR_REGKEY}"
  SetAutoClose true
SectionEnd

#-- 根据 NSIS 脚本编辑规则，所有 Function 区段必须放置在 Section 区段之后编写，以避免安装程序出现未可预知的问题。--#
Var SHOW_PAGE  # 是否显示自定义页面
Function .onInit
  StrCpy $SHOW_PAGE "show" # 初始化设显示自定义页面, 如果你默认不打勾,请用 StrCpy $SHOW_PAGE ""
FunctionEnd

Function PageInitFunc
  StrCmp $SHOW_PAGE "show" 0 +2 # 如果没有设置“show”则跳过下面的页面显示
FunctionEnd                                       
Function test       ;这是一个函数的定义，这个函数名字是test，这个函数是为了生成一个页面，页面里面可以自定义控件，就如同MFC里面的控件一样
    nsDialogs::Create 1018            
    Pop $dialog                         ;创建一个对话框后会返回这个对话框的HWND（同句柄）到堆栈，这个HWND必须保存到某个自定义变量中去，否则会被覆盖掉，
                                        ;  这个HWND可以保存在自定义变量中，为了以后对该控件进行其它操作。
    ${NSD_CreateText} 0% 3% 100% 8% "Testing"
        Pop $editcontrol                                 ;同样创建一个控件也会返回一个HWND到堆栈中，可以保存到自定义变量中去，我就是把这个文本控件的返回值
        ${NSD_OnChange} $editcontrol editchanged         ;从堆栈中Pop到自定义变量，本例中是将其转存在hwnd这个自定义变量中去。

    ;${NSD_CreateLabel} 0% 12% 100% 70% "Testing"
        ;Pop $label

    nsDialogs::Show
FunctionEnd

Function editchanged
    Pop $hwnd                        ;将文本编辑控件的HWND保存到$hwnd这个变量中去。
    ${NSD_GetText} $hwnd $0          ;调用GetText函数获取该控件的输入内容，并保存到$0这个系统变量中去。
    StrCpy $User $0                  ;我直接对$0系统变量进行其他操作，会出现系统错误，所以我把$0这个系统变量中的值保存到一个自定义变量$User中去，这个
                                     ; 变量$User就是保存的输入到文本框的内容，以后要要用到这个变量的内容就可以直接操作这个变量就可以了。
FunctionEnd                          ;在上面修改XML文件是就用到了这个变量来获取控件的输入内容。

Section
SectionEnd

Function un.onInit
  MessageBox MB_ICONQUESTION|MB_YESNO|MB_DEFBUTTON2 "您确实要完全移除 $(^Name) ，及其所有的组件？" IDYES +2
  Abort
FunctionEnd

Function un.onUninstSuccess
  HideWindow
  MessageBox MB_ICONINFORMATION|MB_OK "$(^Name) 已成功地从您的计算机移除。"
FunctionEnd
```
  
  


   
 