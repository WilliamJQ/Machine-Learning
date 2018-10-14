# 吴恩达机器学习笔记
### 引言
学习AndrewNg的机器学习有一周的时间了，收获了不少东西。因此想通过这个学习笔记来将所学的知识从头梳理一遍并做一个总结。

* 编程作业见[Programming HomeWorks](https://github.com/WilliamJQ/Machine-Learning/tree/master/Programming%20HomeWorks)文件夹
* 如果无法显示数学公式请下载谷歌Chrome浏览器并安装[GitHub with MathJax](https://chrome.google.com/webstore/detail/github-with-mathjax/ioemnmodlmafdkllaclgeombjnmnbima) 插件

> ## 1. 绪论
> ### 1.1 什么是机器学习
> 这里主要有两种定义：

>Arthur Samuel (1959). Machine Learning: Field of study that gives computers the ability to learn without being explicitly programmed.<br>
这个定义有点不正式但提出的时间最早，来自于一个懂得计算机编程的下棋菜鸟，编程使得计算机通过不断的对弈，不断地计算布局的好坏来“学习”，从而积累经验，这样，这个计算机程序成为了一个厉害的棋手。

>Tom Mitchell (1998) Well-posed Learning Problem: A computer program is said to learn from experience E with respect to some **task T** and some performance measure P, if its **performance on T**, as measured by P, improves with **experience E**.<br>
Tom Mitchell 的定义更现代，也有点拗口，视频中介绍了一个例子，即垃圾邮件分类。对于垃圾邮件分类，文中的三个字母分别代表：

>**T(task)**: 对垃圾邮件分类这个任务。<br>
**P(Performance)**: 垃圾邮件分类的准确程度。<br>
**E(Experience)**: 用户对于邮件进行是否为垃圾邮件的分类（即帮助机器体验、学习）。<br>

> ### 1.2 监督学习与无监督学习
> 监督学习和无监督学习是机器学习的两种常见的类型，以下是他们的定义：
>> **监督学习** **（Supervised Learning）** 即为教计算机如何去完成预测任务（有反馈），预先给一定数据量的输入和对应的结果，建模拟合，最后让计算机预测未知数据的结果。<br>

>>监督学习一般分为以下两种问题:<br>
>>
>> **回归问题（Classification）** <br>
>> 
>> 回归问题即为预测一系列的连续值。

>> 在房屋价格预测的例子中，给出了一系列的房屋面基数据，根据这些数据来预测任意面积的房屋价格。给出照片-年龄数据集，预测给定照片的年龄。<br>
>> 
>> ![](images/Regression.jpg)
>> 
>> **分类问题（Classification）** <br>
>> 
>> 分类问题即为预测一系列的离散值。即根据数据预测被预测对象属于哪个分类。
>> 
>> 视频中举了癌症肿瘤这个例子，针对诊断结果，分别分类为良性或恶性。还例如垃圾邮件分类问题，也同样属于监督学习中的分类问题。
>> ![](images/Classification.jpg)
> 
>>视频中提到支持向量机这个算法，旨在解决当特征量很大的时候(特征即如癌症例子中的肿块大小，颜色，气味等各种特征)，计算机内存一定会不够用的情况。支持向量机能让计算机处理无限多个特征。<br>
<br>

>>**无监督学习（Unsupervised Learning）**<br>
>>
>>相对于监督学习，训练集不会有人为标注的结果（无反馈），我们不会给出结果或无法得知训练集的结果是什么样，而是单纯由计算机通过无监督学习算法自行分析，从而“得出结果”。

> ## 2. 单变量线性回归（Linear Regression With One Variable）
> ### 2.1 模型描述——房价预测训练集
> ![](images/LinearRegressionModal.jpg)
> 房价预测训练集中，同时给出了输入 $x$ 和输出结果 $y$ ，即给出了人为标注的”正确结果“，且预测的量是连续的，属于监督学习中的回归问题。
![](images/LinearRegressionHx.jpg)

>其中 $h$ 代表结果函数，也称为假设(hypothesis) 。这个函数 $h$ 根据输入(房屋的面积)，给出预测结果输出(房屋的价格)，即是一个 $X\rightarrow Y$ 的映射。
> $$h_{\theta}(x) = {\theta_0}+{\theta_1}x$$
> 上式中， $\theta$ 为参数， $\theta$ 的变化才决定了输出结果，不同以往，这里的 $x$ 被我们视作已知(不论是数据集还是预测前的输入)，所以怎样解得  $\theta$ 以更好地拟合数据，成了求解该问题的最终问题。
> 
> 
> 单变量，即只有一个特征(如例子中房屋的面积这个特征)。
> ### 2.2 代价函数（Cost Function）
> ![](images/Jietu20180921-095941.jpg)
> 刚才我们说了 $h_\theta(x)$ 函数的作用是通过给定一个输入 $x$ 返回一个预测的结果 $h$ ，而 $h_\theta(x)$ 中两个参数 $\theta_1$、$\theta_2$ 的取值决定了拟合曲线的好坏程度。因此我们现在所要做的就是要找到拟合最好的 $\theta_1$、$\theta_2$ 的取值。因此我们引入了代价函数 $J(\theta)$ 。
> 
> ![](images/Jietu20180921-100542.jpg)
> 
> 当我们把数据集表示在坐标系之后，上图中的红色的‘x’即是离散的数据，使用线性回归的方法拟合数据只需要绘制一条离所有的离散点距离最近的直线，如上图所示。因此当唯一确定 $\theta$ 的值即可唯一确定这一条曲线。我们可以用一个式子来表示这一直线的性质，也就是上图右上角的式子。我们都知道直线上对应着X的取值即为预测值。因此我们只需要限制预测值与真实值的平方差之和最小即可最小化 $\theta$ 的值。式子中m为离散数据的数量。如下为代价函数 $J(\theta)$ 。 

> $$J(\theta_0,\theta_1) = \frac{1}{2m}\sum_{i=1}^m(\hat{y_i}-y_i)^2 = \frac{1}{2m}\sum_{i=1}^m(h_\theta(x_i) - y_i)^2$$
> ### 2.3 代价函数直观理解
> ![](images/Jietu20180921-102209.jpg)
> 
> 为了更加容易理解代价函数到底做了什么，我们先让第一个参数 $\theta_0=0$ 。当前 $h_\theta(x) = \theta_1x$ ，拟合出来的直线是一条过原点的直线。现在我们来最小化 $J(\theta)$ 。
>
> ![](images/Jietu20180921-102605.jpg) 
> 
> 假设当前给定一个数据集如上图左边所示，当 $\theta_1=1$ 的时候， $h_\theta(x)$ 刚好穿过所有的数据点，也就是说完全拟合。我们通过 $J(\theta)$ 公式算出当前情况下 $J(\theta)$ 的值并在右边的坐标系中标出。
> 
> ![](images/Jietu20180921-103023.jpg)
> 
> ![](images/Jietu20180921-103117.jpg)
> 
> 同理标注出 $\theta$ 取不同的值时代价函数的取值，用一条平滑的曲线连接各点即可得到上图中右侧的图形。我们可以看到 $J(\theta)$ 的函数图像是一个类似于二次函数的图像，开口向上，因此当 $\theta_1$ 取值为1的时候函数达到最低点也就是 $J(\theta)$ 取最小值。而当 $\theta_1=1$ 的时候拟合程度最高。因此说明了代价函数值越小拟合程度越高。
>
> 接下来，我们来讨论 $\theta_1$ 和 $\theta_2$ 都不为零的情况。
> ![](images/Jietu20180921-103931.jpg)
> ![](images/Jietu20180921-103947.jpg)
> 由上图可以看到，$J(\theta_1,\theta_2)$ 的图像不再是二次函数，而是一个向下凹的曲面。我们用等高线的表示方法来表示可以更直观的表示。如下图所示：

>![](images/Jietu20180921-104532.jpg)
>![](images/Jietu20180921-104550.jpg)
>![](images/Jietu20180921-104612.jpg)
>![](images/Jietu20180921-104627.jpg)
>
>由上列举了不同 $\theta$ 的取值，当 $\theta$ 位于圆中心的时候拟合的最好也就是 $J(\theta_1,\theta_2)$ 最小。
>
> ### 2.4 梯度下降算法（Gradient Descent）
> 上面介绍了代价函数以及它的直观表示。我们拟合曲线的时候需要使代价函数最小，因此我们引入梯度下降算法。
> ![](images/Jietu20180921-105725.jpg)
> ![](images/Jietu20180921-105740.jpg)
> ![](images/Jietu20180921-105850.jpg)
> 我们首先定义一个学习率 $\alpha$ ，它决定了梯度下降时沿着梯度每一次迭代的步长。
> 
> 我们让 $\alpha$ 和代价函数的梯度相乘，并用 $\theta_j$ 减去他们的乘积即可更新 $\theta_j$ 的值。我们只需要循环执行这个式子，当下降到局部最小的时候，代价函数的梯度值为零，因此 $\theta$ 的值将不会再改变。
> 
> ![](images/Jietu20180921-110711.jpg)
> 
> **有几点得引起我们的注意：** <br>

> 
> * 学习率 $\alpha$ 的取值如果太小的话会导致梯度下降变得十分缓慢，而如果学习率过大的话，可能会导致越过最低点并且代价函数的值可能会越来越大。
>
> ![](images/Jietu20180921-110835.jpg)
> 
> * 梯度下降算法可能没法找到全局最低点。当我们从不同的两个点执行梯度下降算法的时候，可能会得到完全不同的两个最小值。因此我们得知道，梯度下降算法往往只能得到一个局部最小值。
> ![](images/Jietu20180921-111332.jpg)
> ![](images/Jietu20180921-111713.jpg)
> 
> ### 2.5 线性回归中的梯度下降（Gradient Descent For Linear Regression）
> 
> ![](images/Jietu20180921-112215.jpg)
> 
> 上图中给出了之前所说的线性回归模型和梯度下降模型。我们将两边式子合在一起，即可得到线性回归中的梯度下降模型。
> ![](images/Jietu20180921-112513.jpg)
> ![](images/Jietu20180921-112529.jpg)
> 
> 以下是迭代过程
> ![](images/Jietu20180921-114858.jpg)
> ![](images/Jietu20180921-114914.jpg)
> ![](images/Jietu20180921-114929.jpg)
> ![](images/Jietu20180921-114944.jpg)
> ![](images/Jietu20180921-115002.jpg)
> ![](images/Jietu20180921-115018.jpg)
> ![](images/Jietu20180921-115034.jpg)
> ![](images/Jietu20180921-115047.jpg)
> ![](images/Jietu20180921-115101.jpg)
> 
> ## 3. 多变量线性回归（Linear Regression With Multiple Variable）
> ### 3.1 多元梯度下降算法以及代价函数
> 我们先来回顾一下单变量的线性回归，以预测房价为例，单变量的线性回归相当于我们只通过房子的面积来预测房价，因此决定房价高低的只是房子的面积一个变量。如下所示：
> ![](images/Jietu20180921-131326.jpg)
> 但是生活中并不是这样的，往往有多个变量影响着房价，比如房间的个数、使用的年数等等。因此针对这种情况我们就要引入多变量的线性回归。
> ![](images/Jietu20180921-131342.jpg)
> ![](images/Jietu20180921-131821.jpg)
> 如上图中所示，当我们要增加变量时，$\theta$ 参数也要随之增加。因此 $h_\theta(x)$ 将改写成如下形式：
> ![](images/Jietu20180921-132021.jpg)
> 但是在处理多变量的线性回归之前，我们还得在数据集前加一列长度为 $m$ 的单位向量。接下来我们会详细介绍如何进行多变量的线性回归。
> ![](images/Jietu20180921-132255.jpg)
> 我们将 $\theta$ 与 $x$ 的线性组合简写成 $\theta^{T}X$ 的形式，$\theta$ 和 $X$ 均为向量，因此代价函数 $J(\theta_1,\theta_2,...,\theta_n)$ 便可以表示为 $J(\theta)$ 。其余的与单变量的代价函数以及梯度下降算法写法一致。
> ![](images/Jietu20180921-132912.jpg)
> 
> ### 3.2 特征缩放
> 在很多情况下，我们的特征值得范围往往并不是等比的，比如说房子的面积取值范围可以从0到2000，而房间的数量取值范围仅为 $\theta$ 到5，如果画出$J(\theta)$的图像可能会看起来非常的细长，如果在这种情况下运行梯度下降算法会导致迭代次数非常多并且下降的十分缓慢。解决办法就是需要使得两个特征取值的比例基本一致。因此我们要使用特征缩放来解决这个问题。
> ![](images/Jietu20180921-133416.jpg)
> 一般的，我们执行特征缩放之后使得特征变量的取值在-1到1这个区间附近为最佳。
> ![](images/Jietu20180921-210802.jpg)
> 特征缩放的方法是使用原特征值减去特征值取值的平均值再除以标准差。
> ![](images/Jietu20180921-211127.jpg)
> 
> ### 3.3 学习率
> 开始的时候我们讲了学习率会影响每一次执行梯度下降时下降的步长，并且学习率不能太大也不能太小。现在我们就来讲一讲如何选择一个合适的学习率。
> 首先我们先来看一看如何判断梯度下降算法运行得正确？
>> 1.每一次迭代，$J(\theta)$下降的值小于 $10^{-3}$。
>> ![](/Users/william/Documents/GitHub/Machine-Learning/images/Jietu20180921-211727.jpg)
>> 2.判断$J(\theta)$的图像来判断学习率 $\alpha$ 是否合适。
>> ![](images/Jietu20180921-211947.jpg)
>
> 我们可以试一试多个学习率的取值，如下，可以让后一个取值约为前一个取值的三倍。选择不同学习率中最优的那一个即可。

> ### 3.4 特征与多项式回归
> 首先我们来举一个例子：房价预测
![](images/Jietu20180921-214442.jpg)
![](images/Jietu20180921-214257.jpg)
> 当前我们给定两个特征分别为房子临街宽度以及纵向深度。同样我们也可以自己创造特征，比如我们用x表示房子的占地面积，即可得到 $h_\theta(x) = \theta_0+\theta_1x$。有时候通过定义一个新的特征可以得到一个更好的模型。与选择特征想法密切相关的是多项式回归。
> 同样拿房价来举例，比如你现在想要拟合一个数据，你可能不会选用一次函数，因为直线没法完全准确拟合数据，你可能会选择一个二次函数来拟合数据。但是你可能会发现二次函数在上升到最高点之后终究会下降，而房子并不可能随着面积的增大而贬值。因此可能会选用一个三次函数。如图上绿色的线。
> ![](images/Jietu20180921-221543.jpg)
> 接下来，我们通过以下方法，将多项式回归转变为多元线性回归方程
> 
> ![](images/Jietu20180921-223615.jpg)
> 需要注意的是多项式回归中特征缩放更加重要，因为式子中带有平方项、三次方项，会将特征值放得很大。
> 
> ![](images/Jietu20180921-224239.jpg)
> 
> 最后一个例子是当选择的是二次函数来拟合数据但是我们并不希望看到函数曲线到最高点之后开始下降，除了使用三次函数之外还可以将函数的二次项改成根号项如下图所示。这样函数曲线最后会逐渐趋于平缓并且不会继续下降。
> ![](images/Jietu20180922-230850.jpg)
> 
> ### 3.5 正规方程
> 之前我们一直在讲使用梯度下降的方法来确定最优的 $\theta$ 值，现在我们再来介绍一个新的方法，正规方程法。正规方程法不同于梯度下降法需要我们自己去确定学习率 $\alpha$ 和迭代执行，正规方程法只需通过一个公式即可直接算出最优的 $\theta$ 值。
> ![](images/Jietu20180922-233504.jpg)
> ![](images/Jietu20180922-233525.jpg)
> 当 $\theta$ 为一维的变量的时候，我们写出的 $ J(\theta) = a_{\theta}^2+b_{\theta}+c$ 中如果需要求出 $\theta$ 的最小值需要对 $\theta$ 求偏导并且赋值为零。当 $\theta$ 为一个向量的时候同样可以逐个对 $\theta_j$ 求偏导并赋值为零即可求出最优的 $\theta_j$ 。
> ![](images/Jietu20180922-234050.jpg)
> 我们举一个例子，还是预测房价的例子，我们先看$x_1$到 $y$ 这五列数据，他们是原先给定的数据，我们现在在 $x_1$ 前面加一列值等于1的 $x_0$ 向量，将$x_0$ ~ $x_4$合成一个矩阵$X$，$y$ 作为目标向量。此时我们想要求最优的 $\theta$ 只需把 $X$ 和 $y$ 代入红框里面的式子中即可。下面会具体说怎么使用这个式子。
> ![](images/Jietu20180922-234517.jpg)
> 当我们现在有 $m$ 个特征的问题，如上图所示，给出的 $x^{(i)}$即是特征矩阵，每一行都是它的特征向量。因此在构建特征矩阵 $X$ 的时候需要将每一个特征向量拿出来转置之后作为一列。矩阵 $X$ 也可以称为**设计矩阵**。接下来再将 $y$ 组成一个目标向量即可。
> ![](images/Jietu20180922-235153.jpg)
> 在Octave中执行计算 $\theta$ 的式子可以写成`pinv(X'*X)*X'*y`。需要注意的是在使用正规方程求 $\theta$ 的方法时并不需要对特征值进行特征缩放。<br>
> 
> 接下来我们来讨论一下梯度下降算法和正规方程的优缺点以及什么时候该使用梯度下降什么时候使用正规方程。
> ![](images/Jietu20180922-235516.jpg)
>> **梯度下降的缺点：**<br>
>> 1.需要选择学习率 $\alpha$。<br>
>> 2.需要进行多次迭代。<br>
>> **优点：**<br>
>> 当特征 $n$ 很大的时候仍然能运行的很好。
>> 
>> **正规方程的优点：**<br>
>> 1.不需要选择学习率 $\alpha$。<br>
>> 2.不需要迭代。<br>
>> **缺点：**
>> 需要计算$(X^TX)^{-1}$，当特征数量n很大的时候时间和空间代价较大。
> 
> 从上面可以看出来梯度下降算法和正规方程各有优缺点。因此一般当特征数量 $n$ 少于1000的时候两种方法都可以使用，当特征数量 $n$ 大于10000的时候选择梯度下降算法更好。
> 
> ![](images/Jietu20180923-000600.jpg)
> 最后说一下当$X^TX$为奇异矩阵也就是不可逆的时候，很可能是因为特征中存在冗余，特征个数大于等于数据个数，比如有一个特征是大小为XXX平方英尺，另一个是大小为XXX平方米，显然他们之间就存在冗余。因此只需删除其中一个，取出冗余一般就可以解决不可逆的问题。
> 
> 
> ## 4. Logistic 回归
> ### 4.1 分类
> ![](images/Jietu20180923-112137.jpg)
> **Logistic回归**应用最多的就是分类问题。分类问题在生活中随处可见，比如判断一封邮件是否是垃圾邮件、网上的事物是否是否存在欺骗性、肿瘤是良性还是恶性等等。其中分类问题的预测结果可以是0和1，其中0叫做负类，1叫做正类。就对应着比如预测肿瘤中的恶性肿瘤和良性肿瘤。0和1代表什么没有硬性的规定，也就是说可以任意设定。之后还会讨论多预测值的分类问题。
> 
> 我们如何来开发一个分类算法呢？例如我们使用肿瘤分类的例子来说，我们可以将肿瘤分类的数据集表示成下图这种形式。<br/>
> 
> ![](images/Jietu20180923-113426.jpg)
> 
> 如果我们使用我们之前学的线性回归的方式来拟合数据的话我们得到的将是这样的。
> ![](images/Jietu20180923-113802.jpg)
> 上图中我们使用了一条直线来拟合数据集，我们将$h_\theta(x)=0.5$作为一个分界，当$h_\theta(x)<0.5$的时候我们预测 $y=0$，当$h_\theta(x)\geq 0.5$的时候我们预测 $y=1$。这看起来还不错，似乎也能拟合数据集中的数据得到正确的结果。
> 
> 下面，我们尝试改变一下数据集，并表示出来如下图
> ![](images/Jietu20180923-114337.jpg)
> 如果我们现在再用线性回归来拟合这个数据集则对应着蓝色那条线，如果我们还是当$h_\theta(x)<0.5$的时候我们预测 $y=0$，当$x\geq 0.5$的时候我们预测 $y=1$的话，我们可以看到这样并不能很好的预测。因此在分类问题中使用线性回归也许有的时候能得到正确的数据，但是还是不建议在分类中使用线性回归的方法。
> 
> ![](images/Jietu20180923-114745.jpg)
> 从刚才的例子中看到，数据集的结果中只有0或1两种结果，但是使用线性回归的话结果会有大于1或小于0的情况，因此并不是特别适合。接下来我们将开发一个Logistic回归，它的$h_\theta(x)$取值介于0到1之间。
> 
> ![](images/Jietu20180923-115352.jpg)
> 之前我们简单说了Logistic回归，它的$h_\theta(x)$取值介于0到1之间，因此我们得找到一个函数，它的值域在0到1之间。我们只需在线性回归$h_\theta(x)$函数上稍做修改即可。如下图所示，通过修改之后的函数图像可以看到$g(z)$的值是介于0和1的。因此满足我们的需要。
> ![](images/Jietu20180923-115747.jpg)
> $h_\theta(x)$的作用可以看成通过输入的 $x$ 来预测 $y=1$ 的概率。例如我们举一个例子，给定一个$x$如上图所示，我们将$x_1$置为1，$x_2$是肿瘤的大小。如果我们算得 $h_\theta(x) = 0.7$，代表着有70%的概率 $y = 1$,也就是说有70%的概率肿瘤是恶性的。
> 
> ### 4.2 决策边界
> ![](images/Jietu20180923-120513.jpg)
> 我们规定，当 $h_\theta(x)\geq 0.5$的时候 $y=1$，当 $ $h_\theta(x)$ \leq 0.5$ 的时候 $y=0$
> ![](images/Jietu20180923-120725.jpg)
> 例如， $h_\theta(x)$ 表示成上图所示，若解出 $\theta$ 的值是 $[-3,1,1]$ ，则我们判定$-3+x_1+x_2\geq0$ 的区域中 $y=1$，之外的区域 $y=0$。因此我们将这一条线表示在数据集的图上便可以发现这条线将数据分为两部分，我们叫这一条线为决策边界。
> 
> ![](images/Jietu20180923-121307.jpg)
> 同样我们可以画出非线性的决策边界，和上面步骤类似在这里就不细讲了。
> 
> ### 4.3 代价函数(Cost Function)
> ![](images/Jietu20180923-121614.jpg)
> 接下来我们来讨论代价函数。Logistic回归的代价函数与线性回归的代价函数有一些区别，一是结果集中只有0和1两种结果。表达式如下图所示。现在我们要讨论的则是如何选择参数 $\theta$ 。
> ![](images/Jietu20180923-141421.jpg)
> 我们将 $J(\theta)$ 中求和项定义成 $Cost(h_\theta(x^{(i)}),y^{(i)})$ ,则代价函数 $J(\theta)$ 可以表示成 $\frac{1}{m}$ 乘以对代价Cost的求和。
> 
> 但是这个函数在线性回归中可以运行很好的运行，由于Logistic分类问题中，$h_\theta(x)$ 是一个非线性函数（非凸函数），我们将 $h_\theta(x)$ 带入式子之后画出函数图像可能是下图中左边的样子。如果在这样的函数中运行梯度下降的话很难找到全局最低点。因此我们得找到另一个线性的 $J(\theta)$ 函数然后画出来类似下图中右边的图像，这样就可以很好地运行梯度下降算法了。
> ![](images/Jietu20180923-142603.jpg)
> 
> 下面我们给出一个新的Cost函数，我们先看当 $y=1$ 的时候的图像，我们可以发现当 $h_\theta(x)= 1$ 的时候，Cost代价值刚好为零，这正是我们想要的，说明预测的准确性很高，预测的越准确代价越低，反过来，当$h_\theta(x)$ 趋向 $0$ 的时候Cost代价值趋于无穷。
> ![](images/Jietu20180923-142921.jpg)
> ![](images/Jietu20180923-143438.jpg)
> 当 $y=0$ 的时候和上面分析一样就不多阐述了。
> 
> ### 4.4 简化代价函数和梯度下降
> ![](images/Jietu20180923-143944.jpg)
> 现在我们将上面的两种情况合成一个表达式如上图所示。我们现在需要最小化代价函数 $J(\theta)$ 之后即可得到最优的 $\theta$ 值。这里梯度下降的方法和线性回归类似，只是 $h_\theta(x)$ 表达式变了一下，重复执行更新 $\theta$ 值即可。
> ![](images/Jietu20180923-144526.jpg)
> 
> ### 4.5 高级优化
> ![](images/Jietu20180923-150348.jpg)
> 除了梯度下降可以得到最优的 $\theta$ ，还可以使用其他的算法，上面列举了一些算法，他们相对于梯度下降法来说并不需要确定 $\alpha$ 而且比梯度下降算法速度更快。但是相应的也更加复杂。
> ![](images/Jietu20180923-155537.jpg)
> 在Octave上可以通过调用`fminunc`函数来实现优化。
> 
> ### 4.6 多类型的分类问题
> ![](images/Jietu20180923-160602.jpg)
> ![](images/Jietu20180923-160708.jpg)
> ![](images/Jietu20180923-170240.jpg)
> 多类型的分类问题目标值 $y$ 有多个取值。从上图中可以看出来。当我们要给它们画决策边界的时候我们需要将多个特征看成一个特征，将多特征的简化为两个特征的分类问题，再使用Logistic回归解决即可。
> 
> 
> ## 5. 正则化
> ### 5.1 欠拟合和过拟合问题
> 在之前我们已经讲了线性回归和逻辑回归，当特征集中特征数目过于多的时候，我们可能会看到数据拟合曲线太过于弯曲，而当特征集中的特征过于少的时候出现拟合并不是特别好的情况，我们先以线性回归为例，如下图。
> ![](images/Jietu20181007-203030.jpg)
> 上图中第一个例子我们只用了一个一次项来拟合这一个数据，可以看到这个拟合曲线并不能很好地拟合这个数据，我们称这种情况为欠拟合。第二个例子我们为曲线添加了二次项变成了一个弯曲的曲线，这个曲线很好的拟合了数据。而当式子中高阶项很多的时候，曲线会想方设法去拟合数据，造成第三幅图中那样弯弯折折的曲线，这种虽然拟合了所有的数据，但是并不是一个很好的现象，我们称其为过拟合。
> 
> 我们再来看看Logistic回归的情况，同样可以发现下图中从第一个盒第三个例子分别是欠拟合和过拟合。
> ![](images/Jietu20181007-203820.jpg)
> 
>要解决上面这种问题，我们可以使用以下方法：
>![](images/Jietu20181007-204122.jpg) 
>第一种方法，我们可以减少特征的数量来避免过拟合现象，比如说我们可以筛选一些不是那么重要的特征，这样的话我们就可以很有效的避免过拟合问题。然而，这种方法往往会造成训练出来的模型并不是最好的，因为当我们去了一些特征之后，少了这些特征的约束，这个模型也就会相应缺少一些性质，这当然是我们不希望看见的。因此我们介绍第二种方法，也就是正则化。
>
>正则化并不会使得特征数目减少，只是会使高阶的theta值尽可能小。这样每一个特征都会起到约束作用。
>
>### 5.2 代价函数
>![](images/Jietu20181007-205113.jpg)
>上图中，左边的曲线很好地拟合了数据，而右边则过拟合了。当我们观察两条曲线 $h_\theta(x)$ 式子我们可以发现，过拟合的曲线多了两个高阶项 $\theta_3x^3$ 和 $\theta_4x^4$ 。当将这两项去了之后就变成了左边曲线的样子了。也就是说我们只需要使 $\theta_3$、$\theta_4$尽可能的小即可。
>发现这个规律之后我们写出线性回归的代价函数，我们在代价函数之后加上两项分别是 $1000\times\theta_3x^2$ 和 $1000\times\theta_4x^2$ ,我们给他们加一个很大的数作为系数，这里我们将 $1000$ 作为系数。分析这个式子可以看出，要使得代价函数最小则需要 $\theta_3$ 和 $\theta_4$ 趋于零。执行玩这个步骤之后我们在画出曲线，可以看到曲线变得更加平滑了（如上图中粉红色曲线）
>
>由此我们便可以得到正则化的式子如下所示。
>![](images/Jietu20181007-210128.jpg)
>值得注意的是，当λ取值过于大的时候会导致欠拟合的情况，因为当λ过于大会使得 $\theta$ 均趋于零，最后使得$h_\theta(x) = \theta_0$ ，变成一条直线，如下图所示。
>![](images/Jietu20181007-210510.jpg)
>因此 $\lambda$ 不宜过大。
>
>### 5.3 线性回归中的正则化
>![](images/Jietu20181007-210858.jpg)
>同理我们可以将正则化推广到线性回归中，和上面一样只需在代价函数求和项末尾加入一项约束项即可，这里就不再详细概述。
>
>接下来我们执行梯度下降算法，如下图， $\theta$ 的下标从1开始，因此我们单独将 $\theta_0$ 提出来， $\theta_j$ 在原有的式子的基础上加上一个约束项。接下来只需要重复迭代这两个式子即可。
>
>![](images/Jietu20181008-161809.jpg)
>
>我们将两个式子整理一下可以得到一个综合的式子，我们将其看成两项之差，分析前一项中 $\theta_j$ 的系数我们可以发现，实质上是 $\theta_j$ 乘上一个比1稍微小一点的像0.99这样的数，后面那一项和之前一般的梯度下降式子一样。这样一看也就是将每一个 $\theta$ 值都减小。
>
>若采用一般方程来最小化代价函数，我们只需执行以下的式子即可。
>![](images/Jietu20181008-163142.jpg)
>很幸运的是如果采用正则化的一般方程来最小化代价函数将不会出现矩阵不可逆的情况。
>![](images/Jietu20181008-163723.jpg)
>
>### 5.4 Logistic回归中的正则化
>在Logistic回归中，正则化和上面类似，即在代价函数中添加约束项，如下图所示，值得注意的是约束项中 $\theta$ 脚标仍然是从1开始的。
>![](images/Jietu20181008-164553.jpg)
>在梯度下降的式子中，看起来和上面线性回归的式子差不多，但是我们要注意到，Logistic回归的 $g(x)$ 函数和线性回归的 $g(x)$ 函数是不一样的。因此两个式子看似相同但是在本质上是不同的。
>
>在使用更高级的算法来最小化代价函数时，我们需要在函数的实现中也加上正则化项。如下图所示：
>![](images/Jietu20181008-170047.jpg)
>
>## 6. 神经网络
>### 6.1 非线性假设
>之前我们说的线性回归和Logistic回归都是限于一些特征比较少的问题，对于一些特征比较多的问题，例如我们想要知道一间房子多长时间可以卖出，这个问题是一个分类问题，而特征值可以有很多个，房子的面积、卧室个数、层数、年龄等等，假设我们有100个特征，那么他们组成的 $\theta^{T}X$ 式子中将有数以万计的一次项以及高次项，这个时候如果我们在使用梯度下降来最小化代价函数将是一个运算量极大的问题，显然这是不可取的。因此我们引入了一种更强大的工具——神经网络，它能够应付特征数很大的机器学习问题。
>
>![](images/Jietu20181008-172029.jpg)
>
>首先我们从图像识别方面来举个例子，机器是如何识别一辆车的呢？其实一辆汽车在我们眼中和在计算机眼中并不是一样的，在计算机眼中，一辆车是一串数字信息，如下图。
>![](images/Jietu20181008-172215.jpg)
>
>我们会将一些训练集交给计算机，这些训练集的每一个训练样本都会有一个标签，其中有一些是汽车有些不是汽车，计算机通过分类来训练出一个模型，我们通过这个模型即可实现图像的识别。
>![](images/Jietu20181008-172435.jpg)
>大家都知道计算机是通过存储每一个像素点上颜色信息来存储一张图片的，因此，我们将每张图片提交给计算机之后，学习算法就会将每一个像素点作为特征分类，例如我们在每一张图片中都取出两个像素点，分别为 $pixel1$ 和 $pixel2$ ，通过分类我们可以在坐标系中画出一个分类图如下所示。
>![](images/Jietu20181008-172707.jpg)
>
>最后我们用一条平滑的曲线来约束边界就像下图中那样。
>![](images/Jietu20181008-173123.jpg)
>
>注意：如果是一张50x50的图像的话，就拥有2500个像素点，即特征，而且只是在图像为黑白的情况下，如果是RGB图像的话就会有7500个像素（特征）。可以看到这个特征数量非常的庞大。
>
>### 6.2 神经网络模型
>在人体中，神经细胞的构成如下图。
>![](images/Jietu20181008-201758.jpg)
>![](images/Jietu20181008-201812.jpg)
>
>我们对应着这个突触结构可以画出神经元模型。
>![](images/Jietu20181008-202057.jpg)
>上图中 $x_1$ , $x_2$ , $x_3$ 可以看做是上面的树突，中间的橘色圆圈可以对应成轴突， $h_\theta(x)$ 连接到其他神经元的树突。就像人体中的神经网一样，每一个小的神经细胞执行运算之后再将结果传送给其他神经细胞。上图中，我们添加一个 $x_0$ 作为偏置单元。我们用向量形式表示x为输入，每一个输入都对应着一个 $\theta$ ，我们称之为权重或者参数。这样一个单元就可以看成是一个Logistic回归单元，因此输出$h_\theta(x)=\frac{1}{1+e^{-\theta^{T}X}}$。
>
>下面我们来看看神经网络模型
>![](images/Jietu20181011-132254.jpg)
>
>上图中我们可以看到神经网络分成三层，我们称第一层为输入层，最后一层为输出层，中间的为隐藏层。隐藏层可以有多个，非输入输出层即为隐藏层。
>![](images/Jietu20181011-132958.jpg)
>* 隐藏层中有3个隐藏单元，我们称之为激活单元，用 $a^{(j)}_i$ 表示第j层中的第i个单元
>* $\Theta^{(j)}$ 是第 $j$ 层映射到第 $j+1$ 层的权重矩阵(参数矩阵)
>* 如果网络中第 $j$ 层有 $s_j$ 个单元, $j+1$ 层有 $s_{j+1}$个单元，那么 $\Theta^{(j)}$ 是一个 $s_{j+1} \times (s_j+1)$ 维的矩阵。
>
>![](images/Jietu20181011-140559.jpg)
>
>在上面给出的神经网络中，我们在每一层的开头都添加一个值为1的偏置单元，以第二层中 $a_1^{(2)}$ 激活单元的式子为例，我们令$$z_1^{(2)} = \Theta_{10}^{(1)}x_0 + \Theta_{11}^{(1)}x_1 + \Theta_{12}^{(1)}x_2 + \Theta_{13}^{(1)}x_3$$
>其中，$x$ 为 $[x_0,x_1,x_2，x_3]$ 向量，则同理可以得到向量 $$z^{(2)} = [z^{(2)}_1,z^{(2)}_2,z^{(2)}_3]$$。
>在这里我们也可以将 $x$ 看成 $a^{(1)}$，则有每一层的推导过程如下：
>> $z^{(2)} = \Theta^{(1)}a^{(1)}$
>> $a^{(2)} = g(z^{(2)})$
>> 添加一个偏置单元 $a^{(2)}_0 = 1$
>> $z^{(3)} = \Theta^{(2)}a^{(2)}$
>> $h_{\theta}(x) = a^{(3)} = g(z^{(3)})$
 
>![](images/Jietu20181011-142536.jpg)
>神经网络中每一层到下一层都是一个Logistic回归，如上图可以写出对应的 $h_{\Theta}(x)$ 的式子。$g_{\Theta}$则是`sigmoid`函数。
>
>刚才我们看的神经网络只有一个隐藏层，下面的神经网络则是由两个隐藏层，但是输入层和输出层都是不变的，只有一个。
>![](images/Jietu20181011-142912.jpg)
>
>### 6.3 神经网络例子直观理解
>我们现在有一个非线性的分类问题，我们想要通过神经网络实现异或和非异或功能。
>我们可以看到简化的分类模型如下图左边坐标系所示。
>![](images/Jietu20181011-143107.jpg)
>在实现这个复杂的功能之前，我们先来看一下简单的逻辑运算实现。
>![](images/Jietu20181011-143429.jpg)
>我们实现两项的逻辑与运算，使用sigmoid函数来实现结果约束在0和1之间，我们使用现有的θ值，由于只有一层，因此θ是一个向量。我们写出 $h_{\Theta}(x)$ 并通过真值表可以看到结果。
>同样的思路我们实现逻辑或运算和逻辑非运算如下：
>![](images/Jietu20181011-144032.jpg)
>![](images/Jietu20181011-144142.jpg)
>接下来我们通过组合就可以实现XOR和XNOR运算如下：
>![](images/Jietu20181011-144333.jpg)
>可以看到这个是一个三层的神经网络。
>
>### 6.4 多分类问题
>上面我们所说的神经网络输出层只有一个单元，而现实生活中输出层往往不止一个单元。因此我们引入多分类问题。
>![](images/Jietu20181011-144545.jpg)
>当我们要训练一个图像识别的模型的时候，我们假设有四类图像，分别是人、汽车、摩托车、卡车。很显然，输出的结果有四个，即输出层有4个单元。那我们如何来表示这四个结果呢？我们只需将识别出来的结果处置1，其他置0即可，如当识别出来是行人的时候输出:
>$$h_{\Theta}(x) \approx
\left[
\begin{matrix}
1\\
0\\
0\\
0\\
\end{matrix} \right]
$$

>## 7. 神经网络参数的反向传播算法
>>### 7.1 代价函数
>>之前我们讲了输出层只有一个输出单元的神经网络以及输出层有多个输出单元的神经网络。我们通过下面几张图来回忆一下。
>>![](images/Jietu20181014-153457.jpg)
>>我们给定一个拥有两个隐藏层的神经网络。当输出层只有一个输出单元的时候，我们称之为而二值分类问题，即输出值只有0和1两种情况，当输出层有多个输出单元我们称之为多分类问题，我们可以用类似二进制串来表示结果。
>>
>>接下来我们来回忆一下正则化的Logistic回归的代价函数，对于m个训练样本，有：
>>$$
>>J(\theta) = - \frac{1}{m}[\sum_{i=1}^{m}y^{(i)}\log{h_{\theta}(x^{(i)})}+(1-y^{(i)})\log{(1-h_{\theta}(x^{(i)}))}] + \frac{\lambda}{2m}\sum_{j=1}^{n}\theta_{j}^{2}
>>$$
>>
>>我们都知道，其实神经网络每一层中每一个激活单元对下一层中每个单元的映射实质上是一个Logistic回归问题。因此我们只需在Logistic回归代价函数基础上做一个求和即可得到神经网络的代价函数。我们规定$h_{\Theta}(x) \in R^{K}$并且 $(h_{\Theta}(x))_i$ 表示第 $i$ 个输出结果，则代价函数 $J(\Theta)$ 定义如下：
>>$$J(\theta) = - \frac{1}{m}[\sum_{i=1}^{m}\sum_{k=1}^{K}y^{(i)}_{k}\log({h_{\theta}(x^{(i)})})_k+(1-y^{(i)}_k)\log{(1-h_{\theta}(x^{(i)}))_k}] + \frac{\lambda}{2m}\sum_{l=1}^{L-1}\sum_{i=1}^{s_l}\sum_{j=1}^{s_{l+1}}(\Theta_{ji}^{(l)})^{2}$$
>>
>>
