## 什么是可变参数？  
可变参数允许调用参数数量不同的方法。请看下面例子中的求和方法。此方法可以调用1个int参数，或2个int参数，或多个int参数。
```
    public static int sum(int... numbers) {
        int sum = 0;
        for (int num : numbers){
            sum += num;
        }
        return sum;
    }
```
