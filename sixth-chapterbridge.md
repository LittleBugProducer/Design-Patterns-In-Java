# 第六章 桥接\(Bridge\)模式

### 概念：

将抽象与抽象方法的实现相互分离来实现解耦，以便二者可以相互独立地变化。

### 重要核心模块：

Abstraction（抽象类）

定义抽象类的接口，它一般是抽象类而不是接口，其中定义了一个Implementor（实现类接口）类型的对象并可以维护该对象，它与Implementor之间具有关联关系，它既可以包含抽象业务方法，也可以包含具体业务方法。

RefinedAbstraction（扩充抽象类）

扩充由Abstraction定义的接口，通常情况下它不再是抽象类而是具体类，它实现了在Abstraction中声明的抽象业务方法，在RefinedAbstraction中可以调用在Implementor中定义的业务方法。

Implementor（实现类接口）

定义实现类的接口，这个接口不一定要与Abstraction的接口完全一致，事实上这两个接口可以完全不同，一般而言，Implementor接口仅提供基本操作，而Abstraction定义的接口可能会做更多更复杂的操作。Implementor接口对这些基本操作进行了声明，而具体实现交给其子类。通过关联关系，在Abstraction中不仅拥有自己的方法，还可以调用到Implementor中定义的方法，使用关联关系来替代继承关系。

ConcreteImplementor（具体实现类）

具体实现Implementor接口，在不同的ConcreteImplementor中提供基本操作的不同实现，在程序运行时，ConcreteImplementor对象将替换其父类对象，提供给抽象类具体的业务操作方法。

### **特点：**

桥接模式中体现了“单一职责原则”、“开闭原则”、“合成复用原则”、“里氏代换原则”、“依赖倒转原则”等。

### **技巧：**

在使用桥接模式时，首先应该识别出一个类所具有的多个独立变化的维度，将它们设计为多个独立的继承等级结构，为多个维度都提供抽象层，并建立抽象耦合。通常情况下，我们将具有多个独立变化维度的类的一些普通业务方法和与之关系最密切的维度设计为“抽象类”层次结构（抽象部分），而将另一个维度设计为“实现类”层次结构（实现部分）。

\([http://blog.csdn.net/yanbober/article/details/45366781\](http://blog.csdn.net/yanbober/article/details/45366781\)\)

案例\(http://blog.csdn.net/yanbober/article/details/45366781\):

**假设问题环境：**

就拿程序猿编程来作为例子说明。程序猿既有移动开发攻城狮，也有服务器开发攻城狮，也有嵌入式开发攻城狮，他们可以使用Windows开发、也可以使用Linux开发，还可以使用MAC OS开发。这时你会发现对于程序猿来说他们有不同的类型，对于开发平台来说他们也有不同的类型。在软件系统中要适应两个方面（不同的程序猿和不的平台）的变化我们就需要动动脑袋了。

先看最顺手的继承方式代码实现：

`class Platform{`

`	public void program() {`

`		System.out.println("使用的平台");`

`	}`

`}`

`class WindowsPlatform extends Platform{`

`	@Override`

`	public void program() {`

`		System.out.println("使用Windows平台");`

`	}`

`}`

`class LinuxPlatform extends Platform{`

`	@Override`

`	public void program() {`

`		System.out.println("使用Linux平台");`

`	}	`

`}`

`class ServerWindowsPlatform extends WindowsPlatform{`

`	@Override`

`	public void program() {`

`		System.out.println("服务器攻城狮使用Windows平台开发");`

`	}`

`}`

`class ServerLinuxPlatform extends LinuxPlatform{`

`	@Override`

`	public void program() {`

`		System.out.println("服务器攻城狮使用Linux平台");`

`	}`

`}`

`class MobileWindowsPlatform extends WindowsPlatform{`

`	@Override`

`	public void program() {`

`		System.out.println("移动端攻城狮使用window平台");`

`	}`

`}`

`class MobileLinuxPlatform extends LinuxPlatform{`

`	@Override`

`	public void program() {`

`		System.out.println("移动端攻城狮使用Linux平台");`

`	}`

`}`

`public class Test {`

`	public static void main(String[] args) {`

`		ServerLinuxPlatform serverLinuxPlatform = new ServerLinuxPlatform();`

`		serverLinuxPlatform.program();`

`		ServerWindowsPlatform serverWindowsPlatform = new ServerWindowsPlatform();`

`		serverWindowsPlatform.program();`

`		MobileWindowsPlatform mobileWindowsPlatform = new MobileWindowsPlatform();`

`		mobileWindowsPlatform.program();`

`		MobileLinuxPlatform mobileLinuxPlatform = new MobileLinuxPlatform();`

`		mobileLinuxPlatform.program();`

`	}	`

`}`

运行结果:

![](/assets/image6_1.png)

利用桥接模式升级:

`//Implementor实现类接口`

`public abstract class Monkey {`

`	public abstract void type();`

`}`

`//ConcreteImplementor具体实现类`

`public class ServerMonkey  extends Monkey{`

`	@Override`

`	public void type() {`

`		System.out.print("服务器攻城狮");`

`	}`

`}`

`//ConcreteImplementor具体实现类`

`public class MobileMonkey extends Monkey{`

`	@Override`

`	public void type() {`

`		System.out.print("移动端攻城狮");`

`	}`

`}`

`//Abstraction抽象类`

`public abstract class Platform {`

`	protected Monkey monkey;	`

`	public abstract void program();`

`}`

`//RefinedAbstraction扩充抽象类`

`public class WindowsPlatform extends Platform{`

`	public WindowsPlatform(Monkey monkey) {`

`		this.monkey = monkey;`

`	}	`

`	@Override`

`	public void program() {`

`		monkey.type();`

`		System.out.println("使用window平台!");`

`	}`

`}`

`//RefinedAbstraction扩充抽象类`

`public class LinuxPlatform extends Platform{`

`	public LinuxPlatform(Monkey monkey) {`

`		this.monkey = monkey;`

`	}	`

`	@Override`

`	public void program() {`

`		monkey.type();`

`		System.out.println("使用linux平台!");`

`	}`

`}`

`//测试类`

`public class Test {`

`	public static void main(String[] args) {`

`		Monkey monkeyS = new ServerMonkey();`

`		Monkey monkeyM = new MobileMonkey();`

`		Platform platform = new WindowsPlatform(monkeyS);`

`		platform.program();`

`		platform = new WindowsPlatform(monkeyM);`

`		platform.program();`

`		platform = new LinuxPlatform(monkeyS);`

`		platform.program();`

`		platform = new LinuxPlatform(monkeyM);`

`		platform.program();`

`	}`

`}`

运行结果：

![](/assets/image6_2.png)



