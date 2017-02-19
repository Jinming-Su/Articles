### cs231n notes
* lecture 1 介绍计算机视觉
    * 生物视觉的起源从5.4亿年前的物种大爆发开始--眼睛的出现是物种多样性迅速增多的一个重要因素.
    * 早期计算机视觉研究的两大结论：视觉的前期功能是识别边缘和简单的形状；视觉功能是分层的（影响了后来包括深度学习分层在内的很多研究）
    * 计算机视觉的两大研究领域：3D建模；图像识别（主流）
    * 计算机视觉当前的两大愿景：看图写作；视觉智能（视觉理解，涵盖文化范畴）
* Lecture 2 线性分类
    * 曼哈顿距离：又称L1距离，对应坐标的差的绝对值之和
    * KNN(k-NearestNeighbor)，见https://github.com/su526664687/Articles/blob/master/deep_learning/knn.md
    * SVM
    * softmax分类（一般化的logistic回归）,参考http://blog.csdn.net/xbinworld/article/details/45291009 
* Lecture 3 误差比较和优化
   * 梯度下降法
   * 学习率问题：通常开始使用大的学习率，之后对学习率慢慢减小
* Lecture 4 反向传播和简单神经网络
   * 反向传播常用模式
      * add gate: 梯度分配器——两边梯度相等
      * max gate: 梯度路由——最大值梯度看做1，小值梯度看做0
   * 常用激活函数
      * 在机器学习历史上，最早使用tanh函数
      * 2012年，ReLU(修正线性单元) max(0,x)开始流行【当今的默认选择】
      * 最近，leaky ReLU, Maxout ELU
   * 计数神经网络层数是计算有权值的层数， 输入层并不计算在内
   * http://cs.stanford.edu/people/karpathy/convnetjs/demo/classify2d.html 在线二分类二层神经网络测试
   * 加入适当的正则化参数，可以防止过拟合问题
* Lecture 5 训练神经网络细节
   * 使用CNN进行迁移学习
      * 首先，在大数据集上进行预训练（如ImageNet），这个已经有许多人做好了，可以利用pre training好的模型，比如Caffe Model Zoo的。CNN当做一个特征提取器进行对待
      * 其次，finetune自己的own data.如果数据量比较小，只需要训练分类器就可以了；如果数据量比较大，可以训练部分网络或者整个网络。
   * 激活函数
      * 上一节已经提到了很多的激活函数
      * 历史上最常使用的激活函数是sigmoid函数，存在许多问题
         * 1. [梯度弥散或者梯度消失]：sigmoid处理的神经元会成为一个饱和神经元（输出存在极限），输出要么为0，要么为1，导致反向传播中出现梯度趋向于0
         * 2. sigmoid输出值不是中心对称的
         * 3. exp()计算耗时
      * tanh()为了改进sigmoid而提出来的，解决了中心对称问题
      * ReLU在2012年被提出，成为一种默认使用的激活函数，在x>0区域解决了sigmoid的饱和问题，但是存在问题
         * 1. 不是中心对称
         * 2. x<0区域存在梯度弥散(dead ReLU)
      * Leaky ReLU(max(ax, x))/ELU/Maxout   
      ![1](https://cloud.githubusercontent.com/assets/16068384/23091672/523b698e-f5f6-11e6-86ae-9c63172255f0.png)
   * 数据预处理
      * 图像处理的数据预处理  
      ![1](https://cloud.githubusercontent.com/assets/16068384/23093337/4a7eab82-f61b-11e6-8a03-7086c5fc6771.png)
   * 权重初始化
      * 根据对称性，初始化的时候如果weight不随机那训练出来的weight就都一毛一样了
      * 随机化常采用的是N(0,0.01)，关于不同的激活函数，tanh和ReLU等初始化形式不同
   * 批处理正则化（这种方法可以省去考虑权重初始值问题）
   * 学习过程
      * 交叉验证
   * 超参数优化
* Lecture 6 训练神经网络细节
   * 参数更新
      * SGD最慢
      * momentum(v = mu * v - lr * dx; x += v)
      * Nesterov Momentum(先用更新后的位置的梯度)  
      ![1](https://cloud.githubusercontent.com/assets/16068384/23098737/27c5b768-f691-11e6-9838-e3dc58261c70.png)
      * adaGrad 梯度大的抑制一下步长, RMSProp优化adaGrad
      * Adam： momentum + RMSProp[默认比较好的选择]
   * regularization: dropout
      * 前向传播的时候，随机把一些神经元置0，反向传播同样进行
      * 作用：防止过拟合
      * test的时候可以多次dropout采样求平均,但是效率比较低（Monte Carlo approximation）。另一种方法就是，不进行dropout（神经元失活），而是在输出过程中对输出值乘以一个概率P（P的值与神经网络的结构有关）
### Lecture 7 
   * CNN例图  
   ![1](https://cloud.githubusercontent.com/assets/16068384/23103275/c8a03ca0-f6f3-11e6-8d2e-060db225ad0f.png)
   * CNN特点：局部感知；参数共享（全图同一个卷积核）；
   * 卷积层 http://blog.csdn.net/real_myth/article/details/51824193
      * 滤波器就是卷积核
      * 步长stripe, 填充padding
   * pool层 下采样（down sampling）
      * conv层通常不会在空间上减小数据的size，而是保持数据的size,通常是在下采样层减小数据的尺寸
      * 常见方式： max pooling（在一个滤波器范围内取最大值）; average pooling
   * fc全连接层
   * http://cs.stanford.edu/people/karpathy/convnetjs/demo/cifar10.html 在线CNNjs训练
