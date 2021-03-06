Vector
---
1. 需要头文件`#include<vector>`
2. 如果不`using namespace std;`会编译错误
3. 向量(Vector)是一个封装了动态大小数组的顺序容器，可以存放各种类型的对象。

<!-- TOC -->

- [1. 容器特性](#1-容器特性)
- [2. 声明初始化](#2-声明初始化)
  - [2.1. 简单初始化](#21-简单初始化)
  - [2.2. 二维vector的初始化](#22-二维vector的初始化)
- [3. 元素输入](#3-元素输入)
- [4. 特殊方法](#4-特殊方法)
- [5. 了解更多](#5-了解更多)

<!-- /TOC -->

# 1. 容器特性
1. **顺序序列**
    + 其中元素按照严格的线性顺序排序，可以通过元素在序列中的位置访问相应的元素。
2. **动态数组**
    + 支持对序列中的任意元素进行快速直接访问，甚至可以通过指针算术进行该操作。
3. **能够感知内存分配器的**
    + 使用了一个内存分配器对象来动态地处理它的存储需求。

# 2. 声明初始化

## 2.1. 简单初始化

```c++
vector<int> a ;
//声明一个int型向量a
vector<int> b(10);
//声明一个初始大小为10的向量
vector<int> c(10, 1);
//声明一个初始大小为10且初始值都为1的向量
vector<int> b(a);
//声明并用向量a初始化向量b
vector<int> b(a.begin(), a.begin()+3);
//将a向量中从第0个到第2个(共3个)作为向量b的初始值
```

## 2.2. 二维vector的初始化
```c++
vector<vector<int>> array;
```

# 3. 元素输入
1. 可以直接向普通数组一样使用cin>>，cout<<进行输入输出

# 4. 特殊方法

| 方法                                    | 作用                       |
| --------------------------------------- | -------------------------- |
| `a.size()`                              | 返回长度                   |
| `a.empty()`                             | 判断是否为空               |
| `a.clear()`                             | 清空向量中的元素           |
| `a.insert(a.begin(),共几个,插入的数字)` | 在一个位置上插入           |
| `a.erase(b.begin(),b.begin()+3)`        | 删除某个位置或者之间的元素 |
| `sort.(a.begin(),a.end())`              | 排序                       |

2. 遍历器:`vector<int>::iterator t;`
    + t = a.begin()
    + t!= a.end()
    + t ++

# 5. 了解更多
1. <a href ="https://www.runoob.com/w3cnote/cpp-vector-container-analysis.html">菜鸟教程</a>