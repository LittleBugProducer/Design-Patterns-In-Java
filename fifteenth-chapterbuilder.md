# 第十五章 构建者\(Builder\)模式

### 建造者模式介绍

建造者模式定义：

将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。

建造者模式包括的角色：

（1）Builder：给出一个抽象接口或抽象类，以规范产品的建造。这个接口规定要实现复杂对象的哪些部分的创建，并不涉及具体的对象部件的创建，一般由子类具体实现。

（2）ConcreteBuilder：Builder接口的实现类，并返回组建好对象实例。

（3）Director：调用具体建造者来创建复杂对象的各个部分，在指导者中不涉及具体产品的信息，只负责保证对象各部分完整创建或按某种顺序创建。

（4）Product：要创建的复杂对象，产品类。

建造者模式的使用场景：

（1）当产品有复杂的内部构造时（参数很多）。

（2）需要生产的产品的属性相互依赖，这些属性的赋值顺序比较重要时（因为在调用ConcreteBuilder的赋值方法时是有先后顺序的）。

\(http://blog.csdn.net/jason0539/article/details/44992733\)

案例:

`//Product`

`public class Person {`

`	private String head;`

`	private String body;`

`	private String foot;`

`	public String getHead() {`

`		return head;`

`	}`

`	public String getBody() {`

`		return body;`

`	}`

`	public String getFoot() {`

`		return foot;`

`	}`

`	public void setHead(String head) {`

`		this.head = head;`

`	}`

`	public void setBody(String body) {`

`		this.body = body;`

`	}`

`	public void setFoot(String foot) {`

`		this.foot = foot;`

`	}`

`	@Override`

`	public String toString() {`

`		return "Person [head=" + head + ", body=" + body + ", foot=" + foot + "]";`

`	}	`

`}`

`//男product`

`public class Man extends Person{`

`	public Man() {`

`		System.out.println("开始建造男人");`

`	}`

`}`

`//女Product`

`public class Woman extends Person{`

`	public Woman() {`

`		System.out.println("开始建造女人");`

`	}`

`}`

`//角色`

`public interface Builder {`

`	void buildHead();`

`	void buildBody();`

`	void buildFoot();`

`	Person buildPerson();`

`}`

`//具体角色`

`public class ManBuilder implements Builder{`

`	Person person;	`

`	public ManBuilder() {`

`		person = new Man();`

`	}	`

`	@Override`

`	public void buildHead() {`

`		person.setHead("建造男人的脑袋");`

`	}`

`	@Override`

`	public void buildBody() {`

`		person.setBody("建造男人的身体");`

`	}`

`	@Override`

`	public void buildFoot() {`

`		person.setFoot("建造男人的脚");`

`	}`

`	@Override`

`	public Person buildPerson() {`

`		return person;`

`	}	`

`}`

`//具体角色`

`public class WomanBuilder implements Builder{`

`	Person person;	`

`	public WomanBuilder() {`

`		person = new Woman();`

`	}	`

`	@Override`

`	public void buildHead() {`

`		person.setHead("建造女人的脑袋");`

`	}`

`	@Override`

`	public void buildBody() {`

`		person.setBody("建造女人的身体");`

`	}`

`	@Override`

`	public void buildFoot() {`

`		person.setFoot("建造女人的脚");`

`	}`

`	@Override`

`	public Person buildPerson() {`

`		return person;`

`	}`

`}`

`//角色 director`

`public class PersonDirector {`

`	public Person constructPerson(Builder p) {`

`		p.buildHead();`

`		p.buildBody();`

`		p.buildFoot();`

`		return p.buildPerson();`

`	}`

`}`

`//测试类`

`public class Test {`

`	public static void main(String[] args) {`

`		PersonDirector pd = new PersonDirector();`

`		Person womanPerson = pd.constructPerson(new WomanBuilder());`

`		System.out.println(womanPerson);`

`		Person manPerson = pd.constructPerson(new ManBuilder());`

`		System.out.println(manPerson);`

`	}`

`}`

运行结果：

![](/assets/image15_1.png)





