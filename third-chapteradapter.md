# 第三章 适配者\(Adapter\)模式

3.1 类的适配器

类适配器是客户类有一个接口规范的情况下可用，此时适配类只需作为功能类的子类，并实现接口并可，直接用功能类实现了客户类的要求。\([http://blog.csdn.net/kkforwork/article/details/46348245](http://blog.csdn.net/kkforwork/article/details/46348245\)\)

类适配器案例\(http://blog.csdn.net/kkforwork/article/details/46348245\)：

`//已有功能类`

`public class Action {`

`public int ActionMethod() {`

`System.out.println("服务：提供一个苹果");`

`return 1;`

`}`

`}`

`//客户接口`

`public interface ICustomer {`

`public int CustomMethod();//需要把苹果切成10块`

`}`

`//适配器`

`public class AdapterClass extends Action implements ICustomer{`

`@Override`

`public int CustomMethod() {`

`int res = ActionMethod();`

`res*=10;`

`System.out.println("适配:把苹果切成块");`

`return res;`

`}`

`}`

`//测试类`

`public class Test {`

`public static void main(String[] args) {`

`ICustomer customer = new AdapterClass();`

`System.out.println(customer.CustomMethod()+"块");`

`}`

`}`

运行结果：

![](/assets/image3_1.png)

3.2 对象适配器

对象适配类是在客户类没有提供接口的情况下用的，适配类作为客户类的子类，并在其中实例化一个功能类的对象，并调用此对象的方法实现适配，故称对象适配。\([http://blog.csdn.net/kkforwork/article/details/46348245](http://blog.csdn.net/kkforwork/article/details/46348245\)\)

对象适配器案例\(http://blog.csdn.net/kkforwork/article/details/46348245\)：



