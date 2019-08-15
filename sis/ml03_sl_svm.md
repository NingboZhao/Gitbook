# SVM---分类

## [](https://www.yuque.com/u170062/yw5ver/uh7hlz#jc6RQ)1.函数最优化问题

### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#xqfyA)1.1 无约束条件的最优化问题

![image.png](resources/4EC330B5FB5DEC3E534EED9694C25937.png "image.png")

### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#k3xpf)1.2 有约束条件的最优化问题

以下约束条件中没有考虑 \>0 的情况，因为可以由小于等于0反推出来。

![image.png](resources/F8D176545C9EF9E97657DBC38296BBF5.png "image.png")

将以上最优化问题命名为**原始（最优化）问题**。

**凸优化问题**：对于上述有约束条件的最优化问题，目标函数 f(x) 和约束函数 ![](resources/86DFBC02B1D731875DB368E58661481D.png)都是R上连续可微的凸函数，![](resources/B6DEF819A704A94F0B6ED2F1A399D469.png)是R上的仿射函数（满足![image.png](resources/9E6F8A0FFE1DA249775B51A3CD3CD7DF.png "image.png")）

### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#8ac6l)1.3 求解最优化问题

方法：梯度下降、L-BFGS、IIS等

### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#zbg3y)1.4 拉格朗日函数

拉格朗日函数是将原始问题的f(x)和约束条件进行**整合**，

使有约束条件的最优化问题\>\>\>\>\>转为\>\>\>\>\>无约束条件的最优化问题。

使原始问题的一次优化问题\>\>\>\>\>转为\>\>\>\>\>极小极大的二次优化问题。

在约束最优化问题中，常利用**拉格朗日对偶性**（Lagrange duality）将原始问题转换为对偶问题，通过求解对偶问题得到原始问题的解。

![image.png](resources/0829D07BEC10ACB0EF594A8E4C0D3E5B.png "image.png")

拉格朗日乘子（Lagrange multiplier）:![image.png](resources/8F329932D3CBD7FA3D1CA3D0C834B937.png "image.png")

拉格朗日乘子向量：![image.png](resources/5327F023605786390515F5CD9A49E73D.png "image.png")

#### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#kmHCI)拉格朗日函数的特性

![image.png](resources/4CD672A1EFAA2351ED1103D583917D36.png "image.png")

#### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#sTUvm)极小极大

由拉格朗日函数的特性可知，当x满足原始问题约束时，![image.png](resources/8CE2B4551FE2EEB8192B16F194109E4F.png "image.png") 就是原始问题的**f(x)**，此时进行极小化就等同于对原始（最优化）问题进行极小化，解是相同的，即：

![image.png](resources/C381065885A3A90110D8FF7A078B86E1.png "image.png")

#### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#jZjVn)对偶问题

![image.png](resources/3FE6DFB64A5A723CB54C0B0B2BE2326D.png "image.png")被称为拉格朗日函数的极大极小问题，也被称为原始问题的对偶问题。

定义![image.png](resources/72EEF1B82EE499D54949C133F4EA72C4.png "image.png") 为对偶最优化问题的最优解，当** f(x)**和![image.png](resources/86DFBC02B1D731875DB368E58661481D.png "image.png")为凸函数，![image.png](resources/B6DEF819A704A94F0B6ED2F1A399D469.png "image.png")是仿射函数时，有：

![image.png](resources/411256579850FD57318653598851045B.png "image.png")，其中的约束条件为**KKT条件**：

![image.png](resources/215D97CE1ABE7239DA2688768C97CD82.png "image.png")

## [](https://www.yuque.com/u170062/yw5ver/uh7hlz#KjYqg)2.Support Vector Machine

SVM：线性的分类（二分类）算法。

**基本思想**：求解能够正确划分训练数据集，且几何间隔最大的分离超平面。间隔最大化，意味着以充分大的确信度对训练数据进行分类。

支持向量机(Support Vector Machine)是Cortes和Vapnik于1995年首先提出的，它在解决**小样本、非线性及高维模式**识别中表现出许多特有的优势。

### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#CDvec)2.1 线性可分SVM

#### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#9ppI7)线性可分性

给定一个数据集![](resources/B70EA4B6DA93001F6444FBD4CC7A933D.png)，如果存在某个超平面![](resources/44186B32909DCD14A9A305105981E0AB.png)，满足：

* 将数据集的正实例点和负实例点完全正确的划分到超平面的两侧；
* 对所有![image.png](resources/6271FDF88F86C1827FF7DAFF3586E319.png "image.png")的实例，都有![image.png](resources/9E419A4742D099940E0199446023AE45.png "image.png")；对所有![image.png](resources/273873976680167D3E823BD5A19676CD.png "image.png")的实例，都有![image.png](resources/D90DE4C2DAF9294A1A1A787D4FB5706E.png "image.png")。

则称以上数据集是线性可分（linearly separable）的。

#### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#Uc0Cd)模型判别式：

![image.png](resources/87C2DA35CCA63FDEC2EC4A44F64FCD15.png "image.png")，是线性的公式，本质是在**高维空间**找到一个**超平面**来分隔数据。

通常这样用于分隔的超平面有很多，哪一个最好？

#### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#kgVXG)最好的超平面：

* 完美的分类正负例
* **距离**最近的点越远越好（对点位置变动的容忍度大），即**硬间隔**越大越好。（最大间隔法）
* （根据以上两点确定的超平面，求出y=0时的w值和b值。）

![image.png](resources/AB744CA619D87A6B10CEE1F8B47900B9.png "image.png")

#### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#4pfFB)几何距离和函数距离

![image.png](resources/88BCEECFC1870CB34819EB21ACB71EED.png "image.png")

函数距离（functional margin）可以表示分类的**正确性**和分类预测的**确信度**。

#### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#z1Rm3)最优化问题

* 根据距离公式，可以将最好的超平面的定义转换为函数的最优化问题：

![image.png](resources/8914E8A74B3CB0E6227E3D3C7345D672.png "image.png")

* 如果将 w 和 b 等比例缩放为 λw 和 λb，函数间隔变成 λγ'，函数间隔的改变对不等式约束没有影响，对目标函数的优化也没有影响。于是，为了方便求解，直接令 γ' = 1，改变上述最优化问题，得到：

![image.png](resources/2C956913538056401FE3F159299CC7CF.png "image.png")

* 进一步根据上述最优化问题，构建拉格朗日函数：

![image.png](resources/BC3751362E08EBBD6EB0C171962F7D3C.png "image.png")

* 根据对偶函数，先求极小值![image.png](resources/6513C4889376634273A88D589EDAAAD2.png "image.png")，拉格朗日函数分别对 w 和 b 求导，求出 w 和 b 各自与 α 的关系。

![image.png](resources/B141B1B404172B9500B1C4DBE9F28182.png "image.png")

根据以上推导，求导得出：

![image.png](resources/5DEFA30A8940C27AEF07A1A800DFE8F8.png "image.png")

* 将求导结果代入原来的函数：

![image.png](resources/00492160C54CF3983E545C649B154A8B.png "image.png")

* 进一步转换对偶问题的优化问题为：

![image.png](resources/97C7EE6D768878E84A175ED14876610F.png "image.png")

![image.png](resources/71C60D4A2535A37444AA66D3003BAF65.png "image.png")

* 通常使用SMO算法（序列最小最优化算法）进行求解，可以求得一组 α\*使得函数最优化。求解之后，α\*成为已知数，根据KKT条件，再求出 w\*，b\* .

![image.png](resources/DFF250322CF5F5182DE1DCA26ACCD268.png "image.png")

#### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#0kxjX)支持向量

由上述根据 α\* 推导 w\*，b\* 的过程可知，![image.png](resources/4D8C7259A8F57B05B255D1E60A559414.png "image.png")时，![image.png](resources/D90B86C53797477A36B0D307D0920A61.png "image.png")，此时对应的样本点 ![image.png](resources/C96334CFC90C2A09EF1F5A7ED51B75CF.png "image.png")的实例![image.png](resources/C6773CDDFB5C81809C4870CE28821E71.png "image.png")就是支持向量，支持向量一定是在间隔边界上。

### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#WEkoz)2.2线性SVM

#### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#ZcX58)线性不可分

为了得到最好的分隔超平面，满足的条件有：

* （满足约束条件时）可以把正负例完美分开
* 找到间隔最大的点（函数优化问题）

对于线性不可分问题，会有噪声点，不满足线性可分SVM中的约束条件，为了放松约束条件，而引入松弛变量![image.png](resources/88C0CD01BA2211BA00ADF2B108600A87.png "image.png")。

ξ代表异常点嵌入间隔面的深度,我们要在能选出符合约束条件的最好的w和b的同时,让嵌入间隔面的总深度越小越好。

#### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#Iei6j)最优化问题

* 线性SVM的原始问题（凸二次规划问题）如下：

![image.png](resources/EB54ED463CA5744946E62C1CEBFAAE25.png "image.png")

* 分类决策函数

求出原始问题的最优解，代入![](resources/87C2DA35CCA63FDEC2EC4A44F64FCD15.png)，即得到分类决策函数。

分离超平面为![image.png](resources/BC5384A82A0AAF7D473B35322F10B318.png "image.png")

* 构建拉格朗日函数

![image.png](resources/3AF9C3F4BA4A569012F49859CE488828.png "image.png")

* 求解![image.png](resources/682025A658E16E627B9EFB2870D45822.png "image.png")

![image.png](resources/F8B126065783D925BE9F2ECD18C1C224.png "image.png")

![image.png](resources/71CB02A69C3A318C93EFB98AF353F195.png "image.png")

* 用SMO算法求出 α\*.

![image.png](resources/1D98A34B0C6C914D03CD68228408DF9E.png "image.png")

#### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#F7ceP)支持向量

![image.png](resources/C9460F5E63CDE55E3C41A12A127AB139.png "image.png")

SVM模型判别式的另一种表示：

![image.png](resources/37791573D69F9F5AD1805EAF8175C006.png "image.png")

**结论：每一次计算判别函数的结果时，需要求得判别点和所有训练集样本点的内积。**

**
**

### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#YNB0o)2.3非线性SVM

#### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#yCqY7)SVM升维

为了处理线性不可分问题，可以引入升维，即把原始的 x 映射到更高维度的空间：

![image.png](resources/116583DD57C1325753B078579DE5AF3D.png "image.png")

【问题】升维之后，再做向量的内积，会出现**维度爆炸**。

【解决】引入核函数。思路：不具体计算向量的内积，而是直接定义![image.png](resources/84693F1D4EBC871F8B6C21DE767C4626.png "image.png")的结果。

#### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#MR2eL)核函数

![image.png](resources/7620D63207704F09576B0A3D25F696A2.png "image.png")

### [](https://www.yuque.com/u170062/yw5ver/uh7hlz#kMriS)2.4 SVM算法流程总结

* 选择核函数及对应的超参数；
* 选择惩罚系数C；
* 构造最优化问题；
* 利用SMO算法求出一组 α\* ；
* 根据 α\*计算w\*
* 根据α\*找到全部的支持向量，计算每个支持向量对应的![image.png](resources/BE89F7F1AB1B7526234DF43B2D98809D.png "image.png")
* 对![image.png](resources/BE89F7F1AB1B7526234DF43B2D98809D.png "image.png")求均值，得到 b\*
* 得到判别函数和超平面。

【参考】

《统计学习方法》第二版 李航著；

<https://blog.csdn.net/u014540876/article/details/80172623>