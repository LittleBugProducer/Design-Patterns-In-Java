# 第十一章 代理\(proxy\)模式

代理\(Proxy\)是一种设计模式,提供了对目标对象另外的访问方式;即通过代理对象访问目标对象.这样做的好处是:可以在目标对象实现的基础上,增强额外的功能操作,即扩展目标对象的功能。 这里使用到编程中的一个思想:不要随意去修改别人已经写好的代码或者方法,如果需改修改,可以通过代理的方式来扩展该方法。

### 静态代理 {#静态代理}

静态代理在使用时,需要定义接口或者父类,被代理对象与代理对象一起实现相同的接口或者是继承相同父类.

案例：

模拟保存动作,定义一个保存动作的接口:IUserDao.java,然后目标对象实现这个接口的方法UserDao.java,此时如果使用静态代理方式,就需要在代理对象\(UserDaoProxy.java\)中也实现IUserDao接口.调用的时候通过调用代理对象的方法来调用目标对象。 需要注意的是,代理对象与目标对象要实现相同的接口,然后通过调用相同的方法来调用目标对象的方法。

`//动作接口`

\`public interface IUserDao {

\`

`void save();`

`}`

`//被代理对象`

\`public class UserDao implements IUserDao{

\`

`@Override`

`public void save() {`

`System.out.println("------已经保存数据-------");`

`}`

`}`

`//代理`

\`public class UserDaoProxy implements IUserDao{

\`

`private IUserDao target;`

`public UserDaoProxy(IUserDao target) {`

\`        this.target = target;

\`

\`    }

\`

`@Override`

`public void save() {`

`System.out.println("开始事务...");`

`target.save();`

`System.out.println("提交事务");`

`}`

`}`

`//测试类`

\`public class Test {\`

`public static void main(String[] args) {`

`UserDao target = new UserDao();`

`UserDaoProxy proxy = new UserDaoProxy(target);`

`proxy.save();`

`}`

`}`

运行结果：

![](/assets/image11_1.png)

静态代理总结:  
 1.可以做到在不修改目标对象的功能前提下,对目标功能扩展.  
 2.缺点:

* 因为代理对象需要与目标对象实现一样的接口,所以会有很多代理类,类太多.同时,一旦接口增加方法,目标对象与代理对象都要维护.

### 动态代理 {#动态代理}

动态代理有以下特点:  
 1.代理对象,不需要实现接口  
 2.代理对象的生成,是利用JDK的API,动态的在内存中构建代理对象\(需要我们指定创建代理对象/目标对象实现的接口的类型\)  
 3.动态代理也叫做:JDK代理,接口代理

案例：

IUserDao接口和UserDao的实现与上例相同

`//代理工厂类`

`public class ProxyFactory {`

`	private Object target;`

`	public ProxyFactory(Object target) {`

`		this.target = target;`

`	}`

`	public Object getProxyInstance() {`

`		return Proxy.newProxyInstance(`

`				target.getClass().getClassLoader(), target.getClass().getInterfaces(), new InvocationHandler() {			`

`			@Override`

`			public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {`

`				System.out.println("开始事务2");`

`				Object returnValue = method.invoke(target, args);`

`				System.out.println("提交事务2");`

`				return returnValue;`

`			}`

`		});`

`	}`

`}`

`//测试类`

`public class Test {`

`	public static void main(String[] args) {`

`		IUserDao target = new UserDao();`

`		System.out.println(target.getClass());`

`		IUserDao proxy = (IUserDao)new ProxyFactory(target).getProxyInstance();`

`		System.out.println(proxy.getClass());`

`		proxy.save();`

`	}`

`}`

运行结果：

![](/assets/image11_2.png)



