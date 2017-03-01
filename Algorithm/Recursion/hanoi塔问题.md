---
title: hanoi塔问题
date: 2017-03-02 00:00:01
categories:
- Algorithm
- Recursion
tags:
- recursion
description: ...
---

### 问题
* 问题分析
    * 最简单的递推问题
    * n个disk的总的移动次数是$2^n-1$, 推导过程$x_{k} = x_{k-1}*2 + 1$.

### 代码实例
```
#include <iostream>
#include <cstdio>

using namespace std;

/**
 * n is number of disks
 *
 */
void move(int n, int a, int b) {
    printf("move %d: %d --&gt; %d\n", n, a, b);
}
void hanoi(int n, int a, int b, int c) {
    if(n > 0) {
        hanoi(n-1, a, c, b);
        move(n, a, c);
        hanoi(n-1, b, a, c);
    }
}

int main() {

    int n;
    while(scanf("%d", &n) != EOF) {
        hanoi(n, 1, 3, 2);
    }

    return 0;
}
```
