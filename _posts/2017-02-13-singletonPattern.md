---
layout: post
title: 单例模式
categories: DesignPattern
description: 实现单例模式的两种方法：懒汉式、饿汉式。
keywords: Windows, Process
---

# 单例模式(Singleton pattern)

单例模式的意义在于确保整个应用中某个实例，有且只有一个。

有些对象我们只需要一个，如：配置文件、工具类、线程池、缓存、日志对象等。
如果创造出多个实例，就会导致很多问题，比如占用过多资源，不一致的结果等。

### 方法一：懒汉式

#### 示例代码：

```java
/**
 * 懒汉模式
 * Created by muyun on 2017/2/10.
 */
public class LazySingleton {

    //构造方法私有化，禁止外界创造实例
    private LazySingleton() {

    }

    //创建单例
    private static LazySingleton ourInstance;

    //提供get()方法
    public static LazySingleton getInstance() {
        if(ourInstance == null){
            ourInstance = new LazySingleton();
        }
        return ourInstance;
    }
}
```

### 方法二：饿汉式

#### 示例代码：

```java
/**
 * 饿汉模式
 * Created by muyun on 2017/2/10.
 */
public class HungrySingleton {

    //构造方法私有化，禁止外界创造实例
    private HungrySingleton() {

    }

    //创建单例
    private static HungrySingleton ourInstance = new HungrySingleton();

    //提供get()方法
    public static HungrySingleton getInstance() {
        return ourInstance;
    }
}

```

## 区别：

* 懒汉模式：加载类的时候，没有创建类的实例，系统的空间开销会相对较小并且加载相对较快，但是每次获取实例的时候，都要判断实例是否存在
              而且第一次获取实例的时候需要创建实例，所以实例获取速度较慢。线程不安全。
* 饿汉模式：加载类的时候，就会创建类的实例，系统的空间开销会相对较大，但是获取实例速度较快。线程安全。
