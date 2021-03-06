## 教程6：遥感图像的分块处理

### 1. 基本原理

在遥感图像处理中一般采取分块读写的方法，因为遥感图像尺寸巨大（长宽可达30000像素以上），不可能开辟一个大内存把整个图像读进来。分块读取的道理一般大家都懂，但处理上却是有学问的。

在大图像处理中磁盘I/O一般是效率的主要瓶颈。因此如何分块的着眼点应该是如何减少磁盘I/O。一般的图像处理系统采取将块分成256 * 256或者512 * 512的块。实际上这是不利于减少磁盘I/O次数。如下图：

 ![](http://ww1.sinaimg.cn/mw690/6deb72a3ly1fxlxw7330oj20me0ehq3w.jpg)



假设上图是按照256 * 256进行分块的，一般而言图像文件在磁盘上是按行存贮的，就是从第一行到最后一行的，那么我们很快就可以看到其中弊端，就是当将数据写入到某一块时，其写入顺序是怎样的呢？首先当然是从块的起始地址写，将块的第一行的数据写入，这时你要问块的第二行数据和第一行数据在连续的？很显然，它们不是连续的。那么你读写时必须先移动文件指针，那么读取一块数据你就需要移动256次文件指针。整幅图像就需要至少移动块数 * 256次文件指针，这样的磁盘I/O次数有点惊人。

因此，一般遥感图像处理的分块策略为如下：

![](http://ww1.sinaimg.cn/mw690/6deb72a3ly1fxlxzkh2bjj20me0jgjry.jpg)



这种分块方法是采取一次读取若干行的方法。这种方法有两个好处：首先降低了程序的逻辑复杂度，每块只是行号变化，块的起始列位置都是0，不需要变化；另一方面，这种分块能大大减少磁盘的I/O次数，这是因为每一块的每行数据在磁盘上都是连续的，因此在读写时只需将文件指针定位到块的起始位置，就能实现整块的读写。毫无疑问，这种分块方法比前一种分块方法磁盘I/O次数要少很多。



### 2. 本周任务

使用上周学习的IHS方法，对宽幅遥感图像分别使用两种分块方式进行融合：

第一种方式：使用256 * 256 大小的块；

第二种方式：每块的宽度为图像宽度，高度为256像素。

比较两种分块方式的处理效率。

图像下载地址：https://www.jianguoyun.com/p/DWFm_ksQrKKIBhjRkIgB





