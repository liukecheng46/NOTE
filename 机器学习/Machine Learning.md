# Machine Learning Note

生成模型generative model：Model how the data is generated using probability distributions。适合少量数据

prior distribution： p(y)

Posterior p(y|x)

class conditional distribution CCD: p(x|y)

极大似然估计和极大后验概率(MAP)

极大似然估计(MLE) 是已知假设的分布模型，用MLE去求分布模型的参数，然后再结合prior可以求出Posterior。即先求出CCD和prior，再算出Posterior,选择Posterior最大作为类别

MLE is not alwasys has a closed form solution.

Maximum Likelihood Estimation: maximizes the likelihood of the observed data.

![Untitled](Machine%20Le%206bb85/Untitled.png)

**离散分布**：

1）二项概率分布（伯努利分布）：两种可能性每次相互独立，且每次概率相同

![Untitled](Machine%20Le%206bb85/Untitled%201.png)

多项式分布是伯努利分布的扩展，二项变成N项

2）泊松分布：根据平均期望次数 求得各个次数的概率，也要求相互独立且每次概率相同

![Untitled](Machine%20Le%206bb85/Untitled%202.png)

**连续分布：**

1)高斯分布（正态分布）:

均值和标准差

![Untitled](Machine%20Le%206bb85/Untitled%203.png)

Bayesian Decision Rule: by assusming the distribution and using MLE，we can ge p(x|y), p(y) is easy to observe, so we can then get p(y|x) by bayes rule, 最大的 p(y|x) 的y即为预测类

朴素贝叶斯的朴素是指假设多个特征相互独立，这样就可以拓展到多个x：

![Untitled](Machine%20Le%206bb85/Untitled%204.png)

多元高斯分布贝叶斯（Multivariate Gaussian）

（现实中的各个特征维度不是完全独立的，因此使用协方差矩阵covariance matrix考虑维度之间的关联，提高分类正确率）

文本处理 
1）词袋模型 ：不考虑顺序

2）TF-IDF模型： TF ：某词出现次数/文档单词总数   

                       IDF：所有文档/包含该词的文档。都包含时为0，包含该词文档越少，IDF越大

![Untitled](Machine%20Le%206bb85/Untitled%205.png)

3）word2vec (调库)

拉普拉斯平滑：如果一个词在某个文档没有出现，那计算概率时该文档出现的几率就会是0，但这样是不合理的，等同于过拟合很严重。所以需要加1来防止过拟合。下图中的2是因为二项分布

![Untitled](Machine%20Le%206bb85/Untitled%206.png)

Discriminative model判别模型：learn p(y|x) directly

线性分类：0为分界线 

线性回归：最小二乘法求最小残差和，

逻辑回归：sigmoid函数映射到0到1之间

![Untitled](Machine%20Le%206bb85/Untitled%207.png)

用MLE和data去调整参数w

逻辑回归推导：

![Untitled](Machine%20Le%206bb85/Untitled%208.png)

![Untitled](Machine%20Le%206bb85/Untitled%209.png)

逻辑回归的正则化：给w加一个先验概率分布

![Untitled](Machine%20Le%206bb85/Untitled%2010.png)

假设这个分布是高斯分布：

![Untitled](Machine%20Le%206bb85/Untitled%2011.png)

得出：

![Untitled](Machine%20Le%206bb85/Untitled%2012.png)

最后用梯度下降求optimization

多元逻辑回归：

1）ova 一对多 选择概率最高的 种类多的时候可能不平衡

2）ovo 一对一 选择票数最多的 

稳定 但分类器较多，资源占用多

3）使用softmax函数

![Untitled](Machine%20Le%206bb85/Untitled%2013.png)

模型损失函数的比较和选取：[https://zhuanlan.zhihu.com/p/35709485](https://zhuanlan.zhihu.com/p/35709485)

**MLE equivalent to cross-entropy loss 交叉熵公式等价于MLE**

为什么逻辑回归不用MSE作为损失函数：sigmoid开始训练时速率非常慢，而交叉熵会使用反馈所以在模型效果差的时候学习速度比较快，在模型效果好的时候学习速度变慢。

逻辑回归本身其实是线性分类

### svm：

查找最优分类平面：boundry两侧所有点中最近的点离boundry距离最大（maxmize the margin）

为什么 maxmize the margin：

![Untitled](Machine%20Le%206bb85/Untitled%2014.png)

![Untitled](Machine%20Le%206bb85/Untitled%2015.png)

因为margin distance和w的长度无关，只和方向有关，因此可以对上式进行normalization,将分子归一化为1 来方便计算：

![Untitled](Machine%20Le%206bb85/Untitled%2016.png)

goal：maxmize the margin

![Untitled](Machine%20Le%206bb85/Untitled%2017.png)

![Untitled](Machine%20Le%206bb85/Untitled%2018.png)

所以最终：

![Untitled](Machine%20Le%206bb85/Untitled%2019.png)

用f(x)的正负来进行预测

![Untitled](Machine%20Le%206bb85/Untitled%2020.png)

![Untitled](Machine%20Le%206bb85/Untitled%2021.png)

处理非线性数据：允许某些点在margin中

![Untitled](Machine%20Le%206bb85/Untitled%2022.png)

![Untitled](Machine%20Le%206bb85/Untitled%2023.png)

svm的hinge损失函数：

![Untitled](Machine%20Le%206bb85/Untitled%2024.png)

核函数：

为解决非线性不可分问题，映射到高维。原有数据映射到高维计算非常非常慢

Langrange multipliers+duality， if the optimization problem is convex(凸), then this can be solved

The dual and primal problems are related through the Lagrange multipliers. if it is convex,solving the dual is equivalent to solving the primal

and SVM problem can be rewritten to a dual peoblem. 而重写后的式子只用求两个高维向量的内积，因此与其映射到高维并计算高维向量的积，我们只用找到一种方法可以快速映射/计算两个高维向量的内积：

![Untitled](Machine%20Le%206bb85/Untitled%2025.png)

kernel trick：Replacing the inner product with a kernel function in the optimization problem

customized kernel: can use any kernel function, as long as it is positive definite

多项式、RBF(similiar to Gaussian)..

![Untitled](Machine%20Le%206bb85/Untitled%2026.png)

![Untitled](Machine%20Le%206bb85/Untitled%2027.png)

集成算法：

![Untitled](Machine%20Le%206bb85/Untitled%2028.png)

Boosting can do feature selection：每一个分类器focus on 一个feature

Boosting is sensitive to outlier

Adaboost：train weak classifiers sequentially，focus on data that is misclassified.

![Untitled](Machine%20Le%206bb85/Untitled%2029.png)

![Untitled](Machine%20Le%206bb85/Untitled%2030.png)

![Untitled](Machine%20Le%206bb85/Untitled%2031.png)

![Untitled](Machine%20Le%206bb85/Untitled%2032.png)

gradient boosting ：in each iteration, the weak learner fits the gradient of the loss

random forest: can also do feature selection,fast.sensitive to outliers

![Untitled](Machine%20Le%206bb85/Untitled%2033.png)

some feature dimensions with largerranges do not dominate the optimization process by：

**standardize标准化**: scale each feature dimension so the mean is 0 and variance is 1.

**normalize归一化**: scale features to a fixed range, -1 to 1.

Unbalanced data：classifier will focus more on the class with more points. decision boundary is pushed away from class with more points, apply weights are inversely proportional to the class size.

Class imbalance: mistakes on some classes are more critical. reweight class to focus classifier on correctly predicting one class at the expense of others.

"No Free Lunch" ：The best classifier depends on the particular problem.

分类总结：

![Untitled](Machine%20Le%206bb85/Untitled%2034.png)

![Untitled](Machine%20Le%206bb85/Untitled%2035.png)

回归模型：

线性回归：y=wx+b(w0), use Ordinary Least Squares(OLS)

![Untitled](Machine%20Le%206bb85/Untitled%2036.png)

lasso回归和ridge回归；线性回归基础上分别增加L1和L2正则化。L1 正则化会比 L2 正则化让线性回归的权重更加稀疏，使得线性回归中很多权重为 0，即 L1 正则化（lasso）可以进行 feature selection，而 L2 正则化（ridge）不行（lasso 更容易使得权重变为 0，而 ridge 更容易使得权重接近 0）。

![Untitled](Machine%20Le%206bb85/Untitled%2037.png)

![Untitled](Machine%20Le%206bb85/Untitled%2038.png)

![Untitled](Machine%20Le%206bb85/Untitled%2039.png)

L2 focuses more on large weights. L1 treats all weights equally.

OMP（正交匹配追踪法） 加入L0正则化并使用贪心算法进行优化 ,只考虑非0 entry数量超过K的w

![Untitled](Machine%20Le%206bb85/Untitled%2040.png)

去除数据异常值很重要（outliers）

随机抽样一致算法（RANSAC）：把数据分为内点群（inliers）和外点群（outliers）,训练时忽略outlier。the Threshold typically set as the median absolute deviation of y

![Untitled](Machine%20Le%206bb85/Untitled%2041.png)

**非线性回归**：

多项式回归: 交叉验证选择degree

![Untitled](Machine%20Le%206bb85/Untitled%2042.png)

核函数岭回归：加kernel-trick，很慢，比SVR慢

高斯过程回归(Gaussian Process Regression)：GP is prior distribution over functions. 

**assumes values are zero-mean and unit variance，所以必须归一化**

GPR with a linear kernel is equivalent to Bayesian linear regression

![Untitled](Machine%20Le%206bb85/Untitled%2043.png)

GPR可以用复合核函数：kernels can be summed, multiplied, and exponentiated to make new kernels

the uncertainty (stddev of the prediction) increases when less data is available.

maximizing the marginal likelihood:

![Untitled](Machine%20Le%206bb85/Untitled%2044.png)

![Untitled](Machine%20Le%206bb85/Untitled%2045.png)

支持向量回归(加入核函数变为非线性): tube width is important

![Untitled](Machine%20Le%206bb85/Untitled%2046.png)

随机森林回归

xgboost回归

![Untitled](Machine%20Le%206bb85/Untitled%2047.png)

回归前的归一化标准化很关键. 

当y的范围特别大或特别小时，会影响RMSE，最好用log(y) 映射一下

![Untitled](Machine%20Le%206bb85/Untitled%2048.png)

无监督学习：

Demension Reduction：

Dimensions in the low-dim data represent co-occuring features in high-dim data.

Dimensions in the low-dim data may have semantic meaning.

why do DR:

![Untitled](Machine%20Le%206bb85/Untitled%2049.png)

线性降维：简单的从高维映射到低维平面，minimize the reconstruction error 

![Untitled](Machine%20Le%206bb85/Untitled%2050.png)

pca主成分分析：无监督，降维后特征互相独立，本质是对数据的协方差矩阵求特征值及特征向量取前k个   to  find basis vectors that are orthogonal.

![Untitled](Machine%20Le%206bb85/Untitled%2051.png)

![Untitled](Machine%20Le%206bb85/Untitled%2052.png)

Denoising去除噪声：

![Untitled](Machine%20Le%206bb85/Untitled%2053.png)

Radndom Projection：维数高计算很慢，从高斯分布随机生成初始基向量，fatser, 选的好可以保留相对信息

Sparse Random Projection：many entries in the basis vector are zero, so we can ignore those entries when multiplying. more faster

PCA并不考虑标签，所以降维反而可能对分类没有帮助：

FLD（Fisher's Linear Discriminant）有监督降维：

find a lower-dim space so as to minimize the class overlap，最小化同类方差

data from each class is modeled as a Gaussian.

![Untitled](Machine%20Le%206bb85/Untitled%2054.png)

![Untitled](Machine%20Le%206bb85/Untitled%2055.png)

文本降维：

LSA 潜在语义分析：本质是对词-文本矩阵进行SVD奇异分解(按主题划分) 高维太慢

![Untitled](Machine%20Le%206bb85/Untitled%2056.png)

Advantage: Finds relations between terms (synonymy and polysemy). distances/similarities are now comparing topics rather than words, so can do higher-level semantic representation, 有closed-form solution

topic的weight可以是负数,bad bad

NMF非负矩阵分解:  比LSA快很多，且没有负数，但是没有closed-form solution，需要用optimizer

而且 While the weights and component vectors are non-negative, NMF does not enforce them to be probabilities（没有概率）

PCA和LDA（FLD）是基于协方差矩阵，所以归一化和标准化会导致协方差矩阵的变化，从而降维结果不同

![Untitled](Machine%20Le%206bb85/Untitled%2057.png)

非线性降维(try to preserve inherent structure of data, calculate low-dim coefficients using non-linear projections (kernel).)：

KPCA：add kernel

![Untitled](Machine%20Le%206bb85/Untitled%2058.png)

Using RBF kernel, KPCA can split the data into clusters.

流形学习/流形嵌入（Manifold Learning）：认为高维空间由低维空间流形卷曲而成(preserve some more property from high-dim)

**After training, manifold embedding methods cannot transform a novel (new) point. need to re-train the whole thing.**

![Untitled](Machine%20Le%206bb85/Untitled%2059.png)

![Untitled](Machine%20Le%206bb85/Untitled%2060.png)

LLE(Locally-linear Embedding）：sensitive to the number of neighbors for defining the local region

HLLE:

LSTA:rather than preserve distances, align local tangent spaces of the neighborhoods.

MDS: find a low-dimensional embedding that preserves the pairwise distances. 

MDS use Euclidean dis, but two points may be far away along the manifold (geodesic distance), but close in Euclidean distance.

Isomap:Find embedding that preserves geodesic distances between points

![Untitled](Machine%20Le%206bb85/Untitled%2061.png)

SE: Preserve the neighborhood structure

t-SNE: Preserve pairwise similarities,Tends to group together similar items

![Untitled](Machine%20Le%206bb85/Untitled%2062.png)

![Untitled](Machine%20Le%206bb85/Untitled%2063.png)

聚类算法：

kmeans：minimize the total sum-squared difference between points and their centers。一直到converge 停止。

初始cluster center的位置非常重要，所以Try several times using different initializations. Pick the answer with lowest objective score.

![Untitled](Machine%20Le%206bb85/Untitled%2064.png)

**Bag-of-X Representation**：用kmeans来构造新的特征向量，n个cluster代表n维的新特征

![Untitled](Machine%20Le%206bb85/Untitled%2065.png)

kmeans认为簇都是圆形的，对椭圆或者其他形状的簇效果不好（skewed or elliptical）

**Gaussian mixture model(GMM)混合高斯模型**:

A multivariate Gaussian can model a cluster with an elliptical shape

![Untitled](Machine%20Le%206bb85/Untitled%2066.png)

GMM是很多个多元高斯模型的组合：

![Untitled](Machine%20Le%206bb85/Untitled%2067.png)

Difficult to optimize because the "sum" is inside the "log". 

'soft' assignment: a data can have a fractional assignment to different cluster, kmeans is hard assignment

But We can use **Expectation Maximization (EM) Algorithm** finding the MLE solution when there are hidden (unseen) variables

![Untitled](Machine%20Le%206bb85/Untitled%2068.png)

![Untitled](Machine%20Le%206bb85/Untitled%2069.png)

![Untitled](Machine%20Le%206bb85/Untitled%2070.png)

协方差矩阵：O（d^2） 维度大的时候计算量很大

可以使用 **diagonal covariance matrices对角协方差矩阵**来减少计算量：O（d）

或者 **spherical covariance matrices：O(1)**

Dirichlet Process (DPGMM)：可以自动选择K

use a "Dirichlet Process" to model K as a random variable.

![Untitled](Machine%20Le%206bb85/Untitled%2071.png)

concentration parameter a，larger a may yield more clusters, bu tis is not that important

Non-parametric estimation: We want to estimate a probability density without assuming a parametric model (e.g., Gaussian),we can put a small Gaussian at each data point, and sum it up. this is called **kernel density estimator(KDE),** the kernel here is small Gaussian

![Untitled](Machine%20Le%206bb85/Untitled%2072.png)

then we get **mean-shift**: iteratively shift towards the largest concentration of points. start from an initial point   (e.g., one of the data points).

![Untitled](Machine%20Le%206bb85/Untitled%2073.png)

Run the mean-shift algorithm for many initial points .

the set of converged points contain the cluster centers.

need to remove the duplicate centers.

data points that converge to the same center belong to the same cluster.

![Untitled](Machine%20Le%206bb85/Untitled%2074.png)

对于other shape的聚类：

**谱聚类**：pair-wise affinity

cut the graph into clusters such that weights of cut edges is small compared to the total edge weight within each cluster.find "blocks" of high affinity in the affinity matrix.

![Untitled](Machine%20Le%206bb85/Untitled%2075.png)

![Untitled](Machine%20Le%206bb85/Untitled%2076.png)

聚类也是需要归一化标准化的（基于欧几里得距离）

![Untitled](Machine%20Le%206bb85/Untitled%2077.png)

![Untitled](Machine%20Le%206bb85/Untitled%2078.png)

Perceptron：无法处理非线性数据

Different initializations can yield different weights

![Untitled](Machine%20Le%206bb85/Untitled%2079.png)

![Untitled](Machine%20Le%206bb85/Untitled%2080.png)

Multilayer Perceptron：

![Untitled](Machine%20Le%206bb85/Untitled%2081.png)

![Untitled](Machine%20Le%206bb85/Untitled%2082.png)

![Untitled](Machine%20Le%206bb85/Untitled%2083.png)

Relu：

![Untitled](Machine%20Le%206bb85/Untitled%2084.png)

Backpropagation：calculate the gradient of each node from last to first layer

**the gradients multiply in each layer**

梯度消失：（1）if two gradients are small (<1), their product will be even smaller. finally converges to 0. This is the "vanishing gradient" problem.

（2）using backprop, the gradient at a node is the summation over paths，the original loss signal gets "washed out".

**Stochastic Gradient Descent (SGD)**：

![Untitled](Machine%20Le%206bb85/Untitled%2085.png)

![Untitled](Machine%20Le%206bb85/Untitled%2086.png)

**Ealry stoppping**：Training can be stopped when the validation loss is stable for a number of iterations. stable means change below a threshold. this is to prevent overfitting the training data.

we can limit the number of iterations.

反向传播(梯度的反向下降)：对于每一个训练实例，将它传入神经网络，计算它的输出；然后测量网络的输出误差（即期望输出和实际输出之间的差异），并**计算出上一个隐藏层中各神经元为该输出结果贡献了多少的误差**；反复一直从后一层计算到前一层，直到算法到达初始的输入层为止。此反向传递过程有效地测量网络中所有连接权重的误差梯度，最后**通过在每一个隐藏层中应用梯度下降算法来优化该层的参数**（反向传播算法的名称也因此而来）。

**Universal Approximation Theorem**:

可以用一层隐藏层和任意节点来表示任何一个连续映射的函数(under desired error)

A multi-layer perceptron with a single hidden layer and a finite number of nodes

can approximate any continuous function up to a desired error.

![Untitled](Machine%20Le%206bb85/Untitled%2087.png)

Deep learning corollary（深层网络相比一层）:

The number of functions representable by a deep network requires an exponential number of nodes for a shallow network with 1 hidden-layer.

A deep network can learn the same function using less nodes.

Given the same number of nodes, a deep network can learn more complex functions.

中间层节点权重可以是负数

**network is still sensitive to initialization对初始权重敏感：train several networks and combine them as an ensemble.**

![Untitled](Machine%20Le%206bb85/Untitled%2088.png)

Locality : at low-level, features from 1 region are independent (do not depend on)

features from a far-away region.

Translation: the same features can appear anywhere in the signal

![Untitled](Machine%20Le%206bb85/Untitled%2089.png)

**MLP ignores the spatial relationship between pixels**

Convolution卷积：

![Untitled](Machine%20Le%206bb85/Untitled%2090.png)

![Untitled](Machine%20Le%206bb85/Untitled%2091.png)

（1）原始图像通过与卷积核的数学运算，可以提取出图像的某些指定特征（features)。

（2）不同卷积核，提取的特征也是不一样的。

（3）提取的特征一样，不同的卷积核，效果也不一样。

cnn的两种解释：

1. 从频域角度：卷积核是频域上的一个过滤器(Convolution kernel is a filtering operation )

convolution in the time domain is equivalent to multiplication in the frequency domain

![Untitled](Machine%20Le%206bb85/Untitled%2092.png)

![Untitled](Machine%20Le%206bb85/Untitled%2093.png)

2.从模式匹配角度（学习特征模式如车、人脸）

![Untitled](Machine%20Le%206bb85/Untitled%2094.png)

卷积对边缘数据如何进行填充padding：

"valid" means no padding. "same" results in padding with zeros evenly to the left/right or up/down of the input such that output has the same height/width dimension as the input. 

输出相比输入：valid变小，sam不变，full会增大

![Untitled](Machine%20Le%206bb85/Untitled%2095.png)

"same" is better since it looks at structures around border

卷积层：for image with C channels. apply F convolution filters to get F feature maps. Each feature map uses a 3D convolution filter (CxKxK) on the input .K is the spatial extent of the filter; total FCKK parameters

多个卷积层由上到下：spatial resolution decreases，number of feature maps increases，Can extract high-level features in the final layers（从角、边到零件最后到整个车）

sub-sampling（to reduce the feature map size）的两种方法：

1）stride：step size when moving the windows across the image. x=2那就是只在x轴上移动2步，有时可以用来替代池化层

2）池化层max-pooling ：

1. local translation invariance(不变性)，这种不变性包括translation(平移)，rotation(旋转)，scale
2. 保留主要的特征同时减少参数(降维，效果类似PCA)和计算量，防止过拟合，提高模型泛化能力(robust)

![Untitled](Machine%20Le%206bb85/Untitled%2096.png)

![Untitled](Machine%20Le%206bb85/Untitled%2097.png)

Receptive field ：what pixels in the input affect a particular node. larger receptive fields can see larger patterns.

Receptive field size ：

![Untitled](Machine%20Le%206bb85/Untitled%2098.png)

![Untitled](Machine%20Le%206bb85/Untitled%2099.png)

防止过拟合：

dropout层:randomly remove a node with some probability(output = 0) 可以避免梯度消失，加速训练过程，防止过拟合(increases robustness)

kernel_regularizer +l2 norm

Early Stopping

ensemble：Bagging: Bootstrap Aggregation. combine predictions from several models (model averaging).If the errors are uncorrelated , then the MSE decreases!

Data augmentation数据增强：artificially permute the data to increase the dataset size，make the network invariant to the permutations，translate image, flip image, add pixel noise, rotate image, deform image or add some noise. improves robustness or invariance to selected transformations

至少需要多少样本：

![Untitled](Machine%20Le%206bb85/Untitled%20100.png)

**RELU**：ReLU(z)=max(0,z)

![Untitled](Machine%20Le%206bb85/Untitled%20101.png)

![Untitled](Machine%20Le%206bb85/Untitled%20102.png)

internal covariate shift ：change in the distribution of activations during training, due to

changes in the parameters. bad Parameterization

**BatchNormalization**：For each node in each layer, normalize the outputs to zero mean and unit variance, over each mini-batch. 

prevent gradient vanish and speed up the training for it can  use higher learning rates,and better generalization because no need for dropout or L2 regularization.

alter the learning rate:

![Untitled](Machine%20Le%206bb85/Untitled%20103.png)

Adaptive schedule: reduce the learning rate when the validation loss no longer improves

Fixed schedule: 自定义

add a Momentum动量: The estimated gradient is noisy, can jump around. we keep a running average of the gradients across mini-batches.

![Untitled](Machine%20Le%206bb85/Untitled%20104.png)

SGD smoothes the loss function. higher learning rate -> larger noise -> smoother loss. smoother loss removes the local minimum, making it easier to get near the global minimum.

Adaptive Learning Rates optimizer:AdaGrad RMSProp Adam

ImageNet DataSet

Auxiliary tasks: strengthen the supervisory signal to the early layers

![Untitled](Machine%20Le%206bb85/Untitled%20105.png)

Multitask learning:

![Untitled](Machine%20Le%206bb85/Untitled%20106.png)

残差学习Residual Learning：**equivalent to an ensemble**

![Untitled](Machine%20Le%206bb85/Untitled%20107.png)

![Untitled](Machine%20Le%206bb85/Untitled%20108.png)

residual connection every two layers to solve the ***Degradation problem***

![Untitled](Machine%20Le%206bb85/Untitled%20109.png)

Attention Mechanisms:ignore unimportant features (channels or spatial regions) in the feature map by using a (soft) mask. **SEnet**

Pre-trained model: The learned features generalize well to other tasks ,c**apture high-level discriminative information about objects**

so we can use Pre-trained mode as feature extractor without the last layer(再加一个池化层)

Transfer learning and finetuning：pre-trained model + our network, only fine-tune our network.

![Untitled](Machine%20Le%206bb85/Untitled%20110.png)

Autoencoder: 降维训练再升维回来

Weight sharing is used to reduce the number of trainable parameters

![Untitled](Machine%20Le%206bb85/Untitled%20111.png)

![Untitled](Machine%20Le%206bb85/Untitled%20112.png)

Denoising Autoencoder（DAE）：randomly corrupt the input (by setting values to 0), implemented by applying Dropout on the inputs.

learn about the data manifold and enables better latent representation

两种解释：

![Untitled](Machine%20Le%206bb85/Untitled%20113.png)

Convolutional AutoEncoder：use CNN,Decoder is the opposite architecture which replace maxpooling with upsampling

Semantic Segmentation:

![Untitled](Machine%20Le%206bb85/Untitled%20114.png)

U-net: Mix high-level and low-level features.

![Untitled](Machine%20Le%206bb85/Untitled%20115.png)

Deep Generative Models:

![Untitled](Machine%20Le%206bb85/Untitled%20116.png)

Reparameterization Trick

![Untitled](Machine%20Le%206bb85/Untitled%20117.png)

Variational AutoEncoder (VAE)

![Untitled](Machine%20Le%206bb85/Untitled%20118.png)

Generative Adversarial Networks (GAN)

![Untitled](Machine%20Le%206bb85/Untitled%20119.png)

![Untitled](Machine%20Le%206bb85/Untitled%20120.png)

![Untitled](Machine%20Le%206bb85/Untitled%20121.png)

The VAE is trained to maximize the data marginal likelihood, while the GAN is trained to maximize confusion.

![Untitled](Machine%20Le%206bb85/Untitled%20122.png)

project1思路：

**特征提取：nltk分词**（去掉停顿词词性还原、词干提取 ）+ **数据清洗** :

[https://pythonprogramming.net/tokenizing-words-sentences-nltk-tutorial/](https://pythonprogramming.net/tokenizing-words-sentences-nltk-tutorial/)

[https://cloud.tencent.com/developer/article/1555662](https://cloud.tencent.com/developer/article/1555662)

词干提取和词性还原参考：[https://zhuanlan.zhihu.com/p/266976172](https://zhuanlan.zhihu.com/p/266976172)‘

词干提取主要是采用“缩减”的方法，将词转换为词干，如将“effective”处理为“effect”。词形还原主要采用“转变”的方法，将词转变为其原形，如将“drove”处理为“drive”。

对句子简单分词后，大小写转换并去掉停顿词stop word和无用词(根据数据进行分析，比如@、#)

去除低频词?

eda?

**特征处理：gensim.word2vec 建立词向量** 

**keras.lstm 进行分类**

期末project todo：

cnn：

映射函数 使用L-Softmax、SM-Softmax、AM-Softmax等

梯度优化 先用Adam/nadam快速下降，然后换sgd: [https://zhuanlan.zhihu.com/p/32262540](https://zhuanlan.zhihu.com/p/32262540)  

SGD -> SGDM -> NAG ->AdaGrad -> AdaDelta -> Adam -> Nadam

`learning_rate=lr_schedule`

ranger optimizer

节点选取：**训练样本数必须多于网络模型的连接权数，一般为*2~10*倍**

通常，**对所有隐藏层使用相同数量的神经元就足够了**。对于某些数据集，拥有较大的第一层并在其后跟随较小的层将导致更好的性能，因为第一层可以学习很多低阶的特征，这些较低层的特征可以馈入后续层中，提取出较高阶特征。

需要注意的是，与在每一层中添加更多的神经元相比，**添加层层数将获得更大的性能提升**。因此，**不要在一个隐藏层中加入过多的神经元**。

![Untitled](Machine%20Le%206bb85/Untitled%20123.png)

还可引入early_stopping

层数选取：**对于一般简单的数据集，一两层隐藏层通常就足够了。但对于涉及时间序列或计算机视觉的复杂数据集，则需要额外增加层数**

![Untitled](Machine%20Le%206bb85/Untitled%20124.png)

- **隐藏层数=1**：可以拟合任何“包含从一个有限空间到另一个有限空间的连续映射”的函数
- **隐藏层数=2**：搭配适当的激活函数可以表示任意精度的任意决策边界，并且可以拟合任何精度的任何平滑映射
- **隐藏层数>2**：多出来的隐藏层可以学习复杂的描述（某种自动特征工程 视觉 时间序列）

数据集不够多：数据增强:图像的scaling, rotating, skewing, flipping 及其他

使用一些论文里的cnn结构 如**MobileNetV2、Swin Transformer Resnet EfficientNet**

迁移学习 

读取data加速 

+emsemble

聚类评价指标：

chi -类中各点与类中心的距离平方和来度量类内的紧密度，通过计算各类中心点与数据集中心点距离平方和来度量数据集的分离度。CH指标由分离度与紧密度的比值得到。从而，CH越大代表着类自身越紧密，类与类之间越分散，即更优的聚类结果。

dbi - 类内距离和/类间距离和  DB指数越小说明聚类效果越好

轮廓系数：

![Untitled](Machine%20Le%206bb85/Untitled%20125.png)