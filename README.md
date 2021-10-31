# 图像有损压缩(Image Lossy Compression)
- [图像有损压缩(Image Lossy Compression)](#图像有损压缩image-lossy-compression)
- [文献](#文献)
  - [JEPG(Joint Photographic Experts Group)](#jepgjoint-photographic-experts-group)
- [JPEG图像压缩算法原理](#jpeg图像压缩算法原理)
  - [1、图像分割](#1图像分割)
  - [2、颜色空间转换](#2颜色空间转换)
  - [DCT变换](#dct变换)
  - [量化](#量化)
- [代码](#代码)
- [结果](#结果)
  - [评价指标](#评价指标)
- [参考资料](#参考资料)

# 文献
常见的图像压缩算法
![常见的图像压缩算法](./images/20151112120615499.gif)
- 行程长度压缩RLE
- 霍夫曼编码
- LZW算法
- 算术压缩方法
- [JEPG(Joint Photographic Experts Group)](#jepgjoint-photographic-experts-group)
## JEPG(Joint Photographic Experts Group)


# JPEG图像压缩算法原理
## 1、图像分割
将图像分为若干 $8 \times 8 \neq 0$ 大小的子块
## 2、颜色空间转换
需要将 RGB 颜色空间转化为 YCbCr 颜色空间，其中，Y是亮度(Luminance)，Cb 和 Cr 是绿色和红色的“色差值”

研究表明，红绿蓝三基色所贡献的亮度不同，绿色所贡献亮度最多，蓝色所贡献亮度最少。假定红色贡献为 $K_R$，蓝色贡献为 $K_B$，则亮度可以表示为
$$\begin{aligned}
    Y = K_R \cdot R + (1-K_R-K_B) \cdot G + K_B \cdot B
\end{aligned}$$

根据经验值 $K_R=0.299, K_B=0.114$，则有
$$\begin{aligned}
    Y = 0.299 \cdot R + 0.587 \cdot G + 0.114 \cdot B
\end{aligned}$$

蓝色和红色的色差为
$$\begin{aligned}
    Y   &= 0.299   \cdot R + 0.587    \cdot G + 0.114 \cdot B \\
    C_b &= -0.1687 \cdot R - 0.3313   \cdot G + 0.5 \cdot B +128\\
    C_r &= 0.5   \cdot R - 0.4187   \cdot G - 0.0813 \cdot B +128\\
\end{aligned}$$

## DCT变换
将一系列离散的一维数据 $[x_0,x_1,...,x_n]$ 分解为一系列
$$\begin{aligned}
    \begin{bmatrix}
        x_0 \\ x_1 \\ x_2 \\ ... \\ x_{n-1}
    \end{bmatrix}
    = \frac{F_0}{n}
    \begin{bmatrix}
        1 \\ 1 \\ 1 \\ ... \\ 1
    \end{bmatrix}
    +
    \sum_{k=1}^{n-1} \frac{2F_k}{n}
    \begin{bmatrix}
        \cos\frac{k}{2n}\pi \\
        \cos\frac{2k}{2n}\pi \\
        \cos\frac{3k}{2n}\pi \\
        ... \\
        \cos\frac{(2n-1)k}{2n}\pi
    \end{bmatrix}
\end{aligned}$$

其中，变换系数 $F_m$ 为
$$\begin{aligned}
    F_m=\sum_{k=0}^{n-1} x_k \cos[\frac{\pi}{n}m(k+\frac{1}{2})],\quad m=0,1,...,n-1
\end{aligned}$$


一般的二维DCT变换
$$\begin{aligned}
    F(u,v) &=c(u)c(v) \sum_{i=0}^{M-1} \sum_{j=0}^{N-1} f(i,j) \cos(\frac{i+0.5}{M}u\pi) \cos(\frac{j+0.5}{N}u\pi) \\
    c(u) &=\left\{\begin{aligned}
        & \sqrt{\frac{1}{N}}, & \quad u=0 \\
        & \sqrt{\frac{2}{N}}, & \quad u\neq 0
    \end{aligned}\right.
    \quad u,v=0,1,2,...,7
\end{aligned}$$

当原始图像从 RGB 颜色空间转换到 YCbCr 颜色空间之后，需要对每一个 $8 \times 8$ 的图像块进行二维DCT变换
$$\begin{aligned}
    F(u,v) &=c(u)c(v) \sum_{i=0}^{7} \sum_{j=0}^{7} f(i,j) \cos(\frac{i+0.5}{8}u\pi) \cos(\frac{j+0.5}{8}u\pi) \\
    c(u) &=\left\{\begin{aligned}
        & \sqrt{\frac{1}{8}},   & \quad u=0 \\
        & \frac{1}{2},          & \quad u\neq 0
    \end{aligned}\right.
    \quad u,v=0,1,2,...,7
\end{aligned}$$

## 量化
量化的目的是为了丢弃不显著信息分块


# 代码

# 结果
## 评价指标
- 图像质量：客观和主观。主观就是用人去观察评分；客观就是对压缩还原后的图像与原始图像误差进行定量计算。一般都是进行某种平均，得到均方误差；另一种是信噪比。
- 压缩效果：压缩比=原始图像每像素的比特数同压缩后图像每像素的比特数的比值。

# 参考资料
- [Difference between Lossy and Lossless Compression](https://www.thecrazyprogrammer.com/2019/12/lossy-and-lossless-compression.html)
- [JPEG算法解密](https://thecodeway.com/blog/?tag=%e5%8e%8b%e7%bc%a9)
- [数字图像处理（八）图像压缩-有损压缩/压缩算法+matlab](https://blog.csdn.net/packdge_black/article/details/107230600)