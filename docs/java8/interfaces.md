# 接口的默认方法(Default Methods for Interfaces)

Java 8使我们能够通过使用 default 关键字向接口添加非抽象方法实现。 此功能也称为虚拟扩展方法。
```java
public interface Formula {

	double calculate(int a);

	default double sqrt(int a) {
		return Math.sqrt(a);
	}

	public static void main(String[] args) {
		Formula formula = new Formula() {
			@Override
			public double calculate(int a) {
				return sqrt(a * 100);
			}
		};

		System.out.println(formula.calculate(100)); // 100.0
		System.out.println(formula.sqrt(16));
	}

}
```

# Lambda表达式(Lambda expressions)





























