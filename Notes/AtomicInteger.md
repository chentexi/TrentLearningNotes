# 什么是AtomicInteger

AtomicInteger类是系统底层保护的int类型，通过提供执行方法的控制进行值的原子操作。AtomicInteger它不能当作Integer来使用

从JAVA 1.5开始，AtomicInteger 属于java.util.concurrent.atomic 包下的一个类。

## 创建AtomicInteger 设置值获取值

AtomicInteger通过调用构造函数可以直接创建。在AtomicInteger提供了两种方法来获取和设置它的实例的值



```csharp
//初始值是 0
AtomicInteger atomicInteger = new AtomicInteger(); 
 
//初始值是 100
AtomicInteger atomicInteger = new AtomicInteger(100);
 
int currentValue = atomicInteger.get();         //100
 
atomicInteger.set(1234);                        //当前值1234
```

## 什么情况下使用AtomicInteger

在现实生活中，我们需要AtomicInteger两种情况：

> 1、作为多个线程同时使用的原子计数器。
>  2、在比较和交换操作中实现非阻塞算法。

### AtomicInteger作为原子计数器**

 要将它用作计数器，AtomicIntegerclass提供了一些以原子方式执行加法和减法操作的方法。



```undefined
addAndGet()- 以原子方式将给定值添加到当前值，并在添加后返回新值。
getAndAdd() - 以原子方式将给定值添加到当前值并返回旧值。
incrementAndGet()- 以原子方式将当前值递增1并在递增后返回新值。它相当于i ++操作。
getAndIncrement() - 以原子方式递增当前值并返回旧值。它相当于++ i操作。
decrementAndGet()- 原子地将当前值减1并在减量后返回新值。它等同于i-操作。
getAndDecrement() - 以原子方式递减当前值并返回旧值。它相当于-i操作。
```



```csharp
public class Main{
    public static void main(String[] args)
    {
        AtomicInteger atomicInteger = new AtomicInteger(100);
         
        System.out.println(atomicInteger.addAndGet(2));         //102
        System.out.println(atomicInteger);                      //102
         
        System.out.println(atomicInteger.getAndAdd(2));         //102
        System.out.println(atomicInteger);                      //104
         
        System.out.println(atomicInteger.incrementAndGet());    //105  
        System.out.println(atomicInteger);                      //105  
                 
        System.out.println(atomicInteger.getAndIncrement());    //105
        System.out.println(atomicInteger);                      //106
         
        System.out.println(atomicInteger.decrementAndGet());    //105
        System.out.println(atomicInteger);                      //105
         
        System.out.println(atomicInteger.getAndDecrement());    //105
        System.out.println(atomicInteger);                      //104
    }
}
```

### 比较和交换操作

 1、比较和交换操作将内存位置的内容与给定值进行比较，并且只有它们相同时，才将该内存位置的内容修改为给定的新值。这是作为单个原子操作完成的。
 2、原子性保证了新值是根据最新信息计算出来的; 如果在此期间该值已被另一个线程更新，则写入将失败。
 为了支持比较和交换操作，此类提供了一种方法，如果将该值原子地设置为给定的更新值current value == the expected value。



```java
boolean compareAndSet(int expect, int update)
```

我们可以compareAndSet()在Java并发集合类中看到amy实时使用方法ConcurrentHashMap。



```csharp
import java.util.concurrent.atomic.AtomicInteger;
 
public class Main{
    public static void main(String[] args){
        //1、默认初始值
        AtomicInteger atomicInteger = new AtomicInteger(100);
        //2、默认初始值和给定值，都是100，所以会更改成功
        boolean isSuccess = atomicInteger.compareAndSet(100,110);   //current value 100
        //3、返回true
        System.out.println(isSuccess);      //true
        //4、默认初始值是11,给定值是100，所以会更改失败
        isSuccess = atomicInteger.compareAndSet(100,120);       //current value 110
        //5、返回false
        System.out.println(isSuccess);      //false
         
    }
}
```

程序输出



```bash
true
false
```

## 总结

如上所述，主要用途AtomicInteger是当我们处于多线程上下文时，我们需要在不使用关键字的情况下对值执行原子操作。intsynchronized

AtomicInteger与使用同步执行相同操作相比，使用它同样更快，更易读。



作者：因为我的心
链接：https://www.jianshu.com/p/073096a729f6
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。