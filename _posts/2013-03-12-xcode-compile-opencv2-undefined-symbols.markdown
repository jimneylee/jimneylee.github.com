---
author: jimneylee
layout: post
title: "XCode编译基于opencv2的demo，出现Undefined symbols问题"
date: 2013-03-12 22:06:57
comments: true
tags:
- opencv
- macos
- xcode
---

今天发现使用新的opencv2版本的方法显示一张图片时，代码如下：
{% highlight c %}
#include
#include
#include
using namespace cv;
using namespace std;
int main(int argc, const char * argv[])
{
    Mat image;
    image = imread("./baby.jpg");
    if (!image.data) {
        cout << "Could not read the image" <<endl;
        return -1;
    }
    char windowName[] = "Study01";
    namedWindow(windowName, CV_WINDOW_AUTOSIZE);
    imshow(windowName, image);
    waitKey(0);
    return 0;
}
}
{% endhighlight %}
编译器显示如下错误：

`Undefined symbols for architecture x86_64:
  "cv::namedWindow(std::__1::basic_string, 
  std::__1::allocator > const&, int)", 
  referenced from:
      _main in main.o
  "cv::imread(std::__1::basic_string, std::__1::allocator > const&, int)", 
  referenced from:_main in main.o
  "cv::imshow(std::__1::basic_string, std::__1::allocator > const&, 
  cv::_InputArray const&)", referenced from:
      _main in main.o
ld: symbol(s) not found for architecture x86_64
clang: error: linker command failed with exit code 1 (use -v to see invocation)`

google后初步分析为C++库和opencv库编译不一致，将XCode的C++ Standard Library改为GNU C++ standard library即可正常编译显示图片。此处仍有疑问，待日后考虑清楚。

具体见stackoverflow：[http://stackoverflow.com/questions/13461400/opencv-unresolved-symbols-name-mangling-mismatch-xcode](http://stackoverflow.com/questions/13461400/opencv-unresolved-symbols-name-mangling-mismatch-xcode)