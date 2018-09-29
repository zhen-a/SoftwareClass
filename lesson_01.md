## 教程1：GDAL + Visual Studio 配置

### 1. 什么是遥感？

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvpl64k86xj20ig0jr4ee.jpg)



### 2. 为什么我们需要遥感图像？

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvpl79l9f6j210m0gb7wh.jpg)

周末班级活动，提前瞧瞧樱桃采摘园附近停车位。一般地图软件只提供矢量底图形式，可以定位到采摘园的位置，但是无法反映周边环境，比如是否方便停车等。通过遥感图像可以清晰俯瞰采摘园，如果是樱桃成熟季节，甚至可以看到果实长势。一图胜千言，影像可以直观表达，如果你看到了，你就清楚那是什么。

### 3. 全色、多光谱、融合图像

本课程的主要任务：**实现多光谱图像与全色图像融合，生成融合图像**

下面三幅图像依次为：多光谱图像、全色图像、融合图像。可以看出来，多光谱图像颜色信息丰富，全色图像清晰度高，融合图像（由多光谱和全色生成）颜色信息丰富，同时清晰度也较高。下面通过华为手机介绍一些这方面的原理。

![多光谱图像](http://ww1.sinaimg.cn/large/6deb72a3ly1fvple5jro2j20b309l440.jpg)

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvplhfmekwj20b309kgpd.jpg)

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvplht8v3cj20b309lqb4.jpg)

2016年，华为发布了华为P9，除了在外观配置之外，非常值得关注的是那两颗摄像头，华为P9上的摄像头其中一颗为黑白镜头，这又是为什么呢？ 

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvpllncftkj20d009htfe.jpg)

目前绝大多数的数码相机所使用的图像传感器都是CMOS或者CCD，这两种技术都只能感应光线的强度（即只能拍黑白及灰度），而不能记录光线的颜色。柯达公司发现黄色滤镜可以滤除或减小红、蓝光对照片的影响，在传统黑白感光元件的每一个像素上都装上不同种彩色滤镜，不就可以记录光线里面三原色的色彩信息。于是通过合成，彩色数码照片就诞生。这种滤镜则被称为“拜耳滤镜”。 

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvplndrvkaj20dw091aax.jpg)



但是这种技术依然存在弊端：拜耳滤镜虽然让感光元件有了记录色彩的能力，但是它也会挡住三分之二的入射光线，这也导致实际照射到CMOS上感光成像的光线只有原来的三分之一。 

![](C:\Users\Administrator\Desktop\gdal.jpg)

▲每个像素都只接受了一种颜色，其余颜色则无法射入 

光线减少，ISO的感光度则需要提高，而ISO提高之后噪点则会增加，为了增加进光量开门时间也需要增加，手抖的因素则会导致成片质量下降。在光照充足的白天这种影响并不大，但是到了夜晚之后成像效果则要大打折扣。 

但是如果放弃拜耳滤镜，直接采用黑白摄像头，那么进光量的问题不是就可以解决了吗？黑白摄像头就是优点再多，如果只是拍摄黑白照片那么依旧只是一个小众产品，人们早已习惯了色彩绚丽的彩色照片，不可能再回到黑白年代。所以黑白摄像头的最佳用途就是作为一颗辅助摄像头，来弥补彩色摄像头的不足，把拜尔滤镜损失的那些东西重新找回来。 

华为P9所使用的是与索尼合作深度定制的IMX286彩色传感器，也是1200万像素。彩色+黑白双摄像头这种创新技术确实值得赞赏，但真正影响照片画质的，应该还是最后的融合算法。所以发布会上华为强调了与莱卡联合完成的成像质量软件调校，照片质量会更上一层楼。

------

和华为手机原理类似，早期的遥感卫星只搭载一个全色传感器（只能感应光线的强度信息，可理解为黑白相机）。但是人类不喜欢长期看黑白的地物，还是喜欢看彩色的，因此目前卫星上一般同时搭载一个多光谱传感器，但是，和前面提取滤镜的原理类似，多光谱传感器往往空间分辨率低。所以，需要使用算法融合全色和多光谱图像，生成的融合图像既有全色图像的高分辨率，又有多光谱图像的光谱信息。

### 4. GDAL库介绍

接下来，要提到遥感图像处理，就不得不提一个开源库：GDAL。



![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvpl0bzqldj205w06bgls.jpg)

GDAL 的全称为： Geospatial Data Abstraction Library

也许你不玩遥感，不懂这个库到底有什么用。但是只要你涉及遥感领域，你就必须要知道这个库的价值。

GDAL is a translator library for raster and vector geospatial data formats that is released under an X/MIT style Open Source license by the Open Source Geospatial Foundation. 

链接：https://www.gdal.org/

简单地说，GDAL是一个操作各种栅格地理数据格式的库，包括读取、写入、 转换、 处理各种栅格数据格式（ 有些特定的格式对一些操作如写入等不支持）。

最最最重要的是这个库是跨平台的， 开源的！ 如今这个库对各种数据格式的支持强大到令人啧啧的地步了。 如果你对他的强大有什么怀疑的话， 看看 这里一大串的GDAL所支持格式清单， 吓到了吧！再看看它的主页最后那些使用了它作为底层数据处理的软件列表吧！你总该知道 Google Earth吧！ 不知道？ 赶快下一个去玩玩－－ 会当临绝顶， 一览众山小！

------

### 5. 安装 VC 2010 express

在本课程设计中，我们使用的是Visual C++ 2010 express （指学习版），下载链接：https://ouceducn-my.sharepoint.com/:u:/g/personal/gaofeng_ouc_edu_cn/EeLSAzv5H5tIgXPm3bADAJcBklbVYqMXw-G6G39t4ZGmEg?e=PZSmoS

现在VS版本非常多，有2010，2012，2013，2015，2017，貌似2019也快要出了。开发平台版本更新太快，并不影响我们对代码本身的研究。VC++ 2010安装程序非常小，不到100MB，使用起来效率非常高，因此本课程设计，选择2010版本。

另外，gdal库在使用中，需要三种文件：头文件、lib文件、dll文件。我已经把三种文件整理好，方便同学们自由下载。下载链接：https://ouceducn-my.sharepoint.com/:u:/g/personal/gaofeng_ouc_edu_cn/ES8fvuffgxdOs7lSwpTzocYBd4gJFrDde6vNIXFhQyMWeQ?e=7LbJgt

其中，头文件就是在程序编译时需要的头文件（在gdal目录中），gdal18.dll就是程序在运行中可能会用到的dll动态链接库文件，gdal_i.lib 就是程序在编译过程中需要链接的库文件。 

按照我的习惯，不喜欢在系统里安装现有库（opencv一类的），然后把这些库添加到系统环境变量。因为经常项目需要换一台电脑运行程序时，因为开发库安装的目录不同，很容易导致程序无法运行。因此，下面给出一种简单的解决思路。

**编译分为debug和release两种模式。** Debug 通常称为调试版本，它包含调试信息，并且不作任何优化，便于程序员调试程序。Release 称为发布版本，它往往是进行了各种优化，使得程序在代码大小和运行速度上都是最优的，以便用户很好地使用。

因为本人在做项目过程中，基本上很少使用debug模式，所以开发中，一般只使用release版本的库。所以，我提供的都是release版本的lib和dll文件。 

### 6. 第一个程序

首先，新建一个工程firstDemo，选择win32控制台程序。注意，不要为解决方案创建目录。

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvpmsszo4aj20zw0modim.jpg)

然后给项目添加一个main.cpp，先添加头部引用，如下：

```c++
#include <iostream>
using namespace std;
#include "./gdal/gdal_priv.h"
```

可以发现，头文件的引用上会提示错误。因为当前目录下并没有 gdal 的头文件。因此，把 gdal 文件夹拷到当前项目目录里，再次编译就能找到文件了。

接着，再添加以下代码：

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvpo85ectqj20oq10wdhr.jpg)

试着运行，发现会提示error LNK2001: 无法解析的外部符号 ”void __cdel“。 这是因数光有头文件，没有lib文件引起的。所以，我们要加入lib文件。这里具体方法是，把需要用到的lib文件拷到项目目录下。一般熟悉OPENCV的同学可能知道，这时的解决方案是，把opencv的lib文件写到vs的配置里。但这样如果换一台电脑运行，会导致程序找不到库文件，会报错。因此，我们使用另一种方法：

把需要用到的lib文件拷到项目目录下，然后在 #include "./gdal/gdal_priv.h" 这一行下面，添加如下代码：

```
#pragma comment(lib, "gdal_i.lib")
```

这样就能编译通过了。

但是，此时执行程序，系统会报错：

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvpnpim87fj20it06yt8v.jpg)

这是因为运行时，还需要提供gdal的动态链接库。把gdal18.dll拷到项目的Release目录下，程序就可以运行了。

本程序的效果为：输入一个名为 trees.jpg 的图像，然后把这个图像数据写入到 res.tif 文件中。

trees.jpg 图像如下：

![](./pic/trees.jpg)



### 7. 本周编程作业

完成这个程序，把主要步骤过程、遇到的问题记录下来，保存到小组博客的 reporsitory 中。

使用 markdown 格式，保存为md文件（可以建立一个MD文件，本地用 typora 编辑，然后利用Github Desktop实现服务器端与本地的同步）。

图片可使用新浪微博图床chrome插件（地址：http://www.cnplugins.com/photos/xinlangweibotuchuang/），图床的使用方法请自已摸索，或向老师同学咨询。

有问题欢迎随时联系我~~~(^_^)



![](http://ww1.sinaimg.cn/large/6deb72a3ly1fvq9mvnemwj20ev07mjrz.jpg)





