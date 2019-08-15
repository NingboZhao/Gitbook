# 线性回归---回归

线性回归 linear regression属于线性模型的一种，广义线性模型主要包括：

![image.png](resources/886A2DC68128C5CF337AA2781342AA38.png "image.png")

## [](https://www.yuque.com/u170062/yw5ver/bzug2w#d1eeb82d)1.回归问题

回归模型是表示输入变量到输出变量之间映射的函数， 回归问题的学习等价于函数拟合： 使用一条函数曲线

使其很好的拟合已知函数且很好的预测未知数据。

## [](https://www.yuque.com/u170062/yw5ver/bzug2w#ab153672)2.线性回归公式

![image.png](resources/24E22BFA4265B507DC8B0C39513A28F5.png "image.png")

将截距单独拎出来。

---

![image.png](resources/39D019194789F8450A28AFAE2A4952E7.png "image.png")

将截距算进向量相乘的结果中。x 表示m行（n+1）列的矩阵，m代表样本数量，n表示数据集中的维度，1表示增加的一列（数值都为1）。

## [](https://www.yuque.com/u170062/yw5ver/bzug2w#3f31df21)3.最小化损失函数

一元线性回归中，对于每一个样本，ε 表示真实值和预测值之间的差异：

![image.png](resources/445996896BA4133F0D2F646671BDA94B.png "image.png") ![image.png](resources/06A07F2738C9C0096CB0B53783F6A181.png "image.png") 

一元线性回归的损失函数loss function （MSE: mean square error）(最小二乘法公式)：

![image.png](resources/7079972819B59800F90C95A3A94DACD4.png "image.png")

**MSE 只与正态分布和似然估计有关，换做回归以外其他机器学习算法，损失函数也可以是MSE。**

【目标】使ε差异在总体上尽可能的小，求出误差最小时的w、b值。

【问题】**为什么损失函数是计算平方和**？

#### [](https://www.yuque.com/u170062/yw5ver/bzug2w#cf613a73)3.1 正态分布normally distributed

每个样本的实际值 = 预测值 + 误差。

假设所有样本都是独立同分布的，**假设数据服从****正态分布**，实际值在预测曲线上下震荡，根据中心极限定理，**误差值服从****正态分布**（也服从高斯分布）。误差的均值是0，所以不能用误差的简单求和来评估损失的大小。

同分布：

做特征工程前后，数据的分布情况是一致的，例如之前是正态分布，特征工程之后也是正态分布。深度学习中，每一层的数据的分布也是一致的。

#### [](https://www.yuque.com/u170062/yw5ver/bzug2w#ad3c3ce4)3.2 概率密度函数

正态分布的概率密度函数如下：

![image.png](resources/0D01643539D705F63527F844AF1DA05E.png "image.png")

（在数学中， 连续型随机变量的概率密度函数是一个描述这个随机变量的输出值， 在某个确定的取值点附近的可能性的函数。 而随机变量的取值落在某个区域之内的概率则为概率密度函数在这个区域上的积分。）

由于误差ε符合正态分布，将其代入概率密度函数：

![image.png](resources/460DC911DCE39414818550F48EF70837.png "image.png")

#### [](https://www.yuque.com/u170062/yw5ver/bzug2w#39594b57)3.3 似然函数

最大似然估计（找出一组参数，使模型产生出观测数据的概率最大）思想下的似然函数是关于统计模型参数的函数。

关于参数θ的似然函数L(θ|x)等于给定参数θ后变量X的概率： L(θ|x)=P(X=x|θ)。

![image.png](resources/EA5B5E2881BAE9D04479CD311583A4C2.png "image.png")

由于误差之间是相互独立的，整体样本的误差输出取值的可能性如下（**即目标函数**）：

![image.png](resources/766EB7D58481EF772A4A78BDAFC78FB8.png "image.png")

#### [](https://www.yuque.com/u170062/yw5ver/bzug2w#a4001699)3.4 自然对数

为了便于将上述公式展开化解，对两边分别取以e为底的自然对数：
![image.png](resources/17B5BAA36C7EDBE50D067C541494B5A0.png "image.png")

（由于![image.png](resources/46CD3CE829A6858BF04A536208115F18.png "image.png")）

![image.png](resources/4D08035E417EEF7B203B6FA4B474667D.png "image.png")让似然函数代表的概率越大越好，就代表误差取值在正态分布中更加集中；所以就要使 J(w)越小越好，即需要求解 J(w) 最小时的 w值 和 b值。

![image.png](resources/7FAEA78048FF322B2964697E53493F05.png "image.png") ![image.png](resources/B7CCD465755F53BB1CC9AC1BE6DA1A84.png "image.png")

#### [](https://www.yuque.com/u170062/yw5ver/bzug2w#rXMFK)3.5.1 偏导与解析解(正规方程)

**一元线性回归：**

将w，b看为未知变量，x，y看为常数。多个变量情况下求J(w)极值，所以要对 w、b求偏导。

复合函数的求导：

![image.png](resources/C5EC67ABA6184E319DEEE9467E66E295.png "image.png")

![image.png](resources/DC128DE65896C161519AC993D897E36F.png "image.png")

![image.png](resources/5C3961F83D8EA4E0F9CB53E3D6C8D39D.png "image.png")

**多元线性回归**：

theta值有多个，不可能逐个对theta求偏导，所以使用矩阵：m行 \* (n+1)列。

将误差函数转化为有确定解的代数方程组，从而可求解出这些未知参数，而不用迭代的方法，这就是**正规方程(normal equations)法**。得到的参数的解就是**解析解**。

![image.png](resources/6A62775C0543FBD5DCE7621566C8079C.png "image.png")

![image.png](resources/BF3B51D69C2658A28124155FD35AC527.png "image.png")

![image.png](resources/0C91FEFA33F73D9E5B713452E2C64761.png "image.png")

#### [](https://www.yuque.com/u170062/yw5ver/bzug2w#Terte)3.5.2 梯度下降

#### [](https://www.yuque.com/u170062/yw5ver/bzug2w#3D4b9)为什么用梯度？

![](resources/D6815B3446C8BDB80D29F9EC0EA606B6.png)

对于n维矩阵，以上的解析解的复杂度是O(n)的三次方，所以我们使用梯度下降（gradient descent）的方法来求解局部最优解；因为线性回归损失函数是一个凸函数，此时使用梯度下降求解得到的其实是全局最小值。

#### [](https://www.yuque.com/u170062/yw5ver/bzug2w#gA5sy)凸函数的判定

![image.png](resources/18E349D82DEDD2A13830A43AC26F2BB1.png "image.png")

![image.png](resources/7F3F6C9AFEF1CCB1F4A19E3724F29E6C.png "image.png")![](resources/C2FA1F93398C00A627108F7F6A671060.png)

#### [](https://www.yuque.com/u170062/yw5ver/bzug2w#39SDF)什么是梯度？

对点x0的导数反映了函数在点x0处的**瞬时变化速率**。推广到多维函数中，就有了梯度的概念，梯度是一个**向量组合**，向量是有方向的，反映了多维图形中变化速率最快的方向（下坡最快）。

在微积分里面，对多元函数的参数求∂偏导数，把求得的**各个参数的偏导数以向量的形式写出来**，就是梯度。

比如函数f(x,y), 分别对x,y求偏导数，求得的梯度向量就是(∂f/∂x, ∂f/∂y)T,简称grad f(x,y)或者▽f(x,y)。

对于在点(x0,y0)的具体梯度向量就是(∂f/∂x0, ∂f/∂y0)T.或者▽f(x0,y0)。

#### [](https://www.yuque.com/u170062/yw5ver/bzug2w#ZI2Ba)什么是梯度下降？

我们把要最小化或最大化的函数称为 目标函数（objective function）或 准则（criterion）。当我们对其进行最小化时，我们也把它称为 代价函数（cost function）、损失函数（loss function）或 误差函数（error function）。

通常使用一个上标 ∗ 表示最小化或最大化函数的 x 值。如我们记 **x****∗ ****=****arg min ****f****(****x****)**。 

导数 f ′(x) 代表 f(x) 在点 x 处的斜率。换句话说，它表明如何缩放输入的小变化才能在输出获得相应的变化： f(x + ϵ) ≈ f(x) + ϵf ′(x)。 

导数对于最小化一个函数很有用，因为它告诉我们如何更改 x 来略微地改善 y。例如，我们知道对于足够小的 ϵ 来说， f(x - ϵsign(f ′(x))) 是比 f(x) 小的，我们可以将 x 往导数的反方向移动一小步来减小 f(x)。这种技术被称为 梯度下降（gradient descent）。

![image.png](resources/3547D372528991AA20538C044DE40794.png "image.png")

#### [](https://www.yuque.com/u170062/yw5ver/bzug2w#BHeXb)驻点

f ′(x) = 0 的点称为 **临界点****（****critical point****）**或** ****驻点****（****stationary point****）**。

有些临界点既不是最小点也不是最大点，这些点被称为 **鞍点（saddle point）**。

![image.png](resources/E1B2DD0395E1EAC10A475C0C2AD8E088.png "image.png")

使 f(x) 取得绝对的最小值（相对所有其他值）的点是 全局最小点（global minimum）。

函数可能只有一个全局最小点或存在多个全局最小点，还可能存在不是全局最优的局部极小点（local minimum） 。

#### [](https://www.yuque.com/u170062/yw5ver/bzug2w#SSZct)梯度的意义？

从几何意义上讲，就是函数变化增加最快的地方。

具体来说，对于函数f(x,y),在点(x0,y0)，沿着梯度向量的方向就是(∂f/∂x0, ∂f/∂y0)T的方向是f(x,y)增加最快的地方；或者说，更加容易找到函数的最大值。

反过来说，沿着梯度向量相反的方向，也就是 -(∂f/∂x0, ∂f/∂y0)T的方向，梯度减少最快，也就是更加容易找到函数的最小值。

![image.png](resources/D31907827A6F5A00D995F39B433213E2.png "image.png")

#### [](https://www.yuque.com/u170062/yw5ver/bzug2w#Gxkhc)梯度思想的三要素

梯度法思想的三要素：**出发点、下降方向、下降步长**。

alpha表示**学习率（learning rate）**，表示梯度下降的步长。例如：

![image.png](resources/3CCE67A593977CE28207D798589DF098.png "image.png")

#### [](https://www.yuque.com/u170062/yw5ver/bzug2w#l21cH)求解梯度

![image.png](resources/ECFA76B11EDED3D947952049C6CF7FDF.png "image.png")

![image.png](resources/1BBC2BDD302A5E477DA3DEF394F89F45.png "image.png")

## [](https://www.yuque.com/u170062/yw5ver/bzug2w#8DEEh)4.批量梯度下降BGD

![image.png](resources/C762E665436DF4A5BF06AA91B254290D.png "image.png")

每一次的参数更新都用到了所有的训练数据， 所以BGD算法会非常耗时。

## [](https://www.yuque.com/u170062/yw5ver/bzug2w#rkyZm)5.随机梯度下降SGD

Stochastic Gradient Descent

![image.png](resources/57ACFE73CD2ABFC6B39202AD8DA0DA63.png "image.png")

SDG求出的解是可用的解，即符合定义的误差阈值内的解，但这个解可能不够精准。

SGD伴随的一个问题是噪音较BGD要多， 使得SGD并不是每次迭代都向着整体最优化方向。

## [](https://www.yuque.com/u170062/yw5ver/bzug2w#BiN44)6.小批量梯度下降MBGD

Mini-Batch Gradient Descent

解决BGD和SGD的缺点， 使得算法的训练过程比较快， 而且也要保证最终参数训练的准确率。

不同问题设置的batch是不一样的。