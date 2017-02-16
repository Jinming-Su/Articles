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
