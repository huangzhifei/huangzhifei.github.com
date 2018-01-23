
## 子线程RunLoop

参考文章：
[https://juejin.im/entry/587c2c4ab123db005df459a1]()

子线程利用 RunLoop 保活(Demo) [https://juejin.im/entry/58b93b72ac502e006bdf7527]()


RunLoop 就是为线程而生的，线程的作用就是执行一个特定的任务，在默认情况下线程执行完后就不能再次执行任务，这是因为默认情况下线程是没有开启 RunLoop 的（UI 线程是苹果爸爸帮我开启的），但是如果线程开启了 RunLoop 之后，线程执行完，会一直处于等待状态，直到再次接受到任务，继续执行任务，在线程销毁前，会先释放这个线程对应的 RunLoop。

### RunLoop 基本作用

1、保持程序的持续运行，保持线程的持续运行；

2、处理 App 中各种事件（触摸、定时器、selector等）；

3、节省 CPU 资源，提高程序性能，该做事时做事，该休息时休息

### RunLoop 与 线程

1、每条线程都有唯一的一个与之对应的 RunLoop 对象；

2、主线程的 RunLoop 系统已经创建好了，子线程的 RunLoop 需要手动创建且手动；

3、RunLoop 在第一次获取时由系统自动创建，在线程结束时销毁；

4、如果给子线程创建 RunLoop，不能直接 alloc & init，要使用 [NSRunLoop currentRunLoop]; 系统有一个全局的 Dictionary  
来存放他们，如果没有找到子线程对应的 RunLoop，他就会创建一个，然后将与之对应的子线程一起存入字典中；

5、Mode 中有一些Timer、Source、Observer，这些 Mode 不为空时保证 RunLoop 没有空转并且是在运行的，发 Mode 中有空的时候，RunLoop 会立刻退出。

6、简单的运行逻辑如下图

![](https://huangzhifei.github.com/images/RunLoop.png) 


### RunLoop 常见场景

1、事件响应：
苹果注册了一个 Source1 来接收系统事件，然后把其包装成 UIEvent 进行处理或分发，包括识别 UIGesture、屏幕旋转、发送给 UIWindow 等等。

2、当识别到一个手势时，其首先会调用 Cancel 将当前的 touchesBegin/Move/End系列回调打断，随后系统将对应的 UIGestureRecognizer 标记为待处理。

苹果注册了一个 Observer 监测 BeforeWaiting (Loop即将进入休眠) 事件，这个Observer的回调函数是 _UIGestureRecognizerUpdateObserver()，其内部会获取所有刚被标记为待处理的 GestureRecognizer，并执行GestureRecognizer的回调。
当有 UIGestureRecognizer 的变化(创建/销毁/状态改变)时，这个回调都会进行相应处理。

3、界面刷新时机？
什么时候会真正执行刷新、为什么会刷新不及时？

当在操作 UI 时，比如改变了 Frame、更新了 UIView/CALayer 的层次时，或者手动调用了 UIView/CALayer 的 setNeedsLayout/setNeedsDisplay方法后，这个 UIView/CALayer 就被标记为待处理，并被提交到一个全局的容器去。

苹果注册了一个 Observer 监听 BeforeWaiting(即将进入休眠) 和 Exit (即将退出Loop) 事件，回调去执行一个很长的函数：_ZN2CA11Transaction17observer_callbackEP19__CFRunLoopObservermPv()。这个函数里会遍历所有待处理的 UIView/CAlayer 以执行实际的绘制和调整，并更新 UI 界面。 所以说界面刷新并不一定是在setNeedsLayout相关的代码执行后立刻进行的。

4、自动释放池的创建和销毁时机？
App 启动后，苹果在主线程 RunLoop 里注册了两个 Observer

第一个监视的事件是 Entry（即将进入Loop），其回调内会调用_objc_autoreleasePoolPush() 创建自动释放池，优先级最高，保证创建释放池发生在其他所有回调之前。

第二监视了两个事件：BeforeWaiting(准备进入休眠) 和 Exit(即将退出Loop)，用来释放旧的池并创建新的池和释放自动释放池。

在主线程中执行的代码，都被线程自动创建好的 AutoreleasePool 环绕着，所以一般不会出现内存泄漏。

