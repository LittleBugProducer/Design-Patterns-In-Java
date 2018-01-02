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

`//代理工厂类`

\`public class ProxyFactory {

\`

`private Object target;`

`public ProxyFactory(Object target) {`

`this.target = target;`

`}`

`public Object getProxyInstance() {`

`return Proxy.newProxyInstance(`

\`                target.getClass\(\).getClassLoader\(\), target.getClass\(\).getInterfaces\(\), new InvocationHandler\(\) {

\`

`@Override`

`public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {`

`System.out.println("开始事务2");`

`Object returnValue = method.invoke(target, args);`

`System.out.println("提交事务2");`

`return returnValue;`

`}`

`});`

`}`

`}`

`//测试类`

\`public class Test {

\`

`public static void main(String[] args) {`

`IUserDao target = new UserDao();`

`System.out.println(target.getClass());`

`IUserDao proxy = (IUserDao)new ProxyFactory(target).getProxyInstance();`

`System.out.println(proxy.getClass());`

`proxy.save();`

`}`

`}`

运行结果：

![](/assets/image11_2.png)

总结:

代理对象不需要实现接口,但是目标对象一定要实现接口,否则不能用动态代理

### Cglib代理 {#cglib代理}

上面的静态代理和动态代理模式都是要求目标对象是实现一个接口的目标对象,但是有时候目标对象只是一个单独的对象,并没有实现任何的接口,这个时候就可以使用以目标对象子类的方式类实现代理,这种方法就叫做:Cglib代理

Cglib代理,也叫作子类代理,它是在内存中构建一个子类对象从而实现对目标对象功能的扩展.

* JDK的动态代理有一个限制,就是使用动态代理的对象必须实现一个或多个接口,如果想代理没有实现接口的类,就可以使用Cglib实现.
* Cglib是一个强大的高性能的代码生成包,它可以在运行期扩展java类与实现java接口.它广泛的被许多AOP的框架使用,例如Spring AOP和synaop,为他们提供方法的interception\(拦截\)
* Cglib包的底层是通过使用一个小而块的字节码处理框架ASM来转换字节码并生成新的类.不鼓励直接使用ASM,因为它要求你必须对JVM内部结构包括class文件的格式和指令集都很熟悉.

Cglib子类代理实现方法:  
 1.需要引入cglib的jar文件和它的依赖文件asm-3.3.1.jar，cglib-2.2.2.jar。  
 2.引入功能包后,就可以在内存中动态构建子类  
 3.代理的类不能为final,否则报错  
 4.目标对象的方法如果为final/static,那么就不会被拦截,即不会执行目标对象额外的业务方法.

案例:

`//被代理对象`

\`public class UserDao {

\`

`public void save() {`

`System.out.println("----已经保存数据------");`

`}`

`}`

`//代理工厂`

\`public class ProxyFactory implements MethodInterceptor{

\`

\`    private Object target;

\`

`public ProxyFactory(Object target) {`

`this.target = target;`

\`    }

\`

`public Object getProxyInstance() {`

`Enhancer enhancer = new Enhancer();`

`enhancer.setSuperclass(target.getClass());`

`enhancer.setCallback(this);`

`return enhancer.create();`

\`    }

\`

`@Override`

`public Object intercept(Object arg0, Method arg1, Object[] arg2, MethodProxy arg3) throws Throwable {`

`System.out.println("开始事务3");`

`Object returnValue = arg1.invoke(target, arg2);`

`System.out.println("提交事务...");`

`return returnValue;`

\`    }\`

`}`

`//测试类`

\`public class Test {\`

`public static void main(String[] args) {`

`UserDao target = new UserDao();`

`System.out.println(target.getClass());`

`UserDao proxy = (UserDao)new ProxyFactory(target).getProxyInstance();`

`System.out.println(proxy.getClass());`

`proxy.save();`

`}`

`}`

运行结果：

![](/assets/image11_3.png)

 在Spring的AOP编程中:

 如果加入容器的目标对象有实现接口,用JDK代理

 如果目标对象没有实现接口,用Cglib代理

然而，运用代理模式会使得代理对象与被代理对象造成紧耦合。

