# 第十七章 抽象工厂\(Abstract Factory\)模式

### 抽象工厂模式简介：

就是对一组具有相同主题的工厂进行封装；

例如：生产一台PC机，使用工厂方法模式的话，一般会有cpu工厂，内存工厂，显卡工厂...但是使用抽象工厂模式的话，只有一个工厂就是PC工厂，但是一个PC工厂涵盖了cpu工厂，内存工厂，显卡工厂等要做的所有事；

注意这里的“相同主题”的概念，表示的是同一个产品族，不能将cpu工厂，面粉工厂封装成一个工厂，因为他们不属于同一个产品族；

* 另外，还有一个产品等级的概念，还是以生产PC机为例，所谓的产品等级指的是不同厂商生产的CPU，如Intel和AMD的CPU,他们是同一个产品等级，如果只涉及产品等级的话，是不需要应用抽象工厂模式，使用工厂方法模式即可；
* 工厂方法模式解决的范畴是产品等级（AMD处理器，Intel处理器等）；抽象工厂模式解决的范畴是产品族等级（联想PC、惠普PC等）；

### 角色：

抽象工厂

* 具体工厂
* 抽象产品
* 具体产品
* 产品使用者

### 说明：

* 具体工厂“继承”抽象工厂；
* 具体产品”继承“抽象产品；
* 每个具体工厂（如PC工厂）包含若干个子工厂方法（如cpu工厂方法、显卡工厂方法...），
  子工厂方法
  负责生产对应的具体子产品，所有具体子产品（cpu、内存、显卡...）组合成一个具体产品（如惠普XXX型号PC）；
* 产品使用者使用每个具体工厂生产的具体产品；

\([https://www.cnblogs.com/chenpi/p/5156801.html\](https://www.cnblogs.com/chenpi/p/5156801.html%29\)

案例\([http://blog.csdn.net/jason0539/article/details/44976775\)：](http://blog.csdn.net/jason0539/article/details/44976775%29：)

随着客户的要求越来越高，宝马车需要不同配置的空调和发动机等配件。于是这个工厂开始生产空调和发动机，用来组装汽车。这时候工厂有两个系列的产品:空调和发动机。

宝马320系列配置A型号空调和A型号发动机，宝马523系列配置B型号空调和B型号发动机。

`//抽象产品`

`public interface Engine {`

`}`

`//具体产品`

`public class EngineA implements Engine{`

```
public EngineA\(\) {

    System.out.println\("制造--&gt;EngineA"\);

}
```

`}`

`//具体产品`

`public class EngineB implements Engine{`

```
public EngineB\(\) {

    System.out.println\("制造--&gt;EngineB"\);

}
```

`}`

`//抽象产品`

`public interface AirCondition {`

`}`

`//具体产品`

`public class AirConditionA implements AirCondition{`

```
public AirConditionA\(\) {

    System.out.println\("制造--&gt;AirConditionA"\);

}
```

`}`

`//具体产品`

`public class AirConditionB implements AirCondition{`

```
public AirConditionB\(\) {

    System.out.println\("制造--&gt;AirConditionB"\);

}
```

`}`

`//抽象工厂`

`public interface Factory {`

```
public Engine createEngine\(\);

public AirCondition createAircondition\(\);
```

`}`

`//具体工厂`

`public class FactoryBMW320 implements Factory{`

```
@Override

public Engine createEngine\(\) {

    return new EngineA\(\);

}



@Override

public AirCondition createAircondition\(\) {

    return new AirConditionA\(\);

}
```

`}`

`//具体工厂`

`public class FactoryBMW523 implements Factory{`

```
@Override

public Engine createEngine\(\) {

    return new EngineB\(\);

}



@Override

public AirCondition createAircondition\(\) {

    return new AirConditionB\(\);

}
```

`}`

`//测试类`

`public class Test {`

```
public static void main\(String\[\] args\) {

    FactoryBMW320 factoryBMW320 = new FactoryBMW320\(\);

    factoryBMW320.createEngine\(\);

    factoryBMW320.createAircondition\(\);

    FactoryBMW523 factoryBMW523 = new FactoryBMW523\(\);

    factoryBMW523.createEngine\(\);

    factoryBMW523.createAircondition\(\);

}
```

`}`

运行结果：

![](/assets/image17_1.png)

总结：

无论是简单工厂模式，工厂方法模式，还是抽象工厂模式，他们都属于工厂模式，在形式和特点上也是极为相似的，他们的最终目的都是为了解耦。在使用时，我们不必去在意这个模式到底工厂方法模式还是抽象工厂模式，因为他们之间的演变常常是令人琢磨不透的。经常你会发现，明明使用的工厂方法模式，当新需求来临，稍加修改，加入了一个新方法后，由于类中的产品构成了不同等级结构中的产品族，它就变成抽象工厂模式了；而对于抽象工厂模式，当减少一个方法使的提供的产品不再构成产品族之后，它就演变成了工厂方法模式。

所以，在使用工厂模式时，只需要关心降低耦合度的目的是否达到了。

