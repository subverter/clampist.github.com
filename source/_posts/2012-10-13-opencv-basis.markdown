---
layout: post
title: "OpenCV Basis"
date: 2012-10-13 18:30
comments: true
categories: [OpenCV, Python]
---

1.Load an image
---

    In [1]: import cv

    In [2]: im1 = cv.LoadImageM('foo.jpg')

    In [3]: print type(im1)
    <type 'cv2.cv.cvmat'>

    In [4]: im2 = cv.LoadImage('foo.jpg')

    In [5]: print type(im2)
    <type 'cv2.cv.iplimage'>

    In [6]: cv.SaveImage('foo1.png', im1)

    In [7]: cv.SaveImage('foo2.png', im2)

可以看到`LoadImageM`得到的是一个`cvmat`图像，而`LoadImage`得到的是一个`iplimage`图像，两者均可以输出为图像

2.About DPI PPI and Resolution(分辨率)
---

* DPI 是指每一英吋长度中，取样或可显示或输出点的数目，英文为 Dots Per Inch（点每英寸）。主要说的是打印机。
* PPI 是指每英寸的长度中所具有的像素，英文为 Pixels Per Inch（像素每英寸）。主要说的是显示器。

{% img left http://upload.wikimedia.org/wikipedia/commons/3/3d/DPI_and_PPI.png %}

* 分辨率(resolution)则泛指量测或显示系统对细节的分辨能力。
* 由分辨率中X或Y轴的数字除以该轴的长度(英寸)，可得该轴的像素每英寸密度。一般的像素是方形或接近方形，X与Y轴像素密度相同，但也有不相同的显示器。
* 同样分辨率的一张图像，在越小的屏幕上显示或者在越小的尺寸上打印出来，PPI或者DPI就越高。看起来就越清晰，越细腻。
* 也就是说，如果想要把一张大小为 800X600 且 PPI/DPI 为 200 图像，降低到 100 PPI/DPI ，在显示或打印__尺寸不变__的情况下，只需把大小变为原来的一半 400X300（假设X与Y轴像素密度相同）。
* 同时也可以说，单纯的高分辨率与 DPI/PPI 是扯不上关系的，只有高分辨率的图像体现在具体尺寸上，才能说它的 DPI/PPI。一张 2880x1800 图像在 15.4 英寸的显示器上会有很高的 PPI， 但在 42 英寸的显示器上 PPI 则很低。

以下`halvedimage.py`是将图像大小变为一半

    #!/usr/bin/env python2
    # -*- coding: utf-8 -*-

    import cv
    import sys

    def main(img):
        im = cv.LoadImageM(img)
        halved = cv.CreateMat(im.rows / 2, im.cols / 2, cv.CV_8UC3)
        cv.Resize(im, halved)
        cv.SaveImage('halved_'+img, halved)

    if __name__=='__main__':
        try:
            main(sys.argv[1])
        except IndexError:
            print '''Usage:
            halvedimage.py image.file'''
