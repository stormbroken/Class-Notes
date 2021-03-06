java_泛型
---
<!-- TOC -->

- [1. 什么是泛型](#1-什么是泛型)
- [2. 特性](#2-特性)
- [3. 泛型的使用](#3-泛型的使用)
  - [3.1. 泛型类](#31-泛型类)
  - [3.2. 泛型接口](#32-泛型接口)
- [4. 参考网页](#4-参考网页)

<!-- /TOC -->
# 1. 什么是泛型
1. 泛型:即参数化类型。
2. 其本质是为了参数化类型(在不创建新的类型的情况下，通过泛型指定的不同类型来控制形参来控制形参具体限制的类型)
3. 泛型可以让部分错误在编译阶段可以被发现。

# 2. 特性
1. 泛型只在编译阶段是有效的，在编译之后，程序会采取泛型化的措施
2. 总而言之:泛型类型在逻辑上可以看成十多个不同的类型，但实际上都是相同的基本类型。

# 3. 泛型的使用
1. 三种使用方式:泛型化、泛型接口和泛型方法

## 3.1. 泛型类
1. 泛型类型用于类的定义中，被称为泛型类。通过泛型可以完成对一组类的操作对外开放相同的接口。
2. 相应的泛型类的例子:list、Set、Map。
3. 泛型类的最基本写法:
```java
class 类名称 <泛型标识：可以随便写任意标识号，标识指定的泛型的类型>{
  private 泛型标识 /*（成员变量类型）*/ var; 
  .....

  }
}
```
4. 基本的泛型类的示例:
```java
//此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
//在实例化泛型类时，必须指定T的具体类型
public class Generic<T>{ 
    //key这个成员变量的类型为T,T的类型由外部指定  
    private T key;

    public Generic(T key) { //泛型构造方法形参key的类型也为T，T的类型由外部指定
        this.key = key;
    }

    public T getKey(){ //泛型方法getKey的返回值类型为T，T的类型由外部指定
        return key;
    }
}
```
5. 泛型类:泛型的时候传入泛型实参，则会根据传入的泛型实参做相应的限制，这时候才会应起到的限制作用。

## 3.2. 泛型接口
1. 泛型接口与泛型类的定义及使用基本相同。
```java
//定义一个泛型接口
public interface Generator<T> {
    public T next();
}
```

# 4. 参考网页

<a href = "https://www.cnblogs.com/coprince/p/8603492.html">详解java泛型</a>