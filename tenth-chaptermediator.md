# 第十章 调停者\(Mediator\)模式



### 定义

     调停者模式包装了一系列对象相互作用的方式，使得这些对象不必相互明显作用。从而使他们可以松散偶合。当某些对象之间的作用发生改变时，不会立即影响其他的一些对象之间的作用。保证这些作用可以彼此独立的变化。调停者模式将多对多的相互作用转化为一对多的相互作用。调停者模式将对象的行为和协作抽象化，把对象在小尺度的行为上与其他对象的相互作用分开处理。

### 调停者模式的优点

●　松散耦合

　调停者模式通过把多个同事对象之间的交互封装到调停者对象里面，从而使得同事对象之间松散耦合，基本上可以做到互补依赖。这样一来，同事对象就可以独立地变化和复用，而不再像以前那样“牵一处而动全身”了。

●　集中控制交互

　多个同事对象的交互，被封装在调停者对象里面集中管理，使得这些交互行为发生变化的时候，只需要修改调停者对象就可以了，当然如果是已经做好的系统，那么就扩展调停者对象，而各个同事类不需要做修改。

●　多对多变成一对多

　没有使用调停者模式的时候，同事对象之间的关系通常是多对多的，引入调停者对象以后，调停者对象和同事对象的关系通常变成双向的一对多，这会让对象的关系更容易理解和实现。

### 调停者模式的缺点

　　调停者模式的一个潜在缺点是，过度集中化。如果同事对象的交互非常多，而且比较复杂，当这些复杂性全部集中到调停者的时候，会导致调停者对象变得十分复杂，而且难于管理和维护。

### 应用场景：

一个系统内部通过许多的类互相之间相互调用来完成一系列的功能，这个系统内部的每个类都会存在至少一次的调用与被调用，多者数不胜数，这种情况下，一旦某个类发生问题，进行修改，无疑会影响到所有调用它的类，甚至它调用的类，可见这种情况下，类与类之间的耦合性极高（体现为太多的复杂的直接引用）。

这正是调停者模式的主场，调停者犹如第三方中介一般，将所有的类与类之间的引用都导向调停者类，所有类的请求，一致发向调停者，由调停者再发向目标类，这样原本复杂的网状的类关系，变成了简单的星型类关系，调停者类位于核心，所有其他类位于外围，指向调停者。如此这般，类与类之间的直接调用耦合被解除（通过统一的第三方来发起调用），某个类发生问题，发生修改，也只会影响调停者，而不会直接影响到简介发起调用的那些类。

\([https://www.cnblogs.com/V1haoge/p/6518603.html\](https://www.cnblogs.com/V1haoge/p/6518603.html\)\)

### 案例：

一个公司部门，有一个经理来充当调停者，其下的员工充当互相作用的类，这是一个很形象的实例。如果所有职员之间的互动都由职工之间直接进行，一旦某个员工不在，那么必须由此员工操作的事情便无法互动起来，或者某个员工被更换，员工之间不熟悉，也无法进行互动，这样，经理这个调停者的作用就来了，发起需求的员工将需求告诉经理，经理再找其他员工操作这个需求，明显的调停者模式。

`//职员抽象类`

\`public abstract class Engineer {

\`

`String name;`

`private Mediator mediator;`

`public Engineer(Mediator mediator,String name) {`

`this.mediator = mediator;`

`this.name = name;`

`}`

`//被调停者调用的方法`

`public void called(String message,String nname) {`

`System.out.println(name+"接收到来自"+nname+"的需求："+message);`

`}`

`//调用调停者`

`public void call(String message,Engineer engineer,String nname) {`

`System.out.println(nname+"发起需求："+message);`

`mediator.change(message, engineer, nname);`

`}`

`}`

`//调停者接口`

`public interface Mediator {`

\`    void change\(String message,Engineer engineer,String name\);

\`

`}`

`//调停者实现类`

\`public class Manager implements Mediator{

\`

`@Override`

`public void change(String message, Engineer engineer, String name) {`

`System.out.println("经理收到"+name+"的需求："+message);`

`System.out.println("经理将"+name+"的需求发送给目标职员");`

`engineer.called(message, name);`

\`    }

\`

`}`

`//员工类的实现`

\`public class Worker extends Engineer{

\`

`public Worker(Mediator mediator, String name) {`

`super(mediator, name);`

`}`

`}`

`//资料类`

\`public class Test {

\`

`public static void main(String[] args) {`

`Mediator man = new Manager();`

`Worker workerA = new Worker(man, "apple");`

`Worker workerB = new Worker(man, "banana");`

`Worker workerC = new Worker(man, "cat");`

`workerA.call("这些资料需要B操作", workerB, "apple");`

`workerB.call("这些资料需要C签名", workerC, "banana");`

`}`

`}`

运行结果：

![](/assets/image10_1.png)

