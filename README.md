## 空域滤波实验报告
#### 摘要
&emsp;&emsp;本实验首先利用不同大小的模板，采用中值滤波和高斯低通滤波方法对前两幅图像进行了平滑处理，对比和分析了两种方法和模板大小不同的区别；

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
