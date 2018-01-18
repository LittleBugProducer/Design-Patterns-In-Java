# 第二十三章 策略\(Strategy\)模式

策略模式的意图是将可互换的方法封装在各自独立的类中，并且让每个方法都实现一个公共的操作。

角色：

[环境类\(Context\)](http://blog.csdn.net/hguisu/article/details/7558249):用一个ConcreteStrategy对象来配置。维护一个对Strategy对象的引用。可定义一个接口来让Strategy访问它的数据。  
[抽象策略类\(Strategy\):](http://blog.csdn.net/hguisu/article/details/7558249)定义所有支持的算法的公共接口。 Context使用这个接口来调用某ConcreteStrategy定义的算法。  
[具体策略类\(ConcreteStrategy\)](http://blog.csdn.net/hguisu/article/details/7558249):以Strategy接口实现某具体算法。

案例：

出行旅游交通方式选择\([http://blog.csdn.net/hguisu/article/details/7558249/\](http://blog.csdn.net/hguisu/article/details/7558249/\)\)

`public interface TravelStrategy {`

`	void travelAlgorithm();`

`}`

`public class AirplaneStrategy implements TravelStrategy{`

`	@Override`

`	public void travelAlgorithm() {`

`		System.out.println("travel by air");`

`	}	`

`}`

`public class TrainStrategy implements TravelStrategy{`

`	@Override`

`	public void travelAlgorithm() {`

`		System.out.println("travel by train");`

`	}	`

`}`

`public class BicycleStrategy implements TravelStrategy{`

`	@Override`

`	public void travelAlgorithm() {`

`		System.out.println("travel by bike");`

`	}	`

`}`

`public class Context {`

`	private TravelStrategy travelStrategy = null;`

`	public Context(TravelStrategy travelStrategy) {`

`		this.travelStrategy = travelStrategy;		`

`	}`

`	public TravelStrategy getTravelStrategy() {`

`		return travelStrategy;`

`	}`

`	public void setTravelStrategy(TravelStrategy travelStrategy) {`

`		this.travelStrategy = travelStrategy;`

`	}	`

`	public void travel() {`

`		travelStrategy.travelAlgorithm();`

`	}`

`}`

`public class Test {`

`	public static void main(String[] args) {`

`		Context context = new Context(new AirplaneStrategy());`

`		context.travel();		`

`		context.setTravelStrategy(new TrainStrategy());`

`		context.travel();		`

`		context.setTravelStrategy(new BicycleStrategy());`

`		context.travel();`

`	}`

`}`

运行结果：

![](/assets/image23_1.png)





