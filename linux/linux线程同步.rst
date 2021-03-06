+++++++++++
Linux线程同步方式
+++++++++++
线程最大的特点就是资源共享性，但是资源共享中的同步问题是多线程编程的难点。
linux下提供了多种方式来处理线程同步，最常用的是互斥锁、条件变量和信号量

1. 互斥锁
************

1. 初始化锁 

在linux下线程的互斥量数据类型是pthread_mutex_t。在使用前，要对它进行初始化。

静态分配： ``pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;``  

动态分配：``int pthread_mutex_init(pthread_mutex_t *mutex, const pthread_mutex_atrr_t *mutexattr)``

2. 加锁 

对共享资源的访问，要对互斥量进行加锁，如果互斥量已经上锁，调用线程会阻塞，直到互斥量被解锁

``int pthread_mutex_lock(pthread_mutex *mutex);``   

``int pthread_mutex_trylock(pthread_mutex_t *mutex);``  
    
3. 解锁

完成了对共享资源的访问后，要对互斥量进行解锁


``int pthread_mutex_unlock(pthread_mutex_t *mutex);``

4. 销毁锁 

锁在是使用完成后，需要进行销毁以释放资源。

``int pthread_mutex_destroy(pthread_mutex *mutex);``

2. 条件变量
***************

与互斥锁不同，条件变量是用来等待而不是用来上锁的。条件变量是用来自动阻塞一个线程，知道某特殊情况发生为止。通常条件变量和互斥锁同时使用。
条件变量分为两部分: **条件** 和 **变量** 。条件本身是有互斥量保护的。线程在改变条件状态前先要锁住互斥量。条件变量是我们可以睡眠等待某种条件出现。
条件变量是利用线程共享的全局变量进行同步的一种机制。主要包括两个动作：

+ 一个线程等待 **条件变量的条件成立** 而挂起 

+ 另一个线程使 **条件成立** 

条件的检测是在互斥锁的保护下进行的。如果一个条件为假，一个线程自动阻塞，并释放等待状态改变的互斥锁。
如果另一个线程改变了条件，他信号给关联的条件变量，唤醒一个或多个等待他的线程。重新获得互斥锁,重新评价条件。如果两进程共享可读写内存。条件变量
可以被用来实现这两进程间的线程同步。

1. 初始化条件变量

静态态初始化: ``pthread_cond_t cond = PTHREAD_COND_INITIALIER;`` 

动态初始化: ``int pthread_cond_init(pthread_cond_t *cond, pthread_condattr_t *cond_attr);`` 

2. 等待条件成立。

释放锁,同时阻塞等待条件变量为真才行。timewait()设置等待时间,仍未signal,返回ETIMEOUT(加锁保证只有一个线程wait)

``int pthread_cond_wait(pthread_cond_t *cond, pthread_mutex_t *mutex);``

``int pthread_cond_timewait(pthread_cond_t *cond,pthread_mutex *mutex,const timespec *abstime);``


+ 激活条件变量。``pthread_cond_signal,pthread_cond_broadcast`` （激活所有等待线程）

``int pthread_cond_signal(pthread_cond_t *cond);``

``int pthread_cond_broadcast(pthread_cond_t *cond);`` //解除所有线程的阻塞

+ 清除条件变量。无线程等待,否则返回EBUSY

``int pthread_cond_destroy(pthread_cond_t *cond);``

3. 信号量
**************
如同进程一样，线程也可以通过信号量实现通信。虽然是轻量级的。信号量行数的名字都以 ``sem_`` 打头、线程使用的基本信号量函数有四个。
1. 信号量初始化

``int sem_init(sem_t *sem. int pshared, unsigned int value);``    

这是对在sem指定的信号量进行初始化，设置好他的共享选项，然后给他一个初始值VALUE 

2. 等待信号量

给信号量减去1，然后等待知道信号量的值大于0；

``int sem_wait(sem_t sem);``

3. 释放信号量。
信号量值加1。并通知其他等待线程。

``int sem_post(sem_t *sem);``

4. 销毁信号量。
我们用完信号量后都它进行清理。归还占有的一切资源。

``int sem_destroy(sem_t *sem);``
 
