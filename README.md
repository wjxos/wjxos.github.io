# WJXOS 读书笔记
为了记录学习的过程，便于复习，作者整理了markdown语法

## 目录

- [Markdown语法基础](#Markdown语法基础)
    - [Markdown入门体验](#Markdown入门体验)
- [Java](#Java)
    - [基础](#基础)
    - [Java8](#Java8)
    
- [虚拟化技术](#虚拟化技术)
    - [docker](#docker)
    - [kubernetes](#kubernetes)
    
- [SpringBoot与Kubernetes](#SpringBoot与Kubernetes)
    - [SpringBoot与Kubernetes](#docker)
 
 
 
- [SpringBoot与Spring家族](#SpringBoot与Spring家族)
    - [SpringBoot](#SpringBoot) 
    - [SpringCloud](#SpringCloud) 

- [mysql基础篇](docs/mysql/basic.md) 
- [mysql索引篇](docs/mysql/mysql.md)

**Markdown语法基础**

工欲善其事，必先利其器，选择Markdown来做笔记是非常方便的
* [Markdown入门体验](docs/markdown/markdown.md)
 
**Java**

**基础**
```java
java 中的基本数据类型8个
4个整形: byte short int long 
2个浮点型: double float 
1个字符型: char 
1个boolean型: boolean
```
Integer 下面问题的疑问？ 为什么123 == 是true，而 129 == 是false的解释
```java
Integer x = 123;     //调用了Integer.valueOf(123);
Integer y = 123;  //如果数值在[-128,127]之间，便返回指向缓冲池中已经存在的对象的引用；否则创建一个新的Integer对象。
System.out.println(x==y);     //true

Integer x = 129; 
Integer y = 129; 
System.out.println(x==y);     //false

在Integer类中有一段代码如果你看了下面这段代码，那么你对上面的问题没有疑问了

private static class IntegerCache {
    static final int low = -128;
    static final int high;
    static final Integer cache[];

    static {
        // high value may be configured by property
        int h = 127;
        String integerCacheHighPropValue =
            sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
        if (integerCacheHighPropValue != null) {
            try {
                int i = parseInt(integerCacheHighPropValue);
                i = Math.max(i, 127);
                // Maximum array size is Integer.MAX_VALUE
                h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
            } catch( NumberFormatException nfe) {
                // If the property cannot be parsed into an int, ignore it.
            }
        }
        high = h;

        cache = new Integer[(high - low) + 1];
        int j = low;
        for(int k = 0; k < cache.length; k++)
            cache[k] = new Integer(j++);

        // range [-128, 127] must be interned (JLS7 5.1.7)
        assert IntegerCache.high >= 127;
    }

    private IntegerCache() {}
}

//Integer 的valueOf(int i) 方法的实现
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}
```
**Java8**
 * [Java 8 的新特性](docs/java8/interfaces.md)

**虚拟化技术**
* [docker](docs/docker/docker.md)
* [kubernetes](docs/docker/kubernetes.md)

**SpringBoot与Kubernetes**
* [SpringBoot与Kubernetes](docs/springbootAndKubernetes/springbootAndKubernetes.md)

**SpringBoot**
* [SpringBoot](docs/springboot/Springboot.md)

**SpringCloud**
* [SpringCloud](docs/springcloud/springcloud.md)


[学习参考](https://github.com/CyC2018/CS-Notes)

[xxl-job](http://www.xuxueli.com/page/projects.html)

[记一次java程序CPU占用过高问题排查](https://blog.csdn.net/puhaiyang/article/details/78663942)

