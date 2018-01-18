# 第二十三章 策略\(Strategy\)模式

策略模式的意图是将可互换的方法封装在各自独立的类中，并且让每个方法都实现一个公共的操作。

角色：

[环境类\(Context\)](http://blog.csdn.net/hguisu/article/details/7558249):用一个ConcreteStrategy对象来配置。维护一个对Strategy对象的引用。可定义一个接口来让Strategy访问它的数据。  
[抽象策略类\(Strategy\):](http://blog.csdn.net/hguisu/article/details/7558249)定义所有支持的算法的公共接口。 Context使用这个接口来调用某ConcreteStrategy定义的算法。  
[具体策略类\(ConcreteStrategy\)](http://blog.csdn.net/hguisu/article/details/7558249):以Strategy接口实现某具体算法。

案例：

出行旅游交通方式选择\([http://blog.csdn.net/hguisu/article/details/7558249/\](http://blog.csdn.net/hguisu/article/details/7558249/%29\)

\`public interface TravelStrategy {

\`

`void travelAlgorithm();`

`}`

\`public class AirplaneStrategy implements TravelStrategy{

\`

`@Override`

`public void travelAlgorithm() {`

`System.out.println("travel by air");`

\`    }

\`

`}`

\`public class TrainStrategy implements TravelStrategy{

\`

`@Override`

`public void travelAlgorithm() {`

`System.out.println("travel by train");`

\`    }

\`

`}`

\`public class BicycleStrategy implements TravelStrategy{

\`

`@Override`

`public void travelAlgorithm() {`

`System.out.println("travel by bike");`

\`    }

\`

`}`

`public class Context {`

`private TravelStrategy travelStrategy = null;`

`public Context(TravelStrategy travelStrategy) {`

\`        this.travelStrategy = travelStrategy;

\`

`}`

`public TravelStrategy getTravelStrategy() {`

`return travelStrategy;`

`}`

`public void setTravelStrategy(TravelStrategy travelStrategy) {`

`this.travelStrategy = travelStrategy;`

\`    }

\`

`public void travel() {`

`travelStrategy.travelAlgorithm();`

\`    }

\`

`}`

\`public class Test {

\`

`public static void main(String[] args) {`

`Context context = new Context(new AirplaneStrategy());`

\`        context.travel\(\);

\`

`context.setTravelStrategy(new TrainStrategy());`

\`        context.travel\(\);

\`

`context.setTravelStrategy(new BicycleStrategy());`

`context.travel();`

`}`

`}`

运行结果：

![](/assets/image23_1.png)

### 优点：

1\) 相关算法系列

 Strategy类层次为Context定义了一系列的可供重用的算法或行为。 继承有助于析取出这些算法中的公共功能。

2\) 提供了可以替换继承关系的办法

继承提供了另一种支持多种算法或行为的方法。你可以直接生成一个Context类的子类，从而给它以不同的行为。但这会将行为硬行编制到 Context中，而将算法的实现与Context的实现混合起来,从而使Context难以理解、难以维护和难以扩展，而且还不能动态地改变算法。最后你得到一堆相关的类 , 它们之间的唯一差别是它们所使用的算法或行为。 将算法封装在独立的Strategy类中使得你可以独立于其Context改变它，使它易于切换、易于理解、易于扩展。

3\) 消除了一些if else条件语句

Strategy模式提供了用条件语句选择所需的行为以外的另一种选择。当不同的行为堆砌在一个类中时 ,很难避免使用条件语句来选择合适的行为。将行为封装在一个个独立的Strategy类中消除了这些条件语句。含有许多条件语句的代码通常意味着需要使用Strategy模式。

4\) 实现的选择

 Strategy模式可以提供相同行为的不同实现。客户可以根据不同时间 /空间权衡取舍要求从不同策略中进行选择。

### 缺点：

1\)客户端必须知道所有的策略类，并自行决定使用哪一个策略类

本模式有一个潜在的缺点，就是一个客户要选择一个合适的Strategy就必须知道这些Strategy到底有何不同。此时可能不得不向客户暴露具体的实现问题。因此仅当这些不同行为变体与客户相关的行为时 , 才需要使用Strategy模式。

2 \) Strategy和Context之间的通信开销

无论各个ConcreteStrategy实现的算法是简单还是复杂, 它们都共享Strategy定义的接口。因此很可能某些 ConcreteStrategy不会都用到所有通过这个接口传递给它们的信息；简单的 ConcreteStrategy可能不使用其中的任何信息！这就意味着有时Context会创建和初始化一些永远不会用到的参数。如果存在这样问题 , 那么将需要在Strategy和Context之间更进行紧密的耦合。

3 \)策略模式将造成产生很多策略类

可以通过使用享元模式在一定程度上减少对象的数量。 增加了对象的数目 Strategy增加了一个应用中的对象的数目。有时你可以将 Strategy实现为可供各Context共享的无状态的对象来减少这一开销。任何其余的状态都由 Context维护。Context在每一次对Strategy对象的请求中都将这个状态传递过去。共享的 Strategy不应在各次调用之间维护状态。

### 和状态模式的比较：

策略模式和其它许多设计模式比较起来是非常类似的。策略模式和状态模式最大的区别就是策略模式只是的条件选择只执行一次，而状态模式是随着实例参数（对象实例的状态）的改变不停地更改执行模式。换句话说，策略模式只是在对象初始化的时候更改执行模式，而状态模式是根据对象实例的周期时间而动态地改变对象实例的执行模式。

可以通过环境类状态的个数来决定是使用策略模式还是状态模式。•

策略模式的环境类自己选择一个具体策略类，具体策略类无须关心环境类；而状态模式的环境类由于外在因素需要放进一个具体状态中，以便通过其方法实现状态的切换，因此环境类和状态类之间存在一种双向的关联关系。

使用策略模式时，客户端需要知道所选的具体策略是哪一个，而使用状态模式时，客户端无须关心具体状态，环境类的状态会根据用户的操作自动转换。

如果系统中某个类的对象存在多种状态，不同状态下行为有差异，而且这些状态之间可以发生转换时使用状态模式；如果系统中某个类的某一行为存在多种实现方式，而且这些实现方式可以互换时使用策略模式。





