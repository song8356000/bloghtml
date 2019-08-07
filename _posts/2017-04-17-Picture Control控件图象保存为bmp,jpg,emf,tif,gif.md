---
title: Picture Control控件图象保存为bmp,jpg,emf,tif,gif
date: 2018-01-16 18:08:16
tags: CSDN迁移
---
 版权声明：需要转载的请注明出处 https://blog.csdn.net/qq_22642239/article/details/79077673   
   今天在做项目的时候，遇见了一个需求：如何把MFC控件 Picture Control 中的图像保存到一个文件夹下？

 平时也没做过这些，在网上寻求解决方案，一位大神的方法，完美的解决了我的问题，也让我get到了新的技能，感谢！

 解决方法如下：

 **解决方法1：**

 以下代码要求.net2003以上环境，因为vc6.0无atlimage.h.如果你要用vc6.0,那么请在.net2003下去拷altimage.h,它是gdi+的包装类   
   


 
```
HBITMAP   hBitmap   =   NULL;
  //创建位图段
  BITMAPINFO   bmi;
  LPBYTE   pBits;
  ZeroMemory(&bmi,sizeof(bmi));
  //m_bmpShow为Picture Control控件变量名称
  CDC *pShowDC = m_bmpShow.GetDC();
  //获取Picture Control控件的宽度和高度
  CRect m_rcShow
  m_bmpShow.GetWindowRect(&m_rcShow);
  bmi.bmiHeader.biSize   =   sizeof(BITMAPINFOHEADER);
  bmi.bmiHeader.biWidth   =   m_rcShow.Width();
  bmi.bmiHeader.biHeight   =  m_rcShow.Height();
  bmi.bmiHeader.biPlanes   =   1;
  bmi.bmiHeader.biBitCount   =   24;
  bmi.bmiHeader.biCompression   =   BI_RGB;
  hBitmap     =   CreateDIBSection(pShowDC->m_hDC,&bmi,DIB_RGB_COLORS,(void   **)&pBits,0,0   );
  //创建兼容dc并选择位图段
  CDC   dcMem;
  dcMem.CreateCompatibleDC(pShowDC);
  dcMem.SelectObject(hBitmap);
  dcMem.BitBlt(0,0,m_rcShow.Width(),m_rcShow.Height(),pShowDC,0,0,SRCCOPY);
  m_bmpShow.ReleaseDC(pShowDC);
  if( hBitmap )
  {
   CImage   img;
   img.Attach(hBitmap);
   img.Save(_T("f:\\1.bmp"));
   img.Save(_T("f:\\1.jpg"));
   //其它文件格式同理
   DeleteObject(hBitmap);
   AfxMessageBox(_T("OK!!"));
  }
```
 

 

 

 **解决方法2：** 用CImage类更简洁的代码如下：  
  


 

 
```
//m_bmpShow为Picture Control控件变量名称
 CDC *pdc = m_bmpShow.GetDC();
 CImage   imag;
 //获取Picture Control控件的宽度和高度
 CRect   rcClient;
 m_bmpShow.GetWindowRect(&rcClient);
 imag.Create(   rcClient.Width(),rcClient.Height(),32);
 ::BitBlt(imag.GetDC(),0,0,rcClient.Width(),rcClient.Height(),pdc->m_hDC,0,0,SRCCOPY);
 imag.Save(_T("f:\\2.bmp"));
 imag.Save(_T("f:\\21.jpg"));
 //ReleaseDC(pdc);
 imag.ReleaseDC();
 AfxMessageBox(_T("OK!"));
```
  
  


   
 