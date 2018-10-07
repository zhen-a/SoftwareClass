## 教程2：图像像素值的修改

### 1. 物理图像 VS 数字图像

客观世界中，以自然形式呈现出的图像通常称作物理图像。如下图是一张青岛市区的风景图。

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvzrg96x4wj20s80kcx36.jpg)



但是，计算机并不能直接处理物理图像！因为计算机只认识离散数字，所以一幅图像在用计算机处理前必须转化为数字形式，即数字图像 。

- 数字图像是指物理图像的连续信号值被离散化后，由被称作像素的小块区域组成的二维矩阵。
- 将物理图像行列划分后，每个小块区域称为像素（Pixel）。每个像素包括两个属性：位置和色彩（或亮度）。

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvzqhfqtssj211d0msarp.jpg)

### 2. 灰度图像

灰度图像，每个像素的亮度用一个整数来表示，通常数值范围在0到255之间，0表示纯黑、255表示纯白，而其它表示灰色。在计算机中存储时，使用格式为 unsigned char 。GDAL中为 GByte (头文件中有这样的定义：typedef unsigned char 	GByte)

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvzqid90k2j205k06imx2.jpg)

下面为灰度图像的一个实例：

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvzqndcw4qj20ra0d9ag6.jpg)

### 3. 彩色图像

- 彩色图像可以用“红、绿、蓝”三基色的二维矩阵来表示。
- 通常，三基色的每个数值也是在0到255之间，0表示相应的基色在该像素中没有，而255则代表相应的基色在该像素中取得最大值。

下面为彩色图像的实例：

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvzupptuchj20qr0e913d.jpg)

### 4. GDAL对彩色图像的处理

GDAL可以用GetRasterBand()函数得到图像的通道，

GetRasterBand(1)得到的是红色通道；

GetRasterBand(2)得到的是绿色通道；

GetRasterBand(3)得到的是蓝色通道；

所以，计算机一般颜色表示为RGB，也是遵从这个顺序。

### 5. 任务1：把图像指定区域的红色通道置为255

任务描述：把输入图像中起始点（100，100），宽度为200像素，高度为150像素的区域红色通道置为255。

**核心代码如下：**

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvztsd1e4aj20pb0f30tc.jpg)

**源图像如下：**

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvzrg96x4wj20s80kcx36.jpg)

**结果图像如下：**

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvztu64zwij20s80kc42d.jpg)



### 任务2：把图像指定区域置为白色和黑色

任务描述：把输入图像中起始点（300，300），宽度为100像素，高度为50像素的区域置为白色，同时，把输入图像中起始点为（500，500），宽度为50像素，高度为100像素的区域置为黑色。

这个任务不提供示例代码，请自己完成。

**结果图像如下所示：**

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvzu1glva5j20s80kc77k.jpg)



