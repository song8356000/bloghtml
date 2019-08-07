---
title: C++语音识别接口快速入门（Microsoft Speech SDK）
date: 2017-08-18 16:55:17
tags: CSDN迁移
---
   ### 目录

 

 * [C语音识别接口快速入门Microsoft Speech SDK](http://blog.csdn.net/michaelliang12/article/details/51317531#c%E8%AF%AD%E9%9F%B3%E8%AF%86%E5%88%AB%E6%8E%A5%E5%8F%A3%E5%BF%AB%E9%80%9F%E5%85%A5%E9%97%A8microsoft-speech-sdk) 
      * * [目录](http://blog.csdn.net/michaelliang12/article/details/51317531#%E7%9B%AE%E5%BD%95)
      * [一安装SDK](http://blog.csdn.net/michaelliang12/article/details/51317531#%E4%B8%80%E5%AE%89%E8%A3%85sdk)
      * [二新建工程配置环境](http://blog.csdn.net/michaelliang12/article/details/51317531#%E4%BA%8C%E6%96%B0%E5%BB%BA%E5%B7%A5%E7%A8%8B%E9%85%8D%E7%BD%AE%E7%8E%AF%E5%A2%83)
      * [三语音识别代码](http://blog.csdn.net/michaelliang12/article/details/51317531#%E4%B8%89%E8%AF%AD%E9%9F%B3%E8%AF%86%E5%88%AB%E4%BB%A3%E7%A0%81) 
          * [1文字转语音](http://blog.csdn.net/michaelliang12/article/details/51317531#1%E6%96%87%E5%AD%97%E8%BD%AC%E8%AF%AD%E9%9F%B3)
          * [2语音转文字](http://blog.csdn.net/michaelliang12/article/details/51317531#2%E8%AF%AD%E9%9F%B3%E8%BD%AC%E6%96%87%E5%AD%97)
      * [源代码下载](http://blog.csdn.net/michaelliang12/article/details/51317531#%E6%BA%90%E4%BB%A3%E7%A0%81%E4%B8%8B%E8%BD%BD)
      * [参考网站](http://blog.csdn.net/michaelliang12/article/details/51317531#%E5%8F%82%E8%80%83%E7%BD%91%E7%AB%99)  
   
 

 
## []()一、安装SDK

 安装MicrosoftSpeechPlatformSDK.msi，默认路径安装即可。   
 下载路径：   
[http://download.csdn.net/detail/michaelliang12/9510691](http://download.csdn.net/detail/michaelliang12/9510691)

 
## []()二、新建工程，配置环境

 设置：   
 1，属性–配置属性–C/C++–常规–附加包含目录：C:\Program Files\Microsoft SDKs\Speech\v11.0\Include（具体路径与安装路径有关）   
 2，属性–配置属性–链接器–输入–附加依赖项：sapi.lib;

 
## []()三、语音识别代码

 语音识别接口可分为文字转语音和语音转文字

 
### []()1、文字转语音

 需要添加的头文件：

 
```
#include <sapi.h> //导入语音头文件
#pragma comment(lib,"sapi.lib") //导入语音头文件库

```

  * 1
  * 2
  * 3
  * 1
  * 2
  * 3函数：

 
```
void  CBodyBasics::MSSSpeak(LPCTSTR speakContent)// speakContent为LPCTSTR型的字符串,调用此函数即可将文字转为语音
{
    ISpVoice *pVoice = NULL;

    //初始化COM接口

    if (FAILED(::CoInitialize(NULL)))
        MessageBox(NULL, (LPCWSTR)L"COM接口初始化失败！", (LPCWSTR)L"提示", MB_ICONWARNING | MB_CANCELTRYCONTINUE | MB_DEFBUTTON2);

    //获取SpVoice接口

    HRESULT hr = CoCreateInstance(CLSID_SpVoice, NULL, CLSCTX_ALL, IID_ISpVoice, (void**)&pVoice);


    if (SUCCEEDED(hr))
    {
        pVoice->SetVolume((USHORT)100); //设置音量，范围是 0 -100
        pVoice->SetRate(2); //设置速度，范围是 -10 - 10
        hr = pVoice->Speak(speakContent, 0, NULL);   //Speak函数的第二个参数如果设置成SPF_IS_FILENAME  则第一个参数指向一个文本(txt)文件

        pVoice->Release();

        pVoice = NULL;
    }

    //释放com资源
    ::CoUninitialize();
}


```

  * 1
  * 2
  * 3
  * 4
  * 5
  * 6
  * 7
  * 8
  * 9
  * 10
  * 11
  * 12
  * 13
  * 14
  * 15
  * 16
  * 17
  * 18
  * 19
  * 20
  * 21
  * 22
  * 23
  * 24
  * 25
  * 26
  * 27
  * 28
  * 29
  * 30
  * 1
  * 2
  * 3
  * 4
  * 5
  * 6
  * 7
  * 8
  * 9
  * 10
  * 11
  * 12
  * 13
  * 14
  * 15
  * 16
  * 17
  * 18
  * 19
  * 20
  * 21
  * 22
  * 23
  * 24
  * 25
  * 26
  * 27
  * 28
  * 29
  * 30
### []()2、语音转文字

 这个稍微麻烦一点，因为需要实时监控麦克风，涉及到windows的消息机制。   
 （1）首先设置工程属性：   
 属性–配置属性–C/C++–预处理器–预处理器定义：_WIN32_DCOM;

 （2）需要添加的头文件：

 
```
#include <sapi.h> //导入语音头文件
#pragma comment(lib,"sapi.lib") //导入语音头文件库
#include <sphelper.h>//语音识别头文件
#include <atlstr.h>//要用到CString

#pragma once
const int WM_RECORD = WM_USER + 100;//定义消息
```

  * 1
  * 2
  * 3
  * 4
  * 5
  * 6
  * 7
  * 1
  * 2
  * 3
  * 4
  * 5
  * 6
  * 7（3）在程序的.h头文件中定义变量

 
```
//定义变量
CComPtr<ISpRecognizer>m_cpRecoEngine;// 语音识别引擎(recognition)的接口。
CComPtr<ISpRecoContext>m_cpRecoCtxt;// 识别引擎上下文(context)的接口。
CComPtr<ISpRecoGrammar>m_cpCmdGrammar;// 识别文法(grammar)的接口。
CComPtr<ISpStream>m_cpInputStream;// 流()的接口。
CComPtr<ISpObjectToken>m_cpToken;// 语音特征的(token)接口。
CComPtr<ISpAudio>m_cpAudio;// 音频(Audio)的接口。(用来保存原来默认的输入流)
ULONGLONG  ullGrammerID;

```

  * 1
  * 2
  * 3
  * 4
  * 5
  * 6
  * 7
  * 8
  * 9
  * 1
  * 2
  * 3
  * 4
  * 5
  * 6
  * 7
  * 8
  * 9（4）创建语音识别初始化函数（程序刚开始执行的时候调用，例如文末示例代码中，将此初始化函数放在对话框初始化消息WM_INITDIALOG的响应代码里）

 
```
//语音识别初始化函数
void  CBodyBasics::MSSListen()
{

    //初始化COM接口

    if (FAILED(::CoInitialize(NULL)))
        MessageBox(NULL, (LPCWSTR)L"COM接口初始化失败！", (LPCWSTR)L"提示", MB_ICONWARNING | MB_CANCELTRYCONTINUE | MB_DEFBUTTON2);


    HRESULT hr = m_cpRecoEngine.CoCreateInstance(CLSID_SpSharedRecognizer);//创建Share型识别引擎
    if (SUCCEEDED(hr))
    {


        hr = m_cpRecoEngine->CreateRecoContext(&m_cpRecoCtxt);//创建识别上下文接口

        hr = m_cpRecoCtxt->SetNotifyWindowMessage(m_hWnd, WM_RECORD, 0, 0);//设置识别消息

        const ULONGLONG ullInterest = SPFEI(SPEI_SOUND_START) | SPFEI(SPEI_SOUND_END) | SPFEI(SPEI_RECOGNITION);//设置我们感兴趣的事件
        hr = m_cpRecoCtxt->SetInterest(ullInterest, ullInterest);

        hr = SpCreateDefaultObjectFromCategoryId(SPCAT_AUDIOIN, &m_cpAudio);
        m_cpRecoEngine->SetInput(m_cpAudio, true);



        //创建语法规则
        //dictation听说式
        //hr = m_cpRecoCtxt->CreateGrammar(GIDDICTATION, &m_cpDictationGrammar);
        //if (SUCCEEDED(hr))
        //{
        //  hr = m_cpDictationGrammar->LoadDictation(NULL, SPLO_STATIC);//加载词典
        //}

        //C&C命令式，此时语法文件使用xml格式
        ullGrammerID = 1000;
        hr = m_cpRecoCtxt->CreateGrammar(ullGrammerID, &m_cpCmdGrammar);

        WCHAR wszXMLFile[20] = L"";//加载语法
        MultiByteToWideChar(CP_ACP, 0, (LPCSTR)"CmdCtrl.xml", -1, wszXMLFile, 256);//ANSI转UNINCODE
        hr = m_cpCmdGrammar->LoadCmdFromFile(wszXMLFile, SPLO_DYNAMIC);


        //MessageBox(NULL, (LPCWSTR)L"语音识别已启动！", (LPCWSTR)L"提示", MB_CANCELTRYCONTINUE );
        //激活语法进行识别
        //hr = m_cpDictationGrammar->SetDictationState(SPRS_ACTIVE);//dictation
        hr = m_cpCmdGrammar->SetRuleState(NULL, NULL, SPRS_ACTIVE);//C&C
        hr = m_cpRecoEngine->SetRecoState(SPRST_ACTIVE);

    }

    else
    {
        MessageBox(NULL, (LPCWSTR)L"语音识别引擎启动出错！", (LPCWSTR)L"警告", MB_OK);
        exit(0);
    }


    //释放com资源
    ::CoUninitialize();
    //hr = m_cpCmdGrammar->SetRuleState(NULL, NULL, SPRS_INACTIVE);//C&C


}


```

  * 1
  * 2
  * 3
  * 4
  * 5
  * 6
  * 7
  * 8
  * 9
  * 10
  * 11
  * 12
  * 13
  * 14
  * 15
  * 16
  * 17
  * 18
  * 19
  * 20
  * 21
  * 22
  * 23
  * 24
  * 25
  * 26
  * 27
  * 28
  * 29
  * 30
  * 31
  * 32
  * 33
  * 34
  * 35
  * 36
  * 37
  * 38
  * 39
  * 40
  * 41
  * 42
  * 43
  * 44
  * 45
  * 46
  * 47
  * 48
  * 49
  * 50
  * 51
  * 52
  * 53
  * 54
  * 55
  * 56
  * 57
  * 58
  * 59
  * 60
  * 61
  * 62
  * 63
  * 64
  * 65
  * 66
  * 67
  * 1
  * 2
  * 3
  * 4
  * 5
  * 6
  * 7
  * 8
  * 9
  * 10
  * 11
  * 12
  * 13
  * 14
  * 15
  * 16
  * 17
  * 18
  * 19
  * 20
  * 21
  * 22
  * 23
  * 24
  * 25
  * 26
  * 27
  * 28
  * 29
  * 30
  * 31
  * 32
  * 33
  * 34
  * 35
  * 36
  * 37
  * 38
  * 39
  * 40
  * 41
  * 42
  * 43
  * 44
  * 45
  * 46
  * 47
  * 48
  * 49
  * 50
  * 51
  * 52
  * 53
  * 54
  * 55
  * 56
  * 57
  * 58
  * 59
  * 60
  * 61
  * 62
  * 63
  * 64
  * 65
  * 66
  * 67（5）定义消息处理函数   
 需要和其他的消息处理代码放在一起，如本文代码中，放在文末示例代码的DlgProc()函数尾部。本文整个其他的代码块都可以直接照搬，只需要更改如下的消息反应模块即可

 
```
//消息处理函数
USES_CONVERSION;
    CSpEvent event;

    if (m_cpRecoCtxt)
    {
        while (event.GetFrom(m_cpRecoCtxt) == S_OK){

            switch (event.eEventId)
            {
            case SPEI_RECOGNITION:
            {
                                     //识别出了语音
                                     m_bGotReco = TRUE; 

                                     static const WCHAR wszUnrecognized[] = L"<Unrecognized>";

                                     CSpDynamicString dstrText;

                                     ////取得识别结果 
                                     if (FAILED(event.RecoResult()->GetText(SP_GETWHOLEPHRASE, SP_GETWHOLEPHRASE, TRUE, &dstrText, NULL)))
                                     {
                                         dstrText = wszUnrecognized;
                                     }

                                     BSTR SRout;
                                     dstrText.CopyToBSTR(&SRout);
                                     CString Recstring;
                                     Recstring.Empty();
                                     Recstring = SRout;

                                    //做出反应（*****消息反应模块*****）
                                    if (Recstring == "发短信")
                                     {
                                         //MessageBox(NULL, (LPCWSTR)L"好的", (LPCWSTR)L"提示", MB_OK);
                                         MSSSpeak(LPCTSTR(_T("好，马上发短信！")));

                                     }

                                     else if (Recstring == "李雷")
                                     {
                                         MSSSpeak(LPCTSTR(_T("好久没看见他了，真是 long time no see")));
                                     }   

            }
                break;
            }
        }
    }


```

  * 1
  * 2
  * 3
  * 4
  * 5
  * 6
  * 7
  * 8
  * 9
  * 10
  * 11
  * 12
  * 13
  * 14
  * 15
  * 16
  * 17
  * 18
  * 19
  * 20
  * 21
  * 22
  * 23
  * 24
  * 25
  * 26
  * 27
  * 28
  * 29
  * 30
  * 31
  * 32
  * 33
  * 34
  * 35
  * 36
  * 37
  * 38
  * 39
  * 40
  * 41
  * 42
  * 43
  * 44
  * 45
  * 46
  * 47
  * 48
  * 49
  * 50
  * 51
  * 1
  * 2
  * 3
  * 4
  * 5
  * 6
  * 7
  * 8
  * 9
  * 10
  * 11
  * 12
  * 13
  * 14
  * 15
  * 16
  * 17
  * 18
  * 19
  * 20
  * 21
  * 22
  * 23
  * 24
  * 25
  * 26
  * 27
  * 28
  * 29
  * 30
  * 31
  * 32
  * 33
  * 34
  * 35
  * 36
  * 37
  * 38
  * 39
  * 40
  * 41
  * 42
  * 43
  * 44
  * 45
  * 46
  * 47
  * 48
  * 49
  * 50
  * 51（6）修改语法文件   
 修改CmdCtrl.xml文件，可以提高某些词汇的识别度，对里面的词识别效果会很好多，如人名等。（此外，单独运行exe时也需要将此文件和exe放在同一文件夹内，不放也不会报错，只是语法文件里的词汇识别效果变差）

 
```
<?xml version="1.0" encoding="utf-8"?>
<GRAMMAR LANGID="804">
  <DEFINE>
    <ID NAME="VID_SubName1" VAL="4001"/>
    <ID NAME="VID_SubName2" VAL="4002"/>
    <ID NAME="VID_SubName3" VAL="4003"/>
    <ID NAME="VID_SubName4" VAL="4004"/>
    <ID NAME="VID_SubName5" VAL="4005"/>
    <ID NAME="VID_SubName6" VAL="4006"/>
    <ID NAME="VID_SubName7" VAL="4007"/>
    <ID NAME="VID_SubName8" VAL="4008"/>
    <ID NAME="VID_SubName9" VAL="4009"/>
    <ID NAME="VID_SubNameRule" VAL="3001"/>
    <ID NAME="VID_TopLevelRule" VAL="3000"/>
  </DEFINE>
  <RULE ID="VID_TopLevelRule" TOPLEVEL="ACTIVE">
    <O>
      <L>
        <P>我要</P>
        <P>运行</P>
        <P>执行</P>
      </L>
    </O>
    <RULEREF REFID="VID_SubNameRule" />
  </RULE>
  <RULE ID="VID_SubNameRule" >
    <L PROPID="VID_SubNameRule">
      <P VAL="VID_SubName1">发短信</P>
      <P VAL="VID_SubName2">是的</P>
      <P VAL="VID_SubName3">好的</P>
      <P VAL="VID_SubName4">不用</P>
      <P VAL="VID_SubName5">李雷</P>
      <P VAL="VID_SubName6">韩梅梅</P>
      <P VAL="VID_SubName7">中文界面</P>
      <P VAL="VID_SubName8">英文界面</P>
      <P VAL="VID_SubName9">English</P>

    </L>
  </RULE>
</GRAMMAR>
```

  * 1
  * 2
  * 3
  * 4
  * 5
  * 6
  * 7
  * 8
  * 9
  * 10
  * 11
  * 12
  * 13
  * 14
  * 15
  * 16
  * 17
  * 18
  * 19
  * 20
  * 21
  * 22
  * 23
  * 24
  * 25
  * 26
  * 27
  * 28
  * 29
  * 30
  * 31
  * 32
  * 33
  * 34
  * 35
  * 36
  * 37
  * 38
  * 39
  * 40
  * 1
  * 2
  * 3
  * 4
  * 5
  * 6
  * 7
  * 8
  * 9
  * 10
  * 11
  * 12
  * 13
  * 14
  * 15
  * 16
  * 17
  * 18
  * 19
  * 20
  * 21
  * 22
  * 23
  * 24
  * 25
  * 26
  * 27
  * 28
  * 29
  * 30
  * 31
  * 32
  * 33
  * 34
  * 35
  * 36
  * 37
  * 38
  * 39
  * 40
## []()源代码下载

 注意，本代码是在原来的项目中截取出来的，但可以独立运行。   
 Microsoft Speech SDK 安装包下载：   
[http://download.csdn.net/detail/michaelliang12/9510691](http://download.csdn.net/detail/michaelliang12/9510691)   
 文中示例程序下载（之前下载分数太高，我已经重新上传了新版本，也解决了kincect20.lib报错的问题。由于自己经常在csdn上下东西，也需要积分，需要还是需要各位捧场，2积分。。）：   
[http://download.csdn.net/detail/michaelliang12/9766783](http://download.csdn.net/detail/michaelliang12/9766783)

 存在的bug：每次运行完程序，Windows的语音识别助手不会自动关闭，需要自己手动关闭。若不关闭，则下次启动程序可能会出错。大家如果有好的解决办法，请联系我，谢了！

 
## []()参考网站

 1，[http://www.cnblogs.com/eping/archive/2010/05/23/1742201.html](http://www.cnblogs.com/eping/archive/2010/05/23/1742201.html)   
 2，[http://blog.csdn.net/pamchen/article/details/7856207](http://blog.csdn.net/pamchen/article/details/7856207)   
 3，[http://blog.csdn.net/jmxiaocai/article/details/7036033](http://blog.csdn.net/jmxiaocai/article/details/7036033)   
 4，[http://blog.csdn.net/buaalei/article/details/5372544（主要参考）](http://blog.csdn.net/buaalei/article/details/5372544)   
 5，[http://blog.csdn.net/itcastcpp/article/details/5313204](http://blog.csdn.net/itcastcpp/article/details/5313204)   
 6，[http://blog.csdn.net/artemisrj/article/details/8723095（MFC的消息处理响应版本）](http://blog.csdn.net/artemisrj/article/details/8723095)

   
 