# Java



# No.4 object and Class 

## 4.3 用户自定义类

### 4.3.1 实体类

实例域标记应为private

### 4.3.2 构造器

构造器与类同名,在构造类对象时构造器会运行,以便初始化实例域. 构造器伴随new操作符的执行被调用

* 构造器与类同名
* 每个类可以有一个以上的构造器
* 构造器可以有>=0个参数
* 构造器没有返回值
* 构造器伴随new操作一起调用

注意: 不要再构造器里面进行与实例域重名的局部变量


![image-20210526000133301](Image/image-20210526000133301.png)

### 4.3.3隐式参数与显式参数

显式参数，就是平时见到的在方法名括号中间的参数，就是所谓能看得见的参数。

隐式参数，是在类的方法中调用了类的实例域。这个被调用的实例域就是隐式参数。隐式参数被this指定。

显隐式转换:

```java
public class test {
	public static void main(String[] args) {
		int a = 2;
		double b = 1.2;
		// 由a->b, 取值范围小的转向取值范围大的，为隐式转换
		b = a;
		System.out.println(b);
 
		int a1 = 3;
		double b1 = 5.8;
		// 由b->a, 取值范围大的转向取值范围小的，为强制转换（显示转换）
		a1 = (int) b1;
		System.out.println(a1);
	}
}
```



###  4.3.6 封装的优点

封装杜绝了外部类对实例域值得破坏

优点一:对于内部改变不会影响其他得代码

优点二:可以在对实例域值修改可以进行检查或其它操作.而直接改实例域值则不会有这些操作

### 4.3.7 Final实例域

```java
private final String n="";

private final StringBuilder  nn=new StringBuilder("");
```

final关键字在修饰StringBuilder类时:存储在nn变量中的对象引用不会在指向其它的StringBuilder对象.不过该对象可以更改,与变量n不同

## 4.4 静态域/方法

### 4.4.1 静态域

```java
public class Example01_ObjectStatic{
	private int id;
	private static int nextId;
}
```

被static关键字修饰的实例域为静态域

对于Example01_ObjectStatic来说:

​	有1000个Example01_ObjectStatic类的对象,就有1000个实例域id。但是只有一个静态域nextId。即使没有Example01_ObjectStatic类的对象，静态域nextId依然存在，静态域nextId只属于类，不属于对象。

==对于静态与可以被类直接调用==

![image-20210526010141984](Image/image-20210526010141984.png)

### 4.4.2 静态常量

<u>静态变量使用的少当静态常量使用的多</u>

```java
private[public] final static String constant="changliangbukebian";
```

### 4.4.3 静态方法

* 静态方法是没有this参数的方法
* 静态方法内可以访问类中的静态域



在下面两种情况下使用静态方法：

* 一方法不需要访问对象状态，其所需参数都是通过显式参数提供（例如： Math.pow)
* 一个方法只需要访问类的静态域（例如：Employee.getNextId)

### 4.4.4 方法参数

方法参数是指方法得到所有参数值的一个拷贝

方法参数共有两种类型:

* 基本数据类型(数值,布尔)
* 对象引用

## 4.5 对象构造

* 重载:多个方法相同的名字,不同的参数,不同的返回值类型
* 无参数构造器:在类中会默认提供一个无参数构造器,并默认给实例域赋初始值
* 如果构造器的第一个语句形如this(...)， 这个构造器将调用同一个类的另一个构造器
* 初始化数据域的方法：1.在构造器中设置值 2.在声明中赋值 3.初始代码块

调用构造器的具体步骤：

    1. 所有数据域被初始化为默认值（0、false 或null)。
  2. 按照在类声明中出现的次序， 依次执行所有域初始化语句和初始化块。
  3. 如果构造器第一行调用了第二个构造器， 则执行第二个构造器主体
  4. 执行这个构造器的主体

## 4.6 包

### 4.6.1 静态导入

```java
import static chapter_four.Example01_ObjectStatic.ss;
```

使用 import static 可以导入类中的静态方法\静态域,而省去加类名前缀

# 

导读：

​	-类、超类和子类					-参数数量可变的方法

​	-object：所有类的超类			  -枚举类

​	-泛型数组列表					  -反射

​	-对象包装器与自动装箱			   -继承的设计技巧

# No.5 Inherit

## 5.1 类、超类和子类

### 5.1.1 定义子类

使用关键字extend表示继承，继承是表明在子类上继承已知已存在的父类类上。已存在的可以称作超类、基类、父类。新创建的类可以称作子类、派生类、孩子类。

### 5.1.2 覆盖方法

子类继承超类之后可以把超类的方法进行覆盖 

可以使用super来引用超类的方法

```java
  @Override
	public double getSalary(){
		
		super.getSalary();
		return super.getSalary();
	}
```

子类不能直接的初始化超类的私有域，但可以借助super来进行超类的私有域初始化

```java
	public EmployeeTest(String n, double s, LocalDate h){
    
			super(n, s, h);
	}
```

继承层次：

![image-20210602231340478](Image/image-20210602231340478.png)

==多态==：一个对象变量可以指向多个实际类型的现象称为多态

```java
		Object o1=new String();
		Object o2=new StringBuilder();
		Object o3=new StringBuffer();
```



​	



