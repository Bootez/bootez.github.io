---
layout: post
title: "Tesseract-OCR"
description: "本文主要介绍如何在Debian Wheezy上安装Tesseract OCR及其开发环境。"
category: 
tags: [linux ocr tutorial]
---
{% include JB/setup %}

### Overview
Tesseract的OCR引擎最先由HP实验室于1985年开始研发，至1995年时已经成为OCR业内最准确的三款识别引擎之一。然而，HP不久便决定放弃OCR业务，Tesseract也从此尘封。 数年以后，HP意识到，与其将
Tesseract束之高阁，不如贡献给开源软件业，让其重焕新生--2005年，Tesseract由美国内华达州信息技术研究所获得，并求诸于Google对 Tesseract进行改进、消除Bug、优化工作。
Tesseract目前已作为开源项目发布在Google Project，其项目主页在[这里](https://code.google.com/p/tesseract-ocr/)查看。为了方便起见，我们直接使用apt-get进行安装， 想自己编译的可参考官方
文档配置编译环境进行编译。

### Environment

{% highlight sh %}

uname -a
Linux debian 3.2.0-4-amd64 #1 SMP Debian 3.2.57-3 x86_64 GNU/Linux

{% endhighlight %}

### Steps:

#### 1.安装tesseract-ocr以及相关依赖库：

{% highlight sh %}

sudo apt-get install tesseract-ocr-dev tesseract-ocr libtesseract-dev libleptonica-dev libpng-dev libtiff-dev libjpeg-dev

{% endhighlight %}

#### 2.根据[官方API文档](https://code.google.com/p/tesseract-ocr/wiki/APIExample)编译第一个demo出来，验证开发环境的正确性：

{% highlight c %}

/*tesseract.cpp*/
#include <tesseract/baseapi.h>
#include <leptonica/allheaders.h>

int main()
{
    char *outText;

    tesseract::TessBaseAPI *api = new tesseract::TessBaseAPI();
    // Initialize tesseract-ocr with English, without specifying tessdata path
    if (api->Init(NULL, "eng")) {
        fprintf(stderr, "Could not initialize tesseract.\n");
        exit(1);
    }

    // Open input image with leptonica library
    Pix *image = pixRead("/usr/src/tesseract-3.02/phototest.tif");
    api->SetImage(image);
    // Get OCR result
    outText = api->GetUTF8Text();
    printf("OCR output:\n%s", outText);

    // Destroy used object and release memory
    api->End();
    delete [] outText;
    pixDestroy(&image);

    return 0;
}

g++ tesseract.cpp -o tesseract -ltesseract -llept

{% endhighlight %}

#### 3.如果没有什么错误的话会生成tesseract，执行该应用：

{% highlight sh %}

./tesseract

OCR output:
This is a lot of 12 point text to test the
ocr code and see if it works on all types
of file format.

The quick brown dog jumped over the
lazy fox. The quick brown dog jumped
over the lazy fox. The quick brown dog
jumped over the lazy fox. The quick
brown dog jumped over the lazy fox.

{% endhighlight %}

这个Demo主要是将/usr/src/tesseract-3.02/phototest.tif这幅图片上的文字通过tessert-ocr的API转化为文本，然后打印到终端。

### Notice
其他发行版的Linux也可参考本文进行配置Tessert-OCR开发环境。可能会遇到的问题汇总：

#### 编译错误：

#### 1.未正确安装tesseract-ocr的开发库(本文中提到的是:libtesseract-dev)
{% highlight sh %}

tesseract.cpp:1:32: error: tesseract/baseapi.h: No such file or directory

{% endhighlight %}

#### 2.未正确安装leptonica的开发库(本文中提到的是:libleptonica-dev)
{% highlight sh %}

tesseract.cpp:2:35: error: leptonica/allheaders.h: No such file or directory

{% endhighlight %}

#### 链接错误：

#### 1.未正确链接libtesseract

{% highlight sh %}

tesseract.cpp:(.text+0x21): undefined reference to `tesseract::TessBaseAPI::TessBaseAPI()'
tesseract.cpp:(.text+0x94): undefined reference to `pixRead'
tesseract.cpp:(.text+0xab): undefined reference to `tesseract::TessBaseAPI::SetImage(Pix const*)'
tesseract.cpp:(.text+0xb7): undefined reference to `tesseract::TessBaseAPI::GetUTF8Text()'
tesseract.cpp:(.text+0xdd): undefined reference to `tesseract::TessBaseAPI::End()'
tesseract.cpp:(.text+0xfc): undefined reference to `pixDestroy'
/tmp/ccqn3zaC.o: In function `tesseract::TessBaseAPI::Init(char const*, char const*)':

{% endhighlight %}

#### 2.未正确链接libleptonica
{% highlight sh %}

/usr/bin/ld: /tmp/ccUzaM6g.o: undefined reference to symbol 'pixRead'
/usr/bin/ld: note: 'pixRead' is defined in DSO /usr/lib/liblept.so.3 so try adding it to the linker command line
/usr/lib/liblept.so.3: could not read symbols: Invalid operation
collect2: ld returned 1 exit status

{% endhighlight %}
