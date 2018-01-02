# 第十一章 代理\(proxy\)模式

代理\(Proxy\)是一种设计模式,提供了对目标对象另外的访问方式;即通过代理对象访问目标对象.这样做的好处是:可以在目标对象实现的基础上,增强额外的功能操作,即扩展目标对象的功能。 这里使用到编程中的一个思想:不要随意去修改别人已经写好的代码或者方法,如果需改修改,可以通过代理的方式来扩展该方法。

### 1.1.静态代理 {#静态代理}

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



