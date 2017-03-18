---
title: python:调用工具包实现决策树算法ID3
date: 2017-03-16 00:00:01
categories:
- Machine_Learning
- Machine_Learning_Practice
tags:
- machinelearning
- python
description: ...
---

### 决策树算法
决策树学习的目的是为了产生一棵泛化能力强，即处理未见示例能力强的决策树，其基本流程遵循简单且直观的“分治”策略.  
**算法**  

    ```
    输入: 训练集D = {(x1, y1), (x2, y2), ..., (xm, ym)}
        属性值A = {a1, a2, ..., ad}.
    过程: 函数TreeGenerate(D, A)
    
    生成节点node
    if D中样本全属于一个类别C
        将node标记为C类叶节点
        return
    end if
    if A = 空集 or D中样本在A上取值相同
        将node标记为叶节点，类别为D中样本数最多的类
        return
    end if
    
    从A中选择最优划分属性a*
    
    for a*的每一个值a*v
        为node生成一个分支,另Dv表示D中在a*上取值为a*v的样本子集
        if Dv为空
            将分支节点标记叶节点，其类别标记为D中样本最多的类
            return
        else 
            以TreeGenerate(Dv, A-{a*})为分支结点
        end if
    end for
    
    输出: 以node为根节点的一棵决策树
    ```  
**划分选择**  
决策树的学习关键是最有划分属性的选择.一般而言，随着划分过程的不断进行，我们希望决策树的分支结果所包含的样本尽可能属于同一类别，即结点“纯度”越来越高.  
几种主要的划分方法: **信息增益(ID3), 增益率(C4.5), 基尼系数(CART)**

### python调用工具包实现ID3  
[决策树算法测自制数据.zip](https://github.com/su526664687/PictureLibrary/files/852269/default.zip)

``` python
from sklearn.feature_extraction import DictVectorizer
import csv
from sklearn import preprocessing
from sklearn import tree
from sklearn.externals.six import StringIO

allElectronicsData = open('/home/sujinming/test.csv')
reader = csv.reader(allElectronicsData)
headers = reader.next()
# print headers

featureList = []
labelList = []

for row in reader:
    labelList.append(row[len(row) - 1])
    rowDict = {}
    for i in range(1, len(row) - 1):
        rowDict[headers[i]] = row[i]
    featureList.append(rowDict)
# print featureList

vec = DictVectorizer()
dummyX = vec.fit_transform(featureList).toarray()
# print dummyX

lb = preprocessing.LabelBinarizer()
dummyY = lb.fit_transform(labelList)
# print dummyY

clf = tree.DecisionTreeClassifier(criterion='entropy')
clf = clf.fit(dummyX, dummyY)
# print str(clf)

with open('allEallElectronicInfomationGainori.dot', 'w') as f:
    f = tree.export_graphviz(clf, out_file = f, 
                             feature_names = vec.get_feature_names())

# predict
oneRowX = dummyX[0,:]
newRowX = oneRowX  
newRowX[0] = 1  
newRowX[2] = 0  
  
predictedY = clf.predict(newRowX)  
print "predictedY:" + str(predictedY)
```
