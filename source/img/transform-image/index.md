# 360全景图像

通过物理设备可以拍摄到360图像，比如下图的设备就是通过前后镜头捕捉到两个鱼眼图，再通过转换得到360图像。

![](/img/transform-image/theta_s.png)

用物理设备拍摄到的360全景图片后，以什么形式数字化储存呢？一个是鱼眼图，一个是等距圆柱投影（Equirectangular），一种是CubeMap正方体投影，还有一种是金字塔投影（Pyramid Projection）。


# 鱼眼图

鱼眼图是我们经常见到的一种投影图像。

![](/img/transform-image/fisheye.jpg)

它实际上是半球图像的圆形投影，双鱼眼图就是将一整个球的像素点分两个半球投影到两个圆上。

# 等距圆柱投影（Equirectangular）

等距圆柱投影图也是我们经常见到的图像，比如世界地图就是利用了等距圆柱投影。

![](/img/transform-image/equirectangular.jpg)

它是利用了球体到内切圆柱体的投影技术。


# CubeMap

CubeMap是立方体投影，它用到了球体到内切立方体的投影技术。

![](/img/transform-image/cubemap.jpg)

# 鱼眼图转换为等距圆柱投影

主要的思想是通过输出图像等距圆柱投影图像的坐标xy计算对应的三维空间的坐标，再求出对应鱼眼图的xy坐标进行赋值。

代码见[GitHub](https://github.com/MinoGump/360ImageTransform)

# 等距圆柱投影转换为CubeMap

主要的思想是通过输出的CubeMap图像判断是六个面中的哪一个面，根据判断得出的方向获得三维空间内球体上的坐标，再对应到等距圆柱图像上。

代码见[GitHub](https://github.com/MinoGump/360ImageTransform)

# CubeMap转换为鱼眼图

主要的思想是利用鱼眼图的坐标计算得到三维空间中球体的坐标，再对应到内切正方体内的坐标就是对应的CubeMap坐标，赋像素值即可。

代码见[GitHub](https://github.com/MinoGump/360ImageTransform)

# Pyramid Projection

金字塔投影是Facebook研究出的新型投影方式，它能够减少图像存储的空间（减少重复的像素点），facebook的360视频就是用这种投影方式在服务器上存储视频的。

[Facebook金字塔投影](https://code.facebook.com/posts/1126354007399553/next-generation-video-encoding-techniques-for-360-video-and-vr/)
