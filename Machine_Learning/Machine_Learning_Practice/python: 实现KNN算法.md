---
title: python:实现KNN算法
date: 2017-03-18 00:00:01
categories:
- Machine_Learning
- Machine_Learning_Practice
tags:
- knn
- python
description: 分别使用python工具包和手写KNN算法
---



### 说明
* 具体算法说明可参考matlab实现knn文章
* 两个代码均使用的是iris的数据集，这个数据集直接可以在网上下载.

### 工具包运行KNN

  ```python
  from sklearn import neighbors
  from sklearn import datasets

  knn = neighbors.KNeighborsClassifier()

  iris = datasets.load_iris()
  # print iris

  knn.fit(iris.data, iris.target)

  predictedLabel = knn.predict([[0.1, 0.2, 0.3, 0.4]])

  print predictedLabel
  ```

### 手工实现KNN

  ```python
  import csv
  import random
  import math
  import operator
  from networkx.classes.function import neighbors

  def loadDataset(filename, split, trainingSet = [], testSet = []):
      with open(filename, 'rb') as csvfile:
          lines = csv.reader(csvfile)
          dataset = list(lines)
          for x in range(len(dataset) - 1):
              for y in range(4):
                  dataset[x][y] = float(dataset[x][y])
              if random.random() < split:
                  trainingSet.append(dataset[x])
              else:
                  testSet.append(dataset[x])

  def euclideanDistance(instance, instance2, length):
      distance = 0
      for x in range(length):
          distance += pow((instance[x] - instance2[x]), 2)
      return math.sqrt(distance)

  def getNeighbors(trainingSet, testInstance, k):
      distances = []
      length = len(testInstance) - 1
      for x in range(len(trainingSet)):
          dist = euclideanDistance(trainingSet[x], testInstance, length)
          distances.append((trainingSet[x], dist))
      distances.sort(key = operator.itemgetter(1)) ###
      neighbors = []
      for x in range(k):
          neighbors.append(distances[x][0]) 
      return neighbors

  def getResponse(neighbors):
      classVotes = {}
      for x in range(len(neighbors)):
          response = neighbors[x][-1]
          if response in classVotes:
              classVotes[response] += 1
          else:
              classVotes[response] = 1
      # print classVotes
      sortedVotes = sorted(classVotes.iteritems(), key = operator.itemgetter(1),
                           reverse = True)
      # print classVotes
      # print type(sortedVotes[0])  # 对Dict进行排序，返回值是tuple类型的
      return sortedVotes[0][0] ###

  def getAccuracy(testSet, predictions):
      correct = 0
      for x in range(len(testSet)):
          if testSet[x][-1] == predictions[x]:
              correct += 1
      return (correct/float(len(testSet)))*100.0 

  def main():
      trainingSet = []
      testSet = []
      split = 0.67
      loadDataset('iris.txt', split, trainingSet, testSet)
      print 'Train set: ' + repr(len(trainingSet)) # cannot concatenate 'str' and 'int' objects
      print 'Test set: ' + repr(len(testSet))

      predictions = []
      k = 3

      for x in range(len(testSet)):
          neighbors = getNeighbors(trainingSet, testSet[x], k)
          result = getResponse(neighbors)
          predictions.append(result)
          print 'Predicted = ' + result + ', actual = ' + testSet[x][-1]
      accuracy = getAccuracy(testSet, predictions)
      print 'Accuracy : ' + repr(accuracy) + '%'

  main()
  ```
  
