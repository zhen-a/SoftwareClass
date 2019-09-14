## 教程5：IHS遥感图像融合

### 1. 颜色模型

目前常用的颜色模型一种是RGB三原色模型，另外一种广泛采用的颜色模型是亮度、色调、饱和度（IHS颜色模型）。 

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fxibwt3dgbj20sg0d0wmx.jpg)



亮度表示光谱的整体亮度大小，对应于图像的空间信息属性；

色调描述纯色的属性，决定于光谱的主波长，是光谱在质的方面的区别；

饱和度表征光谱的主波长在强度中的比例，色调和饱和度代表图像的光谱分辨率。  

IHS变换图像融合就是建立在IHS空间模型的基础上，其基本思想就是在IHS空间中，将低空间分辨率的多光谱图像的亮度分量用高空间分辨率的灰度图象替换。  

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fxibypa9spj20hv08ndfz.jpg)



### 2. 基本步骤

(1) 将多光谱影像重采样到与全色影像具有相同的分辨率；

(2) 将多光谱图像的Ｒ、Ｇ、Ｂ三个波段转换到IHS空间，得到Ｉ、Ｈ、Ｓ三个分量；

(3) 以Ｉ分量为参考，对全色影像进行直方图匹配 

(4) 用全色影像替代Ｉ分量，然后同Ｈ、Ｓ分量一起逆变换到RGB空间，从而得到融合图像。



RGB与IHS变换的基本公式： 

![](http://ww1.sinaimg.cn/large/6deb72a3ly1fxid5nkncij20i7077wf5.jpg)





### 3.本周任务

使用IHS变换，对下面图像进行融合。

多光谱图像（下载地址：https://ouceducn-my.sharepoint.com/:i:/g/personal/gaofeng_ouc_edu_cn/EW7E8fA-R31Lpy8Jg8z4V1sBo0qc8d39LKjp7P_dJKh68g?e=yJxI6w）

全色图像（下载地址：https://ouceducn-my.sharepoint.com/:i:/g/personal/gaofeng_ouc_edu_cn/ESZEvOTn3ElJkgJkMFZ146UBjQVSHheBuL9GNwUVglu7fA?e=61BXaE）

附MATLAB实现供参考。

```matlab

clear;
clc;

% ====== parameter begin ======================= 
im_mul = double(imread('American_mul.bmp'));
im_pan = double(imread('American_pan.bmp'));
[ylen, xlen, c] = size(im_mul);

R = im_mul(:,:,1);
G = im_mul(:,:,2);
B = im_mul(:,:,3);

im_fus = zeros(ylen, xlen, 3);
% ====== parameter end =========================


% RGB ==> IHS 正变换矩阵
tran1 = [ 1/3,         1/3,         1/3;       
          -sqrt(2)/6,  -sqrt(2)/6,  sqrt(2)/3;      
          1/sqrt(2),   -1/sqrt(2),  0         ];
% IHS ==> RGB 逆变换矩阵
tran2 = [ 1,  -1/sqrt(2),  1/sqrt(2);       
          1,  -1/sqrt(2),  -1/sqrt(2);      
          1,  sqrt(2),     0           ];

trans_in = [reshape(R, 1, ylen*xlen); ...
            reshape(G, 1, ylen*xlen); ...
            reshape(B, 1, ylen*xlen)];

% 进行 RGB ==> IHS 正变换
trans_res = tran1 * trans_in;
% 利用 PAN 替换 I 分量
trans_res(1,:) = reshape(im_pan, 1, ylen*xlen);
% 进行 IHS ==> RGB 反变换
trans_res = tran2 * trans_res;

% 把变换结果保存到 im_fus 中
im_fus(:,:,1) = reshape(trans_res(1, :), ylen, xlen);
im_fus(:,:,2) = reshape(trans_res(2, :), ylen, xlen);
im_fus(:,:,3) = reshape(trans_res(3, :), ylen, xlen);
im_fus = uint8(im_fus);

imwrite(im_fus, 'American_FUS.bmp');
```

























