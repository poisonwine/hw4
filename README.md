## 空域滤波实验报告
#### 摘要
&emsp;&emsp;本实验首先利用不同大小的模板，采用中值滤波和高斯低通滤波方法对前两幅图像进行了平滑处理，对比和分析了两种方法和模板大小不同的区别；针对test2图像，加入高斯白噪声和椒盐噪声，分别用高斯滤波器和中值滤波器对其进行处理，对比和分析了两种滤波器的优缺点。对于图像锐化，采用了
Unsharp masking, Sobel edge detector, Laplace edge detection和Canny algorithm 对图像进行了实验，对比分析了各算法的优缺点。

#### 一.空域低通滤波
#### 1.1 高斯滤波器
&emsp;&emsp;高斯滤波器是根据高斯函数的形状来选择权值的线性平滑滤波器。高斯平滑滤波器对去除服从正态分布的噪声是很有效果的。一维零均值高斯函数为：
<img src="https://github.com/poisonwine/hw4/blob/master/picture/%E5%85%AC%E5%BC%8F%E4%B8%80.png"/>
二维零均值离散高斯函数表达式为：
<img src="https://github.com/poisonwine/hw4/blob/master/picture/%E5%85%AC%E5%BC%8F2.png"/>

&emsp;&emsp;以二维3*3模板为例，假设模板中心点为原点，则模板坐标为：
<img src="https://github.com/poisonwine/hw4/blob/master/picture/%E5%85%AC%E5%BC%8F4.PNG" height="120"/>

&emsp;&emsp;对应高斯卷积模板的数值为：
<img src="https://github.com/poisonwine/hw4/blob/master/picture/%E5%85%AC%E5%BC%8F3.PNG" height="120"/>

&emsp;&emsp;可以看出，高斯卷积模板是中心对称的，由此很容易推广到n * n。对于一个n * n的高斯卷积模板，我们可以通过以下步骤计算出值：
* 给定标准差sigma；
* 按照二维高斯分布函数计算数值,注意中心点为（(n+1)/2,(n+1)/2）；
* 归一化

&emsp;&emsp;利用MATLAB编写的主程序如下：
```
function tem=gausstemplet(n,sigma)
b=zeros(n,n)
n1=floor((n+1)/2);%计算中心
weight=2*pi*sigma*sigma
 for i=1:n
    for j=1:n
       b(i,j) =exp(-((i-n1)^2+(j-n1)^2)/(2*sigma*sigma))/weight;
    end
 end
 tem=b/sum(b(:))
 ```
 &emsp;&emsp;计算出模板的数值，利用卷积函数`conv2`即可计算滤波后的图像。采用不同大小模板进行高斯滤波的结果如下：
 <img src="https://github.com/poisonwine/hw4/blob/master/picture/test1.png" width="420"/> <img src="https://github.com/poisonwine/hw4/blob/master/picture/gausstest1_3.png" width="425"/>
 <img src="https://github.com/poisonwine/hw4/blob/master/picture/gausstest1_5.png" width="425"/> <img src="https://github.com/poisonwine/hw4/blob/master/picture/gausstest1_7.png" width="425"/>
 <img src="https://github.com/poisonwine/hw4/blob/master/picture/test2.png" width="425"/> <img src="https://github.com/poisonwine/hw4/blob/master/picture/gausstest2_3.png" width="425"/>
 <img src="https://github.com/poisonwine/hw4/blob/master/picture/gausstest2_5.png" width="425"/> <img src="https://github.com/poisonwine/hw4/blob/master/picture/gausstest2_7.png" width="425"/>
 
 &emsp;&emsp;观察第一幅图像可以发现，随着高斯滤波模板大小的增加，图像的平滑效果变得更好。而第二幅图像效果不太明显，于是本文对第二幅图像加入高斯白噪声（均值为0,方差为0.05），然后利用不同大小的高斯滤波模板进行处理，结果如下：
 <img src="https://github.com/poisonwine/hw4/blob/master/picture/gaussnoise1.png" width="425"/> <img src="https://github.com/poisonwine/hw4/blob/master/picture/gaussnoise3.png" width="425"/>
 <img src="https://github.com/poisonwine/hw4/blob/master/picture/gaussnoise5.png" width="425"/> <img src="https://github.com/poisonwine/hw4/blob/master/picture/gaussnoise7.png" width="425"/>
 
&emsp;&emsp;可以明显看出，当加入高斯白噪声后，随着高斯模板增大，图像的平滑效果更好。

#### 1.2 中值滤波
&emsp;&emsp;中值滤波法是一种非线性平滑技术，它将每一像素点的灰度值设置为该点某邻域窗口内的所有像素点灰度值的中值。基本原理是把数字图像或数字序列中一点的值用该点的一个邻域中各点值的中值代替，让周围的像素值接近的真实值，从而消除孤立的噪声点，对消除椒盐噪声效果很好。

&emsp;&emsp;基于原理编写的MATLAB程序见`源代码.md`。实现的效果如下：
<img src="https://github.com/poisonwine/hw4/blob/master/picture/test1.png" width="425"/> <img src="https://github.com/poisonwine/hw4/blob/master/picture/test1_3.png" width="425"/>
 <img src="https://github.com/poisonwine/hw4/blob/master/picture/test1_5.png" width="425"/> <img src="https://github.com/poisonwine/hw4/blob/master/picture/test1_7.png" width="425"/>

&emsp;&emsp;可以看出，和高斯滤波一样，中值滤波随着模板的增加平滑效果也变得更好。但是也可以看出，针对第一幅`test1`图像，中值滤波的效果要好于高斯滤波。

&emsp;&emsp;第二幅图像采用中值滤波的效果如下：

<img src="https://github.com/poisonwine/hw4/blob/master/picture/test2.png" width="425"/> <img src="https://github.com/poisonwine/hw4/blob/master/picture/test2_3.png" width="425"/>
<img src="https://github.com/poisonwine/hw4/blob/master/picture/test2_5.png" width="425"/> <img src="https://github.com/poisonwine/hw4/blob/master/picture/test2_7.png" width="425"/>

#### 1.3 高斯滤波与中值滤波的对比
&emsp;&emsp;都选取7*7的模板对`test2`图像进行平滑处理。分别加入高斯白噪声（均值0，方差0.05）和椒盐噪声，实验的效果如下：
<img src="https://github.com/poisonwine/hw4/blob/master/picture/duibi1.png" />
<img src="https://github.com/poisonwine/hw4/blob/master/picture/duibi2.png" />

&emsp;&emsp;可以观察出，加入高斯白噪声后，高斯滤波的效果要优于中值滤波；加入椒盐噪声后，中值滤波要优于高斯滤波。由此可知，高斯滤波针对分布较规律的噪声效果较好，而中值滤波针对单点噪声效果更好。

#### 二.高通滤波器
&emsp;&emsp;与低通滤波器相反，高通滤波器允许图像高频部分通过，而高频部分证实图像的边缘信息（或者噪声）,主要用于图像锐化。常用的算子有sobel算子,laplace算子。
#### 2.1 Unsharp masking
&emsp;&emsp;反锐化掩膜Unsharp Masking用于增强图像的边缘;算法步骤如下：
* 模糊原图；
* 原图减去模糊图，得到Mask；
* Mask乘以一个正系数kk加到原图上，输出。特别的，k=1时算法称为Unsharp masking。

&emsp;&emsp;MATLAB程序见`源代码.md`中`task2_unsharp.m`。针对`test3`,`test4`图像的实验效果如下。结果得到了边缘更加清晰的图像，与预期改进效果一致。但同时看到也会引进一些不希望看到的噪声。

<img src="https://github.com/poisonwine/hw4/blob/master/picture/test3_unsharp.png" width="650"/> 
<img src="https://github.com/poisonwine/hw4/blob/master/picture/test41_unsharp.png" width="650"/> 

#### 2.2 Sobel edge detector
&emsp;&emsp;Sobel算子也叫 Sobel 滤波, 是两个 3*3 的矩阵, 主要用来计算图像中某一点在横向/纵向上的梯度,。若用A表示原始图像，则G(x)和G(y)分别表示经过水平和垂直边缘检测得到的图像灰度值，然后我们通过G(x)和G(y)的平方和开根号作为最终图像的灰度值。

<img src="https://github.com/poisonwine/hw4/blob/master/picture/sobel.png" width="450" height="120"/> 

&emsp;&emsp;MATLAB程序见`源代码.md`中`task2_sobel.m`。针对`test3`,`test4`图像的实验效果如下。
<img src="https://github.com/poisonwine/hw4/blob/master/picture/test3_sobel.png" width="425"/> <img src="https://github.com/poisonwine/hw4/blob/master/picture/test41_sobel.png" width="425"/> 

#### 2.3 Laplace edge detection
&emsp;&emsp;Laplacian 算子是n维欧几里德空间中的一个二阶微分算子，定义为梯度的散度。因此如果f是二阶可微的实函数，则f的拉普拉斯算子定义为：
<img src="https://github.com/poisonwine/hw4/blob/master/picture/laplace1.jpg"/>

&emsp;&emsp;为了更适合于数字图像处理，将该方程表示为离散形式： 
<img src="https://github.com/poisonwine/hw4/blob/master/picture/laplace2.jpg"/>

&emsp;&emsp;MATLAB程序见`源代码.md`中`task2_laplace.m`。针对`test3`,`test4`图像的实验效果如下:
<img src="https://github.com/poisonwine/hw4/blob/master/picture/test3_laplace.png" width="425"/> <img src="https://github.com/poisonwine/hw4/blob/master/picture/test41_laplace.png" width="425"/> 
<img src="https://github.com/poisonwine/hw4/blob/master/picture/test42_laplace.png" width="425"/> 

#### 2.4 Canny边缘检测
&emsp;&emsp;Canny边缘检测是一种非常流行的边缘检测算法，是John Canny在1986年提出的。它是一个多阶段的算法，是图像边缘检测算法中最经典、有效的算法之一。它由多个步骤构成：
* 使用高斯滤波器，以平滑图像，滤除噪声；
* 计算图像中每个像素点的梯度强度和方向;
* 应用非极大值（Non-Maximum Suppression）抑制，以消除边缘检测带来的杂散响应。有多个像素宽的边缘变成一个单像素宽的边缘。即“胖边缘”变成“瘦边缘”；
* 应用双阈值（Double-Threshold）检测来确定真实的和潜在的边缘；
* 通过抑制孤立的弱边缘最终完成边缘检测。

&emsp;&emsp;
