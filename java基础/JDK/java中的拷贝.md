# java中的拷贝

## 深拷贝和浅拷贝的概念

在 Java 中，主要包括**基本数据类型**（元类型）和 **类的实例对象** 数据类型。而一般使用 『 **=** 』号做赋值操作的时候。对于基本数据类型，实际上是拷贝的它的值，但是对于对象而言，其实赋值的只是这个对象的引用，将原对象的引用传递过去，他们实际上还是指向的同一个对象。 

而浅拷贝和深拷贝就是在这个基础之上做的区分，如果在拷贝这个对象的时候，只对基本数据类型进行了拷贝，而对引用数据类型只是进行了引用的传递，而没有真实的创建一个新的对象，则认为是浅拷贝。反之，在对引用数据类型进行拷贝的时候，创建了一个新的对象，并且复制其内的成员变量，则认为是深拷贝。

1、浅拷贝：对基本数据类型进行值传递，对引用数据类型进行引用传递般的拷贝，此为浅拷贝。

![浅拷贝](assets/006tKfTcly1fij5l5nx2mj30e304o3yn.jpg)

2、深拷贝：对基本数据类型进行值传递，对引用数据类型，创建一个新的对象，并复制其内容，此为深拷贝。

![/深拷贝](assets/006tKfTcly1fij5l1dm3uj30fs05i74h.jpg)

## java中的浅拷贝

 Java 中，所有的 Class 都继承自 Object ，而在 Object 上，存在一个 clone() 方法，它被声明为了 `protected` ，所以我们可以在其子类中，使用它。而无论是浅拷贝还是深拷贝，都需要实现 clone() 方法，来完成操作。

下面通过代码来看下深拷贝和浅拷贝的区别：

```
public class Boss  implements Cloneable{
	public int age;
	public String name;
	public Car car;
	
	public Object clone() {
		try {
			return super.clone();
		} catch (CloneNotSupportedException e) {
			e.printStackTrace();
		}
		return null;
	}
}
```

```
public class Car {
	
	public String name;
	public String type;
	public String[] wheels;
}
```



```
public class CloneTest {

	public static void main(String[] args) {
		//构造拷贝对象
		Boss boss=new Boss();
		boss.age=30;
		boss.name="DaemonSu";
		//构造汽车
		Car car=new Car();
		car.name="swhite";
		car.type="ford";
		String[] wheels={"左一","右一"};
		car.wheels= wheels;
		boss.car=car;	
		Boss boss2=(Boss) boss.clone();//执行拷贝
		System.out.println(boss2==boss);
		System.out.println(boss2.name);
		System.out.println(boss2.car==boss.car);	
	}
}
```

最终执行结果是 false DaemonSu  true，说明两个boss实例是不同对象，但是两个boss对象的car属性是相同对象。这就是执行力浅拷贝。这里需要说明下下，Car是否是同一个对象和Car是否实现cloneable接口没有关系。

## java中的深拷贝

一般来说，java中实现深拷贝的方式主要有两种：序列化和clone方法

### 通过clone方法实现深拷贝

重写上面Boss类中的clone方法

```
	public Object clone() {
		try {
			DeepBoss boss=(DeepBoss) super.clone();
			DeepCar car=(DeepCar) this.car.clone();
			boss.car=car;
			return boss;
		} catch (CloneNotSupportedException e) {
			e.printStackTrace();
		}
		return null;
	}
```

将上面中的Car类修改成为是实现了Cloneable 接口的类

```
public class Car implements Cloneable {
	
	public String name;
	public String type;
	public String[] wheels;
	
	public Object clone() {
		try {
			return super.clone();
		} catch (CloneNotSupportedException e) {
			e.printStackTrace();
		}
		return null;
	}
}
```

同样执行上面main函数，得到的结果是 false  DaemonSu  false，其中car是不同对象。这就是实现了深拷贝。

### 通过序列化实现深拷贝

通过序列化实现深度拷贝其实就是将实例先变成流，然后再从流中还原数据

	public Object Serialize(Boss boss) throws Exception {
		 //通过序列化方法实现深拷贝
	    ByteArrayOutputStream bos=new ByteArrayOutputStream();
	    ObjectOutputStream oos=new ObjectOutputStream(bos);
	    oos.writeObject(boss);
	    oos.flush();
	    ObjectInputStream ois=new ObjectInputStream(new 		 ByteArrayInputStream(bos.toByteArray()));
	    Boss boss2=(Boss)ois.readObject();
	    
	    System.out.println(boss2==boss);
		System.out.println(boss2.name);
		System.out.println(boss2.car==boss.car);
	    return boss2;
	}
最终程序执行的结果是false  DaemonSu  false 说明car是两个不同的对象

详情参加java基础中的IO相关文章