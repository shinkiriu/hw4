# hw4
###第一题：空域低通滤波器：分别用高斯滤波器和中值滤波器去平滑测试图像test1和2，模板大小分别是3x3 ， 5x5 ，7x7； 分析各自优缺点；


## 摘要：直接调用了imfilter和medfilt2来构建高斯滤波器和中值滤波器，对图像直接处理。


 **运行结果**	见文件夹图片


 ------------------------------------------------------------
 
###第二题:2.-利用固定方差 sigma=1.5产生高斯滤波器. 附件有产生高斯滤波器的方法； 分析各自优缺点；
   
分析：自定义了一个sigma值=1.5，同时构筑了循环写了一个高斯滤波器.

 **运行结果**

H =

    0.0101    0.0307    0.0597
    0.0307    0.0931    0.1814
    0.0597    0.1814    0.3533



 ------------------------------------------------------------
###第三题：利用高通滤波器滤波测试图像test3,4：包括unsharp masking, Sobel edge detector, and Laplace edge detection；Canny algorithm.分析各自优缺点；

 分析：直方图匹配又称为直方图规定化，是指将一幅图像的直方图变成规定形状的直方图而进行的图像增强方法。T1, T2分别为输入图像，模板直方图的均衡化后得到的变换函数,构建T1,T2的关系得到T3，即最后的转换矩阵。


 **运行结果** 见文件夹图片




 -------------------------------------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------------------------------------
###源代码
-------------------------------------第一题------------------------------

file_path='C:\Users\msi\Desktop\第四次作业\第4-次作业\' ;
image1=imread(strcat(file_path,'test1.pgm'));
image2=imread(strcat(file_path,'test2.tif'));
sigma1 = 3;           %高斯正态分布标准差
n=7;                  %确定模板尺寸

gausFilter = fspecial('gaussian',[n,n]);   %高斯滤波
gau_image1=imfilter(image1,gausFilter);        %对任意类型数组或多维图像进行滤波 
gau_image2=imfilter(image2,gausFilter);        %对任意类型数组或多维图像进行滤波 
mid_image1=medfilt2(image1,[n,n]);
mid_image2=medfilt2(image2,[n,n]);
figure
subplot(2,2,1);
imshow(gau_image1);
title('test1高斯滤波')
subplot(2,2,2);
imshow(gau_image2);
title('test2高斯滤波')
subplot(2,2,3);
imshow(mid_image1);
title('test1中值滤波')
subplot(2,2,4);
imshow(mid_image2);
title('test2中值滤波')

-------------------------------------第二题------------------------------

file_path='C:\Users\msi\Desktop\第四次作业\第4-次作业\' ;
image1=imread(strcat(file_path,'test1.pgm'));
image2=imread(strcat(file_path,'test2.tif'));
sigma = 1.5;           %高斯正态分布标准差
n=1;                  
N=2*n+1;              %确定模板尺寸
sum=0;
for i=1:N
    for j=1:N
        F=double((i-N-1)^2+(j-N-1)^2);
        H(i,j)=exp(-F/(2*sigma*sigma))/(2*pi*sigma);
        sum=H(i,j)+sum;
    end
end
H=H/sum

-------------------------------------第三题------------------------------

file_path='C:\Users\msi\Desktop\第四次作业\第4-次作业\' ;
f=imread(strcat(file_path,'test4 copy.bmp'));
% f = double(f);
f1 = imsharpen(f);
l = fspecial('laplacian');
f2 = imfilter(f,l);
f3 = edge(f,'sobel');
f4 = edge(f,'canny');
figure
subplot(2,2,1);imshow(f1);title('unsharpen masking');
subplot(2,2,2);imshow(f2);title('laplacian');
subplot(2,2,3);imshow(f3);title('sobel');
subplot(2,2,4);imshow(f4);title('canny');
