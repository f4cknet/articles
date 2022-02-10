[TOC]

### 科普时间

**对象**：java实例化一个类的时候，可以称为一个对象，或者一个实例，比如下方的myPuppy （其他方法忽略）

```java
Puppy myPuppy = new Puppy("tommy");
```

**抽象类**：父类是将子类所共同拥有的属性和方法进行抽取，这些属性和方法中，有的是已经明确实现了的，有的还无法确定，那么我们就可以将其定义成抽象，在以后的子类中进行重用，进行具体化(就是继承父类的方法名，重新定义方法内容)

`用abstract关键字来修饰抽象方法，用abstract来修饰抽象类`

**抽象方法**：在抽象类中的用abstract修饰的方法，只定义个方法名字，没有内容，方便子类继承这个类的时候具体将此方法内容定义出来 

```java
package javastudy;

public class AbstractDemo1 {

    public static void main(String[] args) {
        // TODO Auto-generated method stub

    }
}

// 这就是一个抽象类
abstract class Animal {
    String name;
    int age;

    // 动物会叫   这就是一个抽象方法
    public abstract void cry(); // 不确定动物怎么叫的。定义成抽象方法，来解决父类方法的不确定性。抽象方法在父类中不能实现，所以没有函数体。但在后续在继承时，要具体实现此方法。
}

// 抽象类可以被继承
// 当继承的父类是抽象类时，需要将抽象类中的所有抽象方法全部实现。
class cat extends Animal {
    // 实现父类的cry抽象方法
    public void cry() {
        System.out.println("猫叫:");

    }
}
```



**接口**：是抽象方法的集合，接口通常以interface来声明。一个类通过继承接口的方式，从而来继承接口的抽象方法。

```java
//一个简单的接口
interface in1 {
	// 默认为 Public Static Final 
	final int a =10;
	
	// 默认为抽象方法 public and abstract
	void display();

}
```

继承接口的方式是使用implements去实现接口，当你为了实现接口写的类中实现了接口所有的方法，才算这个类实现接口成功：

```java
class testClass implements in1 {
	// 实现接口中的方法
	public void display() {
		System.out.println("Study");
	}
}
```

 写一个测试类，用来测试一下刚才实现的这个接口，因为testclass类的对象t实现了接口规定的display方法，那么自然而然就可以调用display()方法咯。

```java
class testClass implements in1 {
	// 实现接口中的方法
	public void display() {
		System.out.println("Study");
	}
	
	public static void main (String[] args) {
		testClass t = new testClass();
		t.display();
		System.out.println(a);
	}
}
```

### 概念

* 序列化：把对象转换为字节序列的过程称为对象的序列化。

* 反序列化：把字节序列恢复为对象的过程称为对象的反序列化。

在Java中，我们可以通过多种方式来创建对象，并且只要对象没有被回收我们都可以复用此对象。但是，我们创建出来的这些对象都存在于JVM中的堆（heap）内存中，只有JVM处于运行状态的时候，这些对象才可能存在。一旦JVM停止，这些对象也就随之消失，但是在真实的应用场景中，我们需要将这些对象持久化下来，并且在需要的时候将对象重新读取出来，Java的序列化和反序列化可以帮助我们实现该功能。

主要两种用途：

1） 把对象的字节序列永久地保存到硬盘上，通常存放在一个文件中；

2） 在网络上传送对象的字节序列。

最常见的是Web服务器中的Session对象，当有 10万用户并发访问，就有可能出现10万个Session对象，内存可能吃不消，于是Web容器就会把一些session先序列化到硬盘中，等要用了，再把保存在硬盘中的对象还原到内存中。

### 相关接口及类

Java为了方便开发人员将java对象序列化及反序列化提供了一套方便的API来支持，其中包括以下接口和类

```java
java.io.Serializable   //接口1

java.io.Externalizable  //接口2

ObjectOutput

ObjectInput

ObjectOutputStream

ObjectInputStream
```

### Demo

* 使用Java.io.Serializable对类的序列化和反序列化

  ```java
  package com.company;
  
  import java.io.*;
  
  public class SerializableDemo1 {
  
      //建立User类实现java提供的Serializable接口
      public static class User implements Serializable {
          private String userName;
  
  
          public void setName(String userName) {
              this.userName = userName;
          }
  
          public String getName() {
              return userName;
          }
  
  
      }
      //序列化demo
      public static void main(String[] args) throws Exception, IOException {
  
          //初始化对象
          User resOld = new User();    //要序列化的对象
          resOld.setName("awsl");
          System.out.println("序列化前:" + resOld.getName());   //首尾呼应~
  
          //序列化对象到文件中
          FileOutputStream  creatFile = new FileOutputStream("ser");   //创建ser文件
          ObjectOutputStream serResult = new ObjectOutputStream(creatFile);
          serResult.writeObject(resOld);  //将序列化后的字节写入文件
          serResult.close();
  
          //反序列化
          FileInputStream readFile = new FileInputStream("ser");  //读取ser文件
          ObjectInputStream unserFile = new ObjectInputStream(readFile);  //读取序列化后的字节
          User resNew = (User) unserFile.readObject();    //反序列化还原对象
          unserFile.close();
  
          System.out.println("序列化后:" + resNew.getName());  //首尾呼应~
      }
  
  }
  ```

  <img src="/Users/sven/Desktop/image-20201216001105201.png" alt="image-20201216001105201" style="zoom:50%;" />

* 使用java.io.Externalizable对类的序列化和反序列化

  ``````java
  package com.company;
  
  import java.io.*;
  
  public class SerializableDemo2 {
  
      //建立Study类实现java提供的Externalizable接口
      public static class Study implements Externalizable {
          private String  myName;
  
          public void setName(String argsName){
              this.myName = argsName;
          }
  
          public String getName() {
              return this.myName;
          }
  
          @Override  //使用Externalizable接口会强制生成此类
          public void writeExternal(ObjectOutput out) throws IOException {
              out.writeObject(myName);   //不这样会导致序列化后，变量的值被清空
          }
  
          @Override  //使用Externalizable接口会强制生成此类
          public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
              myName = (String)in.readObject(); //不这样会导致序列化后，变量的值被清空
          }
      }
  
      public static void main(String[] args) throws IOException, ClassNotFoundException {
          //初始化参数
          Study resOld = new Study();
          resOld.setName("xmsl");
          System.out.println("序列化前:" + resOld.getName());  //首尾呼应
  
          //创建用来存储序列化字节码的文件
          FileOutputStream creatFile = new FileOutputStream("ser2");
  
          //序列化
          ObjectOutputStream serResult = new ObjectOutputStream(creatFile);
          serResult.writeObject(resOld);
          serResult.close();
          //读取刚才用来存储序列化字节码的文件
          FileInputStream readFile = new FileInputStream("ser2");
  
          //反序列化
          ObjectInputStream unserResult = new ObjectInputStream(readFile);
          Study resNew = (Study) unserResult.readObject();
          unserResult.close();
          System.out.println("序列化后:" + resNew.getName());  //首尾呼应
      }
    
  }
  
  ``````

  <img src="/Users/sven/Desktop/image-20201216215322290.png" alt="image-20201216215322290" style="zoom:50%;" />



### 其他

* 序列化保存的是对象的状态

* 静态变量属于类的状态，因此 序列化并不保存静态变量

* 使用**transient**修饰不想被序列化的变量，在变量声明前加上该关键字，可以阻止该变量被序列化到文件中，在被反序列化后，transient 变量的值被设为初始值，如 int 型的是 0，对象型的是 null。

  ```java
  public transient int youAge;
  ```

