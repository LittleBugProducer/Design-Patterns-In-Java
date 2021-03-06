# 第十六章 工厂方法\(Factory Method\)模式

说出Java类库中两个可以返回新对象的常用方法：

### ![](/assets/image16_1.png)简单工厂模式：

简单工厂模式又称静态工厂方法模式。重命名上就可以看出这个模式一定很简单。它存在的目的很简单：定义一个用于创建对象的接口。

组成：

```
     1\) 工厂类角色：这是本模式的核心，含有一定的商业逻辑和判断逻辑，用来创建产品

     2\) 抽象产品角色：它一般是具体产品继承的父类或者实现的接口。         

     3\) 具体产品角色：工厂类所创建的对象就是此角色的实例。在java中由一个具体类实现。
```

案例：

`//抽象产品`

`public abstract class BMW {`

\`    public BMW\(\) {

\`

\`    }

\`

`}`

`//具体产品`

`public class BMW320 extends BMW{`

`public BMW320() {`

`System.out.println("制造-->BMW320");`

\`    }

\`

`}`

`//具体产品`

`public class BMW523 extends BMW{`

`public BMW523() {`

`System.out.println("制造-->BMW523");`

`}`

`}`

`//工厂类`

\`public class Factory {

\`

`public BMW createBMW(int type) {`

`switch (type) {`

`case 320:`

`return new BMW320();`

`case 523:`

`return new BMW523();`

`default:`

`break;`

`}`

`return null;`

`}`

`}`

`//测试类`

`public class Test {`

`public static void main(String[] args) {`

`Factory factory = new Factory();`

`BMW bmw320 = factory.createBMW(320);`

`BMW bmw523 = factory.createBMW(523);`

\`    }

\`

`}`

运行结果：

![](/assets/image16_2.png)

从开闭原则（对扩展开放；对修改封闭）上来分析下简单工厂模式。当客户不再满足现有的车型号的时候，想要一种速度快的新型车，只要这种车符合抽象产品制定的合同，那么只要通知工厂类知道就可以被客户使用了。所以对产品部分来说，它是符合开闭原则的；但是工厂部分好像不太理想,因为每增加一种新型车，都要在工厂类中增加相应的创建业务逻辑（createBMW\(int type\)方法需要新增case），这显然是违背开闭原则的。可想而知对于新产品的加入，工厂类是很被动的。对于这样的工厂类，我们称它为全能类或者上帝类。

而在实际应用中，很可能产品是一个多层次的树状结构。由于简单工厂模式中只有一个工厂类来对应这些产品，所以这可能会把我们的上帝累坏了，也累坏了我们这些程序员。 于是工厂方法模式作为救世主出现了。 工厂类定义成了接口,而每新增的车种类型,就增加该车种类型对应工厂类的实现,这样工厂的设计就可以扩展了,而不必去修改原来的代码。

### 工厂方法模式

工厂方法模式去掉了简单工厂模式中工厂方法的静态属性，使得它可以被子类继承。这样在简单工厂模式里集中在工厂方法上的压力可以由工厂方法模式里不同的工厂子类来分担。

工厂方法模式组成：

1\)抽象工厂角色： 这是工厂方法模式的核心，它与应用程序无关。是具体工厂角色必须实现的接口或者必须继承的父类。在java中它由抽象类或者接口来实现。

2\)具体工厂角色：它含有和具体业务逻辑有关的代码。由应用程序调用以创建对应的具体产品的对象。

3\)抽象产品角色：它是具体产品继承的父类或者是实现的接口。在java中一般有抽象类或者接口来实现。

4\)具体产品角色：具体工厂角色所创建的对象就是此角色的实例。在java中由具体的类来实现。

案例：

BMW、BMW320、BMW523类同上例

`//抽象工厂`

\`public interface FactoryBMW {

\`

`BMW createBMW();`

`}`

`//具体工厂`

\`public class FactoryBMW320 implements FactoryBMW{

\`

`@Override`

`public BMW createBMW() {`

`return new BMW320();`

\`    }

\`

`}`

`//具体工厂`

\`public class FactoryBMW523 implements FactoryBMW{

\`

`@Override`

`public BMW createBMW() {`

`return new BMW523();`

\`    }

\`

`}`

`//测试类`

\`public class Test {

\`

`public static void main(String[] args) {`

`FactoryBMW320 factoryBMW320 = new FactoryBMW320();`

`BMW320 bmw320 = (BMW320) factoryBMW320.createBMW();`

`FactoryBMW523 factoryBMW523 = new FactoryBMW523();`

`BMW523 bmw523 = (BMW523) factoryBMW523.createBMW();`

`}`

`}`

运行结果：

![](/assets/image16_3.png)

工厂方法模式使用继承自抽象工厂角色的多个子类来代替简单工厂模式中的“上帝类”。正如上面所说，这样便分担了对象承受的压力；而且这样使得结构变得灵活 起来——当有新的产品产生时，只要按照抽象产品角色、抽象工厂角色提供的合同来生成，那么就可以被客户使用，而不必去修改任何已有 的代码。可以看出工厂角色的结构也是符合开闭原则的！

\(http://blog.csdn.net/jason0539/article/details/44976775\)

