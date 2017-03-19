---
title: python:实现logistic_regression
date: 2017-03-19 00:00:01
categories:
- Machine_Learning
- Machine_Learning_Pracitce
tags:
- logistic_regression
description: ...
---

### 理论知识
* 对数几率函数是一种sigmoid函数，sigmoid函数是一族形似S的函数
* Logistic Regression名字里带有回归，实际上是一种分类方法
* 可参考
    * http://blog.csdn.net/dongtingzhizi/article/details/15962797 
    * http://blog.csdn.net/Gamer_gyt/article/details/51232733 

### 使用梯度下降算法求解类似logistic regression  
说明: 其实求解的不是logistic regression,因为函数使用的并不是logistic，但是过程是对logistic的回归

    ```
    #coding:utf-8
    import numpy
    import random
    
    '''''
    梯度下降算法 
    x,y: 训练实例
    theta: 权重，也就是要求的参数
    alpha: 梯度下降时的参数,即学习率
    m: 实例的个数 
    numIteration： 梯度下降迭代次数
    '''  
    def gradientDescent(x, y, theta, alpha, m, numIterations):
        xTrans = x.transpose()
        for i in range(0, numIterations):
            hypothesis = numpy.dot(x, theta)
            loss = hypothesis - y
            cost = numpy.sum(loss ** 2) / (2 * m)
            print 'Iteration %d | Cost: %f' % (i, cost)
            gradient = numpy.dot(xTrans, loss) / m
            theta = theta - alpha * gradient
        return theta
    
    def getData(numPoints, bias, variance):
        x = numpy.zeros(shape = (numPoints, 2))
        y = numpy.zeros(shape = numPoints)
        for i in range(0, numPoints):
            x[i][0] = 1
            x[i][1] = i
            y[i] = (i + bias) + random.uniform(0, 1) * variance
        return x, y
    
    x, y = getData(100, 25, 10)
    m, n = numpy.shape(x)
    
    numIterations = 100000
    alpha = 0.0005
    theta = numpy.ones(n)
    
    theta = gradientDescent(x, y, theta, alpha, m, numIterations)
    
    print theta
    ```
