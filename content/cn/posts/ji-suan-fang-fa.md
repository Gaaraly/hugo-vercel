---
title: '计算方法'
date: 2020-03-12 15:50:52
categories: ['theory']
---
## 序言

计算方法随想笔记，待补充......
<!--more-->
## 非线性方程的求根

### 牛顿迭代法

将非线性方程线性化——*Taylor*展开:
$$
x_{k+1}=x_{k}-\frac{f(x_k)}{f'(x_k)}
$$


## 线性方程组的直接法

### 高斯—若尔当消元法

利用列主元消元法和若尔当消元法可以求出某矩阵的**逆矩阵**：

找出列最大元，进行标准化消元，然后找到下一列的最大元，标准化后进行**上下**消元。

### 三角分解法

- 若A的所有顺序主子式均不为0，则A的LU分解唯一（其中L为单位下三角阵）

~~看不懂~~

## 线性方程组的迭代法

### 线性方程组的迭代原理

将线性方程组转化，建立迭代格式：
$$
x^{k+1} = Bx^k + f
$$
初始向量取\\((0,0,0)^T\\),进行迭代求解。

### 向量范数

向量\\(\vec{X} = (x_1,x_2,\cdots,x_n)^T\\)的\\(L_p\\)范数定义为：
$$
||\vec{X}||\_p = (\sum_{i=1}^n |x_i|^p)^{1/p}
$$
当\\(P=\infty\\)时：
$$
||\vec{X}||\_\infty =\mathop{max}_{1{\leq}i{\leq}n}|x_i|
$$


- \\(R^n\\)上的一切范数都等价。

### 矩阵范数

- *Frobenius*范数：

$$
||\vec{X}||\_F=\sqrt{\sum^n_{i=1}\sum^n_{j=1}|a_{ij}|^2}
$$

​	    对方阵\\(A\in R^{n\times n}\\)以及\\(\vec x\in R^n\\)有
$$
||A\vec x||_2 \leq ||A||_F \cdot ||\vec x||_2
$$

-  算子范数：

  由向量范数$||\cdot||_p$导出矩阵$A\in R^{n\times n}$的*p*范数：
  $$
  ||A||_p=max\frac{||A\vec x||_p}{||\vec x||_p}=\mathop {max}_{||\vec x||_p=1}||A\vec x||_p
  $$
  则$||AB||_p \leq ||A||_p||B||_p$

  ​    $||A\vec x||_p \leq ||A||_p||\vec x||_p$

  

  特别有:
  \\(||A||\_\infty = {max}\_{1\leq i \leq n} \sum^n_{j=1}|a_{ij}|\\)	(行和范数：每一行分量的绝对值之和的最大值)

  \\(||A||\_1 = {max}\_{1\leq j \leq n} \sum^n_{i=1}|a_{ij}|\\)	 (列和范数：每一列分量的绝对值之和的最大值)

  \\(||A||\_2 = \sqrt{\lambda_{max}(A^TA)}\\)			  （谱范数）

------

  谱半径：
  $$
  \rho(A)=\mathop {max}_{1\leq i\leq n}|\lambda_i|
  $$

  - 对任意算子范数$||\cdot||$有$\rho(A) \leq ||A||$

  

### 线性方程组的误差分析

**$||A||\cdot||A^{-1}||$**是关键的**误差方法因子**，称为A的条件数，记为*cond(A)*,其越大，则A越病态，难得准确解。

如果***cond(A)* >> 1**,则称A为坏条件，或称A为**病态**的。

- $$
  cond(A)_1=||A||_1\cdot||A^{-1}||_1
  $$

- $$
  cond(A)_\infty=||A||_\infty\cdot||A^{-1}||_\infty
  $$

- $$
  \begin{aligned}
  cond(A)_2 & =||A||_2\cdot||A^{-1}||_2 \\
  & =\sqrt{\lambda_{max}(A^TA)/\lambda_{min}(A^TA)}
  \end{aligned}
  $$

若A对称，则$cond(A)_2=\frac{max|\lambda|}{min|\lambda|}$

------

注:一般判断矩阵是否病态，并不计算$A^{-1}$，而由经验得出。

- 行列式很大或很小(如某些行、列近似相关) ;
- 元素间相差大数量级， 且无规则;
- 主元消去过程中出现小主元;
- 特征值相差大数量级。

### Jacobi法

当$a_{ii} \neq 0$时，可以将正常方程组的第n个方程移项，解出$x_n$的表达式，如：
$$
x_1=\frac{1}{a_{11}}(-a_{12}x_2-{\dots}-a_{1n}x_n+b_1)
$$
即$A\vec x=\vec b \implies \vec x=B\vec x+\vec f$，

然后建立迭代格式$\vec x^{k+1}=B\vec x^{k}+\vec f$。
$$
\vec x^{k+1}=-D^{-1}(L+U)\vec x^{k}+D^{-1}\vec b
$$


### Gauss - Seidel迭代法

Gauss - Seidel迭代法是将Jacobi法的*L* × $\vec x^{k+1}$所得，即：
$$
\vec x^{k+1}=-D^{-1}(L\vec x^{k+1}+U\vec x^{k})+D^{-1}\vec b
$$
得：
$$
\vec x^{k+1}=\underbrace{-(D+L)^{-1}U}\_{B}x^{k}+\underbrace{(D+L)^{-1}\vec b}_{{\vec f}}
$$
此法较于Jacobi迭代法，收敛速度要更快，迭代结果更精确，内存占用空间更少，单少部分情况并非如此。

### 迭代法的收敛性