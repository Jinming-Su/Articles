> support vector machine

### matlab自带的svm
* 主要使用两个函数
* svmtrain
* svmclassify

### matlab测试
* 代码

  ```
  clc;  
  clear;  
  close all;  

  traindata = [0 1; -1 0; 2 2; 3 3; -2 -1;-4.5 -4; 2 -1; -1 -3];  
  group = [1 1 -1 -1 1 1 -1 -1]';  

  testdata = [5 2;3 1;-4 -3];  
  svm_struct = svmtrain(traindata,group,'Showplot',true);       % training  
  Group = svmclassify(svm_struct,testdata,'Showplot',true);  
  hold on;  
  plot(testdata(:,1),testdata(:,2),'ro','MarkerSize',12);       % testing  
  hold off  
  ```
* 测试结果  
  ![1](https://cloud.githubusercontent.com/assets/16068384/22853462/f1616212-f091-11e6-959d-ee79ab331f73.png)

  
