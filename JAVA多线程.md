# JAVA多线程

###### 1. 创建方式

 1. 继承Thread类

    ​	本质也是实现Runnable接口，启动线程的唯一方法就是通过Thread类的start()方法。

    start()方法是一个native方法，它将启动一个新线程，并执行run()方法。

    ```java
    public class MyThread extends Thread {
    	public void run() {
    		System.out.println("hello world");
    	}
    }
    ```

    2. 实现Runnable接口

       ```java
       public class MyThread implements Runnable {
       	public void run() {
       		System.out.println("hello world");
       	}
       }
       // 启动
       // MyThread myThread = new MyThread();
       // Thread thread = new Thread(myThread);
       // thread.run();
       ```

    3. 匿名内部类

       ```java
       Thread thread = new Thread(() -> {
       	System.out.println("hello world");
       });
       thread.run();
       ```

    4. ExecutorService,Callable<Class>.Future有返回值线程

       ​	有返回值的任务必须实现Callable接口，没有返回值的任务必须实现Runnable接口。只想Callable任务后，可以获取一个Future对象，在该对象上调用get就可以获取到Callable任务返回的Object。结合ExecutorService就可以实现有返回结果的多线程。

       ```java
       ExcutorService pool = Excutors.newFixedThreadPool(taskSize);
       List<Future> list = new ArrayList<Tuture>();
       for (int i = 0; i < taskSize; i++) {
       	Callable c = new MyCallable(i + "");
       	// 执行任务返回Future对象
       	Future f = pool.submit(c);
       	list.add(f)
       }
       // 关闭线程池
       pool.shutdown();
       ```

###### 2.线程池

| 线程池                  | 详情                                                         |
| :---------------------- | ------------------------------------------------------------ |
| newCachedThreadPool     | 创建一个可根据需要创建新线程的线程池，之前构造的线程可以重用。对于短期异步任务而言可提高程序性能。 |
| newfixedThreadPool      | 创建一个可以重用固定线程数的线程池，以共享无界队列方式来运行这些线程。当所有线程都在运行时，任务会在队列中等待，如果在线程关闭前由于失败而导致线程终止，那么一个新线程将代替它执行后续任务（如果必要） |
| newScheduledThreadPool  | 创建一个线程池，它可安排给定延迟后运行命令或定期执行。       |
| newSingleThreadExecutor | 只有一个线程，这个线程死后重启一个线程代替原来的线程继续执行任务 |

###### 3. 线程生命周期

​	新建 New， 就绪（Runnable）， 运行（Running），阻塞（Blocked）和死亡（Dead）五种状态。

 1. 新建状态

    当程序使用new关键字创建一个线程之后，该线程就处于新建状态，此时仅由JVM为其分配内存，并初始化其成员变量的值。

2. 就绪状态

   当线程对象调用start()方法之后，该线程处于就绪状态。java虚拟机会为其创建方法调用栈和程序计数器，等待调度运行。

3. 运行状态

   就绪状态的线程获得了Cpu,就开始执行run()方法的线程执行体。

4. 阻塞状态

   阻塞状态指线程因某种原因放弃了CPU的使用权，暂时停止运行。直到线程进入到可运行状态，才有机会再次获取Cpu转到运行状态。阻塞分为三种情况：

    1. 等待阻塞

       ​	运行的线程执行wait()方法，JVM会把该线程放入等待队列中。

    2. 同步阻塞

       ​	运行的线程在获取对象同步锁时，若该同步锁被其他线程占用，则JVM会把该线程放入锁池中。

    3. 其他阻塞

       ​	运行的线程执行Sleep()或join()方法，或发出了I/O请求，JVM会把该线程置为阻塞状态。当sleep()超时，join()等待线程终止或超时，I/O请求处理完毕，线程进入运行状态。

5. 死亡

   ​	run()方法正常结束，线程捕获到了异常，调用stop()方法（容易导致死锁，不推荐）。

###### 4. sleep和wait区别

​	对于sleep()方法，我们首先要知道该方法属于Thread类中的，而wait()方法，属于Object类中的。

​	sleep()方法导致了程序暂停执行指定时间，让出cpu但是他的监控状态依然保持。当指定时间到了又会恢复到运行状态。

​	在调用sleep()方法中，不会释放对象锁。

​	调用wait()方法，线程会放弃对象锁。进入等待此对象的等待锁定池，只有针对此对象的调用notify()方法才准备获取对象锁进入运行状态。

###### 5. start和run区别

​	start()是真正启动线程，实现多线程的运行，此时无需等待run方法体执行完毕。

​	调用start启动线程，此时线程处于就绪态，并未运行。

​	run()成为线程体，它包含了执行的线程内容，线程就进入了运行状态。

###### 6. Java锁

 1. 乐观锁

    乐观锁是一种乐观思想，认为读多写少，遇到并发写的可能性低，每次拿数据别人都不会修改，所以不上锁，但是在更新的时候会判断一下别人是否更新这个数据，如果有更新则失败。乐观锁基本都是CAS操作实现的，CAS是一种更新的原子操作，比较当前值与传入值是否一样，一样则更新，否则失败。

 2. 悲观锁

    悲观锁就是悲观思想，认为写多，遇到并发写的可能性高，每次拿数据都会认为别人会修改，所以每次读写数据都会上锁，这样别人想写这个数据就会block直到拿到锁。synchonized就是悲观锁，AQS框架下先尝试cas乐观锁，获取不到，才使用悲观锁，如RetreenLock。

 3. 自旋锁

    ​	如果持有锁的线程能在短时间内始方锁资源，那么哪些等待竞争锁的线程就不需要做内核态和用户态之间的切换进入阻塞挂起状态，只需等一等，持有锁的线程释放锁后立即获取锁，避免用户线程和内核切换的消耗。

    ​	自旋锁尽可能的减少线程阻塞，对于竞争不激烈且占用锁时间非常短的代码块来说性能大幅提升，因为自旋的消耗小于线程阻塞挂起再唤醒的操作的消耗。

    ​	但锁的竞争激烈，或持有锁的线程需要长时间占用锁执行同步块，这时就不适合自旋锁了，因为在获取锁之前都在占用cpu做无用功，同时有大量线程竞争一个锁，会导致获取锁时间很长，线程自选的消耗大于线程阻塞挂起操作的消耗。

 4. Synchronized同步锁

    synchronized可以把任意一个非NULL的对象当作锁。属于独占式悲观锁，同时属于可重用锁。

    Synchronized作用范围

    	1. 作用于方法是，锁住的是对象实例（this）
     	2. 作用于静态方法，锁住的是Class实例，相当于类的一个全局锁，会锁所有调用该方法的线程。
     	3. 作用于一个对象实例。锁住的是所有以该对象为锁的代码块

 5. 

    


