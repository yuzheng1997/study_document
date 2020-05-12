# java内部类

###### 1.什么是内部类

​	java类中不仅可以定义变量和方法，还 可以定义类，这样定义在内部的类就称作内部类。根据定义的方式不同，内部类可以分为静态内部类，成员内部类，局部内部类，匿名内部类。

###### 2.静态内部类

```java
public calss Out {
	private static int i;
	private int b;
	public static class Inner {
		public void say() {
			System.out.println(a);
		}
	}
}
```

	1.  静态内部类可以访问外部类所有静态变量和方法
 	2.  静态内部类和一般类一致，可以定义静态变量，方法和构造方法
 	3.  其他类使用静态内部类是要使用  外部类.静态内部类  的方式

###### 3. 成员内部类

定义在类内部的非静态类，就是成员内部类。成员内部类不能定义静态方法和变量（final修改的变量除外）。因为成员内部类是非静态的，类初始化的时候先初始化静态成员，如果允许成员内部类定义静态变量，那么成员内部类的静态变量初始化顺序有歧义。

###### 4. 局部内部类

​	定义在方法中的类，就是局部类。如果某个类只有方法中使用，就可以考虑局部类.

```java
 public class Out {
 	private static int a;
	private int b;
 	public void test(final int c) {
 		final int d = 1;
 		class Inner {
 			public void print() {
 			System.out.println(c);
 			}
 		}
 	}
 }
```



###### 5. 匿名类

​	匿名内部类我们必须要继承一个弗雷或者实现一个接口。