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

>在 java 中，每次程序运行至少启动 2 个线程。个是 main 线程，一个是垃圾收集线程。

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
```java
//创建实现Runnable接口的类
public class ThreadTest implements Runnable{
    @Override
    public void run() {
        //线程任务
    }
}
```
```java
//使用Thread类来启动线程
public class Main{
  public static void main(String[] args) {
    ThreadTest threadTest = new ThreadTest();//创建实现Runnable接口的类的对象
    Thread thread = new Thread(threadTest);//创建Thread类对象，并将实现Runnable接口的类的对象作为参数传入
    thread.start();//启动线程
  }
}
```
##### 比起继承Thread类实现多线程的好处
* 避免了Java单继承的局限性
* 适合多个相同程序的代码去处理同一个资源的情况，把线程同程序的代码、数据有效分离，较好的体现了面向对象的思想
* 线程池只能放进实现Runnable或Callable类线程，不能放进继承Thread类创建的线程

### 线程的生命周期
* **新建 (New)**  
新建一个线程对象，调用start()方法之前，线程处于新建状态。。
* **就绪 (Runnable)**  
当start()方法被调用后，线程进入就绪状态。此时线程位于可运行的线程池中，等待获取cpu使用权
* **运行 (Running)**  
就绪状态的线程获取cpu资源，开始执行代码
* **阻塞 (Blocked)**  
线程因某种原因放弃cpu资源，暂时停止运行。等待线程进入就绪状态后才有机会进入运行状态
* **死亡 (Dead)**  
线程执行完毕或因异常退出run()方法，线程结束生命周期

#### 阻塞
阻塞的情况分为三种：
1. 等待阻塞：运行中的线程执行wait()方法，JVM会把该线程放入等待池中。（wait方法会释放持有的锁）
2. 同步阻塞：运行中的线程执行同步方法，锁被其他线程占用，JVM会把该线程放入锁池中。
3. 其他阻塞：运行中的线程执行sleep()或join()方法，或者发出了I/O请求时，JVM会把该线程置为阻塞状态。当sleep()状态超时、join()等待线程终止或者超时、或者I/O处理完毕时，线程重新转入就绪状态。(sleep不会释放持有的锁)

### 线程调度

#### 线程优先级

* Java中的线程优先级范围从`MAX_PRIORITY = 1`到`MIN_PRIORITY = 10`，默认为`NORM_PRIORITY = 5`。
* 线程优先级高的线程分配CPU时间片的概率会更高，但不代表一定优先执行。
* 使用`setPriority()`和`getPriority()`方法可以设置和获取线程的优先级。

#### 线程睡眠、等待与唤醒

* `Thread.sleep(long millis)`让线程进入休眠（阻塞状态）放弃占有cpu时间片，让给其他线程使用(不释放锁)
* sleep方法可以做到间隔特定的时间去执行代码，每隔多久执行一次

- `wait()`方法让线程无限期等待(释放当前线程的锁)
- `notify()`方法唤醒一个正在等待的线程(不释放当前线程的锁)
- `notifyAll()`唤醒此对象监视器上所有等待的线程

#### 线程礼让

* `Thread.yield()`让当前线程让出cpu时间片，与其他线程一起重新竞争cpu时间片
* yield方法不会阻塞线程，只是让线程从运行状态转为就绪状态，让cpu重新调度线程

> sleep阻塞的时间是固定的，而yield方法让出的时间无法设定

#### 线程加入

* `join()`让一个线程等待另一个线程执行完毕后再继续执行
* `join()`方法要再start()方法调用后才能生效

#### 线程同步

多个线程对同一个内存地址进行写入时，由于cpu时间调度上的问题，写入数据会被多次覆盖，可能会导致数据不一致或未定义行为。
因此需要通过同步机制确保同一时间只有一个线程能进行修改操作，以维护数据的一致性和完整性。

>线程同步的同是指协同运作而非同时运作

##### 互斥锁
* 如果对一个变量加上互斥锁，那么在同一时刻，该变量只能有一个线程能访问，即当一个线程访问临界资源时，其他线程只能等待。
* 在 Java 中，每一个对象都有一个锁标记（ monitor ），也被称为监视器，当多个线程访问对象时，只有获取了对象的锁才能访问。

##### 使用synchronized实现同步互斥访问
* synchornized代码块方式
```java
    public void myThread1(Thread thread) {
        synchronized (this) {
            for (int i = 0; i < 5; i++) {
                System.out.println(thread.getName() + ":" + i);
            }
        }
    }
```
此方法中synchronized修饰一个代码块，锁定当前对象实例

* 在实例方法中使用synchornized
```java
public synchronized void myThread2(Thread thread) {
    for (int i = 0; i < 5; i++) {
        System.out.println(thread.getName() + ":" + i);
    }
}
```
在此方法中synchronized修饰整个方法，锁定当前对象实例

* 在静态方法中使用synchornized
```java
public static synchronized void myThread3(Thread thread) {
    for (int i = 0; i < 5; i++) {
        System.out.println(thread.getName() + ":" + i);
    }
}
```
在此方法中synchronized修饰整个方法，锁定类本身，

如果一个方法或者代码块被 `synchronized` 关键字修饰，当线程获取到该方法或代码块的锁，其他线程是不能继续访问该方法或代码块的。而其他线程想访问该方法或代码块，就必须要等待获取到锁的线程释放这个锁。在这里，释放锁只有两种情况：

- **线程执行完代码块**：线程完成同步代码块或方法中的所有指令后，会自动释放锁。
- **程序报错**：如果在执行过程中遇到未处理的异常，JVM 会让线程自动释放它所持有的锁。

