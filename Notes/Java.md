# Java

# No.4 object and Class 

## 用户自定义类

### 实体类

实例域标记应为private

### 构造器

构造器与类同名,在构造类对象时构造器会运行,以便初始化实例域. 构造器伴随new操作符的执行被调用

* 构造器与类同名
* 每个类可以有一个以上的构造器
* 构造器可以有>=0个参数
* 构造器没有返回值
* 构造器伴随new操作一起调用

注意: 不要再构造器里面进行与实例域重名的局部变量


![image-20210526000133301](Image/image-20210526000133301.png)

### 隐式参数与显式参数

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



###  封装的优点

封装杜绝了外部类对实例域值得破坏

优点一:对于内部改变不会影响其他得代码

优点二:可以在对实例域值修改可以进行检查或其它操作.而直接改实例域值则不会有这些操作

### 4.3.7 Final实例域

```java
private final String n="";

private final StringBuilder  nn=new StringBuilder("");
```

final关键字在修饰StringBuilder类时:存储在nn变量中的对象引用不会在指向其它的StringBuilder对象.不过该对象可以更改,与变量n不同

## 静态域/方法

### 静态域

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

### 静态常量

<u>静态变量使用的少当静态常量使用的多</u>

```java
private[public] final static String constant="changliangbukebian";
```

### 静态方法

* 静态方法是没有this参数的方法
* 静态方法内可以访问类中的静态域



在下面两种情况下使用静态方法：

* 一方法不需要访问对象状态，其所需参数都是通过显式参数提供（例如： Math.pow)
* 一个方法只需要访问类的静态域（例如：Employee.getNextId)

### 方法参数

方法参数是指方法得到所有参数值的一个拷贝

方法参数共有两种类型:

* 基本数据类型(数值,布尔)
* 对象引用

## 对象构造

* ==重载==:多个方法相同的名字,不同的参数,不同的返回值类型 <-- -->(==重写==)

* 无参数构造器:在类中会默认提供一个无参数构造器,并默认给实例域赋初始值

  ==(如果没有初始化类中的域， 将会被自动初始化为默认值（ 0、false 或null)==

* 如果构造器的第一个语句形如this(...)， 这个构造器将调用同一个类的另一个构造器

* 初始化数据域的方法：1.在构造器中设置值 2.在声明中赋值 3.初始代码块

调用构造器的具体步骤：

1. 所有数据域被初始化为默认值（0、false 或null)。

  2. 按照在类声明中出现的次序， 依次执行所有域初始化语句和初始化块。
  3. 如果构造器第一行调用了第二个构造器， 则执行第二个构造器主体
  4. 执行这个构造器的主体

## 包

### 静态导入

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

## 类、超类和子类

### 定义子类

使用关键字extend表示继承，继承是表明在子类上继承已知已存在的父类类上。已存在的可以称作超类、基类、父类。新创建的类可以称作子类、派生类、孩子类。

### 覆盖方法

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

#### ==多态==：

一个对象变量可以指向多个实际类型的现象称为多态

```java
		Object o1=new String();
		Object o2=new StringBuilder();
		Object o3=new StringBuffer();
```

### 方法调用过程

1. 编译器查看对象声明的类型和方法名-->
2. 会找出所引用方法名相同的重载方法如果存在参数类型完全匹配的则调用该方法(重载解析)-->
3. 如果是private 方法、static 方法、final 方法或者构造器， 那么编译器将可以准确地知道应该调用哪个方法， 我们将这种调用方式称为==静态绑定==(static binding)[==动态绑定==] 调用的方法依赖于隐式参数的实际类型-->　　
4. 虚拟机通过创建的每个类的方法表进行搜索实现动态绑定市级的调用的方法[==调用super.f(param)==]编译器将对隐式参数超类的方法进行搜索调用

### 阻止继承:final类和方法

string类引用final, ；

```Java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
  
}
```

final修饰的类:此时子类不能在覆盖类中的方法,默认final类中的方法为final方法

​			 ==[不包括类中的实例域]==

final修饰的域:在构造对象之后就不能允许改变他们的值

[final声明方法或类主要是为了在子类中不会改变其语义]

### 强制类型转换

进行类型转换的唯一原因是：在暂时忽视对象的实际类型之后， 使用对象的全部功能。

```java
		Object object = new Object();

		if(object instanceof String){
      
				String d = (String) object;
		}
```

强制转换的要素:

* 只能在继承层次进行类型转换
* 在将超类转成子类之前,使用instanceof进行检测

###  抽象类

#### 特点

* 包含一个以上的抽象方法，类必须声明为抽象类

* 抽象类可以包含具体的域和具体方法[建议把具体方法和通用的域放在超类中]

* 类即使不含抽象方法，也可以将类声明为抽象类

* 声明为抽象类（abstract）的类不能再实例化对象

* 定义一个抽象类只能引用非抽象子类的对象

  ```java
  Person p = new Student("Vinee Vu" , "Economics") ;
  ```

### 受保护访问

1. -->仅对本类可见private。
2. -->对所有类可见public：
3. -->对本包和所有子类可见protected。
4. -->对本包可见—默认（很遗憾)，不需要修饰符。

## Object类

在没有明确指出超类时，则可以把object类认为是这个类的超类

==所有的数组类塱，不管是对象数组还是基本类型的数组都扩展了Object 类。==

```java
Object obj = new EmployeeC'Harry Hacker", 35000) ;
Employee e = (Employee) obj ;
Employee[] staff = new Employee[10];
obj = staff; // OK
obj = new int[10]; // OK
```

### equals

* 判断两个对象是否具有相同的引用

```Java
public class Employee{
  public boolean equals(Object othe「Object)
  {
      // a quick test to see if the objects are identical
      if (this == otherObject) return true;
      // must return false if the explicit parameter is null
      if (otherObject == null ) return false;
      // if the classes don' t match, they can' t be equal
      if (getClassO != otherObject .getClass())
      return false;
      // now we know otherObject is a non-null Employee
      Employee other = (Employee) otherObject;
      // test whether the fields have identical values
      return name.equals(other.name)
      && salary = other , sal ary
      && hi reDay. equals(other, hi reDay) :
  }
}
```

==为了防备name 或hireDay 可能为null 的情况， 需要使用Objects.equals 方法。如果两个参数都为null， Objects.equals(a，b) 调用将返回true ; 如果其中一个参数为null ,则返回false ;==

equals的特性:

1. 自反性：对于任何非空引用x, x.equals(0) 应该返回true
2. 对称性: 对于任何引用x和y, 当且仅当y.equals(x) 返回true, x.equals(y) 也应该返回true。
3. 传递性：对于任何引用x、y 和z, 如果x.equals(y) 返N true， y.equals(z) 返回true,x.equals(z) 也应该返回true。
4. 一致性：如果x 和y 引用的对象没有发生变化， 反复调用x.eqimIS(y) 应该返回同样的结果。
5. 对于任意非空引用x, x.equals(null) 应该返回false,

### hashCode 方法

散列码（ hash code ) 是由对象导出的一个整型值。散列码是没有规律的。

```java
public static void main(String[] args){
		String s =new String();
		String s2 =new String();
		String s3 =new String("dfsf");
		String s4 =new String();
		s="dfsf";
		s2="cccccccccc";
		s4=s3;
		System.out.println(s.hashCode());
		System.out.println(s2.hashCode());
		System.out.println(s3.hashCode());
		System.out.println(s4.hashCode());
	}
```

![image-20210615151531033](Image/image-20210615151531033.png)

在类中重新定义equals则必须重新定义hashcode

还有更好的做法，需要组合多个散列值时， 可以调用ObjeCtS.hash 并提供多个参数。这个方法会对各个参数调用Objects.hashCode， 并组合这些散列值

```java
	@Override
	public int hashCode() {
        return Objects.hash(name,sex,phone);
	}
```

### toString 方法

它用于返回表示对象值的字符串。

```java
return "Example01[name =" +name+", sex="+sex + ", phone="+phone+ "] ";
return getClass().getName()+"[name =" +name+", sex="+sex + ", phone="+phone+ "] ";
```

tostring也可以供子类调用:

```java
public class Manager extends Employee
    public String toStringO
    {
        return super.toString() +"[bonus=" + bonus + "]";
    }
}
```

随处可见toString方法的主要原因是：只要对象与一个字符串通过操作符“ +”连接起来,Java编译就会自动地调用toString 方法，以便获得这个对象的字符串描述。

tostring是非常通用的调试工具

## 泛型数组列表

一旦确定了数组的大小， 改变它就不太容易了。在Java 中， 解决这个问题最简单的方法是使用Java 中另外一个被称为ArrayList 的类。它使用起来有点像数组，但在添加或删除元素时， 具有自动调节数组容量的功能，而不需要为此编写任何代码。

### ArrayList

ArrayList 是一个采用类型参数（ type parameter ) 的泛型类（ generic class )。为了指定数组列表保存的元素对象类型， 需要用一对尖括号将类名括起来加在后面，

```java
private ArrayList<Example01> example01 = new ArrayList<Example01>();
```

Java SE 5.0 以前的版本没有提供泛型类.在老版本中会使用Vector类实现动态数组.不过现在ArrayList类更加有效



### 访问数组列表元素

使用get 和set 方法实现访问或改变数组元素的操作，而不使用人们喜爱的[ ] 语法格式:

```java
example01.set(0,new Example01("gg","hh","48855598566"));

Example01 example01 = this.example01.get(2);
```

```
Example01 example01 = this.example01.get(2);
```

### 类型化与原始数组列表的兼容性

## 对象包装器与自动装箱

对象包装器类是不可变的， 即一旦构造了包装器， 就不允许更改包装在其中的值。同时， 对象包装器类还是final , 因此不能定义它们的子类。

```java
ArrayList<Integer> i=new ArrayList<>();
```

###  自动装箱

```java
i.add(3);

将自动转化为:
i.add(Integer.valueOf(3));
```

### 自动拆箱

```java
int x = i.get(1);

将自动转化为:
int i1 = i.get(1).intValue();
```

## 参数数量可变的方法

```java
public PrintStream printf(String format, Object ... args) {
        return format(format, args);
}

System.out.printf("%d %s",new Object[]{56,"fffff"});
```

. . . 是Java 代码的一部分，它表明这个方法可以接收任意数量的对象

## 枚举类

```java
public enum ii{aaa,bbb,ccc,ddd};
```

实际上， 这个声明定义的类型是一个类， 它刚好有4 个实例， 在此尽量不要构造新对象。如果需要的话， 可以在枚举类型中添加一些构造器、方法和域。当然， 构造器只是在构造枚举常量的时候被调用。下面是一个示例：

```java
	public enum ii{
		aaa("1"),bbb("2"),ccc("3"),ddd("4");
		private String lk;
		
		private ii(String lk){
			this.lk = lk;
		}
		public String getLk(){
			return lk;
		}
		
	}
```

## 反射

使用反射， Java 可以支持Visual Basic 用户习惯使用的工具。特别是在设计或运行中添加新类时， 能够快速地应用开发工具动态地查询新添加类的能力。

能够分析类能力的程序称为反射（ reflective )。反射机制的功能极其强大，在下面可以看到， 反射机制可以用来：

* 在运行时分析类的能力
* 在运行时查看对象， 例如， 编写一个toString 方法供所有类使用
* 实现通用的数组操作代码
* 利用Method 对象， 这个对象很像中的函数指针

###  Class类

在程序运行期间，Java 运行时系统始终为所有的对象维护一个被称为运行时的类型标识。这个信息跟踪着每个对象所属的类。虚拟机利用运行时类型信息选择相应的方法执行。

获得class对象的三种方法:

1.可以通过专门的Java 类访问这些信息。保存这些信息的类被称为Class,

```java
Example01 e;
...
Class aClass = e.getClass();
...
System.out.println(e.getClass() .getName() + " " + e.getNameO) ;
```

2.还可以调用静态方法forName 获得类名对应的Class 对象。

```java
String dassName = "java.util.Random";
Class cl = Class.forName(dassName) ;
```

如果类名保存在字符串中， 并可在运行中改变， 就可以使用这个方法。当然， 这个方法只有在dassName 是类名或接口名时才能够执行。否则， forName 方法将抛出一个checkedexception ( 已检查异常）。无论何时使用这个方法， 都应该提供一个异常处理器（ exception handler ) 

==为解决类加载所带来的时间消耗,可以使用通过调用Class.forName 手工地加载其他的类==

3.如果T 是任意的Java 类型（或void 关键字)，T.class将代表匹配的类对象。

```java
Class c = Random.class;
Class c1 = String.class;
Class c2 = int.class;
```

请注意， 一个Class 对象实际上表示的是一个类型， 而这个类型未必一定是一种类。例如，int 不是类， 但int.class 是一个Class 类型的对象。

------

Class 类实际上是一个泛型类。例如， Employee.class 的类型是Class<Employee>。没有说明这个问题的原因是： 它将已经抽象的概念更加复杂化了。在大多数实际问题中， 可以忽略类型参数， 而使用原始的Class 类。

虚拟机为每个类型管理一个Class 对象。因此， 可以利用=运算符实现两个类对象比较的操作。例如:

```java
if (e.getClass == Employee.class) . . .
```

还有一个很有用的方法newlnstance( )，可以用来动态地创建一个类的实例例如

```java
e.getClass0.newlnstance() ;


try{
    Example01 example011 = e.getClass().newInstance();
}catch( InstantiationException instantiationException ){
    instantiationException.printStackTrace();
}catch( IllegalAccessException illegalAccessException ){
    illegalAccessException.printStackTrace();
}
```

创建了一个与e 具有相同类类型的实例。newlnstance 方法调用默认的构造器（没有参数的构造器）初始化新创建的对象。如果这个类没有默认的构造器， 就会抛出一个异常

------

将forName 与newlnstance 配合起来使用， 可以根据存储在字符串中的类名创建一个对象

```java
try{
    String s ="java.util.Random";
    Object o = Class.forName(s).newInstance();
}catch( InstantiationException instantiationException ){
    instantiationException.printStackTrace();
}catch( IllegalAccessException illegalAccessException ){
    illegalAccessException.printStackTrace();
}catch( ClassNotFoundException classNotFoundException ){
    classNotFoundException.printStackTrace();
}
```

###  利用反射分析类的能力

反射机制最重要的内容-->检查类的结构(查看任意对象的数据域名称和类型)

在java.lang.reflect 包中有三个类:

Field: 描述类的域,getType方法用来描述域所属类型的class对象

Method: 描述类的方法

Constructor: 描述类的构造器

------

这三个类都有一个getName()的方法,用来返回项目的名称. 

还有一个getModifiers的方法它将返回一个整型数值,用不同的位开关描述public 和static 这样的修饰符使用状况

还可以用用java.lang.refleCt 包中的Modifiei•类的静态方法分析
getModifiers 返回的整型数值例如:

可以使用Modifier 类中的isPublic、isPrivate 或isFinal
判断方法或构造器是否是public、private 或final我们需要做的全部工作就是调用Modifier类的相应方法， 并对返回的整型数值进行分析， 另外， 还可以利用Modifier.toString 方法将修饰符打印出来

------

Class 类:

1. getFields、getMethods 和getConstructors 方法将分别返回类提供的
   public 域、方法和构造器数组， 其中包括超类的公有成员。
2. getDeclareFields、getDeclareMethods 和getDeclaredConstructors 方法将分别返回类中声明的全部域、方法和构造器， 其中包括私有和受保护成员，但不包括超类的成员。

### 在运行时使用反射分析对象

#### 受限于访问权限

查看对象域使用的是field类中的get方法,通过f域(f指对象域)返回一个对象例如:f.get(obj)返回一个对象.其值为obj域的当前值.

```java
Employee harry = new Employee("Harry Hacker", 35000, 10, 1, 1989) ;
Class cl = harry.getClass()；
// the class object representing Employee
Field f = cl.getDeclaredField("name") :
// the name field of the Employee class
Object v = f.get(harry) ;
// the value of the name field of the harry object , i .e., the String object "Harry Hacker"
```

实际上，这段代码存在一个问题。由于name 是一个私有域， 所以get 方法将会抛出一个
IllegalAccessException。只有利用get 方法才能得到可访问域的值。除非拥有访问权限，否则Java 安全机制只允许査看任意对象有哪些域， 而不允许读取它们的值。

![image-20210616133345987](Image/image-20210616133345987.png)

```java
public void method01(){
		
    OrdinaryEntity ordinary = new OrdinaryEntity("mether go home", 565.56, "15688889999");
    Class aClass = ordinary.getClass();
    try{
        Field name = aClass.getDeclaredField("name");
        Object o = name.get(ordinary);
        System.out.println(o.toString());
    }catch( IllegalAccessException e ){
        e.printStackTrace();
    }catch( NoSuchFieldException e ){
        e.printStackTrace();
    }
}
```



#### 实现不受访问权限控制

反射机制的默认行为受限于Java 的访问控制。然而， 如果一个Java 程序没有受到安
全管理器的控制， 就可以覆盖访问控制。为了达到这个目的， 需要调用Field、Method 或
Constructor 对象的setAccessible 方法。例如

```java
f.setAtcessible(true) ;
```

setAccessible 方法是AccessibleObject 类中的一个方法， 它是Field、Method 和Constructor类的公共超类。这个特性是为调试、持久存储和相似机制提供的。

```java
public void method02() {
		OrdinaryEntity ordinary = new OrdinaryEntity("mether go home", 565.56, "15688889999");
		try{
			Field field = ordinary.getClass().getDeclaredField("name");
			field.setAccessible(true);
			System.out.println(field.isAccessible());
			System.out.println(field.getName());;
			Object o = field.get(ordinary);
			System.out.println(o.toString());
		}catch( NoSuchFieldException e ){
			e.printStackTrace();
		}catch( IllegalAccessException e ){
			e.printStackTrace();
		}
}
```

![image-20210616141511315](Image/image-20210616141511315.png)

由于JDK的安全检查耗时较多.所以通过setAccessible(true)的方式关闭安全检查就可以达到提升反射速度的目的 

####  get方法转换用法

但在访问私有double类型的域时可以使用getDouble()方法

```java
public void method02() {
		OrdinaryEntity ordinary = new OrdinaryEntity("mether go home", 565.56, "15688889999");
		Field field=null;
		try{
			//field = ordinary.getClass().getDeclaredField("name");
			field = ordinary.getClass().getDeclaredField("sal");
			System.out.println("解除访问受限前"+field.isAccessible());
			field.setAccessible(true);
			System.out.println("解除访问受限后"+field.isAccessible());
			System.out.println(field.getName());
			//Object o = field.get(ordinary);
			double o = field.getDouble(ordinary);
			System.out.println(o);
		}catch( NoSuchFieldException e ){
			e.printStackTrace();
		}catch( IllegalAccessException e ){
			e.printStackTrace();
		}
	}
```

![image-20210616142529279](Image/image-20210616142529279.png)

### 使用反射编写泛型数组代码

要获得新数组元素类型,要具有三个要素:

1. 首先获得a 数组的类对象。
2. 确认它是一个数组。
3. 使用Class 类（只能定义表示数组的类对象）的getComponentType 方法确定数组对应的类型。

通过aClass.getComponentType()获取对象的类型,根据类型拷贝数组

```java
	public Object copyObjectArray(Object o, int newlength){
		
		Class aClass = o.getClass();
		if(!aClass.isArray()) {
			return null;
		}
		Class componentType = aClass.getComponentType();
		int length = Array.getLength(o);
		//int length = ;
		Object newArray = Array.newInstance(componentType, newlength);
		System.arraycopy(o,0,newArray,0,Math.min(length,newlength));
		return newArray;
	}
```

### 调用任意方法

为了能够看到方法指针的工作过程， 先回忆一下利用Field 类的get 方法查看对象域的过
程。与之类似， 在Method 类中有一个invoke 方法， 它允许调用包装在当前Method 对象中
的方法。invoke 方法的签名是：

```java
Object invoke(Object obj, Object... args)
```

==返回类型是基本类型,invoke方法则返回的是包装类型==

可以通过getDeclaredMethods获得方法数组,通过invoke来调用当前对象的方法

```java
public void getMethodInvoke(){
		
		OrdinaryEntity ordinaryEntity = new OrdinaryEntity("mether go home", 565.56, "15688889999");
		Class aClass = ordinaryEntity.getClass();
		try{
			Method getName1 = aClass.getMethod("getName");
			Object nameValue = getName1.invoke(ordinaryEntity);
			System.out.println(nameValue);
    /**如何得到Method对象呢? 当然,可以通过调用getDeclareMethods方法,然后对返回的Method对象数组进行				查找,直到发现想要的方法为止。也可以通过调用Class类中的getMethod方法得到想要的方法*/
			Method[] declaredMethods = aClass.getDeclaredMethods();
			for (int i = 0; i < declaredMethods.length; i++) {
				Object invoke = declaredMethods[i].invoke(ordinaryEntity);
				System.out.println("for:"+invoke);
			}
			
		}catch( NoSuchMethodException e ){
			e.printStackTrace();
		}catch( InvocationTargetException e ){
			e.printStackTrace();
		}catch( IllegalAccessException e ){
			e.printStackTrace();
		}
	}
```



### 不要轻易的使用反射

1. 出错的可能性也比较大。如果在调用方法的时候提供了一个错误的参数，那么invoke 方法将会抛出一个异常
2. invoke 的参数和返回值必须是Object 类型的。这就意味着必须进行多次的类型转
   换。这样做将会使编译器错过检查代码的机会。查找错误比较困难
3. 使用反射获得方法指针的代码要比仅仅直接调用方法明显慢一些。

特别要重申： 建议Java 开发者不要使用Method 对象的回调功能。使用接口进行回调会使得代码的执行速度更快， 更易于维护。

## 继承设计的技巧

技巧:

1. 将公共操作和域放在超类

2. 不要使用受保护的域 

   ​	例如:protected定义的实例域\方法

   * 子类集合是无限制的， 任何一个人都能够由某个类派生一个子类，并
     编写代码以直接访问protected 的实例域， 从而破坏了封装性
   * 在Java 程序设计语言中，在同一个包中的所有类都可以访问proteced 域，而不管它是否为这个类的子类。
   * protected 方法对于指示那些不提供一般用途而应在子类中重新定义的方法很有用。

3. 使用继承实现“ is-a” 关系

   使用继承可以实现节省代码的目的

4. 除非所有继承的方法都有意义， 否则不要使用继承

5. 在覆盖方法时， 不要改变预期的行为

   在覆盖一个方法的时候， 不应该毫无原由地改变行为的内涵。

6. 使用多态， 而非类型信息
7. 不要过多地使用反射



# Interface

## 接口

接口不是类，而是对类的一组需求描述，这些类要遵从接口描述的统一格式进行定义。

### 接口特征

1.接口中的所有方法自动属于public,在接口声明方法时,不必提供关键字public

​		X.compareTo(y)时:

​			- 当x>y时: 返回一个正数

​			- 当x<y时: 返回一个负数

​			- 当x=y时: 返回0

2.接口中可以定义常量,但接口不能有实例域[实现接口的类会自动的继承定义的常量,在方法中可以直接的引用]

3.接口中在jdk8之前不能在接口中实现方法,但之后可以在接口中提供方法但是不能引用实例域(因为接口中没有实例域)

综合上面的特点可以知道,接口可以看作时没有实例域的抽象类

在实现接口的类中方法必须声明为public,否则,编译器将认为方法的访问范围为包可见(proteced)

### 抽象类与接口

每一个类只限定继承一个类,而抽象类是对通用行为的一个抽象,一个类只能继承一个抽象类,而对与接口可以继承多个.

### 静态方法

在Java SE 8 中，允许在接口中增加静态方法。理论上讲，没有任何理由认为这是不合法的。只是这有违于将接口作为抽象规范的初衷。

### 默认方法

可以为接口方法提供一个默认实现。必须用default 修饰符标记这样一个方法。

```java
public interface InterfaceDome{
	int max_value=56666;
	
	void method01();
	
	default void method02(){
	
	}
}
```

在接口中定义一个默认方法可以解决原先时间接口类的B类不会在自己的类中去实现不必要的方法.解决因不实现接口方法所带来的编译报错.并且可以在实现类中调用接口中的默认方法.

```java
public class InterfaceDomeImpl implements InterfaceDome{
	@Override
	public void method01(){
	    new InterfaceDomeImpl().method02();
	}
	public static void main(String[] args){
		InterfaceDomeImpl interfaceDome = new InterfaceDomeImpl();
		interfaceDome.method02();
	}
}
```

### 解决默认方法的冲突

如果先在一个接口中将一个方法定义为默认方法， 然后又在超类或另一个接口中定义了同样的方法， 会发生什么情况？

1. 超类优先。如果超类提供了一个具体方法， 同名而且有相同参数类型的默认方法会被忽略。
2. 接口冲突。如果一个超接口提供了一个默认方法， 另一个接口提供了一个同名而且参数类型（不论是否是默认参数）相同的方法， 必须覆盖这个方法来解决冲突。

在实现类中把两个接口类中相同的方法进行实现,并在实现类中的方法里面指定要使用接口中的方法:

```java
public class InterfaceDomeImpl implements InterfaceDome02,InterfaceDome{
	@Override
	public void method01(){
	    new InterfaceDomeImpl().method02();
	}
  
  /*********************************************************************/
  在实现类中覆盖method02()方法并在方法中指定要调用哪个接口同样的方法method02()
	@Override
	public void method02(){
		InterfaceDome02.super.method02();
	}
  /*********************************************************************/
  
	public static void main(String[] args){
		InterfaceDomeImpl interfaceDome = new InterfaceDomeImpl();
		interfaceDome.method02();
	}
}
```

==以上的方法解决歧义性==

当一个类继承一个带有method02()方法的类,并且还实现了InterfaceDome02接口[接口中同样有相同的method02()方法]. 这个时候会优先考虑类中的method02()方法.既==[类的优先性规则]==

## 接口示例

### 接口与回调

回调（ callback) 是一种常见的程序设计模式。在这种模式中， 可以指出某个特定事件发生时应该采取的动作。例如，可以指出在按下鼠标或选择某个菜单项时应该采取什么行动。(定时操作)

### Comparator 接口

### 对象克隆

Cloneable 接口，这个接口指示一个类提供了一个安全的clone 方法

```java
public class OrdinaryEntity implements Cloneable{
	....
	
	@Override
	public OrdinaryEntity clone() throws CloneNotSupportedException{
		return (OrdinaryEntity) super.clone();
	}
  ....
    
}

public static void main(String[] args){
		//克隆
		OrdinaryEntity miongminn = new OrdinaryEntity("miongminn", 5698, "15699887");
		try{
			OrdinaryEntity ordinaryEntity=miongminn.clone();
			System.out.println(ordinaryEntity.toString());
		}catch( CloneNotSupportedException e ){
			e.printStackTrace();
		}
}
```

使用clone方法可以初始化一个新的对象,两个对象之间不会彼此影响

![image-20210617163526795](Image/image-20210617163526795.png)

## lambda 表达式

### 为什么引入lambda 表达式

lambda 表达式是一个可传递的代码块， 可以在以后执行一次或多次。

(线程,定时器)

### lambda 表达式的语法

```java
		//lambda 表达式的语法
		String[] s =new String[]{"ds","jj","kk","yy","ss","cc","hh","ff","hh","ll","gg"};
		System.out.println(Arrays.toString(s));
		Arrays.sort(s);
		System.out.println(Arrays.toString(s));
		System.out.println("**********");
		Arrays.sort(s,(first, second)->first.length()-second.length());
		System.out.println(Arrays.toString(s));
		
		Timer timer = new Timer(1000,event-> System.out.println("ddd"));
    timer.start();
    JOptionPane.showMessageDialog(null,"qq");
    System.exit(0);
```

你已经见过Java 中的一种lambda 表达式形式：参数， 箭头（->) 以及一个表达式。如果代码要完成的计算无法放在一个表达式中，就可以像写方法一样，把这些代码放在{}中，并包含显式的return 语句。例如：

```java
(String first, String second) ->
{
  if (first.lengthO < second.lengthO) return -1;
  else if (first.lengthO > second.lengthO) return 1;
  else return 0;
}
```

即使lambda 表达式没有参数， 仍然要提供空括号，就像无参数方法一样：

```java
0 -> { for (int i = 100; i >= 0;i ) System.out.println(i); }
```

如果可以推导出一个lambda 表达式的参数类型，则可以忽略其类型。例如：

```java
Comparator<String> comp
= (first, second) // Same as (String first, String second)
-> first.length() - second.length();
```

在这里， 编译器可以推导出first 和second 必然是字符串， 因为这个lambda 表达式将赋
给一个字符串比较器。

如果方法只有一参数， 而且这个参数的类型可以推导得出，那么甚至还可以省略小括号：

```java
ActionListener listener = event ->
System.out.println("The time is " + new Date()");
// Instead of (event) -> . . . or (ActionEvent event) -> . . .
```

无需指定lambda 表达式的返回类型。lambda 表达式的返回类型总是会由上下文推导得
出。例如，下面的表达式

```java
 (String first, String second) -> first.lengthO - second.lengthO
```

### 函数式接口

对于只有一个抽象方法的接口， 需要这种接口的对象时， 就可以提供一个lambda 表达式。这种接口称为函数式接口（ functional interface )。

### 方法引用

有时， 可能已经有现成的方法可以完成你想要传递到其他代码的某个动作

例子:

```java
Timer t = new Timer(1000, Systei.out::println) ;
Arrays.sort(strings，String::conpareToIgnoreCase)
```

以上可以得出,要用：:操作符分隔方法名与对象或类名。主要有3 种情况：

* object::instanceMethod
* Class::staticMethod
* Class::instanceMethod

![image-20210618000027335](Image/image-20210618000027335.png)

![image-20210618000040853](Image/image-20210618000040853.png)

```java
public class Exe1 {
    public static void main(String[] args) {
        ReturnOneParam lambda1 = a -> doubleNum(a);
        System.out.println(lambda1.method(3));

        //lambda2 引用了已经实现的 doubleNum 方法
        ReturnOneParam lambda2 = Exe1::doubleNum;
        System.out.println(lambda2.method(3));

        Exe1 exe = new Exe1();

        //lambda4 引用了已经实现的 addTwo 方法
        ReturnOneParam lambda4 = exe::addTwo;
        System.out.println(lambda4.method(2));
    }

    /**
     * 要求
     * 1.参数数量和类型要与接口中定义的一致
     * 2.返回值类型要与接口中定义的一致
     */
    public static int doubleNum(int a) {
        return a * 2;
    }

    public int addTwo(int a) {
        return a + 2;
    }
}
```



### 构造器引用

构造器引用与方法引用很类似，只不过方法名为new。例如，Person::new 是Person 构造器的一个引用。哪一个构造器呢？ 这取决于上下文

### 变霣作用域

### 处理lambda 表达式

### 再谈Comparator

## 内部类

内部类（ inner class ) 是定义在另一个类中的类。为什么需要使用内部类呢？ 其主要原因:

* 内部类方法可以访问该类定义所在的作用域中的数据， 包括私有的数据。
* 内部类可以对同一个包中的其他类隐藏起来。
* 当想要定义一个回调函数且不想编写大量代码时，使用匿名（anonymous) 内部类比较便捷。

### 使用内部类访问对象状态

## 代理 

假设有一个表示接口的Class 对象（有可能只包含一个接口)，它的确切类型在编译时无法知道。这确实有些难度。要想构造一个实现这些接口的类， 就需要使用newlnstance 方法或反射找出这个类的构造器。但是， 不能实例化一个接口，需要在程序处于运行状态时定义一个新类。

代理类可以在运行时创建全新的类。这样的代理类能够实现指定的接口

###   创建代理对象

创建一个代理对象， 需要使用Proxy 类的newProxylnstance 方法。这个方法有三个参数：

* 一个类加栽器（ class loader)。 作为Java 安全模型的一部分， 对于系统类和从因特网上下载下来的类，可以使用不同的类加载器。有关类加载器的详细内容将在卷n 第9章中讨论。目前， 用null 表示使用默认的类加载器。
* 一个Class 对象数组， 每个元素都是需要实现的接口。
* 一个调用处理器。

###   代理类的特性

所有的代理类都扩展于Proxy类。一个代理类只有一个实例域---调用处理器，它定义在Proxy的超类中。为了履行代理对象的职责,所需要的任何附加数据都必须存储在调用处理器中。

所有的代理类都覆盖了Object类中的方法toString、equals 和hashCode。如同所有的代理方法一样,这些方法仅仅调用了调用处理器的invoke。

没有定义代理类的名字，Sun虚拟机中的Proxy类将生成一个以字符串SProxy开头的类名。

对于特定的类加载器和预设的一组接口来说,只能有一个代理类。也就是说,如果使用同一个类加载器和接口数组调用两次newProxylustance 方法的话那么只能够得到同一个类的两个对象，也可以利用getProxyClass 方法获得这个类：

```java
Class proxyClass = Proxy.getProxyClass(null, interfaces);
```

代理类一定是public 和final。如果代理类实现的所有接口都是public,代理类就不属于某个特定的包；否则,所有非公有的接口都必须属于同一个包，同时，代理类也属于这个包。

可以通过调用Proxy类中的isProxyClass方法检测一个特定的Class对象是否代表一个代理类。

#  异常\断言\log

## 异常

![image-20210618154506483](Image/image-20210618154506483.png)

## 断言

断言机制允许在测试期间向代码中插入一些检査语句。当代码发布时，这些插人的检测语句将会被自动地移走。

Java 语言引人了关键字assert。这个关键字有两种形式：

​	* assert 条件；

​    * assert 条件：表达式；

# GenericProgramDevise

## 泛型设计使用

泛型设计可以使很多不同类型的对象所重用

### 类型参数的好处

在Java中增加范型类之前， 泛型程序设计是用继承实现的。ArrayList类只维护一个Object引用的数组

```java
ArrayLi st files = new ArrayListO；
String filename = (String) files.get (O) ;
```

使用类型参数后

```java
ArrayList<String> files = new ArrayList<String>() :
String filename = files.get(0);
```

## 定义简单的泛型类

一个类型变量:

```java
public class SimpleGeneric<T>{
	private T name;
	private T sex;
	private T addre;
	public SimpleGeneric(){
		name = null;
		sex = null;
		addre = null;
	}
	public SimpleGeneric(T name, T sex, T addre){
		this.name = name;
		this.sex = sex;
		this.addre = addre;
	}
	public T getName(){
		return name;
	}
	public void setName(T name){
		this.name = name;
	}
	public T getSex(){
		return sex;
	}
	public void setSex(T sex){
		this.sex = sex;
	}
	public T getAddre(){
		return addre;
	}
	public void setAddre(T addre){
		this.addre = addre;
	}
}
```

两个类型变量:

```java
public class SimpleGeneric02<T,U>{
	private T name;
	private U phone;
	
	public SimpleGeneric02(){
		name = null;
		phone = null;
	}
	public SimpleGeneric02(T name, U phone){
		this.name = name;
		this.phone = phone;
	}
	public T getName(){
		return name;
	}
	public void setName(T name){
		this.name = name;
	}
	public U getPhone(){
		return phone;
	}
	public void setPhone(U phone){
		this.phone = phone;
	}
}
```

## 泛型方法

```java
public U getPhone(){
		return phone;
	}

```

## 类型变量的限定

```java
public static <T extends Coiparab1e> T main(T a) . . .
```

通过<T extends Coiparab1e>来限定类型变量从而获得compareTo方法

为什么使用关键字extends 而不是implements ?

T 和绑定类型可以是类， 也可以是接口。选择关键字extends 的原因是更接近子类的概念， 并且Java 的设计者也不打算在语言中再添加一个新的关键字（如sub)。

一个类型变量或通配符可以有多个限定， 例如：

```java
<T extends Comparable & Serializable>
```

通过&可以根据需要拥有多个接口超类型但限定中至多有一个类。如果用一个类作为限定，它必须是限定列表中的第一个。

## 泛型与虚拟机

### 类型擦除

原始类型的名字就是删去类型参数后的泛型类型名。擦除（ erased) 类型变M, 并替换为限定类型（无限定的变量用Object)。

### 翻译泛型表达式

```java
Pair<Employee> buddies = . .
Employee buddy = buddies.getFirstO；
```

擦除getFirst 的返回类型后将返回Object 类型。编译器自动插人Employee 的强制类型转换。也就是说，编译器把这个方法调用翻译为两条虚拟机指令：

* 对原始方法Pair.getFirst 的调用。
* 将返回的Object 类型强制转换为Employee 类型。

### 翻译泛型方法

```java
擦除前
public static <T extends Comparable〉T nrin(T[] a)
擦除后
public static Comparable min(Comparable!] a)  
```

### 调用遗留代码

设计Java 泛型类型时， 主要目标是允许泛型代码和遗留代码之间能够互操作。

在查看了警告之后，可以利用注解（ annotation ) 使之消失。注释必须放在生成这个警告的代码所在的方法之前， 如下：

```java
@SuppressWarnings("unchecked")
Dictionary<Integer, Components〉labelTable = slider.getLabelTableQ; // No warning
或者
@SuppressWarnings("unchecked")
public void configureSliderO { . . . }
```

## 约束与局限性

### 不能用基本类型实例化类型参数

### 运行时类型查询只适用于原始类型

所有类型查询只产生原始类型

```java
if (a instanceof Pair<String>) // Error
```

实际只是测试a是不是与pair的同一个类型

```java
if (a instanceof Pair<T>) // Error
  比如
Pair<String> p = (Pair<String>) a
```

同理使用getCalss总是返回原始类型

```java
Pair<String> stringPair = . .
Pair< Employee〉employeePair = . .
if (stringPair.getClassO == employeePair.getClass() // they are equal
```

其比较的结果是true, 这是因为两次调用getClass 都将返回Pair.class。

### 不能创建参数化类型的数组

不能实例化参数化类型的数组， 例如：

```java
Pair<String>[] table = new Pair<String>[10] ; // Error
```

这有什么问题呢？ 擦除之后， table 的类型是Pair[]。 可以把它转换为Object□ :

数组会记住它的元素类型， 如果试图存储其他类型的元素， 就会抛出一个Array-
StoreException 异常：

需要说明的是， 只是不允许创建这些数组， 而声明类型为Pair<String>[] 的变量仍是合法的。不过不能用new Pair<String>[10] 初始化这个变量。

### Varargs 警告

### 不能实例化类型变置

### 不能构造泛型数组

### 泛型类的静态上下文中类型变量无效

### 不能抛出或捕获泛型类的实例

### 可以消除对受查异常的检查

### 注意擦除后的冲突

## 泛型类型的继承规则

## 通配符类型

### 通配符概念

### 通配符的超类型限定

### 无限定通配符

### 通配符捕获

## 反射和泛型

### 泛型Class 类

现在，Class类是泛型的。例如，String.class实际上是一个：< 以8<8出呢>类的对象（事实上，是唯一的对象)。

==有空看下Class类==

### 使用Class<T> 参数进行类型匹配

示例:

```java
public static <T> Pai r<T> makePair(Class<T> c) throws InstantiationException,IllegalAccessException
{
		return new Pairofc.newInstanceO » c.newInstanceO) ;
}
```

调用这个方法:

```java
makePair(Employee.class)
```

### 虚拟机中的泛型类型信息

Java 泛型的卓越特性之一是在虚拟机中泛型类型的擦除,但是在擦除的类仍然保留一些泛型祖先的微弱记忆。例如， 原始的Pair 类知道源于泛型类Pair<T>， 即使一
个Pair 类型的对象无法区分是由PaiKString> 构造的还是由PaiKEmployee> 构造的。

可以使用反射API来确定:

* 这个泛型方法有一个叫做T 的类型参数。
* 这个类型参数有一个子类型限定， 其自身又是一个泛型类型。
* 这个限定类型有一个通配符参数。
* 这个通配符参数有一个超类型限定。
* 这个泛型方法有一个泛型数组参数。

换句话说，需要重新构造实现者声明的泛型类以及方法中的所有内容。但是，不会知道对于特定的对象或方法调用， 如何解释类型参数。

为了表达泛型类型声明， 使用java.lang.reflect 包中提供的接口Type。这个接口包含下列子类型：

* Class 类，描述具体类型。
* TypeVariable 接口 , 描述类型变量（如T extends Comparable<? super T>)
* WildcardType 接口， 描述通配符（如？super T)。
* ParameterizedType 接口， 描述泛型类或接口类型（如Comparable<? super T>)。
* GenericArrayType 接口， 描述泛型数组（如T[])。

![image-20210630234244986](Image/image-20210630234244986.png)

```java
package chapter_eight;

import java.lang.reflect.*;
import java.util.Arrays;
import java.util.Scanner;

/**
 * @Author: Trent
 * @Date: 2021/6/30 23:06
 * @program: Java
 * @Description:
 */
public class Test{
	
	private SimpleGeneric<String> test1 = new SimpleGeneric<String>();
	
	public static void main(String[] args){
		try{
			SimpleGeneric simpleGeneric = SimpleGeneric.makePair(Employee.class);
			System.out.println(simpleGeneric.toString());
		}catch( InstantiationException e ){
			e.printStackTrace();
		}catch( IllegalAccessException e ){
			e.printStackTrace();
		}
		String name = "";
		
		if( name.length() > 0 ){
			name = args[0];
		}else{
			try( Scanner input = new Scanner(System.in) ){
				System.out.println("输入类名(e.g.java.util.Collections):):");
				name = input.next();
			}
		}
		try{
			Class<?> aClass = Class.forName(name);
			printClass(aClass);
			for( Method m: aClass.getDeclaredMethods() ){
				printMethod(m);
			}
		}catch( ClassNotFoundException e ){
			e.printStackTrace();
		}
	}
	private static void printMethod(Method m){
		String name = m.getName();
		System.out.print(Modifier.toString(m.getModifiers()));
		System.out.print(" ");
		printTypes(m.getParameterTypes(), "<", ",", ">", true);
		
		printType(m.getGenericReturnType(), false);
		System.out.print(" ");
		System.out.print(name);
		System.out.print("(");
		printTypes(m.getGenericParameterTypes(), "", ",", "", false);
		System.out.println(")");
	}
	public static void printClass(Class<?> cl){
		System.out.print(cl);
		printTypes(cl.getTypeParameters(), "<", ",", ">", true);
		Type genericSuperclass = cl.getGenericSuperclass();
		if( genericSuperclass != null ){
			System.out.print(" extend ");
			printType(genericSuperclass, false);
		}
		printTypes(cl.getGenericInterfaces(), " implements ", ", ", "", false);
		System.out.println();
	}
	private static void printType(Type type, boolean b){
		if( type instanceof Class ){
			Class<?> type1 = (Class<?>) type;
			System.out.print(type1.getName());
		}else if( type instanceof TypeVariable ){
			TypeVariable<?> typeVariable = (TypeVariable<?>) type;
			System.out.print(typeVariable.getName());
			if( b ){
				printTypes(typeVariable.getBounds(), " extend ", " & ", "", false);
			}
		}else if( type instanceof WildcardType ){
			WildcardType wildcardType = (WildcardType) type;
			System.out.print("?");
			printTypes(wildcardType.getUpperBounds(), " extend ", " & ", "", false);
			printTypes(wildcardType.getLowerBounds(), " super ", " & ", "", false);
		}else if( type instanceof ParameterizedType ){
			ParameterizedType t = (ParameterizedType) type;
			Type owner = t.getOwnerType();
			if( owner != null ){
				printType(owner, false);
				System.out.print(".");
			}
			printType(t.getRawType(), false);
			printTypes(t.getActualTypeArguments(), "<", " , ", " >", false);
		}else if( type instanceof GenericArrayType ){
			GenericArrayType t = (GenericArrayType) type;
			System.out.print("M");
			printType(t.getGenericComponentType(), b);
			System.out.print("[]");
		}
	}
	
	private static void printTypes(Type[] types, String pre, String sep, String suf, boolean isDefinition){
		if( pre.equals(" extends ") && Arrays.equals(types, new Type[] {Object.class}) ){
			return;
		}
		if( types.length > 0 ){
			System.out.print(pre);
		}
		for( int i = 0; i < types.length; i++ ){
			if( i > 0 ){
				System.out.print(sep);
			}
			printType(types[i], isDefinition);
		}
		if( types.length > 0 ){
			System.out.print(suf);
		}
	}
}


```

![image-20210701004043208](Image/image-20210701004043208.png)

# 集合

## 集合框架

### 将集合的接口与实现分离

队列有两种实现方式:

* 使用循环数组
* 使用链表

### Collection 接口

接口有两个基本得方法:

```java
public interface Collection<E>
{
	boolean add(E element);
	Iterator<E> iterator()；
  ....  
}
```

其中得iterator()方法返回一个实现的iterator()接口的对象:

​	可以使用这个迭代器对象进行依次访问集合中的元素

### 迭代器

迭代器有四个方法:

 ```java
public interface Iterator<E> {
    /**
     * Returns {@code true} if the iteration has more elements.
     * (In other words, returns {@code true} if {@link #next} would
     * return an element rather than throwing an exception.)
     *
     * @return {@code true} if the iteration has more elements
     */
  	如果迭代器对象还有多个访问元素则返回true(判断在next方法访问集合元素时是否到集合尾部防止报错)
    boolean hasNext();

    /**
     * Returns the next element in the iteration.
     *
     * @return the next element in the iteration
     * @throws NoSuchElementException if the iteration has no more elements
     */
  	逐个访问调用集合中每个元素
    E next();

    /**
     * Removes from the underlying collection the last element returned
     * by this iterator (optional operation).  This method can be called
     * only once per call to {@link #next}.  The behavior of an iterator
     * is unspecified if the underlying collection is modified while the
     * iteration is in progress in any way other than by calling this
     * method.
     *
     * @implSpec
     * The default implementation throws an instance of
     * {@link UnsupportedOperationException} and performs no other action.
     *
     * @throws UnsupportedOperationException if the {@code remove}
     *         operation is not supported by this iterator
     *
     * @throws IllegalStateException if the {@code next} method has not
     *         yet been called, or the {@code remove} method has already
     *         been called after the last call to the {@code next}
     *         method
     */
    default void remove() {
        throw new UnsupportedOperationException("remove");
    }

    /**
     * Performs the given action for each remaining element until all elements
     * have been processed or the action throws an exception.  Actions are
     * performed in the order of iteration, if that order is specified.
     * Exceptions thrown by the action are relayed to the caller.
     *
     * @implSpec
     * <p>The default implementation behaves as if:
     * <pre>{@code
     *     while (hasNext())
     *         action.accept(next());
     * }</pre>
     *
     * @param action The action to be performed for each element
     * @throws NullPointerException if the specified action is null
     * @since 1.8
     */
    default void forEachRemaining(Consumer<? super E> action) {
        Objects.requireNonNull(action);
        while (hasNext())
            action.accept(next());
    }
}

 ```



```java
public static void main(String[] args){
		Collection<String> collection = new HashSet<String>();
		collection.add("dd");
		collection.add("ff");
		collection.add("fg");
		collection.add("gg");
		collection.add("hh");
		collection.add("jj");
		
		Iterator<String> iterator = collection.iterator();
		while (iterator.hasNext()){
			System.out.println(iterator.next());
		}
		System.out.println("while结束!");
		
		for(String c: collection ){
			System.out.println(c);
		}
		System.out.println("for each结束!");
		
	}
```

应该将Java迭代器认为是位于两个元素之间。当调用next 时， 迭代器就越过下一个元素并返回刚刚越过的那个元素的引用

在使用remove()方法时,因为remove方法与next方法有相互依赖性,在调用remove方法之前需要调用next方法

### 泛型实用方法

![image-20210703234234270](Image/image-20210703234234270.png)

![image-20210703234255428](Image/image-20210703234255428.png)

### 集合框架中的接口

![image-20210704003025657](Image/image-20210704003025657.png)

#### List

list是一个有序集合:

	* 元素会增加到特定的位置
	* 访问方式可以采用:1.迭代器访问 2.整数索引访问

```java
void add(int index, E element)
void remove(int index)
E get(int index)
E set(int index, E element)

```

实际中有两种有序集合，其性能开销有很大差异。由数组支持的有序集合可以快速地随机访问，因此适合使用List 方法并提供一个整数索引来访问。与之不同， 链表尽管也是有序的， 但是随机访问很慢， 所以最好使用迭代器来遍历。如果原先提供两个接口就会容易一些了。

#### Set

set不允许重复元素,

## 具体的集合

![image-20210704013810010](Image/image-20210704013810010.png)

![image-20210704013905858](Image/image-20210704013905858.png)

### 链表(LinkedList)

数组以及动态的ArrayList类:

​	在删除和插入操作会有很大的代价

链表:

​	链表是一个有序的集合

​	链表的每一个元素都有独立的结点,而每个结点都存放着下一个节点的引用,而且每一个链表都是双向链接

![image-20210704154158717](Image/image-20210704154158717.png)

当链表删除一个元素时,只更新附近的节点连接就行

![image-20210704154526457](Image/image-20210704154526457.png)

Listlterator 接口有两个方法， 可以用来反向遍历链表:

 ```java
E previous()
boolean hasPreviousO
 ```

LinkedList 类的listlterator 方法返回一个实现了Listlterator 接口的迭代器对象:

```java
		List<String> strings = new LinkedList<>();
		strings.add("fff");
		strings.add("ggg");
		strings.add("hhh");
		ListIterator<String> stringListIterator = strings.listIterator();
		stringListIterator.next();
		stringListIterator.add("ddd");
```

![image-20210704231734913](Image/image-20210704231734913.png)

苛以使用Listlterator 类从前后两个方向遍历链表中的元素， 并可以添加、删除元素。



### 数组列表(ArrayList)

ArrayList 封装了一个动态再分配的对象数组。

ArrayList 与 Vector的区别:

在于Vector方法是同步的。可以由两个线程安全地访问一个Vector 对象。但是， 如果由一个线程访问Vector, 代码要在同步操作上耗费大量的时间。这种情况还是很常见的。而ArrayList 方法不是同步的，因此， 建议在不需要同步时使用ArrayList, 而不要使用Vector。

### 散列集

散列集是一个无序的集合

![image-20210704235503381](Image/image-20210704235503381.png)

当然， 有时候会遇到桶被占满的情况， 这也是不可避免的。这种现象被称为散列冲突（ hashcollision) o 这时， 需要用新对象与桶中的所有对象进行比较， 査看这个对象是否已经存在。如果散列码是合理且随机分布的， 桶的数目也足够大， 需要比较的次数就会很少。

### 树集(TreeSet)

树集是一个有序集合

每次将一个元素添加到树中时，都被放置在正确的排序位置上。因此，迭代器总是以排好序的顺序访问每个元素。

```java
	Collection<String> treeSet =new TreeSet<>();
		treeSet.add("sss0");
		treeSet.add("sss1");
		treeSet.add("sss2");
		treeSet.add("sss3");
		for (String s:treeSet){
			System.out.println(s);
		}
```

![image-20210705000957999](Image/image-20210705000957999.png)

### 队列与双端队列

队列可以让人们有效地在尾部添加一个元素， 在头部删除一个元素

### 优先级队列

优先级队列（priority queue) 中的元素可以按照任意的顺序插人，却总是按照排序的顺序进行检索。也就是说，无论何时调用remove 方法， 总会获得当前优先级队列中最小的元素。然而， 优先级队列并没有对所有的元素进行排序。如果用迭代的方式处理这些元素，并不需要对它们进行排序。优先级队列使用了一个优雅且高效的数据结构，称为堆（ heap)。堆是一个可以自我调整的二叉树，对树执行添加（ add) 和删除（ remore) 操作， 可以让最小的元素移动到根，而不必花费时间对元素进行排序。

与TreeSet—样，一个优先级队列既可以保存实现了Comparable 接口的类对象， 也可以保存在构造器中提供的Comparator 对象。

使用优先级队列的典型示例是任务调度。每一个任务有一个优先级， 任务以随机顺序添加到队列中。每当启动一个新的任务时，都将优先级最高的任务从队列中删除（由于习惯上将1 设为“ 最高” 优先级，所以会将最小的元素删除)。

## 映射(map)

### 基本映射操作

Java 类库为映射提供了两个通用的实现：HashMap 和TreeMap。这两个类都实现了Map 接口。

散列映射对键进行散列， 树映射用键的整体顺序对元素进行排序， 并将其组织成搜索树。散列或比较函数只能作用于键。与键关联的值不能进行散列或比较。

```java
	Map<Integer,Object> map = new HashMap<Integer,Object>();
		map.put(1,"ss1");
		map.put(2,"ss2");
		map.put(3,"ss3");
		map.put(4,"ss4");
		map.put(5,"ss5");
		System.out.println(map.get(4));
		map.forEach((K,V)->System.out.println(K+": "+V ));
```

### 更新映射项

处理映射时的一个难点就是更新映射项。正常情况下， 可以得到与一个键关联的原值，完成更新， 再放回更新后的值。不过，必须考虑一个特殊情况， 即键第一次出现。下面来看一个例子， 使用一个映射统计一个单词在文件中出现的频度。看到一个单词（word) 时， 我们将计数器增1， 如下所示：

```java
counts.put(word, counts.get(word)+ 1);
```

这是可以的， 不过有一种情况除外： 就是第一次看到word 时。在这种情况下，get 会返回null , 因此会出现一个NullPointerException 异常。

作为一个简单的补救， 可以使用getOrDefault 方法：

```java
counts,put(word, counts.getOrDefault(word, 0)+ 1);
```

另一种方法是首先调用putlfAbsent 方法。只有当键原先存在时才会放入一个值。

```java
counts.putlfAbsent(word, 0);
counts.put(word, counts.get(word)+ 1)
```

不过还可以做得更好。merge 方法可以简化这个常见的操作。如果键原先不存在，下面的调用：

```java
counts.merge(word, 1, Integer::sum);
```

### 映射视图

映射的视图（ View )-->这是实现了Collection 接口或某个子接口的对象。

有3 种视图： 键集、值集合（不是一个集） 以及键/ 值对集。键和键/ 值对可以构成一个集， 因为映射中一个键只能有一个副本。下面的方法：

```java
Set<K> keySet()
Collection<V> values0
Set<Map.Entry<K, V» entrySetO
```

Set 接口扩展了Collection 接口。因此， 可以像使用集合一样使用keySet。

```java
Map<Integer,Object> map = new HashMap<Integer,Object>();
map.put(1,"ss1");
map.put(2,"ss2");
map.put(3,"ss3");
map.put(4,"ss4");
map.put(5,"ss5");
System.out.println(map.get(4));
map.forEach((K,V)->System.out.println(K+": "+V ));

Set<Integer> integers = map.keySet();
for( Integer s : integers ){
  System.out.println(s);
}
```

![image-20210705004037723](Image/image-20210705004037723.png)

如果想同时查看键和值， 可以通过枚举条目来避免查找值:

```java
for (Map.Entry<Integer, Object> entry : map.entrySet()){
  System.out.println(entry.getKey() + ": "+entry.getValue());
}

//lamda
map.forEach((key, value)-> System.out.println(key + ": "+value));
```

![image-20210705004227451](Image/image-20210705004227451.png)

### 弱散列映射

WeakHashMap 使用弱引用（ weak references) 保存键。WeakReference 对象将引用保存到另外一个对象中， 在这里， 就是散列键。对于这种类型的对象， 垃圾回收器用一种特有的方式进行处理。通常， 如果垃圾回收器发现某个特定的对象已经没有他人引用了， 就将其回收。然而， 如果某个对象只能由WeakReference 引用， 垃圾回收器仍然回收它，但要将引用这个对象的弱引用放人队列中。WeakHashMap 将周期性地检查队列， 以便找出新添加的弱引用。一个弱引用进人队列意味着这个键不再被他人使用， 并且已经被收集起来。于是， WeakHashMap 将删除对应的条目。

### 链接散列集与映射(LinkedHashSet 和LinkedHashMap)

LinkedHashSet 和LinkedHashMap 类用来记住插人元素项的顺序。这样就可以避免在散列表中的项从表面上看是随机排列的。当条目插入到表中时，就会并人到双向链表中

![image-20210705004907965](Image/image-20210705004907965.png)

```java
Map<String, Employee〉staff = new LinkedHashMap<>0;
staff.put ("144-25-5464", new Employee("Amy Lee")) ;
staff.put ("567-24-2546", new Employee("Harry Hacker")) ;
staff.put ("157-62-7935", new Employee("Gary Cooper")) ;
staff.put ("456-62-5527", new Employee("Francesca Cruz"))；
```

### 枚举集与映射(EmimSet)

EmimSet 是一个枚举类型元素集的高效实现。由于枚举类型只有有限个实例， 所以EnumSet 内部用位序列实现。如果对应的值在集中， 则相应的位被置为1。

![image-20210705010707421](Image/image-20210705010707421.png)

### 标识散列映射(IdentityHashMap)

类IdentityHashMap 有特殊的作用。在这个类中,键的散列值不是用hashCode函数计算的,而是用System.identityHashCode 方法计算的。这是Object.hashCode 方法根据对象的内存地址来计算散列码时所使用的方式。而且， 在对两个对象进行比较时， IdentityHashMap 类使用==, 而不使用equals。

## 视图与包装器

取而代之的是：keySet 方法返回一个实现Set接口的类对象， 这个类的方法对原映射进行操作。这种集合称为视图。

### 轻量级集合包装器

Arrays 类的静态方法asList 将返回一个包装了普通Java 数组的List 包装器。这个方法可以将数组传递给一个期望得到列表或集合参数的方法。例如：

```java
Card[] cardOeck = new Card[52];
List<Card> cardList = Arrays.asList(cardDeck):
```

asList 方法可以接收可变数目的参数。例如：

```java
List<String> names = Arrays.asList("A«iy", "Bob", "Carl") ;
```

创建Col1ections.nCopies(n, anObject):

```java
List<String> settings = Collections.nCopies(100, "DEFAULT") ;

List<String> settings = Collections.nCopies(100, "DEFAULT") ;
for (String setting : settings){
  System.out.println(setting);
}
```

将返回一个实现了List 接口的不可修改的对象， 并给人一种包含100个元素， 每个元素都像是一个DEFAULT的错觉。

创建Collections.singleton(anObject):

### 子范围

可以为很多集合建立子范围（subrange) 视图。例如， 假设有一个列表staff, 想从中取出第10 个~ 第19 个元素。可以使用subList 方法来获得一个列表的子范围视图。

```java
List<String> settings = Collections.nCopies(100, "DEFAULT") ;
		for (String setting : settings){
			System.out.println(setting);
		}
		
		Set<List<String>> singleton = Collections.singleton(settings);
		int size = singleton.size();
		System.out.println(size);
		
		List<String> strings = settings.subList(10, 20);
		for(String setting : strings){
			System.out.println(setting);
		}
```

第一个索引包含在内， 第二个索引则不包含在内。这与String 类的substring 操作中的参数情况相同。

### 不可修改的视图

可以使用下面8 种方法获得不可修改视图：

```java
Collections.unmodifiableCollection
Collections.unmodifiableList
Collections.unmodifiableSet
Collections.unmodifiableSortedSet
Collections.unmodifiableNavigableSet
Collections.unmodifiableMap
Collections.unmodifiableSortedMap
Collections. unmodifiableNavigableMap
```

每个方法都定义于一个接口。例如， Collections.unmodifiableList 与ArrayList、LinkedList或者任何实现了List 接口的其他类一起协同工作。

例如， 假设想要查看某部分代码， 但又不触及某个集合的内容， 就可以进行下列操作：

```java
List<String> staff = new LinkedListoO ;
lookAt (Collections.unmodifiableList(staff)) ;
```

不可修改视图并不是集合本身不可修改。仍然可以通过集合的原始引用（在这里是staff)对集合进行修改。并且仍然可以让集合的元素调用更改器方法。

### 同步视图

如果由多个线程访问集合，就必须确保集不会被意外地破坏。例如， 如果一个线程试图将元素添加到散列表中，同时另一个线程正在对散列表进行再散列，其结果将是灾难性的。

类库的设计者使用视图机制来确保常规集合的线程安全， 而不是实现线程安全的集合类。例如， Collections 类的静态synchronizedMap 方法可以将任何一个映射表转换成具有同步访问方法的Map:

```java
Map<String, Employee〉map = Collections.synchronizedMap(new HashMap<String, Employee>0)；
```

### 受查视图

受査” 视图用来对泛型类型发生问题时提供调试支持。实际上将错误类型的元素混人泛型集合中的问题极有可能发生。例如：

```java
List<String> strings1 = Collections.checkedList(strings, String.class);
```

视图的add 方法将检测插人的对象是否属于给定的类。如果不属于给定的类， 就立即抛出一个ClassCastException。这样做的好处是错误可以在正确的位置得以报告：

### 关于可选操作的说明

通常， 视图有一些局限性， 即可能只可以读、无法改变大小、只支持删除而不支持插人，这些与映射的键视图情况相同。如果试图进行不恰当的操作，受限制的视图就会抛出一个UnsupportedOperationException。

在集合和迭代器接口的API 文档中， 许多方法描述为“ 可选操作”。这看起来与接口的概念有所抵触。毕竟， 接口的设计目的难道不是负责给出一个类必须实现的方法吗？ 确实，从理论的角度看，在这里给出的方法很难令人满意。一个更好的解决方案是为每个只读视图和不能改变集合大小的视图建立各自独立的两个接口。不过这将会使接口的数量成倍增长，这让类库设计者无法接受。

是否应该将“ 可选” 方法这一技术扩展到用户的设计中呢？ 我们认为不应该。尽管集合被频繁地使用， 其实现代码的风格也未必适用于其他问题领域。集合类库的设计者必须解决一组特别严格且又相互冲突的需求。用户希望类库应该易于学习、使用方便，彻底泛型化，面向通用性， 同时又与手写算法一样高效。要同时达到所有目标的要求， 或者尽量兼顾所有目标完全是不可能的。但是，在自己的编程问题中， 很少遇到这样极端的局限性。应该能够找到一种不必依靠极端衡量“ 可选的” 接口操作来解决这类问题的方案。

## 算法

### 排序与混排

```java
List<Integer> integers = new LinkedList<>();
		integers.add(1);
		integers.add(5);
		integers.add(8);
		integers.add(9);
		integers.add(7);
		integers.add(6);
		integers.add(2);
		System.out.println(integers.toString());
		
		//升序排序
		Collections.sort(integers);
		System.out.println(integers.toString());
		
		//降序排序
		integers.sort(Comparator.reverseOrder());
		System.out.println(integers.toString());
```



### 二分查找

### 简单算法

### 批操作

![image-20210705021206148](Image/image-20210705021206148.png)

### 集合与数组的转换

如果需要把一个数组转换为集合，Arrays.asList 包装器可以达到这个目的。例如：

```java
String□ values = . .
HashSet<String> staff = new HashSet<>(Arrays.asList(values));
```

从集合得到数组会更困难一些。当然，可以使用toArray 方法：

```java
Object[] values = staff.toArray();
```

### 编写自己的算法

## 遗留的集合

# 并发

多线程程序在较低的层次上扩展了多任务的概念： 一个程序同时执行多个任务。通常，每一个任务称为一个线程(thread), 它是线程控制的简称。可以同时运行一个以上线程的程序称为多线程程序（multithreaded)。

## 什么是线程

### 使用线程给其他任务提供机会

如果需要执行一个比较耗时的任务，应当并发地运行任务。

## 中断线程

当线程的run 方法执行方法体中最后一条语句后， 并经由执行return 语句返冋时， 或者出现了在方法中没有捕获的异常时， 线程将终止

没有可以强制线程终止的方法。然而， interrupt 方法可以用来请求终止线程。

当对一个线程调用interrupt 方法时，线程的中断状态将被置位。这是每一个线程都具有的boolean 标志。每个线程都应该不时地检査这个标志， 以判断线程是否被中断。

要想弄清中断状态是否被置位， 首先调用静态的Thread.currentThread 方法获得当前线
程， 然后调用islnterrupted 方法：

```java
while (!Thread.currentThread().islnterrupted() && more work to do)
{
	do more work
}
```

但是， 如果线程被阻塞， 就无法检测中断状态。这是产生InterruptedExceptioii 异常的地方。当在一个被阻塞的线程（调用sleep 或wait ) 上调用interrupt 方法时， 阻塞调用将会被Interrupted Exception 异常中断

在很多发布的代码中会发现InterruptedException 异常被抑制在很低的层次上， 像这样：

```java
void mySubTaskO
{
  try { sleep(delay); }
  catch (InterruptedException e) {} // Don't ignore!
}
```

不要这样做！ 如果不认为在catch 子句中做这一处理有什么好处的话，仍然有两种合理的选择：

1. 在catch 子句中调用Thread.currentThread().interrupt() 来设置中断状态。于是，调用者
   可以对其进行检测。

   ```java
   void mySubTaskO
   {
   	try { sleep(delay); }
   	catch (InterruptedException e) {
       Thread.currentThreadO -interruptO; 
     }
   }
   ```

2. 更好的选择是， 用throws InterruptedException 标记你的方法， 不采用try 语句块捕获异常。于是， 调用者（或者， 最终的run 方法）可以捕获这一异常。

```java
void mySubTaskO throws InterruptedException{
	sleep(delay);
}
```

## 线程状态

线程可以有如下6 种状态：

* New ( 新创建）
* Runnable (可运行）
* Blocked ( 被阻塞）
* Waiting ( 等待）
* Timed waiting (计时等待）
* Terminated ( 被终止）

### 新创建线程

当用new 操作符创建一个新线程时， 如newThread(r)， 该线程还没有开始运行。这意味着它的状态是new。当一个线程处于新创建状态时， 程序还没有开始运行线程中的代码。在线程运行之前还有一些基础工作要做。

### 可运行线程

一旦调用start 方法，线程处于runnable 状态。==一个可运行的线桿可能正在运行也可能没有运行==， 这取决于操作系统给线程提供运行的时间。

一旦一个线程开始运行，它不必始终保持运行。事实上，运行中的线程被中断，目的是为了让其他线程获得运行机会。线程调度的细节依赖于操作系统提供的服务。==抢占式调度系统==给每一个可运行线程一个时间片来执行任务。当时间片用完， 操作系统剥夺该线程的运行权， 并给另一个线程运行机会（见图14-4 )。当选择下一个线程时， 操作系统考虑线程的优先级。现在所有的桌面以及服务器操作系统都使用抢占式调度。

在具有多个处理器的机器上， 每一个处理器运行一个线程， 可以有多个线程并行运行。当然， 如果线程的数目多于处理器的数目， 调度器依然采用时间片机制。

### 被阻塞线程和等待线程

当线程处于被阻塞或等待状态时， 它暂时不活动。它不运行任何代码且消耗最少的资源。直到线程调度器重新激活它

* 当一个线程试图获取一个内部的对象锁（而不是javiutiUoncurrent 库中的锁)，而该锁被其他线程持有则该线程进人阻塞状态. 当所有其他线程释放该锁，并且线程调度器允许本线程持有它的时候，该线程将变成非阻塞状态。
* 当线程等待另一个线程通知调度器一个条件时， 它自己进入等待状态在调用Object.wait 方法或Thread.join 方法， 或者是等待java,util.concurrent 库中的Lock 或Condition 时， 就会出现这种情况。实际上，被阻塞状态与等待状态是有很大不同的。
* 有几个方法有一个超时参数。调用它们导致线程进人计时等待（ timed waiting ) 状态。这一状态将一直保持到超时期满或者接收到适当的通知。带有超时参数的方法有Thread.sleep 和Object.wait、Thread.join、Lock,tryLock 以及Condition.await 的计时版。

### 被终止的线程

线程因如下两个原因之一而被终止：

* 因为run 方法正常退出而自然死亡。
* 因为一个没有捕获的异常终止了nm 方法而意外死亡。

![image-20210705120135073](Image/image-20210705120135073.png)

## 线程属性

* 线程优先级
* 守护线程
* 线程组
* 处理未捕获异常的处理器

### 线程优先级

每一个线程有一个优先级。默认情况下， 一+线程继承它的父线程的优先级。可以用setPriority 方法提高或降低任何一个线程的优先级。可以将优先级设置为在MIN_PRIORITY (在Thread 类中定义为1 ) 与MAX_PRIORITY (定义为10 ) 之间的
任何值。NORM_PRIORITY 被定义为5。

每当线程调度器有机会选择新线程时， 它首先选择具有较高优先级的线程。但是，线程优先级是高度依赖于系统的。当虚拟机依赖于宿主机平台的线程实现机制时， Java 线程的优先级被映射到宿主机平台的优先级上， 优先级个数也许更多，也许更少。

### 守护线程

可以通过调用 t.setDaemon(true) ;

守护线程的唯一用途是为其他线程提供服务。计时线程就是一个例子，它定时地发送“ 计时器嘀嗒” 信号给其他线程或清空过时的高速缓存项的线程。当只剩下守护线程时， 虚拟机就退出了， 由于如果只剩下守护线程， 就没必要继续运行程序了。

守护线程应该永远不去访问固有资源， 如文件、数据库，因为它会在任何时候甚至在一个操作的中间发生中断。

### 未捕获异常处理器

## 同步

两个或两个以上的线程需要共享对同一数据的存取。如果两个线程存取相同的对象， 并且每一个线程都调用了一个修改该对象状态的方法，将会发线程彼此踩了对方的脚。根据各线程访问数据的次序可能会产生讹误的对象。这样一个情况通常称为竞争条件

### 竞争条件的一个例子

```java
public class UnsynchBankTest{
	public static final int NACCOUNTS = 100;
    public static final double INITIAL_BALANCE = 1000;
    public static final double MAX_AMOUNT = 1000;
    public static final int DELAY = 10;
	
	public static void main(String[] args){
		Bank bank = new Bank(NACCOUNTS, INITIAL_BALANCE);
		for (int i = 0; i < NACCOUNTS;i++){
		   int fromAccount=i;
		   Runnable r=()->{
		     try {
		         while( true ){
			         int toAccount = (int) (bank.size()* Math.random());
			         double amount = MAX_AMOUNT * Math.random();
			         bank.transfer(fromAccount,toAccount, amount);
			         Thread.sleep((int) (DELAY*Math.random()));
		         }
		     }catch (InterruptedException e) {
		     
		     }
		   };
		   Thread t = new Thread();
		   t.start();
		}
	}
}
```

```java
public class Bank{
	private final double[] accounts ;
	public Bank(int n, double initialBalance){
		this.accounts =new double[n];
		Arrays.fill(accounts, initialBalance);
	}
	
	public void transfer(int from, int to, double amount){
		if( accounts[from] < amount ){
			return;
		}
		System.out.print(Thread.currentThread());
		accounts[from] -= amount;
		System.out.printf(" %10.2f from %A to 9W", amount, from, to);
		accounts[to] += amount;
		System.out.printf(" Total Balance: X10.2fXn", getTotalBalance());
	}
	private double getTotalBalance(){
		double sum = 0;
		for (double a : accounts){
		    sum += a;
		}
		return sum;
	}
	public int size() {
	    return accounts.length;
	}
}
```

### 竞争条件详解

上一节中运行了一个程序，其中有几个线程更新银行账户余额。一段时间之后， 错误不知不觉地出现了， 总额要么增加， 要么变少。当两个线程试图同时更新同一个账户的时候，这个问题就出现了。

1. 将accounts[to] 加载到寄存器。
2. 增加amount。
3. 将结果写回accounts[to]。

现在， 假定第1 个线程执行步骤1 和2, 然后， 它被剥夺了运行权。假定第2 个线程被唤醒并修改了accounts 数组中的同一项。然后，第1 个线程被唤醒并完成其第3 步。这样， 这一动作擦去了第二个线程所做的更新。于是， 总金额不再正确。

### 锁对象(Lock)

有两种机制防止代码块受并发访问的干扰。Java 语言提供一个synchronized 关键字达到这一目的， 并且Java SE 5.0 引入了ReentrantLock 类.synchronized 关键字自动提供一个锁以及相关的“ 条件”， 对于大多数需要显式锁的情况， 这是很便利的。

```java
	private Lock lock = new ReentrantLock();
	public void transfer(int from, int to, double amount){
		//if( accounts[from] < amount ){
		//	return;
		//}
		lock.lock();
		try{
			System.out.print(Thread.currentThread());
			accounts[from] -= amount;
			System.out.printf(" %10.2f from %A to 9W", amount, from, to);
			accounts[to] += amount;
			System.out.printf(" Total Balance: X10.2fXn", getTotalBalance());
		}finally {
		    lock.unlock();
		}
	}
```



![image-20210705145109303](Image/image-20210705145109303.png)

### 条件对象

### synchronized 关键字

介绍了如何使用Lock 和Condition 对象。在进一步深人之前， 总结一下有关锁和条件的关键之处：

* 锁用来保护代码片段， 任何时刻只能有一个线程执行被保护的代码。
* 锁可以管理试图进入被保护代码段的线程。
* 锁可以拥有一个或多个相关的条件对象。
* 每个条件对象管理那些已经进入被保护的代码段但还不能运行的线程。

如果一个方法用synchronized 关键字声明，那么对象的锁将保护整个方法。也就是说，要调用该方法， 线程必须获得内部的对象锁。

```java
//换句话说
public synchronized void metho()
{
	method body
}
//等价于
public void methodQ
{
  this.intrinsidock.1ock();
  try
  {
    method body
  }	finally {
    this.intrinsicLock.unlockO; 
  }
}
```

### 同步阻塞

每一个Java 对象有一个锁。线程可以通过调用同步方法获得锁。还有另一种机制可以获得锁，通过进入一个同步阻塞。当线程进入如下形式的阻塞：

```java
synchronized (obj) // this is the syntax for a synchronized block
{
	critical section
}
```

```java
public class Bank
{
  private doublet] accounts;
  private Object lock = new Object();
  public void transfer(int from, int to, int amount)
  {
    synchronized (lock) // an ad-hoc lock
    {
      accounts[from] -= amount;
      accounts[to] += amount;
    }
    System.out.print1n(...)
  }
}
                   
```

