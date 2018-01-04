# 第十三章 享元\(Flyweight\)模式

享元模式有点类似于[单例模式](http://www.cnblogs.com/V1haoge/p/6510196.html)，都是只生成一个对象来被共享使用。这里有个问题，那就是对共享对象的修改，为了避免出现这种情况，我们将这些对象的公共部分，或者说是不变化的部分抽取出来形成一个对象。这个对象就可以避免到修改的问题。

享元的目的是为了减少不会要额内存消耗，将多个对同一对象的访问集中起来，不必为每个访问者创建一个单独的对象，以此来降低内存的消耗。\(https://www.cnblogs.com/V1haoge/p/6542449.html\)

案例:

`//建筑接口`

`public interface Jianzhu {`

`	void use();`

`}`

`//体育馆`

`public class TiYuGuan implements Jianzhu{`

`	private String name;`

`	private String shape;`

`	private String yundong;	`

`	public TiYuGuan(String yundong) {`

`		this.setYundong(yundong);		`

`	}`

`	public String getName() {`

`		return name;`

`	}`

`	public String getShape() {`

`		return shape;`

`	}`

`	public String getYundong() {`

`		return yundong;`

`	}`

`	public void setName(String name) {`

`		this.name = name;`

`	}`

`	public void setShape(String shape) {`

`		this.shape = shape;`

`	}	`

`public void setYundong(String yundong) {`

`		this.yundong = yundong;`

`	}`

`	@Override`

`	public void use() {`

`		System.out.println("该体育馆用来举办"+yundong+"运动"+"形状为："+shape+"名称为"+name);`

`	}`

`}`

`//建筑工厂实现类`

`import java.util.HashMap;`

`import java.util.Map;`

`public class JianZhuFactory {`

`	private static final Map<String, TiYuGuan>tygs = new HashMap<>();`

`	public static TiYuGuan getTyg(String yundong) {`

`		TiYuGuan tyg = tygs.get(yundong);`

`		if(tyg==null) {`

`			tyg = new TiYuGuan(yundong);`

`			tygs.put(yundong, tyg);`

`		}`

`		return tyg;`

`	}`

`	public static int getSize() {`

`		return tygs.size();`

`	}`

`}`

`//测试类`

`public class Test {`

`	public static void main(String[] args) {`

`		String yundong = "足球";`

`		String bas = "篮球";`

`		for(int i = 0;i<5;i++) {`

`			TiYuGuan tyg = JianZhuFactory.getTyg(yundong);`

`			tyg.setName("china");`

`			tyg.setShape("round");`

`			tyg.use();`

`			TiYuGuan t2 = JianZhuFactory.getTyg(bas);`

`			t2.setName("cobe");`

`			t2.setShape("line");`

`			t2.use();`

`			System.out.println("factory中对象数量为:"+JianZhuFactory.getSize());`

`		}`

`	}`

`}`

运行结果:

![](/assets/image13_1.png)

### 模式的优缺点

### 模式的优点

减少对象数量，节省内存空间。

### 模式的缺点

维护共享对象，需要额外开销。（比如：一个线程来回收垃圾）

### 模式本质

分离和共享

### 开发中的应用场景：

* 享元模式由于其共享的特征，可以在任何“池”中操作，比如：线程池，数据库连接池。
* String类的设计也是享元模式。

\(https://www.cnblogs.com/linghu-java/p/5728120.html\)



