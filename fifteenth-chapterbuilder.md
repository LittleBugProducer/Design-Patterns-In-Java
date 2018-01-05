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

\([http://blog.csdn.net/jason0539/article/details/44992733\](http://blog.csdn.net/jason0539/article/details/44992733%29\)

案例:

`//Product`

\`public class Person {

\`

`private String head;`

`private String body;`

`private String foot;`

`public String getHead() {`

`return head;`

`}`

`public String getBody() {`

`return body;`

`}`

`public String getFoot() {`

`return foot;`

`}`

`public void setHead(String head) {`

`this.head = head;`

`}`

`public void setBody(String body) {`

`this.body = body;`

`}`

`public void setFoot(String foot) {`

`this.foot = foot;`

`}`

`@Override`

`public String toString() {`

`return "Person [head=" + head + ", body=" + body + ", foot=" + foot + "]";`

`}`

`}`

`//男product`

\`public class Man extends Person{

\`

`public Man() {`

`System.out.println("开始建造男人");`

`}`

`}`

`//女Product`

\`public class Woman extends Person{

\`

`public Woman() {`

`System.out.println("开始建造女人");`

`}`

`}`

`//角色`

\`public interface Builder {

\`

`void buildHead();`

`void buildBody();`

`void buildFoot();`

`Person buildPerson();`

`}`

`//具体角色`

\`public class ManBuilder implements Builder{

\`

\`    Person person;

\`

`public ManBuilder() {`

`person = new Man();`

\`    }

\`

`@Override`

`public void buildHead() {`

`person.setHead("建造男人的脑袋");`

\`    }

\`

`@Override`

`public void buildBody() {`

`person.setBody("建造男人的身体");`

\`    }

\`

`@Override`

`public void buildFoot() {`

`person.setFoot("建造男人的脚");`

\`    }

\`

`@Override`

`public Person buildPerson() {`

`return person;`

\`    }

\`

`}`

`//具体角色`

\`public class WomanBuilder implements Builder{

\`

\`    Person person;

\`

`public WomanBuilder() {`

`person = new Woman();`

\`    }

\`

`@Override`

`public void buildHead() {`

`person.setHead("建造女人的脑袋");`

\`    }

\`

`@Override`

`public void buildBody() {`

`person.setBody("建造女人的身体");`

\`    }

\`

`@Override`

`public void buildFoot() {`

`person.setFoot("建造女人的脚");`

`}`

`@Override`

`public Person buildPerson() {`

`return person;`

`}`

`}`

`//角色 director`

\`public class PersonDirector {

\`

`public Person constructPerson(Builder p) {`

`p.buildHead();`

`p.buildBody();`

`p.buildFoot();`

`return p.buildPerson();`

`}`

`}`

`//测试类`

`public class Test {`

`public static void main(String[] args) {`

`PersonDirector pd = new PersonDirector();`

`Person womanPerson = pd.constructPerson(new WomanBuilder());`

`System.out.println(womanPerson);`

`Person manPerson = pd.constructPerson(new ManBuilder());`

`System.out.println(manPerson);`

\`    }

\`

`}`

运行结果：

![](/assets/image15_1.png)

建造者模式在使用过程中可以演化出多种形式：

如果具体的被建造对象只有一个的话，可以省略抽象的Builder和Director，让ConcreteBuilder自己扮演指导者和建造者双重角色，甚至ConcreteBuilder也可以放到Product里面实现。

在《Effective Java》书中第二条，就提到“遇到多个构造器参数时要考虑用构建器”，其实这里的构建器就属于建造者模式，只是里面把四个角色都放到具体产品里面了。

《Effective Java》书中对Builder模式的描述：

当遇到许多构造器参数时，不直接生成想要的对象，而是让客户端利用必要的参数调用构造器\(或者静态工厂\)，得到一个builder对象，然后客户端在builder对象上调用类似于setter的方法，来设置每个相关的可选参数，最后，客户端调用无参的builder方法来生成不可变的对象。这个builder是它构建的类的静态成员。

案例：

如果上例只需构建男人：

`//Product`

\`public class Man {

\`

`private String head;`

`private String body;`

`private String foot;`

`public String getHead() {`

`return head;`

`}`

`public String getBody() {`

`return body;`

`}`

`public String getFoot() {`

`return foot;`

`}`

`public void setHead(String head) {`

`this.head = head;`

`}`

`public void setBody(String body) {`

`this.body = body;`

`}`

`public void setFoot(String foot) {`

`this.foot = foot;`

`}`

`@Override`

`public String toString() {`

`return "Man [head=" + head + ", body=" + body + ", foot=" + foot + "]";`

`}`

`}`

`//Builder`

`public class ManBuilder {`

`Man man;`

`public ManBuilder() {`

`man = new Man();`

`}`

`public void buildHead() {`

\`        man.setHead\("建造男人的头"\);

\`

`}`

`public void buildBody() {`

`man.setBody("建造男人的身体");`

`}`

`public void buildFoot() {`

`man.setFoot("建造男人的脚");`

\`    }

\`

`public Man buildMan() {`

`buildHead();`

`buildBody();`

`buildFoot();`

`return man;`

\`    }

\`

`}`

`//测试类`

\`public class Test {

\`

`public static void main(String[] args) {`

`ManBuilder builder = new ManBuilder();`

`Man man = builder.buildMan();`

`System.out.println(man);`

`}`

`}`

运行结果：

![](/assets/image15_2.png)

第二个例子更像我们通常构建对象的方式，是构建者模式的精简版。

### **建造者模式优缺点**

建造者模式的优点：

（1）建造模式是将复杂的内部创建封装在内部，对于外部调用的人来说，只需要传入建造者和建造工具，对于内部是如何建造成成品的，调用者无需关心，良好的封装性是建造者模式的优点之一。

（2）建造者类逻辑独立，易拓展。

建造者模式的缺点：

很明显产生了多余的Build对象以及Dirextor对象，消耗了内存。

\([http://blog.csdn.net/seu\_calvin/article/details/52249885\](http://blog.csdn.net/seu_calvin/article/details/52249885%29\)

