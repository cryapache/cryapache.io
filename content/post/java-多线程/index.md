---
title: Java 多线程
date: 2024-12-09T22:41:54+08:00
draft: false
comments: true
---

## 进程与线程

#### 进程
一个在内存中运行的应用程序。每个进程都有自己独立的一块内存空间，一个进程可以有多个线程，比如在Windows系统中，一个运行的xx.exe就是一个进程。

#### 线程
线程是进程中的一个执行单元，负责当前进程中程序的执行，一个进程中至少有一个线程。一个进程中的多个线程共享进程中的全部资源。

#### 区别
1. 进程是资源分配的基本单位，线程是CPU调度的基本单位。
2. 进程有自己独立的地址空间，每启动一个进程，系统就会为它分配地址空间，建立数据表来维护代码段、堆栈段和数据段，线程没有独立的地址空间，它使用相同的地址空间共享数据。


### 多线程的优点

* **提高CPU利用率**  
  多线程可以充分利用多核处理器的能力，使不同的线程在不同的核心上并行运行，从而提高了CPU的利用率。
* **响应性**  
  对于需要保持用户界面响应的应用（如图形用户界面应用程序），多线程可以让一部分线程处理用户输入，而其他线程进行后台计算或其他任务，这样可以确保应用程序对用户的操作及时作出反应。
* **简化程序结构**  
  通过将复杂任务分解成更小、更易于管理的线程，可以简化程序的设计和实现。
* **资源共享**  
  同一进程内的所有线程共享内存空间和其他资源，这使得线程之间的通信和数据交换变得简单高效。

## Java多线程

### java实现多线程的原理

多线程通过java中的java.long.Thread类来体现  

通过Thread子类的run方法来执行线程的任务，通过调用Thread子类的start方法来启动线程

### 创建java线程

#### 通过继承Thread类创建线程
```java 
//创建继承Thread类的子类
public class ThreadTest extends Thread{
    @Override
    public void run() {
        //线程任务
    }
}
```
```java
//使用start方法启动线程
public class Main{
  public static void main(String[] args) {
    ThreadTest threadTest = new ThreadTest();
    threadTest.start();
  }
}
```
* run方法不会启动线程，不会分配新的分支栈
* start方法可以用于启动一个分支线程，启动成功的线程会自动调用run方法，实现其中的线程任务
* start方法调用后并非立即执行多线程代码，而是使其变为可执行态，具体执行时间由操作系统决定
* 多线程程序乱序执行，执行顺序随机
* start方法重复调用会出现java.lang.lllegaIThreadStateException异常

##### thread类常用方法

* start()：启动线程，调用线程的run方法
* run()：线程执行体，用于定义线程的任务
* currentThread()：静态方法，返回对当前正在执行的线程对象的引用
* getName()：获取当前线程的名字
* setName()：设置当前线程的名字
* yield()：释放当前CPU的执行权
* join()：等待该线程终止
* isAlive()：判断当前线程是否存活
* sleep(long millis)：让当前线程“睡眠”millis毫秒
* setPriority(int newPriority)：设置线程的优先级
* getPriority()：获取线程的优先级
* setDaemon(boolean on)：将该线程标记为守护线程或用户线程
* isDaemon()：判断该线程是否为守护线程
* stop()：强制停止线程（已废弃）

#### 通过实现Runnable接口创建线程

