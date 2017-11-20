参考文章：
[https://juejin.im/entry/587c2c4ab123db005df459a1]()
[http://www.jianshu.com/p/94d61de9e139]()

子线程利用 RunLoop 保活(Demo) [https://juejin.im/entry/58b93b72ac502e006bdf7527]()


RunLoop 就是为线程而生的，线程的作用就是执行一个特定的任务，在默认情况下线程执行完后就不能再次执行任务，这是因为默认情况下线程是没有开启 RunLoop 的（UI 线程是苹果爸爸帮我开启的），但是如果线程开启了 RunLoop 之后，线程执行完，会一直处于等待状态，直到再次接受到任务，继续执行任务，在线程销毁前，会先释放这个线程对应的 RunLoop。

### RunLoop 基本作用

1、保持程序的持续运行，保持线程的持续运行；

2、处理 App 中各种事件（触摸、定时器、selector等）；

3、节省 CPU 资源，提高程序性能，该做事时做事，该休息时休息

### RunLoop 与 线程

1、每条线程都有唯一的一个与之对应的 RunLoop 对象；

2、主线程的 RunLoop 系统已经创建好了，子线程的 RunLoop 需要手动创建；

3、RunLoop 在第一次获取时由系统自动创建，在线程结束时销毁；

4、如果给子线程创建 RunLoop，不能直接 alloc & init，要使用 [NSRunLoop currentRunLoop]; 系统有一个全局的 Dictionary  来存放他们，如果没有找到子线程对应的 RunLoop，他就会创建一个，然后将与之对应的子线程一起存入字典中；