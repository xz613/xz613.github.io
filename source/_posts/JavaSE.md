---
title: JavaSE
date: 2025-03-11 18:48:31
tags:
---

### 第一章：Java语言概述

​	**软件开发**

​		软件:按照特定顺序组织的计算机数据和指令的集合。

​	**常见的DOS命令**:

​		dir:累出当前目录下的文件以及文件夹

​		md:创建目录

​		rd:删除目录

​		cd:进入指定目录

​		del:删除文件

​		exit:退出dos命令行

​	**Java语言特性**

- 面向对象
- 健壮的
- 跨平台性



### 第二章：变量与运算符

​	**关键字**

​	定义：被java语言赋予了特殊含义，用做专门用途的字符串

​	特点:   都为小写

​	地址：[Java Language Keywords (The Java™ Tutorials > Learning the Java Language > Language Basics)](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/_keywords.html)



​	**标识符**:

​	定义:对各种变量，方法和类等要素的命名使用的字符序列称为标识符

​	凡是可以自己起名字的地方都叫标识符



​	**Java中的名称命名规范;**

- 包名:	            多单词组成时所有字母都小写;xxxyyyzzz
- 类名，接口名：   多单词组成时，所有的首字母大写; XxxYyyZzz
- 变量名、方法名：多单词组成时，第一个单词首字母小写，第二个单词开始每个单词首字母大写;  xxxYyyZzz
- 常量名:                  所有字母都大写。多单词每个单词用下划线连接；XXX_YYY_ZZZ



​	**变量的分类**

```
变量
├── 基本数据类型（Primitive Type）
	数值型
│   ├── byte
│   ├── short
│   ├── int
│   ├── long
│   ├── float
│   ├── double
	字符型
│   ├── char
	布尔型
│   └── boolean
│
└── 引用数据类型（Reference Type）
    ├── 类（Class）
    │   └── String, MyClass, ...
    ├── 接口（Interface）
    │   └── List, Runnable, ...
    ├── 数组（Array）
    │   └── int[], String[], ...
    └── 枚举（Enum）
        └── Day, Month, ...

```



​	**强制类型转换**

​	大 --> 小 : 使用强转符号（）;可能会造成精度降低或溢出.





### 第三章：流程控制语句

**基本流程结构**

- 顺序结构
- 分支结构

1. if...else
2. switch-case

- 循环结构

1. while
2. do...while...
3. for



### 第五章：数组

​	**数组的概念:**多个相同数据类型按一定顺序排列的集合。



​	**一维数组的声明方式:**

​	type var[] 或 type[] var；

​	例如：

​		int a[];

​		int[] a1;

​		double b[];

​		String[] c; //引用类型变量数组

​	5. **多维数组**

​		Java支持多维数组，最常见的是二维数组。数组的维度可以是任意的，但通常二维数组		用于表示矩阵或表格。

​	**二维数组的声明与初始化：**

```
java

int[][] matrix = new int[3][3];  // 创建一个3x3的二维数组
```

- **初始化并赋值**：

```
java

int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

**访问二维数组的元素：**

```
java

System.out.println(matrix[0][0]);  // 输出 1
System.out.println(matrix[2][2]);  // 输出 9
```



**数组中涉及的常见算法**

1. 数组元素的赋值(杨辉三角、回形数等)
2. 求数值型数组中的最大值，最小值，平均数，总和等
3. 数组的复制、反转、查找(线性查找，二分查找)
4. 数组元素的排序算法



### 第六章：面向对象(基础)

​	**面向对象的三大特征:**    封装、继承、多态

​	**面向对像的思想概述:**

​		类、和对象 是面向对象的核心概念

- ​	类、是对一类事物的描述，是抽象的，概念上的定义
- ​        对象、是实际存在的该类事务的每个个体，因而也称为实例。



### 第七章：面向对象(进阶)

​	**方法的重写:**

​		在子类中可以根据需要对从父类中继承来的方法进行改造，也称

​		为方法的重置、覆盖。在程序执行时，子类的方法将覆盖父类的方法。



​	**四种访问权限修饰符:**

**1、public**

- 可见性：最宽泛的访问控制修饰符，表示该成员可以被任何其他类访问，不受任何限制。
- 适用范围
    - 对类、方法、字段都有效。
    - 如果一个类被声明为`public`，则该类可以被任何其他类访问。

**2、protected**

- **可见性**：表示该成员可以被同一个包中的类访问，或者被任何继承该类的子类访问（无论子类是否在同一包中）。
- 适用范围:
    - 对字段、方法、构造函数有效。
    - 只能被同一个包中的类或子类访问。

在这个例子中，`name`和`speak()`被声明为`protected`，可以被同一个包中的类以及继承`Animal`类的子类访问。

**3、private**

- **可见性**：最严格的访问控制修饰符，表示该成员只能在当前类内部访问，不能被其他类访问。
- 适用范围:
    - 对字段、方法、构造函数有效。
    - 只能在定义该成员的类内部访问。

在这个例子中，`name`和`setName()`方法被声明为`private`，只能在`Person`类内部访问。外部类无法直接访问这些成员。

**4、缺省**

- **可见性**：如果没有指定访问修饰符，默认使用包私有访问权限。也就是说，成员仅对同一包中的其他类可见，其他包中的类不能访问。

- 适用范围：

    - 对字段、方法、构造函数有效。

    - 仅在同一个包内的类可以访问，其他包中的类无法访问。



**对象的多态性：**

​	父类的引用指向子类的对象。

​	格式:

```
	父类类型 变量名 = 子类对象
	
	Person p = new Student();
```



### 第八章：面向对象(高级)

​	1、**基本数据类型、包装类 和 String 类型的转换：**

​	（1）、基本数据类型、包装类   ==>   String 类型：调用String的重载的静态方法 ---  valueof(xxx); （2）、基本数据类型的变量 +  ""

```
		int i1 = 10;
        String str1 = String.valueOf(i1);
        System.out.print(str1);
        
        String str2 = i1 + "";
```

​	 (2)、 String 类型  ==>  基本数据类型, 调用包装类的parseXXX();

```
		String str3 = "123";
        int i2 = Integer.parseInt(str3);
        System.out.println(i2);
        
```



​	**static关键字:**

​	在Java类中，可用static修饰属性、方法、代码块、内部类

​	被修饰后的成员具备以下特点:

- ​		随着类的加载而加载


- ​		优先于对象存在


- ​		修饰的成员，被所有对象所共享


- ​		访问权限允许时，可不创建对象，直接被类调用



​	**抽象类和抽象方法**

- ​	**抽象方法：**只有方法签名，没有方法体的，我们把没有方法体的方法称为抽象方法；

- ​       **抽象类：**表述不清楚的类，被abstract修饰的类

  注意： 抽象类不一定包含抽象方法，包含抽象方法的类一定是抽象类。



​	**接口Interface**

- **定义：**一种特殊的抽象类型，定义的了一组方法的声明，但没有实现。



​	**接口和抽象类的区别：**

1. ​	接口中的方法是抽象的（除了默认方法），抽象类中的方法可以是抽象方法和已经实现的方法。
2. ​         一个类可以实现多个接口，只能继承一个抽象类。
3. ​         接口不能有构造方法。



​	**内部类**

1. 成员内部类

2. 局部内部类

3. 匿名内部类

   ```
   class OuterClass {
       public void display() {
           // 使用匿名内部类实现 Runnable 接口
           Runnable runnable = new Runnable() {
               @Override
               public void run() {
                   System.out.println("Running in an anonymous inner class.");
               }
           };
           runnable.run();  // 输出: Running in an anonymous inner class.
       }
   }
   
   public class Main {
       public static void main(String[] args) {
           OuterClass outer = new OuterClass();
           outer.display();
       }
   }
   ```

   #### 说明：

    - 匿名内部类没有类名，直接在创建对象时定义并实例化。
    - 在上面的例子中，匿名内部类实现了 `Runnable` 接口，重写了 `run()` 方法。
    - 匿名内部类通常用于那些只需要一次使用的类，通常用于事件监听器、回调函数等。



4. 静态内部类

​		静态内部类可以访问外部类的静态成员，但不能访问外部类的实例成员。



### 第九章：异常处理

​	**异常**

​	1、什么是异常?

​			程序执行过程中，出现的非正常情况。

​	2、异常的抛出机制

​			在程序中不同的异常用不同的类表示，当出现异常时，就创建该类的实例对象，并且抛出(throw)。然后程序员就会捕捉(cathe)到这个异常对象,并处理；如果没有捕获到这个异常对象，那么程序即将终止。

​	3、异常的体系结构

​			java.lang.Throwable：异常体系的根父类

​					java.lang.Error ： JVM不能恢复的严重问题,如内存溢出等。

​					java.lang .Exception: 程序可以处理的异常。



```
Throwable
├── Error
└── Exception
    ├── IOException (Checked Exception)
    └── RuntimeException (Unchecked Exception)
        ├── NullPointerException
        ├── ArrayIndexOutOfBoundsException
        └── ArithmeticException
```

**异常的处理方式之一  try-catch**

```
try {
    // 可能抛出异常的代码
} catch (ExceptionType e) {
    // 处理异常的代码
}

```

**异常处理 finally的使用**

​	在异常的处理中,finally 的代码块总会执行,无论是否发生异常。

​	格式：

```
try {
    // 可能抛出异常的代码
} catch (ExceptionType e) {
    // 处理异常的代码
} finally {
    // 无论是否发生异常，都会执行的代码
}

```

​	特点:

​	（1）、总是执行：无论异常是否发生，都会执行finally块中的代码。

​	（2）、用于清理：通常用于释放资源，或者关闭流。

​	（3）、与return语句结合使用：在try catch块中，如果在有return  语句，finnally 块也会在返回前执行。



**异常的处理方式二   throws**

​	（1）、thorws关键字用于抛出所有可能发生的异常，用逗号将类型隔开。

​	语法：

```
public void someMethod() throws IOException, SQLException {
    // 可能抛出 IOException 或 SQLException 的代码
}

```

​	（2）、子类异常

​	如果一个方法声明抛出一个异常，它的子类也会被视为合法的抛出异常。例如，如果一个方法声明抛出 `IOException`，那么可以抛出 `FileNotFoundException`（`IOException` 的子类）。



**如何自定义异常类**

​	(1)如何自定义异常类

- 通常继承已有的异常体系，通常继承RuntimeExceeption \ Exception
- 申明多个构造器
- 申明一个全局常量，static final long serailVersionUID;





### 第十章：多线程

####  **1、多线程—程序，进程，线程与并发，并行的概念**

- 什么是进程？

  —程序的一次执行过程。或是正在运行中的程序，例如qq，微信等。

  **操作系统调度和分配资源的最小单位。**



- 什么是线程？

  —进程可进一步细化为线程，是程序内部的一条执行路径。

  单线程 ——> 一个进程只支持一个线程

  多线程 ——> 一个进程支持多个线程，一个进程在同一时间并行多个线程。

  线程是cpu调度的最小单位。



- 什么是并行？

  —两个或多个事件同时发生，同一时刻，有两条指令在cpu上共同执行。



- 什么是并发？

  —两个或多个事件在同一时段发生，在一段时间内，有多条指令在cpu上快速的轮换，交替执行，在宏观上造成了并行的效果。



#### **2、多线程，线程的创建方式**

**方式一  继承 Thread类**

​	通过继承thread类，重写run()方法,可以实现自定义线程的创建。通过使用start()方式可以启动自定义的线程.

​	例:

```
class MyThread extends Thread {
    @Override
    public void run() {
        // 线程执行的代码
        System.out.println("Thread is running!");
    }
}

public class ThreadExample {
    public static void main(String[] args) {
        MyThread thread = new MyThread();  // 创建线程对象
        thread.start();  // 启动线程
    }
}

```

解释：

- 继承   thread  类并重写run() 方法，run()方法定义了要实现的任务。
- 使用start() 启动线程，而不是直接调用run(), 否则会在当前线程中执行run()方法，不会创建新线程。



**方式二  实现Runable 接口**

​	通过继承 Runnable 接口,重写 Runnable 的run() 方法,可以创建线程。然后将  Runnable 对象传递给 Thread 类的构造器来启动线程。

```
class MyRunnable implements Runnable {
    @Override
    public void run() {
        // 线程执行的代码
        System.out.println("Thread is running via Runnable!");
    }
}

public class ThreadExample {
    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();  // 创建Runnable对象
        Thread thread = new Thread(myRunnable);    // 将Runnable对象传递给Thread构造器
        thread.start();  // 启动线程
    }
}

```

解释：

- 实现Runnable 接口，重写run()方法，实现线程要执行的任务。
- 将 Runnable 实现类的对象传递给 thread 类的构造方法，创建线程对象。
- 这种方法比继承 `Thread` 更灵活，因为 Java 支持多继承的接口，而继承 `Thread` 类就不能继承其他类。



**方式三、实现 Callable 接口**

- 实现 Callable  接口并重写 call() 方法。Call() 方法有返回值，并且可以抛出异常。
- 通常与 ExecutorService  一起使用，通过线程池来管理线程。

```
import java.util.concurrent.*;

class MyCallable implements Callable<Integer> {
    @Override
    public Integer call() {
        return 123;
    }
}

public class CallableExample {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        ExecutorService executor = Executors.newFixedThreadPool(2);
        MyCallable task = new MyCallable();
        Future<Integer> result = executor.submit(task);
        System.out.println("Result: " + result.get());
        executor.shutdown();
    }
}
```



**方式四、使用线程池(ExecutorService)**

- 线程池是创建和管理线程的最佳方式，他可以高效的复用线程，减少线程的创建和消耗成本。

- 使用ExcutorService( 例如 newFixedThread、newCachedThreadPool)来管理线程池中的线程。

```
ExecutorService executor = Executors.newFixedThreadPool(3);
Runnable task = () -> System.out.println(Thread.currentThread().getName() + " is running");
for (int i = 0; i < 5; i++) {
    executor.submit(task);
}
executor.shutdown();  // 关闭线程池
```



**线程的生命周期:**

1. **新建状态 (New)**：线程对象被创建，但尚未启动。此时，线程处于新建状态。

   ```
   Thread thread = new Thread();  // 线程处于 "新建" 状态
   ```

2. **就绪状态 (Runnable)**：线程准备好执行，但等待系统分配好时间片。

   ```
   Thread thread = new Thread(() -> {
       // 线程执行的任务
   });
   thread.start();  // 线程进入 "就绪" 状态
   ```

3. **运行状态 (Running)**：线程获得 CPU 时间片，正在执行其 `run()` 方法。

   ```
   Thread thread = new Thread(() -> {
       System.out.println("Running thread...");
   });
   thread.start();  // 线程进入 "运行" 状态
   ```

4. **阻塞状态 (Blocked)**：线程因为等待锁或资源，而被阻塞，无法继续执行。

   ```
   synchronized (lock) {
       // 当前线程在等待锁，若锁已被其他线程占用，它将进入 "阻塞" 状态
   }
   ```

5. **等待状态：**线程进入等待状态，等待其他线程的唤醒。

   ```
   synchronized (lock) {
       try {
           lock.wait();  // 当前线程进入 "等待" 状态，直到被唤醒
       } catch (InterruptedException e) {
           e.printStackTrace();
       }
   }
   ```

6. **定时状态:**线程在一定时间内被挂起，直到时间到期或者被唤醒。

   ```
   Thread.sleep(1000);  // 当前线程进入 "定时等待" 状态，等待 1000 毫秒
   ```

7. **死亡状态 (Terminated)**：线程执行完成，进入终止状态，无法再次启动。

   ```
   Thread thread = new Thread(() -> {
       System.out.println("Thread has finished execution.");
   });
   thread.start();  // 执行完后线程进入 "死亡" 状态
   
   ```



**多线程Thread类的常用方法**

1、常用方法：

- start() 	——> 启动线程 ，调用线程 run();

- run()           ——>将线程要中执行的操作写在方法体内。

- currentThread()    ——>获取当前执行代码的线程

- getName()    ——>获取线程名

- setName()    ——> 设置线程名

- sleep(long mills)  ——>  静态方法，可以使当前的线程睡眠指定的毫秒数。

- join()  ——>等待该线程完成，调用该方法的线程会被阻塞，直到目标线程结束。

- yield() ——>让当前线程暂停，主动让出 CPU 时间，允许其他线程执行

- isAlive()  ——>检查线程是否还在运行





#### **3、线程安全问题**

​		原因：当多个线程被创建，且同一时间访问共享资源，就会触发线程的安全问题。

​		解决：添加 同步监视器（锁），那个线程获得了锁，那个线程就能执行需要同步的资源。



**线程的同步方式**(解决线程安全问题的其中一种方式)

**1、使用synchronized关键字**

synchronized 关键字是Java 中最基本的同步机制，他通过加锁来确保在同一时刻只有一个线程可以访问共享资源，从而避免了线程之间的竞争条件。



**1.1同步方法**

通过在方法声明中使用 synchronized 关键字，使得该方法在同一时刻再能被一个线程调用，对于实例方法，加锁的是当前对象实例，对于静态方法，枷锁的是类的Class 对象。

```
class Counter {
    private int count = 0;

    // 同步方法
    public synchronized void increment() {
        count++;
    }

    public synchronized int getCount() {
        return count;
    }
}
```

**说明**：

- 当一个线程调用 `increment()` 方法时，它会自动获得 `Counter` 对象的锁，其他线程只能等待。
- 静态方法：`synchronized` 修饰静态方法时，锁的是类的 Class 对象，所有实例共享这把锁。



**1.2同步代码块**

​	同步代码块是将同步限制在代码块的某一部分。表示对这个区块的资源实行互斥访问。

```
class Counter {
    private int count = 0;

    public void increment() {
        synchronized (this) {  // 对代码块加锁
            count++;
        }
    }

    public int getCount() {
        synchronized (this) {  // 对代码块加锁
            return count;
        }
    }
}
```

- 说明:
    - synchronized(this): 同步代码块通过加锁对象来确保线程安全。
    - 可以使用任何对象作为锁，this是锁的常用选择，也可以使用其他对象作为锁。



**2、Reentranlock类**

​	Reentranlock 是  java.util.concurrent.lock包中的一个类，比synchronized 灵活，它可以更细粒度的提供锁的控制。

```
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Example {
    private final Lock lock = new ReentrantLock();

    public void method() {
        lock.lock();  // 获取锁
        try {
            // 临界区代码
        } finally {
            lock.unlock();  // 确保释放锁
        }
    }
}
```

**特点**：

- `ReentrantLock` 提供了显式的锁获取和释放。

- 可以在 `try-catch` 块中使用，确保锁能够被释放。

- 允许尝试锁定（`tryLock`）和定时锁定（`lockInterruptibly`），这些是 `synchronized` 不具备的功能。



**2.1：尝试锁(trylock)**

```
if (lock.tryLock()) {
    try {
        // 临界区代码
    } finally {
        lock.unlock();
    }
} else {
    // 锁获取失败，可以执行其他操作
}
```

**特点**：`tryLock()` 尝试获取锁，如果获取不到锁则返回 `false`，不会阻塞当前线程。



**3、ReadWriteLock**

ReadWriteLock 是一种更高级的锁，它允许多个线程并发的读取资源，但在写操作时只允许一个线程

访问该资源。ReentranReadWriteLock 是  ReadWriteLock 接口的实现。

```
import java.util.concurrent.locks.ReentrantReadWriteLock;

public class Example {
    private final ReentrantReadWriteLock lock = new ReentrantReadWriteLock();

    public void read() {
        lock.readLock().lock();
        try {
            // 读取操作
        } finally {
            lock.readLock().unlock();
        }
    }

    public void write() {
        lock.writeLock().lock();
        try {
            // 写入操作
        } finally {
            lock.writeLock().unlock();
        }
    }
}

```

**特点**：

- 多个线程可以同时获取读锁，执行读取操作。
- 写锁是互斥的，获取写锁时，其他线程无法获取读锁或写锁。

**适用场景**：当读取操作远远多于写操作时，使用 `ReadWriteLock` 可以提高程序的并发性。





**懒汉式单例模式的实现步骤**

1. 私有构造函数：防止外部直接创造实例
2. 静态私有变量:  用于存储单例对象
3. 公共静态方法:  提供单例对象的全局访问点，并在首次调用时候初始化实例。

```
public class Singleton {
    // 静态私有变量，用于保存单例实例
    private static Singleton instance;

    // 私有构造函数，防止外部创建实例
    private Singleton() {}

    // 公共静态方法，提供访问单例实例的全局访问点
    public static Singleton getInstance() {
        // 首次调用时创建实例
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

```





死锁的必要条件

1. **互斥条件**: 至少有一个资源处于非共享模式，即这个资源只能别一个线程持有。
2. **保持并等待条件**：一个线程持有一个资源，同时等待其他资源。
3. **不抢占条件**：已经分配给线程的资源，在未完成使用之前，不能被其他线程抢占。
4. **循环等待条件**：存在一线程资源循环等待的关系，即A线程等待B线程的资源，B线程等待C线程的资源，以此类推，形成一个等待闭环的关系。



#### **4、线程之间的通信**

1. wait():   使当前线程进入等待状态，同时，释放锁。
2. notify(): 唤醒wait()优先级最高的线程，优先级相同则随机唤醒，被唤醒的线程从wait()的地方继续往下执行。
3. notifyAll();执行此方法，会唤醒所有正在等待的线程。

**注意：**这三个方法的使用，必须在同步代码块 或 同步方法中。



### 第十一章: 常用类与基础API

**字符串的的不可变性：一但创建了字符串对象，其内容就不能被改变。**

```
String str1 = "Hello";
String str2 = str1.concat(", World!");

// str1 仍然是 "Hello"
// str2 是一个新的字符串 "Hello, World!"
```



p143：

```
String str = new String("helloworld!");   //在内存中创建了几个对象，
```

一个是在堆空间中 new 的对象。 另一个是在字符串常量池中生命的字面量。



**String 和 StringBuilder、StringBuffer 的区别**

- String ：不可变，一但创建，内容无法更改，本身是线程安全的，适用于少量字符串操作内容固定的场景。
- StringBuilder ： 可变，允许在原有的对象上进行修改，而不创建新的对象。不是线程安全的，适合单线程使用。使用于频繁的字符操作，如在循环内拼接字      符。
- StringBuffer: 也可变，但是他是线程安全的。性能相对StringBuilder较低，因为它采用了同步机制。适合多线程环境。



**String常用的API**

1. boolean isEmpty():	 字符串是否为空

2. int  length():                      字符串长度

3. String concat(xx):             拼接

4. boolean equals(Object obj)    比较字符串是否相等，区分大小写

5. boolean equalsIgnoreCase(Object obj)   比较字符串是否相等，不区分大小写

   。。。



p147

**JDK8之前**

- **java.util.Date** :  表示特定的时间，精确到毫秒。
- **java.util.Calendar** ： Calendar类是一个抽象类，提供了更多功能来处理时间和日期，例如设置字段，计算时间等。通常通过Calendar.getInstance方法来获取一个Calendar对象.
- **SimpleDateFormat**: 是一个用来格式化和解析时间的类。



p148

**JDK8中新引入的**

java8中引入了新的时间API,主要存放在java.time包下

- **LocalDate**：表示不带时区的日期（年、月、日）。

- **LocalTime**：表示不带时区的时间（小时、分钟、秒）。

- **LocalDateTime**：表示不带时区的日期和时间。

- **ZonedDateTime**：表示带时区的日期和时间。

- **Instant**：表示一个时间戳，精确到纳秒。

- **Duration**：表示两个时间点之间的时间长度。

- **Period**：表示两个日期之间的日期差异。



p149

**Comparator接口实现自然排序**

1. 首先自定义一个实现comparator接口实现的类，然后重写compare方法定义排序逻辑
2. 使用 Arrays.sort(),结合自定义的comparator来排序集合或数组。



p150

Comparable实现定制排序

1. 创建类并实现comparable接口
2. 创建CompareTo方法，定义比较逻辑
3. 创建对象并添加到集合
4. 使用 collections.sort()  排序



### 第十二章: 集合框架

#### 1、集合的框架体系

Java集合框架体系(java.util包下)

```
java.util.Collection :存储单列数据
	├── 子接口: list： 存储有序的，可重复的数据
    		├── ArrayList、LinkList、Vector
    ├──子接口: set 存储无序的，不可重复的数据
    		├──HashSet、LinkHashSet、TreeSet
java.util.map 存储双列数据，保存具有映射关系的(key-value对)的集合
	├── HashMap、LinkHashMap、TreeMap
       
```

**Collection接口及常用方法**

- JDKb不提供此接口的仍和直接实现，而是提供更集体的子接口(如:Set 和 List)

- **常用方法：**
    1. add(E obj): 	添加元素到当前集合
    2. addAll(Collection other):    添加other 集合中的所有元素到当前集合中
    3. int size() :           获取当前集合中下实际存储的元素个数
    4. boolean isEmpty():          判断当前集合是否是空集合
    5. boolean contains(Object obj):      判段当前集合中是否有一个与obj对象 equals返回 true 的元素。
    6. boolean containsAll(Collection coll): 判断coll集合中的元素是否在当前集合中都存在。
    7. boolean equals(Object obj)  判断当前集合与obj是否相等。
    8. void clear():    清空元素集合
    9. boolean   remove(Object obj)：  从当前集合中删除第一个找到的与obj对象equals返回true的元素。
    10. boolean  removeAll(Collection coll):    从当前集合中删除所有与coll 集合中相同的元素。
    11. boolean  retainAll（Collection coll）:从当前集合中删除两个集合中不同的元素。
    12. Object[] toArray()  返回包含当前集合中所有元素的数组
    13. hasCode()   获取集合对象的哈希值
    14. iterator():    返回迭代器对象，用于集合遍历



**迭代器**

Iterator对象称为迭代器,主要用来遍历 Collection 中的元素。Collection 接口继承了 java.lang.Iterable 接口，该接口有一个iterator()方法,所有继承了Collection接口的类都有一个iterator()方法。

**1.接口:**迭代器通常提供以下几个方法：

- hasNext() : 检查是否还有下一个元素。
- next(): 返回下一个元素
- remove() 删除迭代器返回的最后一个元素。

```
import java.util.ArrayList;
import java.util.Iterator;

public class IteratorExample {
    public static void main(String[] args) {
        ArrayList<String> fruits = new ArrayList<>();
        fruits.add("苹果");
        fruits.add("香蕉");
        fruits.add("橙子");

        // 创建迭代器
        Iterator<String> iterator = fruits.iterator();

        // 使用迭代器遍历集合
        while (iterator.hasNext()) {
            String fruit = iterator.next();
            System.out.println(fruit);
        }
    }
}
```



**foreach循环**

- foreach循环(也叫增强for循环) 是JDK5.0中定义的一个高级的for 循环，专门用来遍历数组和集合的。

```
for (元素类型 变量 : 集合或数组) {
    // 使用变量执行操作
}
```



#### 2、**List接口及其主要实现类:**

- list 是一个有序的集合接口,允许元素重复。

- 主要实现类

1. **ArrayList:**

    - 特点：基于动态数组实现，线程不安全。
    - 适用场景:  当你需要频繁的读取数据，而不关心删除和插入性能时。

   ```
   import java.util.ArrayList;
   import java.util.List;
   
   public class ArrayListExample {
       public static void main(String[] args) {
           List<String> list = new ArrayList<>();
           list.add("Apple");
           list.add("Banana");
           list.add("Cherry");
   
           System.out.println("List: " + list);
           System.out.println("Element at index 1: " + list.get(1));  // 输出 "Banana"
       }
   }
   ```

   **Arraylist  实现线程安全的几种方式。**

   **1）、**使用collections工具类的synchronizedList() 方法。该方法返回一个同步的包装类。所有队列的操作的都会被同步，从而保证线程安全。

   ```
   import java.util.*;
   
   public class ThreadSafeArrayList {
       public static void main(String[] args) {
           List<Integer> list = new ArrayList<>();
           List<Integer> syncList = Collections.synchronizedList(list);
   
           // 通过同步列表进行操作
           syncList.add(1);
           syncList.add(2);
   
           // 使用同步块进行遍历（遍历操作需要额外同步）
           synchronized (syncList) {
               for (Integer i : syncList) {
                   System.out.println(i);
               }
           }
       }
   }
   ```

   **注意：**

   `Collections.synchronizedList()` 返回的列表是线程安全的，但是在遍历时，你仍然需要手动使用 `synchronized` 块来保证操作的线程安全性。因为遍历过程中可能会发生其他线程的修改，导致并发问题。



**2）、使用CopyOnWriteArrayList**, 是包java.util.concurrent 包中一个线程安全的list 实现。使用了 “写时复制“（Copy-On-Write）的策略,在写操作（add(),move()）时会复制底层数组。因此每次写操作都是独立的避免了并发问题。

示例：

   ```
   import java.util.*;
   import java.util.concurrent.*;
   
   public class ThreadSafeArrayList {
       public static void main(String[] args) {
           List<Integer> list = new CopyOnWriteArrayList<>();
   
           // 多线程安全地添加元素
           list.add(1);
           list.add(2);
   
           // 直接进行遍历，无需额外同步
           for (Integer i : list) {
               System.out.println(i);
           }
       }
   }
   ```

**优点：**对读操作非常友好，读取操作不需要同步。

**缺点：**写操作性能较低，每次写操作都会创建一个新的数组。



**3）、使用Vector**,vector 是一个线程安全的动态数组类，它通过对所有的方法加锁来确保线程安全。但由于性能较低，所以不推荐。

   ```
   import java.util.*;
   
   public class ThreadSafeArrayList {
       public static void main(String[] args) {
           List<Integer> list = new Vector<>();
   
           // 线程安全的操作
           list.add(1);
           list.add(2);
   
           for (Integer i : list) {
               System.out.println(i);
           }
       }
   }
   
   ```



4）、**自己通过同步机制实现线程安全。**使用synchronized 关键字同步共享关键部分。

   ```
   import java.util.*;
   
   public class ThreadSafeArrayList {
       private List<Integer> list = new ArrayList<>();
   
       public synchronized void add(Integer value) {
           list.add(value);
       }
   
       public synchronized Integer get(int index) {
           return list.get(index);
       }
   
       public synchronized void remove(int index) {
           list.remove(index);
       }
   
       public synchronized int size() {
           return list.size();
       }
   
       public synchronized void printList() {
           for (Integer i : list) {
               System.out.println(i);
           }
       }
   
       public static void main(String[] args) {
           ThreadSafeArrayList threadSafeList = new ThreadSafeArrayList();
           threadSafeList.add(1);
           threadSafeList.add(2);
           threadSafeList.printList();
       }
   }
   ```



2. **linkedList**

    - 特点: 基于双向链表实现，支持在列表的两端进行高效的插入和删除操作，是**线程不安全**的。
    - 适用场景: 当你需要在列表的两端插入/删除元素时

   ```
   import java.util.LinkedList;
   import java.util.List;
   
   public class LinkedListExample {
       public static void main(String[] args) {
           List<String> list = new LinkedList<>();
           list.add("Apple");
           list.add("Banana");
           list.add("Cherry");
   
           System.out.println("List: " + list);
           list.add(1, "Orange");  // 在索引1处插入元素
           System.out.println("After insertion: " + list);
       }
   }
   
   ```

3. **Vector**

​	基于动态数组实现，类似于Arraylis , linklist  ,不同的是Vector 线程安全的。

```
import java.util.Vector;

public class VectorExample {
    public static void main(String[] args) {
        // 创建一个初始容量为 3 的 Vector
        Vector<String> vector = new Vector<>(3);

        // 添加元素
        vector.add("Java");
        vector.add("Python");
        vector.add("C++");

        // 打印 Vector 内容
        System.out.println("Vector contents: " + vector);

        // 获取元素
        System.out.println("Element at index 1: " + vector.get(1));

        // 修改元素
        vector.set(1, "JavaScript");

        // 删除元素
        vector.remove("C++");

        // 打印修改后的内容
        System.out.println("Updated Vector: " + vector);

        // 确保容量至少为 10
        vector.ensureCapacity(10);

        // 查看 Vector 的大小
        System.out.println("Vector size: " + vector.size());
    }
}

```



#### 3、**Set接口及其主要实现类**

Collection 的子接口，没有提供额外的方法，不允许包含相同的元素，

1. **HashSet**

- 简介：	基于 Hashmap 实现，不保证元素的顺序；线程不安全。
- 特点：        元素的存储不可预测，及插入顺序和取出顺序不一样。
- null:            允许

```
import java.util.HashSet;

public class HashSetExample {
    public static void main(String[] args) {
        HashSet<String> set = new HashSet<>();
        set.add("Java");
        set.add("Python");
        set.add("C++");
        set.add("Java");  // 重复元素，不会添加

        System.out.println(set);  // 输出： [Java, Python, C++]
    }
}
```



**2.LinkedHashSet**

- **简介：**保留元素的插入顺序；线程不安全。

- **底层结构：**Hashmap + 双向链表。
- 性能稍慢，但保持顺序。 
- null ： 允许

```
import java.util.LinkedHashSet;

public class LinkedHashSetExample {
    public static void main(String[] args) {
        LinkedHashSet<String> set = new LinkedHashSet<>();
        set.add("Java");
        set.add("Python");
        set.add("C++");
        set.add("Java");  // 重复元素，不会添加

        System.out.println(set);  // 输出： [Java, Python, C++]
    }
}
```

**3.TreeSet**

- **简介：**元素会自动排序（默认升序）。线程不安全的。

- **底层结构：**红黑树

- **使用场景**:  适用于需要有序元素的去重操作。

- **不能添加 null,** (会 nullPointerException)

```
import java.util.TreeSet;

public class TreeSetExample {
    public static void main(String[] args) {
        TreeSet<Integer> set = new TreeSet<>();
        set.add(5);
        set.add(2);
        set.add(8);
        set.add(1);
        set.add(5);  // 重复元素，不会添加

        System.out.println(set);  // 输出： [1, 2, 5, 8]
    }
}
```

**4、CopyOnWriteArraySet**

- **高并发读**，多线程场景推荐。线程安全的。
- 写操作时复制整个数组（开销大，适合读多写少的场景）。
- 元素唯一性基于equals()。
- null : 允许

```
Set<String> set = new CopyOnWriteArraySet<>();
set.add("A");
set.add("B");
```

常用于缓存、白名单、配置更新等**读多写少**的场景。





**比较与总结**

| **特性**                   | **HashSet**          | **LinkedHashSet**                        | **TreeSet**                  | CopyOnWriteArraySet                   |
| -------------------------- | -------------------- | ---------------------------------------- | ---------------------------- | ------------------------------------- |
| **存储顺序**               | 不保证顺序           | 保证插入顺序                             | 按自然顺序或自定义顺序排序   | 保证插入顺序                          |
| **底层实现**               | 哈希表（`HashMap`）  | 哈希表 + 链表                            | 红黑树                       | 底层封装了一个 `CopyOnWriteArrayList` |
| **是否允许 `null`**        | 允许                 | 允许                                     | 不允许                       | 不能插入，会 nullpointerException     |
| **时间复杂度（平均情况）** | O(1)                 | O(1)                                     | O(log n)                     |                                       |
| **适用场景**               | 无序去重，性能要求高 | 保证插入顺序，较少操作需要按顺序访问元素 | 有序去重，元素需要按顺序排列 | 多线程场景使用。                      |

**总结**

- **`HashSet`**：最常用的实现，性能较好，但不保证元素顺序。
- **`LinkedHashSet`**：在 `HashSet` 基础上保持了元素插入顺序，适合需要顺序的去重操作。
- **`TreeSet`**：提供有序集合，元素按照自然顺序或自定义的比较规则排序，适合需要顺序且去重的场景，但不允许 `null` 元素。



#### **4、Map接口及其主要实现类:**

双列集合，保存具有 key-value  映射关系的集合。

**Map接口常用方法**

(1)、Object put(Object key,Object value):	将指定的key-value 添加到当前map中

(2)、void putAll(Map m):     将m中所有的key_value  存放到当前的map中

(3)、Object remove(Object key):    移除指定的key的key-value 对，并返回value

(4)、void  clear():      清空当前map中所有的数据。

(5)、Object get(Object key):   获取指定key对应的value

。。。



**1）、HashMap**

- **实现原理**：JDK1.7： 数组 + 链表，JDK1.8 ：数组+ 链表 + 红黑树。是线程不安全的。

- **应用场景**：适合需要快速查找，插入和删除，通常应用于大多数map普通的应用场景。

- **null:**   允许一个 null key，多个 null value。

```
import java.util.HashMap;
import java.util.Map;

public class HashMapExample {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("apple", 3);
        map.put("banana", 2);
        map.put("orange", 4);

        System.out.println(map.get("banana"));  // 输出：2
        System.out.println(map.containsKey("apple"));  // 输出：true
    }
}
```

面试题：**hashmap  的 put 流程：**

(1)、**计算键的哈希值 (hash code)**：通过ke.hashCode() 获取哈希值。

(2)、**定位桶位置**: 通过哈希值定位桶的位置,**得到bucketIndex**。

(3)、**处理冲突**:

- 如果桶为空，直接插入。
- 如果桶已经有元素，检查键是否存在于该桶中：
    - 如果存在，更新并返回旧值。
    - 如果不存在，将新键值对插入链表(或红黑树中)。

(4)、**检查负载因子**：如果size 超过 负载因子 * 容量，触发扩容。

(5)、**返回旧值**: 如果是更新操作，返回旧值；如果是插入新元素 返回Null。

**示例：**

```
public V put(K key, V value) {
    // 1. 计算键的哈希值
    int hash = hash(key);
    // 2. 计算桶索引
    int bucketIndex = (hash & (table.length - 1));

    // 3. 获取桶中的元素
    Node<K,V> head = table[bucketIndex];
    
    // 4. 如果桶为空，直接插入
    if (head == null) {
        table[bucketIndex] = new Node<>(hash, key, value, null);
        size++;
        return null;
    }

    // 5. 处理哈希冲突（链表法）
    Node<K,V> current = head;
    while (current != null) {
        if (current.hash == hash && (current.key.equals(key))) {
            // 更新值，返回旧值
            V oldValue = current.value;
            current.value = value;
            return oldValue;
        }
        current = current.next;
    }

    // 如果没有找到相同的键，插入新的键值对
    table[bucketIndex] = new Node<>(hash, key, value, head);
    size++;

    // 6. 检查负载因子，是否需要扩容
    if (size >= threshold) {
        resize();
    }
    
    return null;
}
```

**hashmap：相关面试题**

- **HashMap怎么查找元素的呢？**

**1）、计算哈希值：**通过hashCode() 计算键的哈希值。

**2）、计算索引：**通过哈希值和容量计算出数组的索引位置。

**3）、查找桶：**查看该索引中的桶，如果为空，则查找失败。

**4）、处理碰撞：**如果桶中有多个元素，逐个对比键。

**5）、返回值：**找到对应的键，返回对应的值；没有找到返回null。



- **为什么哈希/扰动函数能降hash碰撞？**





**2）、LinkedHashMap**

- **实现原理**：LinkedHashMap 继承自HashMap,并且使用**双向链表**维护键值对的插入顺序或访问顺序。也是**线程不安全**的。
- **应用场景**：适合需要保留插入顺序或访问顺序的应用
- **null**：允许一个null key，允许多个null value

```
import java.util.LinkedHashMap;
import java.util.Map;

public class LinkedHashMapExample {
    public static void main(String[] args) {
        Map<String, Integer> map = new LinkedHashMap<>();
        map.put("apple", 3);
        map.put("banana", 2);
        map.put("orange", 4);

        // 输出按插入顺序的键值对
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
    }
}

```

**3）、TreeMap**

- 实现原理：基于红黑树(白平衡二叉查找树)实现的 Map,也是线程不安全的。
- 应用场景:  适合需要按排序顺序访问为元素的场景
- null： 不允许null key 

```
import java.util.Map;
import java.util.TreeMap;

public class TreeMapExample {
    public static void main(String[] args) {
        Map<String, Integer> map = new TreeMap<>();
        map.put("apple", 3);
        map.put("banana", 2);
        map.put("orange", 4);

        // 输出按键的字典顺序排序的键值对
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
    }
}
```

**4）、HashTable**

- 实现原理：是早期的哈希表实现，类似于Hashmap,但是它是**线程安全的**。所有的方法都被同步。
- 应用场景:主要用于一些老旧的应用。
- null ： 不允许

```
import java.util.Hashtable;
import java.util.Map;

public class HashtableExample {
    public static void main(String[] args) {
        Map<String, Integer> map = new Hashtable<>();
        map.put("apple", 3);
        map.put("banana", 2);
        map.put("orange", 4);

        System.out.println(map.get("banana"));  // 输出：2
    }
}
```

**总结**

| 类名                | 描述                                                        | 线程安全性 | 排序            | 用途                                                         |
| ------------------- | ----------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------------------ |
| `HashMap`           | 基于哈希表的 `Map` 实现，支持 `null` 键和值                 | 非线程安全 | 无序            | 普通的 `Map` 操作，适合快速查找、插入和删除                  |
| `LinkedHashMap`     | `HashMap` 的子类，保持插入顺序或访问顺序                    | 非线程安全 | 按插入/访问顺序 | 保留插入顺序或访问顺序，适合缓存实现等                       |
| `TreeMap`           | 基于红黑树的 `Map` 实现，按键的自然顺序或自定义排序顺序排列 | 非线程安全 | 有序            | 需要排序的 `Map`，如字典、范围查找等                         |
| `Hashtable`         | 早期的哈希表实现，所有方法都被同步                          | 线程安全   | 无序            | 老旧的线程安全 `Map`，性能较差，现多用 `ConcurrentHashMap` 替代 |
| `ConcurrentHashMap` | 线程安全的哈希表实现，采用分段锁保证并发性能                | 线程安全   | 无序            | 高并发环境下的线程安全 `Map`，适用于多线程共享数据等场景     |

- 对于 **普通用途**，一般使用 `HashMap`。

- 如果需要 **保持插入顺序** 或 **访问顺序**，使用 `LinkedHashMap`。

- 如果需要 **按键排序**，使用 `TreeMap`。

- 如果需要 **线程安全**，推荐使用 `ConcurrentHashMap`（而不是 `Hashtable`）



**Collections工具类**

collections工具类中提供了一系列静态的方法对集合元素进行排序，查询和修改等操作。





p155

**list接口常用方法的测试**

增:

​	add(Object   obj);

​	addAll(Collection coll );

删：

​	remove(Object obj);

​	remove(int index)；

改：

​	set(int Index,Object obj);

查：

​	get(int Index)；

插入:

​	add(int Index,Object ele);

​	addAll(int Index,Collection eles)

长度;

​	size()

遍历:

​	iterator():  使用迭代器遍历;

​	增强for循环

​	一般 for() 循环



p156

```
java.util.Collection
		|---子接口 List 存储有序的,可重复的数据。("动态"数组)
			|---ArrayList: List 的主要实现类,线程不安全的，效率高。底层使用object[]存储.
			|---Vector: list 的古老实现类,线程安全的，效率低。底层使用object[]存储.
			|---LinkList: 底层使用双向链表的方式进行存储。
		
```

**集合排序的实现方案:**

1. **使用Comparable 接口排序**

   实现Comparable接口，重写comparTo()方法 。CompareTo()的返回值是:

   **负数**：当前对象小于 `o`。

   **0**：当前对象等于 `o`。

   **正数**：当前对象大于 `o`。

当一个类实现了Comparable 接口后，使用Collection.sort() 或 Arrays.sort() 就可以直接进行排序。

```
import java.util.*;

class Person implements Comparable<Person> {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public int getAge() {
        return age;
    }

    @Override
    public int compareTo(Person other) {
        return Integer.compare(this.age, other.age);  // 按年龄升序排列
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }

    public static void main(String[] args) {
        List<Person> people = new ArrayList<>();
        people.add(new Person("Alice", 30));
        people.add(new Person("Bob", 25));
        people.add(new Person("Charlie", 35));

        // 使用 Collections.sort() 排序
        Collections.sort(people);

        System.out.println("Sorted by age: " + people);
    }
}
```

**2.使用Comparator接口排序**

Comparator接口提供了一个方法

```
int compare(T o1, T o2);
```

- 如果返回负数，表示 `o1` 小于 `o2`；

- 如果返回 0，表示 `o1` 等于 `o2`；

- 如果返回正数，表示 `o1` 大于 `o2`。

通常我们在自定义的Comparator类中实现compare方法,并将其传递给排序方法( Collections.sort() 或者 Arrays.sort() )进行排序。

```
import java.util.*;

class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return name + " (" + age + ")";
    }

    public static void main(String[] args) {
        List<Person> people = new ArrayList<>();
        people.add(new Person("Alice", 30));
        people.add(new Person("Bob", 25));
        people.add(new Person("Charlie", 35));

        // 按年龄升序排序
        Comparator<Person> ageComparator = new Comparator<Person>() {
            @Override
            public int compare(Person p1, Person p2) {
                return Integer.compare(p1.getAge(), p2.getAge());
            }
        };
        Collections.sort(people, ageComparator);
        System.out.println("Sorted by age: " + people);

        // 按名字字母升序排序
        Comparator<Person> nameComparator = new Comparator<Person>() {
            @Override
            public int compare(Person p1, Person p2) {
                return p1.getName().compareTo(p2.getName());
            }
        };
        Collections.sort(people, nameComparator);
        System.out.println("Sorted by name: " + people);
    }
}
```





### 第十三章: 泛型

**1、泛型的概念**

泛型是一种设计概念,允许你编写时可以处理多种数据类型的代码。它使得类，函数，接口可以使用类型参数，从而在运行时确定具体的数据类型，而不在编写时候就确定具体的数据类型。

例如：

```
public class Box<T> {
    private T item;

    public void setItem(T item) {
        this.item = item;
    }

    public T getItem() {
        return item;
    }
}
```

在这个例子中，`Box`类使用了一个类型参数`T`，这意味着你可以创建一个`Box`对象来存储任意类型的对象，比如`Box<Integer>`或`Box<String>`。



**2、为什么要有泛型**

解决获取元素时，需要做类型转换的问题，例如 在集合中有泛型时，只有指定的元素类型才能添加到集合中。保证了类型安全。



**3、自定义泛型结构**

3.1常用范型结构自定义

3.1 .1自定义泛型类

```
public class GenericBox<T> {
    private T item;
	
    public T getItem() {
        return item;
    }

    public void setItem(T item) {
        this.item = item;
    }

}
```

3.1.2 自定义泛型方法

```
public class GenericMethodExample {
    // 泛型方法
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.println(element);
        }
    }

    public static void main(String[] args) {
        Integer[] intArray = {1, 2, 3, 4, 5};
        String[] strArray = {"Hello", "World"};

        System.out.println("Integer Array:");
        printArray(intArray); // 调用泛型方法

        System.out.println("String Array:");
        printArray(strArray); // 调用泛型方法
    }
}
```

3.1.3 自定义多参数泛型类

```
public class GenericPair<K, V> {
    private K key;
    private V value;

    public GenericPair(K key, V value) {
        this.key = key;
        this.value = value;
    }

}
```

3.1.4 自定义泛型接口

```
// 定义一个泛型接口
public interface GenericInterface<T> {
    void performAction(T item);
}

// 实现该接口
public class StringAction implements GenericInterface<String> {
    @Override
    public void performAction(String item) {
        System.out.println("Processing string: " + item);
    }
}
```



### 第十五章：File类与IO流

在 Java 中，**`File` 类**和**I/O 流（Input/Output Streams）**是处理文件和数据输入输出的重要工具。`File` 类用于文件和目录的操作，而 I/O 流用于读写文件内容以及在程序和外部设备之间传输数据。

**1. `File` 类**

`File` 类是位于 `java.io` 包中的一个重要类，它提供了一些方法来操作文件和目录。通过 `File` 类，可以进行以下操作：

- **创建、删除文件和目录**
- **检查文件或目录的属性**
- **获取文件的基本信息（如路径、大小、修改日期等）**
- **列出目录中的文件**



**1.1 创建 `File` 对象**

`File` 类的构造方法主要有两种形式：

- `File(String pathname)`：通过路径字符串创建 `File` 对象。
- `File(String parent, String child)`：通过父目录路径和子文件名创建 `File` 对象。
- `File(File parent, String child)`：通过父 `File` 对象和子文件名创建 `File` 对象。

```
java复制代码import java.io.File;

public class FileExample {
    public static void main(String[] args) {
        // 创建 File 对象
        File file = new File("example.txt");

        // 判断文件是否存在
        if (file.exists()) {
            System.out.println("File exists.");
        } else {
            System.out.println("File does not exist.");
        }
    }
}
```

**1.2 文件的常见操作**

- **判断文件是否存在**

  ```
  file.exists(); // 如果文件存在返回 true，否则返回 false
  ```

- **创建文件**

  ```
  file.createNewFile();  // 如果文件不存在，创建文件，返回 true；如果已存在，返回 false
  
  ```

- **删除文件**

  ```
  file.delete();  // 删除文件，成功返回 true，失败返回 false
  ```

- **判断是否是目录或文件**

  ```
  java复制代码file.isDirectory();  // 判断是否是目录
  file.isFile();       			// 判断是否是文件
  ```

- **获取文件或目录的路径**

  ```
  java复制代码file.getAbsolutePath();  // 获取文件的绝对路径
  file.getPath();          // 获取文件的相对路径
  file.getName();          // 获取文件名
  ```

- **列出目录中的文件**

  ```
  java复制代码File dir = new File("someDirectory");
  String[] files = dir.list(); // 返回目录中的文件和子目录的文件名
  File[] fileList = dir.listFiles(); // 返回目录中的文件和子目录的 File 对象
  ```

- **获取文件大小和修改时间**

  ```
  java复制代码long fileSize = file.length();  // 获取文件的大小（字节）
  long lastModified = file.lastModified();  // 获取文件的最后修改时间
  ```



2. **I/O 流**

Java 提供了两类 I/O 流：

1. **字节流（Byte Stream）**：用于处理所有类型的 I/O（包括二进制数据，如图片、音频文件等），每次读取或写入一个字节。
2. **字符流（Character Stream）**：专门用于处理文本数据（字符），每次读取或写入一个字符。

I/O 流有两个主要的子类：

- **输入流（InputStream）** 和 **输出流（OutputStream）**：字节流
- **Reader** 和 **Writer**：字符流



**3. 字节流（Byte Streams）**

字节流可以处理所有类型的 I/O 数据，包括字符数据、二进制数据等。字节流的根类是 `InputStream`（输入流）和 `OutputStream`（输出流）。



**3.1 `InputStream` 类及其常用子类**

- **`FileInputStream`**：用于从文件中读取字节数据。
- **`BufferedInputStream`**：提供缓冲功能，提高读操作效率。

```
java复制代码import java.io.FileInputStream;
import java.io.IOException;

public class FileInputStreamExample {
    public static void main(String[] args) {
        try (FileInputStream fis = new FileInputStream("example.txt")) {
            int data;
            while ((data = fis.read()) != -1) {
                System.out.print((char) data);  // 打印文件内容
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



**3.2 `OutputStream` 类及其常用子类**

- **`FileOutputStream`**：用于向文件中写入字节数据。
- **`BufferedOutputStream`**：提供缓冲功能，提高写操作效率。

```
java复制代码import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamExample {
    public static void main(String[] args) {
        try (FileOutputStream fos = new FileOutputStream("example.txt")) {
            fos.write("Hello, Java!".getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



4. **字符流（Character Streams）**

字符流用于处理文本数据（字符）。字符流的根类是 `Reader` 和 `Writer`，它们处理的是字符，而非字节。



**4.1 `Reader` 类及其常用子类**

- **`FileReader`**：用于从文件中读取字符数据。
- **`BufferedReader`**：提供缓冲功能，常用于读取字符文件和处理文本行。

```
java复制代码import java.io.FileReader;
import java.io.BufferedReader;
import java.io.IOException;

public class FileReaderExample {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("example.txt"))) {
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);  // 打印文件的每一行
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



**4.2 `Writer` 类及其常用子类**

- **`FileWriter`**：用于向文件写入字符数据。
- **`BufferedWriter`**：提供缓冲功能，提高写入效率。

```
java复制代码import java.io.FileWriter;
import java.io.BufferedWriter;
import java.io.IOException;

public class FileWriterExample {
    public static void main(String[] args) {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter("example.txt"))) {
            bw.write("Hello, World!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



5. **标准输入输出**

Java 提供了对标准输入输出的支持，即控制台的输入输出。

- **`System.in`**：标准输入流，通常用作接收用户输入。
- **`System.out`**：标准输出流，通常用于打印输出到控制台。



**5.1 使用 `System.in` 读取输入**

```
java复制代码import java.io.IOException;

public class ConsoleInputExample {
    public static void main(String[] args) throws IOException {
        System.out.println("Enter a character: ");
        int data = System.in.read();  // 读取一个字符
        System.out.println("You entered: " + (char) data);
    }
}
```



**5.2 使用 `System.out` 输出数据**

```
java复制代码public class ConsoleOutputExample {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```



**6. I/O 缓冲流（Buffered Streams）**

`BufferedReader` 和 `BufferedWriter` 是对字符流的增强，它们通过缓冲区来提高 I/O 操作的效率。在字节流中，`BufferedInputStream` 和 `BufferedOutputStream` 扮演类似角色。



**6.1 缓冲字符流示例**

```
java复制代码import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class BufferedReaderExample {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("example.txt"))) {
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```



7. **文件复制**

下面是一个常见的文件复制操作示例，利用字节流和缓冲流。

```
java复制代码import java.io.*;

public class FileCopyExample {
    public static void main(String[] args) {
        try (FileInputStream fis = new FileInputStream("source.txt");
             FileOutputStream fos = new FileOutputStream("destination.txt");
             BufferedInputStream bis = new BufferedInputStream(fis);
             BufferedOutputStream bos = new BufferedOutputStream(fos)) {
             
            int byteData;
            while ((byteData = bis.read()) != -1) {
                bos.write(byteData);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

8. **总结**

- **`File` 类**：用于文件和目录的操作





### 第十七章：反射

**1、反射的概念:**

​	在类加载完后，会在堆内存中生成一个 Class 对象，这个对象包含了类所有的结构信息。这个对象就像是一个镜子，通过这个镜子可以看到类的所有信息。所以，我们形象的称之为反射。



**2、Class类**

​	在 Object 中定义了一个方法，public final  class getClass();  这个方法将会被所有的子类继承。这个方法的返回值是一个Class类。



**3、Object类，Class类、普通类的关系图：**

```
                           +-------------------+
                           |       Object      |  (所有类的祖先类)
                           +-------------------+
                             |               |
       +---------------------+               +---------------------+
       |                                                       |
  +------------+                                           +-------------+
  |   Class    |  (表示类的类)                            |    MyClass   |  (普通类)
  +------------+                                           +-------------+
       |                                                       |
       +----------------------------+--------------------------+
                                    |
                               +------------+
                               |   Class<?> |  (描述具体类的 Class 对象)
                               +------------+

```



（1）、**`Object`类** 是所有Java类的祖先类（根类）。任何一个普通的Java类（例如 `MyClass`）都默认继承自`Object`类（除非它显式继		承其他类）。`Object`类提供了很多核心方法，比如 `toString()`、`equals()`、`hashCode()` 等，所有类都可以继承这些方		法。



（2）、**`Class`类** 是所有类的元数据类，它本身也是一个普通的Java类，继承自`Object`类。**每一个Java类都有一个与之相关联的`Class`		对象。**通过`Class`对象，我们可以在运行时查询类的信息（如类的方法、字段等），并通过反射来动态操作类的实例。



（3）、**`MyClass`（普通类）** 是一个开发人员定义的普通类。它继承自`Object`类（隐式），并且在JVM中有一个对应的`Class`对象。通		过反射机制，可以通过`Class`类的API来操作`MyClass`的实例。



（4）、**`Class<?>`对象** 对应于类的元数据。每个类（如`MyClass`）都与一个`Class`对象相关联，`Class`对象存储了类的结构信息（如构		造方法、字段、方法等）。反射通过`Class`对象获取类的相关信息，并能够动态地创建类的实例或调用类的方法。



4、获取 Class 实例对象的四种方式

```
//1、调用运行时类的静态属性
 Class cls1 = Person.class;
 System.out.println(cls1);
 
 //2、调用运行时对象的 getClass
 Person p2 = new Person();
 Class cls2 = p2.getClass();

 //3、调用 Class 的静态方法  forName;(常用)
 String className = new String("com.atguigu");
 Class cls3 = Class.forName(className);

//4、使用类的加载器的方式(了解)
Class cls4 = ClassLoader.getSystemClassLoader().loadClass(com.atguigu);
```



**5、反射的基本应用：**

有了class 对象，可以做什么？



**5.1、应用一	 ——— 	创建运行时类的对象**

- **方式一：直接调用 Class 的 newInstance();**

  要 求： 1）类必须有一个无参数的构造器。2）类的构造器的访问权限需要足够。

- **方式2：通过获取构造器对象来进行实例化**



**5.2、应用二	——— 	获取运行时类的完整结构**

可以获取：包、修饰符、类型名、父类（包括泛型父类）、父接口（包括泛型父接口）、成员（属性、构造器、方法）、注解（类上的、方法上的、属性上的）。



**5.3、应用三	———	调用运行时类的指定结构**

(1)、获取该类型的 Class 对象

Class cls = Class.forName();

(2)、获取方法对象

Method met = class.getDeclareMethod("方法名，"方法的形参类型列表);

(3)、创建实例对象

object obj = clazz.newInstance();

(4)、调用方法

Object result =method.invoke(obj,方法的实参列表);



**5.4、应用四	———	读取注解信息**

一个完整的注解因该包含三个部分：(1)、声明	(2)、使用	(3)、读取





#### 常见面试题解答:

1. 为什么需要克隆？如何实现对象的克隆? 深浅拷贝的区别？

   **克隆:克隆是指创建一个对象的副本，通常通过复制一个对象的属性来生成一个新的对象，新的对象和原对象是两个不同的实例。(1)通过对对象的克隆,可以实现数据的备份；通过克隆对现象而不是新建对象，可以提高性能。(2)使用Oject的clone()方法，同时还要实现cloneable接口。(3)浅拷贝只能备份基本数据类型，引用数据类型克隆的仍然是地址，修改克隆对象的引应用数据类型会影响到原对象;深拷贝则是完整的拷贝原对象，修改引用数据的成员不会影响到原对象。需要重写clone()方法。**
