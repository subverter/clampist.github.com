---
layout: post
title: "Install OpenEXR for Python"
date: 2012-10-11 12:50
comments: true
categories: [OpenCV, Python]
---

Using OpenEXR’s Python bindings we can make a simple image viewer.  
So, we should install OpenEXR first, following [OpenEXR bindings for Python](http://www.excamera.com/sphinx/articles-openexr.html)  
Make sure you have already installed OpenEXR's Prerequisite: Python 2.5+ and the OpenEXR C++ library(openexr).

    wget http://excamera.com/files/OpenEXR-1.2.0.tar.gz

    tar zxvf OpenEXR-1.2.0.tar.gz

    cd OpenEXR-1.2.0

    (sudo python2 setup.py build)

    sudo python2 setup.py install

Then, we can follow [OpenCV and OpenEXR](http://opencv.willowgarage.com/documentation/python/cookbook.html#opencv-and-openexr)

References: [Python how to install setup.py](http://blog.csdn.net/ponder008/article/details/6592719)
