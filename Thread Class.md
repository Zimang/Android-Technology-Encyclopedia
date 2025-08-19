两个Thread方法`start`和`run`非常特殊。

start方法不接受任何参数，也不返回任何值。从调用代码的角度来看，它根本不做任何事情。它只是返回并将控制权传递给程序中的下一条语句。**不过，神奇的是，该调用实际上返回两次，一次返回到程序中的下一条语句，另一次返回到由调用生成的新执行线程中的Thread对象的run方法。**

新线程按顺序执行run方法中的语句，直到方法返回。当run方法返回时，新线程被终止。

2.1清单是使用Java Thread的一个简单示例。
```java
public class ThreadedExample {  
    public static void main(String... args) {  
        System.out.println("starting: " + Thread.currentThread());  
        new Thread() {  
            @Override  
            public void run() {  
                System.out.println("running: " + Thread.currentThread());  
            }  
        }.start();  
        System.out.println("finishing: " + Thread.currentThread());  
    }  
}
```
当运行此程序时，将打印三条消息然后终止。它定义了Thread的匿名子类，覆盖了其run方法。覆盖的方法在一个新线程上执行，与调用start方法的线程的执行顺序不同。
程序运行一次的输出示例如下：
```
starting: Thread[main,5,main]
finishing: Thread[main,5,main]
running: Thread[Thread-0,5,main] 
```


请注意，在run方法内部打印的消息验证了新线程的run方法中的当前线程不是执行其他两个打印语句的线程。
这三条消息将以什么顺序出现？“starting”消息将始终首先出现。
Java保证在程序顺序中在调用程序中新线程的start方法之前的任何语句和新线程中的任何语句之间存在happened-before边缘。
换句话说，一切都按照程序顺序发生，直到新线程启动并且新线程看到启动时的世界。