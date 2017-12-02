---
title: Pyramid Projection 对于全景应用的提升
date: 2016-08-06 20:53:13
tags:
- Digital Image Process
categories:
- Tech
- Digital Image Process
---

# Pyramid Projection 对于全景应用的提升

### 原型Equirectangular投影的文件大小

众所周知，在全景应用中，用的比较多的无非是Equirectangular（等距圆柱投影）以及CubeMap（立方体投影）。
前者是360°图像或视频的直接数字化存储格式，还有我们地球的投影也是利用了Equirectangular的投影方式。
Equirectangular的投影方式就如下图所示。

![](/img/pyramid-projection/equirectangular1.png)

其中它的文件像素比为2：1，图像上方4：0.5的图像是顶部的投影，图像下方4：0.5的图像是底部的投影，中间4：1的四个1：1正方形是前后左右四个方向的投影。
以面的像素值来计算的话可以看到文件大小为4：2

### CubeMap图像的文件大小

而Facebook早前提出了一种新的格式用于储存全景投影，就是CubeMap，如下图所示。

![](/img/pyramid-projection/cubemap1.png)

对于CubeMap图像来说它也是由六个方向的投影组成的图像，其中从上至下从左至右分别是右左上下前后六个面，宽高像素比是3：2。
可以很明显的看到文件大小是有所减少了，因为CubeMap图像将Equirectangular图像顶部和底部的两个面重复的像素去除了，文件大小相对Equirectangular减少了25%。
所以在服务器上储存的时候，就可以减少一定的空间了。

### Pyramid Projection图像

现在，Facebook又提出了新的图像投影方式[Pyramid Projection](https://code.facebook.com/posts/1126354007399553/next-generation-video-encoding-techniques-for-360-video-and-vr/)，它能够减少80%的文件大小。
简单来说，它是利用了用户可能会聚集某个时间段只关注一个角度的图像，另一个时间段关注另一个角度，那么只需要根据用户观看的方向设定椎体的底部为正向即可。
根据这一个工程性的方法，我们先静态制作几个不同方向的金字塔投影，再根据用户视角动态读取不同文件。
这样做的好处是什么呢？锥形投影图像的像素大小小了很多！因为只关注一个面，所以锥形投影图像的大小只有Equirectangular投影图像大小的20%，所以GPU在做纹理渲染时，时间成本大幅下降！用户再也不用因为GPU设备性能差而等待视频缓冲了！

这里值得注意的一点是，四个角落的三角形投影时不可能是等腰直角三角形，否则无法构成椎体，需要进行在垂直方向上扩展成等边三角形，至于如何扩展下面会详细说明。

![](/img/pyramid-projection/pyramid1.jpg)

- Pyramid Projection 实现

简单来说，思路是这样的。利用坐标变换使锥形投影平面展开图上的点和椎体模型一一对应，这里要用到坐标系转换的线性代数方法，用变换矩阵乘上点的坐标得到新坐标系下的坐标。
求出每个平面上的点对应的三维空间坐标后，再利用椎体上的点到球心的垂直方向夹角和水平方向夹角，求出椎体到球体的转换，这个方法和Equirectangular转换到CubeMap的方法是一样的。

[我的代码](https://github.com/MinoGump/360ImagePyramidProjection)

```py
# This Function create an pyramid projection image and set the color 
def convertBack(imgIn,imgOut):
    #...

# This Function convert the XY coordinate of pyramid image to XYZ coordinate of pyramid model 
def outImgToXYZ(i,j,face,halfOutSize,toward):
    #...

# This Function calculate the roate matrix of each plane in pyramid image
def convertCoordinate(x, y, z, sequence):
    #...
```

输出的结果是这样的

![](/img/pyramid-projection/output.jpg)
