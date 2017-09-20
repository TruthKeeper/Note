## Java线程池

### 用途

- 重复利用已经创建的线程，降低线程创建和销毁的资源消耗

- 由于不需要重复创建线程，提高了响应速度

- 丰富了线程的可管理性

### 工作原理

```
new Task()↓
if(线程池中的核心线程是否都在处理任务){
    if(线程池中的工作队列是否已满){
        if(线程池中的线程总数是否到达线程池的线程最大容量){
            线程池拒绝执行任务，执行饱和策略
        }else{
            启动一个非核心线程执行任务
        }
    }else{
        将任务插入到任务队列中
    }
}else{
    启动一个核心线程执行任务
}
```

### 重要参数

#### corePoolSize

线程池中核心线程的数量大小

#### maximumPoolSize

线程池中容纳的最大线程数量，即核心线程数量+非核心线程数量

#### keepAliveTime

线程在不执行任务时的存活时间，默认在线程池中线程数量大于核心线程数量时才生效，可以通过`ThreadPoolExecutor#allowCoreThreadTimeOut`方法使**keepAliveTime**对所有线程起作用

#### unit

**keepAliveTime**的时间单位

#### workQueue

用来保存等待执行任务的阻塞队列，有以下默认实现：
- ArrayBlockingQueue 基于数组结构的有界阻塞队列，先进先出的FIFO排序
- LinkedBlockingQueue 基于链表结构的有界阻塞队列，先进先出的FIFO排序，性能比前者高
- PriorityBlockingQueue 可根据优先级的无界阻塞队列，内部通过**Comparator**来实现

#### threadFactory

线程工厂，通过Runnable生成Thread

#### handler

线程池、工作队列满了之后的饱和策略，有以下默认实现：
- AbortPolicy 直接抛出异常，默认的策略，也是Android中**AsyncTask**线程池的饱和策略
- CallerRunsPolicy 只要线程还未关闭，就在调用者的当前线程环境中直接运行此任务
- DiscardPolicy 抛弃这个任务
- DiscardOldesstPolicy 丢弃队列中的第一个任务（也就是即将执行的任务），然后再次提交任务到线程池

