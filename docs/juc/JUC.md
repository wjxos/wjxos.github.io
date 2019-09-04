# JUC 
1. sale ticket 
```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;
/**
 * @author WangJX
 * @version 1.0.0
 * @ClassName ThreadDemo1.java
 * @Description TODO
 * @createTime 2019年09月04日 21:02:00
 * 1.线程   操作   资源类
 * 2.高内聚   低耦合
 */
class Ticket {
    private int number = 30;
    private Lock lock = new ReentrantLock();

    public void sale(){
        lock.lock();
        try {
            if(number > 0 ) {
                System.out.println(Thread.currentThread().getName() + "\t卖出第:" + (number --) + "\n还剩" + number);
            }
        }catch (Exception e) {
            e.printStackTrace();
        }finally {
            lock.unlock();
        }
    }
}

public class ThreadDemo1 {
    public static void main(String[] args) {
        Ticket ticket = new Ticket();

        new Thread(() -> {
            for (int i = 1; i < 40; i++) {
                ticket.sale();
            }
        }, "AA").start();

        new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 1; i <= 40; i++) {
                    ticket.sale();
                }
            }
        }, "A").start();
    }
}
```
上面这段代码是一个典型的多线程操作资源类的`demo`,在这段代码中其实有三种不同的写法
1. 资源类实现`Runnable`,主线程通过`Thread`启动线程. **缺点:** 代码侵入性强
2. `Ticket`是一个干净的资源类,主线程通过`Thread` 实现匿名的内部类启动线程. **缺点:**代码太长,也不够优雅
3. `Ticket`是一个干净的资源类,主线程通过`Thread` 实现匿名的内部类启动线程. 使用Lanbda表达式**优点:**是一种优雅的实现方式