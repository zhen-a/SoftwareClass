## 教程3：简单扣像

考虑到同学们已经掌握了基本的像素值操作，本周布置一个简单的扣像作业。

在电脑、电视制作中经常用绿幕作背景，这么做的道理非常简单，因为便于电脑处理，后期抠像合成。凡是绿色部分做成透明，加上需要的背景，特效就出来了。

《死侍》电影中效果如下：

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fwg83duzvoj20f608f7fl.jpg)

实际中为：

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fwg83sn4prj20f408hdn8.jpg)



《少年派的奇幻漂流》电影中效果如下：

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fwg85k2vnnj20f608cna8.jpg)

实际中为：

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fwg865n27fj20f508bahp.jpg)



《霍比特人》电影中效果如下：

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fwg87c9wybj20f309mgyv.jpg)

实际效果为：

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fwg87refwmj20f009mam1.jpg)



用绿色做幕布是很有道理的，因为主体一般是人物，人的衣服头发肤色中，最少见的颜色是绿色，那么绿色的应用就最为广泛，所在大家渐渐把特效背景，叫做绿幕。

本周布置一道绿幕扣图题：

提供两张图：space.jpg和superman.jpg，两张图的大小均为640*480像素。两张图像分别如下：

space.jpg

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fwg8crwsl4j20hs0dcju4.jpg)



superman.jpg

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fwg8dir8dlj20hs0dcn23.jpg)



要求为：把superman.jpg中的超人抠出，加到 space.jpg 的太空背景中。效果如下图所示：

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fwg8m4r5aej20hs0dd403.jpg)



思路：superman.jpg图像中的像素值用（r,g,b）表示，条件1：10<r<160; 条件2：100<g<220; 条件3：10<b<110；同时满足这三个条件的像素为超人区域像素。把超人区域像素，填到 space.jpg 中即可。

