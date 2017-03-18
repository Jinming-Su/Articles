> k-NearestNeighbor,k近邻算法

### 算法流程
* 1）计算测试数据与各个训练数据之间的距离；
* 2）按照距离的递增关系进行排序；
* 3）选取距离最小的K个点；
* 4）确定前K个点所在类别的出现频率；
* 5）返回前K个点中出现频率最高的类别作为测试数据的预测分类。

### matlab测试

  ```
  %KNN.m
  %% KNN
  function relustLabel = KNN(inx,data,labels,k)
      %% 
      %   inx 为 输入测试数据，data为样本数据，labels为样本标签
      %%
      [datarow , datacol] = size(data);
      diffMat = repmat(inx,[datarow,1]) - data ;
      distanceMat = sqrt(sum(diffMat.^2,2));
      [B , IX] = sort(distanceMat,'ascend');
      len = min(k,length(B));
      relustLabel = mode(labels(IX(1:len)));
  end
  
  %test.m
  clear all;
  clc;

  trainData = [1.0,2.0;1.2,0.1;0.1,1.4;0.3,3.5];
  trainClass = [1,1,2,2];
  testData = [0.5,2.3];
  k = 3;

  ans = KNN(testData, trainData, trainClass, k);
  disp(ans);
  
  %结果为2
  ```
