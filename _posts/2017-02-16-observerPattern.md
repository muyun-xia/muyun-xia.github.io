---
layout: post
title: 观察者模式
categories: DesignPattern
description: 观察者模式的实现与优缺点。
keywords: Subject, Observes
---

# 观察者模式(observer pattern)

在对象之间定义一对多的依赖，这样一来，当一个对象改变状态，依赖它的对象都会收到通知，并自动更新。

有些对象我们只需要一个，如：配置文件、工具类、线程池、缓存、日志对象等。
如果创造出多个实例，就会导致很多问题，比如占用过多资源，不一致的结果等。

## 源生的实现方式

### 目标对象代码：

```java
/**
 *  目标对象，它知道它的观察者，并提供注册（添加）和删除观察者的接口
 * Created by muyun on 2017/2/10.
 */
public class Subject {

    //用来保存注册的观察者对象
    private List<Observer> observers = new ArrayList<Observer>();

    /**
     * 添加观察者
     * @param observer
     */
    public void attach(Observer observer){
        observers.add(observer);
    }

    /**
     * 删除集合中制定的观察者
     * @param observer
     */
    public void detach(Observer observer){
        observers.remove(observer);
    }

    /**
     * 向所有注册的观察者更新信息
     */
    protected void notifyObservers() {
        for(Observer observer : observers){
            observer.update(this);
        }
    }

    protected void notifyObservers(String content) {
        for(Observer observer : observers){
            observer.update(content);
        }
    }
}
```

### 观察者接口代码：

```java
/**
 * 观察者接口，定义一个更新的接口给那些在目标发生改变的时候被通知的对象
 * Created by muyun on 2017/2/10.
 */
public interface Observer {

    /**
     * 更新的接口
     * @param subject 传入的目标对象方便获取相应的目标对象的状态
     */
    public void update(Subject subject);//拉模型

    /**
     * 更新的接口
     * @param content
     */
    public void update(String content);//推模型
}
```

### 具体的目标对象代码：

```java
/**
 * 具体的目标对象，负责把有关状态存入相应 的观察者对象。
 * Created by muyun on 2017/2/10.
 */
public class ConcreteSubject extends Subject {

    private String subjectState;//目标对象的状态

    public String getSubjectState() {
        return subjectState;
    }

    public void setSubjectState(String subjectState) {
        this.subjectState = subjectState;
        //通知观察者
        this.notifyObservers();
    }
}
```


### 具体的观察者对象：

```java
/**
 * 具体的观察者方法，实现更新的方法，使自身的状态和目标的状态保持一致
 * Created by muyun on 2017/2/10.
 */
public class ConcreteObserver implements Observer {

    private String observerState; //观察者状态

    /**
     * 获取目标状态同步到观察者状态中
     */
    @Override
    public void update(Subject subject) {
        String subjectState = ((ConcreteSubject)subject).getSubjectState();
        observerState = subjectState;
    }

    @Override
    public void update(String content) {
        observerState = content;
    }
}
```
## 除了我们手写代码之外，Java 本身也为我们提供了对观察者模式的实现

```java
/**
 * 天气目标的具体实现类
 * Created by muyun on 2017/2/10.
 */
public class ConcreteWeatherSubject extends Observable {

    //天气情况的内容
    private String content;

    public String getContent() {
        return content;
    }


    public void setContent(String content) {
        this.content = content;
        //天气有变化，要通知所有的观察者
        //注意：在使用java的观察者模式的时候，下面这句代码不可少。
        this.setChanged();
        //然后主动通知
        //推模式
        this.notifyObservers(content);
        //拉模式
        //this.notifyObservers();

    }
}
```


```java

/**
 * 具体的天气观察者
 * Created by muyun on 2017/2/10.
 */
public class ConcreteWeatherObserver implements java.util.Observer {

    //观察者名称
    private String observerName;

    public String getObserverName() {
        return observerName;
    }

    public void setObserverName(String observerName) {
        this.observerName = observerName;
    }

    @Override
    public void update(Observable o, Object arg) {
        //推模型
        System.out.println(observerName + "收到了消息，目标推送过来的是："+ arg);
        //拉模型
        System.out.println(observerName + "收到了消息，主动到目标对象中获取的拉取的内容内容是："+
                ((ConcreteWeatherSubject)o).getContent() );
    }
}
```
# 总结
* 目标与观察者之间的关系，目标与观察者是多对多的关系，也要可以简化好一对一的关系，一般下观察者观察者需要                       对不同的目标定义不同的回调方法，便于区分目标。
* 单项依赖，只有观察者依赖目标，而不是目标依赖观察者，联系主动权掌握在目标手里。
* 命名建议， 模式有被称为发布-订阅模式，
    * 目标接口，建议目标接口后面添加Subject作为目标标示。
    * 观察者接口，接口后面添加Observes作为目标标示。
** 观察者接口的更新方法，命名为update。
* 触发通知的时机，一般情况下是在完成状态维护后触发。
* 通知的顺序，通知时，通知的顺序是不确定的，所以多个观察者之间的通知顺序应该是平行的，不应该有依赖关系。

* 观察者模式的实现方法，推模型，拉模型。
    * 推模型：目标对象主动向观察者推送目标的详细信息，推送的信息通常是目标的全部或部分数据。
    * 拉模型：目标对象在通知观察者是，只推送少量的信息，如果观察者需要具体的信息。由观察者主动到目标对象中获取。
		        一般这种模型实现中，会把目标对象自身通过update方法传递给观察者。

* JAVA 本身对观察者模式的实现 java.util.Observable类
    * 不需要自己定义观察者和目标的接口了，JDK已经定义好了
    * 具体的目标实现里不需要再维护观察者的注册信息了，这个已经在Java的Observable类里帮忙实现了
    * 触发通知的方式有所不同，需要先调用setChanged()方法，这个是Java为了帮助实现更加精确的触发控制而
	       提供的功能
    * 具体的观察者的实现里update方法能够同时支持推模型和拉模型，这个是java在定义的时候，已经考虑好了的


* 观察者模式的优点：
    * 实现了观察者和目标之间的抽象耦合
    * 实现了观察者和目标的动态联动
    * 支持广播通信

* 观察者模式的缺点：
    * 可能会引起无谓的操作

* 观察者模式的本质：触发联动。当修改目标对象状态时，会触发通知，然后循环调用所有的观察者对象的相应的方法。

 ###建议在以下情形下使用观察者模式
* 当一个抽象模型有两个方面，一个方面的操作，依赖于另一个方面的状态变化。
* 如果在更改一个对象的时候，需要联动改变其他对象，而且不知道应该有多少对象需要被连带改变。
* 当一个对象必须通知其他对象，但是你又希望这个对象和其他被通知的对象是松散耦合的。

