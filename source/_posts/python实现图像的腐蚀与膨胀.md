---
title: python实现图像的腐蚀与膨胀
date: 2021-04-06 21:40:26
tags: 
---
对于图像的每个像素，取其一定的邻域，计算最大值/最小值作为新图像对应像素位置的像素值。其中，取最大值就是膨胀，取最小值就是腐蚀。
腐蚀膨胀算法步骤：
（1）将输入图像转化为灰度图；
（2）将灰度图进行二值化处理，此处我用otsu算法；
（3）对二值化图像进行腐蚀（取像素点邻域最小值赋于像素点）和膨胀（取像素点邻域最大值赋于像素点）处理；
腐蚀膨胀算法如下：
import cv2 as cv
import numpy as np

# 如果图像过大处理速度慢，可采用以下函数裁剪图像大小
def caijian(img):
    h=img.shape[0]
    w=img.shape[1]
    print("原图像大小为:",h,w)
    img1=cv.resize(img, (300, 300), interpolation=cv.INTER_CUBIC)
    h1=img1.shape[0]
    w1=img1.shape[1]
    print("裁剪后图像大小为:",h1,w1)
    return img1

def rgb2gray(img):
    h=img.shape[0]
    w=img.shape[1]
    img1=np.zeros((h,w),np.uint8)
    for i in range(h):
        for j in range(w):
            img1[i,j]=0.144*img[i,j,0]+0.587*img[i,j,1]+0.299*img[i,j,2]
    return img1

# 二值化
def otsu(img):
    h=img.shape[0]
    w=img.shape[1]
    m=h*w   # 图像像素点总和
    otsuimg=np.zeros((h,w),np.uint8)
    threshold_max=threshold=0   # 定义临时阈值和最终阈值
    histogram=np.zeros(256,np.int32)   # 初始化各灰度级个数统计参数
    probability=np.zeros(256,np.float32)   # 初始化各灰度级占图像中的分布的统计参数
    for i in range (h):
        for j in range (w):
            s=img[i,j]
            histogram[s]+=1   # 统计灰度级中每个像素在整幅图像中的个数
    for k in range (256):
        probability[k]=histogram[k]/m   # 统计每个灰度级占图像中的分布
    for i in range (255):
        w0 = w1 = 0   # 定义前景像素点和背景像素点灰度级占图像中的分布
        fgs = bgs = 0   # 定义前景像素点灰度级总和and背景像素点灰度级总和
        for j in range (256):
            if j<=i:   # 当前i为分割阈值
                w0+=probability[j]   # 前景像素点占整幅图像的比例累加
                fgs+=j*probability[j]
            else:
                w1+=probability[j]   # 背景像素点占整幅图像的比例累加
                bgs+=j*probability[j]
        u0=fgs/w0   # 前景像素点的平均灰度
        u1=bgs/w1   # 背景像素点的平均灰度
        g=w0*w1*(u0-u1)**2   # 类间方差
        if g>=threshold_max:
            threshold_max=g
            threshold=i
    print(threshold)
    for i in range (h):
        for j in range (w):
            if img[i,j]>threshold:
                otsuimg[i,j]=255
            else:
                otsuimg[i,j]=0
    return otsuimg

# 图像腐蚀
def etch(img):
    h=img.shape[0]
    w=img.shape[1]
    img1=np.zeros((h,w),np.uint8)
    for i in range (1,h-1):
        for j in range (1,w-1):
            min=img[i,j]
            for k in range (i-1,i+2):
                for l in range (j-1,j+2):
                    if k<0|k>=h-1|l<0|l>=w-1:
                        continue
                    if img[k,l]<min:
                        min=img[k,l]
            img1[i,j]=min
    return img1

# 图像膨胀
def expand(img):
    h=img.shape[0]
    w=img.shape[1]
    img1=np.zeros((h,w),np.uint8)
    for i in range (1,h-1):
        for j in range (1,w-1):
            max=img[i,j]
            for k in range (i-1,i+2):
                for l in range (j-1,j+2):
                    if k<0|k>=h-1|l<0|l>=w-1:
                        continue
                    if img[k,l]>max:
                       max=img[k,l]
            img1[i,j]=max
    return img1

image = cv.imread("D:/Testdata/jiantou.jpg")   # 输入图像，根据实际情况而改变路径
caijianimage=caijian(image)   # 裁剪图像大小
grayimage = rgb2gray(caijianimage)   # 转灰
otsuimage = otsu(grayimage)   # 二值化
etchimage=etch(otsuimage)   # 腐蚀
expandimage=expand(otsuimage)   # 膨胀
cv.imshow("caijianimage",caijianimage)   # 输出裁剪图像相当于原图
cv.imshow("grayimage",grayimage)   # 输出灰度图
cv.imshow("otsuimage",otsuimage)   # 输出二值化图像
cv.imshow("etch",etchimage)   # 输出腐蚀图像
cv.imshow("expandimage",expandimage)   # 输出膨胀图像
cv.waitKey(0)
cv.destroyAllWindows()
原文链接:https://blog.csdn.net/Skymelu/article/details/89074274