## IMP和SEL

在 oc 中 Method 是含有至少两个参数 (id self, SEL selector) 的 C 函数，中 self 为对象， selecotr 表示方法的 selector，便于查找 IMP， 其实 oc 是做了一层 SEL 与 IMP 的映射。如果 Method 中定义了更多的参数，将会强制放到这两个参数后面，这个函数叫 IMP， 其定义如下

```
typedef id (*IMP)(id, SEL, ...);
```

当一个 c 函数，拥有正确的参数顺序，那么这个 c 函数就可以当做任意的一个已存在或新的方法的 IMP.

看下在的例子

```

if([obj respondsToSelector:NSSelectorFromString(@"xxtest:")]){
        SEL selector = NSSelectorFromString(@"xxtest:");
        IMP imp = [shareData methodForSelector:selector];
        void (*func)(id, SEL, NSString *) = (void *)imp;
        func(obj, selector, parameter);
}
```

我们在 obj 这个对象的类中有一个叫 xxxtest: 的方法，可以是私有的方法，通过这种方式传入参数，调用此方法。

当然如果嫌麻烦就使用

```
if([obj respondsToSelector:NSSelectorFromString(@"xxtest:")]){
        SEL selector = NSSelectorFromString(@"xxtest:");
        [obj performSelector:selector withObject:@""];
}
```

不过这个方法参数受限于 NSObject 类型，也就是 withObject 只能是 id 类型，对于要传 int 还要转一下，也蛋疼，所以不如用第一种方法！
