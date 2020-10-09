[TOC]



# 第三章 k近邻法

k近邻法 (k nearest neighbor, KNN) 是一种基本**分类与回归**方法。本书只讨论分类问题中的近邻法。近邻法的输入为实例的特征向量，对应于特征空间的点；输出为实例的类别，**可以取多类**。 近邻法假设给定一个训练数据集， 其中的实例类别己定。分类时，对新的实例，根据其k个最近邻的训练实例的类别，通过多数表决
等方式进行预测。因此，k近邻法不具有显式的学习过程。 近邻法实际上利用训练数据集对特征向量空间进行划分，井作为其分类的“模型”。**k值的选择**、距离度量及**分类决策规则**是k近邻法的三个基本要素。k近邻法 1968年由Cover和Hart 提出。

kd tree，构造kd tree和搜索kd tree

## 3.1 k近邻算法

k近邻算法简单、直观：给定一个训练数据集，对新的输入实例，在训练数据集中找到与该实例最邻近的k个实例，这k个实例的多数属于某个类，就把该输入实例分为这个类。下面先叙述k近邻算法，然后再讨论其细节。

![算法3.1（k近邻树）](https://raw.githubusercontent.com/DRosemei/statistical_learing_methods/master/imgs/%E7%AE%97%E6%B3%953.1.PNG)

**k近邻法没有显式的学习过程**



## 3.2 k近邻模型

k近邻法使用的模型实际上对应于对特征空间的划分。模型由三个基本要素 一一**距离度量**、**k值**的选择和**分类决策规则**决定。



### 3.2.1模型

k近邻法中，当训练集、距离度量(如欧氏距离)、k值及分类决策规则(如多数表决)确定后，对于任何 个新的输入实例，**它所属的类唯一地确定**。 

单元划分

![图3.1 k近邻法的模型对应特征空间的一个划分](https://raw.githubusercontent.com/DRosemei/statistical_learing_methods/master/imgs/%E5%9B%BE3.1.PNG)



### 3.2.2 距离度量

$L_p$距离或者Minkowski距离（闵氏距离）

设特征空间$\chi$是n维实数向量空间$\mathbb{R}^n$，$x_i,x_j \in \chi, x_i=(x_i^{(1)},x_i^{(2)},...,x_i^{(n)})^T$，$x_i,x_j$的$L_p$距离定义为：
$$
L_p(x_i,x_j)=(\sum_{l=1}^{n}|x_i^{(l)}-x_j^{(l)}|^p)^{\frac{1}{p}}
$$
p>=1，如果p=2，称为欧式距离；如果p=1，称为曼哈顿距离；p=∞，$L_\infty(x_i,x_j)=max_l |x_i^{(l)}-x_j^{(l)}|$



### 3.2.3 k值的选择

k值的减小就意味着整体模型变得复杂，容易发生过拟合。k值的增大就意味着整体的模型变得简单。



### 3.2.4 分类决策规则

往往是**多数表决**，如果损失函数为0-1损失函数，误分类率：
$$
\frac{1}{k} \sum _{x_i\in N_k(x)} {I(y_i \neq c_j)}=1-\frac{1}{k} \sum _{x_i\in N_k(x)} {I(y_i = c_j)}
$$
误分类率最小，就要使$\frac{1}{k} \sum _{x_i\in N_k(x)} {I(y_i = c_j)}$最大，所以**多数表决规则等价于经验风险最小化**。



## 3.3 k近邻法的实现：kd树

实现k近邻法时，主要考虑的问题是如何对训练数据进行快速k近邻搜索。

最简单的办法是**线性扫描**（linear scan）。



### 3.3.1 构造kd树

kd树是一种对k维空间中的实例点进行存储以便对其进行快速检索的树形数据结构。**kd 树是二叉树**，表示对k维空间的一个划分（partition） 。构造kd树相当于不断地用垂直于坐标轴的超平面将k维空间切分，构成一系列的k维超矩形区域。 **kd树的每个结点对应于一个k维超矩形区域**。

**注：kd树的k和k近邻的k不是一回事**

![算法3.2 构造平衡kd树](https://raw.githubusercontent.com/DRosemei/statistical_learing_methods/master/imgs/%E7%AE%97%E6%B3%953.2.PNG)

![例3.2](https://raw.githubusercontent.com/DRosemei/statistical_learing_methods/master/imgs/%E4%BE%8B3.2.PNG)



### 3.3.2 搜索二叉树

给定一个目标点，搜索其最近邻。**首先找到包含目标点的叶结点**；然后从该叶结点出发 ，依次回退到父结点；不断查找与目标点最邻近的结点，当确定不可能存在更近的结点时终止，这样搜索就被限制在空间的局部区域上，效率大为提高。

![算法3.3 用kd树的最近邻搜索](https://raw.githubusercontent.com/DRosemei/statistical_learing_methods/master/imgs/%E7%AE%97%E6%B3%953.3.PNG)

![例3.3](https://raw.githubusercontent.com/DRosemei/statistical_learing_methods/master/imgs/%E4%BE%8B3.3.PNG)

