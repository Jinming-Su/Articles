---
title: matlab:调用工具包实现线性回归
date: 2017-03-19 00:00:01
categories:
- Machine_Learning
- Machine_Learning_Practice
tags:
- matlab
- 线性回归
description: ...
---

### 线性回归
* 使用matlab自带fit函数实现代码
  
  ```
  function [ parameter ] = checkcostfunc(  )  
  %CHECKC2 Summary of this function goes here  
  %   check if the cost function works well  
  %   check with the matlab fit function as standard  

  %check cost function 2  
  x=[1;2;3;4];  
  y=[1.1;2.2;2.7;3.8];  

  EXPR= {'x','1'};  
  p=fittype(EXPR);  
  parameter=fit(x,y,p);  

  end  
    
  % 结果
  ans = 

     Linear model:
     ans(x) = a*x + b
     Coefficients (with 95% confidence bounds):
       a =        0.86  (0.4949, 1.225)
       b =         0.3  (-0.6998, 1.3)
  ```
