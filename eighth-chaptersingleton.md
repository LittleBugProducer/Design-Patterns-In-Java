# 第八章 单例\(Singleton\)模式

单例模式有以下特点：

* 单例类只能有一个实例。
* 单例类必须自己创建自己的唯一实例。
* 单例类必须给所有其他对象提供这一实例。

单例模式的实现：

懒汉式单例VS饿汉式单例\([http://blog.csdn.net/jason0539/article/details/23297037/\](http://blog.csdn.net/jason0539/article/details/23297037/%29\)

从名字上来说，饿汉和懒汉，

饿汉就是类一旦加载，就把单例初始化完成，保证getInstance的时候，单例是已经存在的了。而懒汉比较懒，只有当调用getInstance的时候，才回去初始化这个单例。

另外从以下两点再区分以下这两种方式：

1、线程安全：

饿汉式天生就是线程安全的，可以直接用于多线程而不会出现问题，

懒汉式本身是非线程安全的，为了实现线程安全有几种写法，分别是下面的1、2、3，这三种实现在资源加载和性能方面有些区别。

2、资源加载和性能：

饿汉式在类创建的同时就实例化一个静态对象出来，不管之后会不会使用这个单例，都会占据一定的内存，但是相应的，在第一次调用时速度也会更快，因为其资源已经初始化完成，

而懒汉式顾名思义，会延迟加载，在第一次使用该单例的时候才会实例化对象出来，第一次调用时要做初始化，如果要做的工作比较多，性能上会有些延迟，之后就和饿汉式一样了。

至于1、2、3这三种实现又有些区别，

第1种，在方法调用上加了同步，虽然线程安全了，但是每次都要同步，会影响性能，毕竟99%的情况下是不需要同步的，

第2种，在getInstance中做了两次null检查，确保了只有第一次调用单例的时候才会做同步，这样也是线程安全的，同时避免了每次都同步的性能损耗

第3种，利用了classloader的机制来保证初始化instance时只有一个线程，所以也是线程安全的，同时没有性能损耗，所以一般我倾向于使用这一种。

一：懒汉式单例

`public class Singleton0 {`

`private Singleton0() {}`

`private static Singleton0 single=null;`

`public static Singleton0 getInstance() {`

`if(single==null) {`

`single = new Singleton0();`

`}`

`return single;`

`}`

`}`

Singleton通过将构造方法限定为private避免了类在外部被实例化，在同一个虚拟机范围内，Singleton的唯一实例只能通过getInstance\(\)方法访问。事实上，通过Java反射机制是能够实例化构造方法为private的类的，那基本上会使所有的Java单例实现失效。此问题在此处不做讨论，姑且掩耳盗铃地认为反射机制不存在。）

但是以上懒汉式单例的实现没有考虑线程安全问题，它是线程不安全的，并发环境下很可能出现多个Singleton实例，要实现线程安全，有以下三种方式，都是对getInstance这个方法改造，保证了懒汉式单例的线程安全。

1、在getInstance方法上加同步

`public class Singleton1 {`

`private Singleton1() {}`

`private static Singleton1 single=null;`

`public static synchronized Singleton1 getInstance() {`

`if(single==null) {`

`single = new Singleton1();`

`}`

`return single;`

`}`

`}`

2、双重检查锁定

`public class Singleton2 {`

`private Singleton2() {}`

`private static Singleton2 single=null;`

`public static synchronized Singleton2 getInstance() {`

`if(single==null) {`

`synchronized (Singleton2.class) {`

`if(single==null) {`

`single = new Singleton2();`

`}`

`}`

`}`

`return single;`

`}`

`}`

3、静态内部类

`public class Singleton3 {`

`private static class LazyHolder{`

`private static final Singleton3 INSTANCE = new Singleton3();`

`}`

`private Singleton3() {}`

`public static final Singleton3 getInstance() {`

`return LazyHolder.INSTANCE;`

`}`

`}`

这种比上面1、2都好一些，既实现了线程安全，又避免了同步带来的性能影响。

二、饿汉式单例

`public class Singleton4 {`

`private Singleton4() {}`

`private static final Singleton4 single = new Singleton4();`

`public static Singleton4 getInstance() {`

`return single;`

`}`

`}`

三：枚举\([http://blog.csdn.net/qq\_29542611/article/details/52905516\](http://blog.csdn.net/qq_29542611/article/details/52905516\)\)

`public enum Singleton5 {`

`SingletonEnum("单例的枚举方式",34);`

`private String str;`

`private int num;`

`public int getNum() {`

`return num;`

`}`

`public void setNum(int num) {`

`this.num = num;`

`}`

`public String getStr() {`

`return str;`

`}`

`public void setStr(String str) {`

`this.str = str;`

`}`

`private Singleton5(String str,int num) {`

`this.setStr(str);`

`this.setNum(num);`

`}`

`}`

`//单例模式的测试类`

`public class Test {`

`public static void main(String[] args) {`

`Singleton5 in = Singleton5.SingletonEnum;`

`System.out.println(in.getStr());`

`System.out.println(in.getNum());`

`}`

`}`

运行结果：

![](/assets/image8_1.png)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

恶汉式、懒汉式的方式还不能防止反射来实现多个实例，通过反射的方式，设置ACcessible.setAccessible方法可以调用私有的构造器，可以修改构造器，让它在被要求创建第二个实例的时候抛出异常。

其实这样还不能保证单例，当序列化后，反序列化是还可以创建一个新的实例，在单例类中添加readResolve\(\)方法进行防止。

代码如下：

`public class Singleton implements Serializable{`

`	private static final long serialVersionUID = 1L;`

`	private static Singleton instance = null;`

`	private static int i = 1;`

`	private Singleton() {`

`		if(i == 1) {`

`			i++;`

`		}else {`

`			throw new RuntimeException("只能调用一次构造函数");`

`		}`

`		System.out.println("调用Singleton的私有构造器");`

`	}`

`	public static synchronized Singleton getInstance() {`

`		if(instance==null) {`

`			synchronized (Singleton.class) {`

`				if(instance==null) {`

`					instance = new Singleton();`

`				}				`

`			}`

`		}`

`		return instance;`

`	}`

`	public Object readResolve() {`

`		return instance;`

`	}`

`	public static void main(String[] args) throws Exception{`

`		//test2();`

`		test1();`

`	}`

`	public static void test1()throws Exception{`

`		Singleton singleton = Singleton.getInstance();`

`		Class c = Singleton.class;`

`		Constructor privateConstructor;`

`		try {`

`			privateConstructor = c.getDeclaredConstructor();`

`			privateConstructor.setAccessible(true);`

`			privateConstructor.newInstance();`

`		}catch (Exception e) {`

`			// TODO: handle exception`

`			e.printStackTrace();`

`		}`

`	}`

`	public static void test2()throws Exception{`

`		Singleton s = Singleton.getInstance();`

`		ObjectOutputStream objectOutputStream = new ObjectOutputStream(`

`				new FileOutputStream(new File("C:\\Users\\lC\\Desktop\\Singleton.txt")));`

`		objectOutputStream.writeObject(s);`

`		ObjectInputStream objectInputStream = new ObjectInputStream(`

`				new FileInputStream(new File("C:\\Users\\lC\\Desktop\\Singleton.txt")));`

`		Singleton s1 = (Singleton)objectInputStream.readObject();`

`		System.out.println("s.hashCode():"+s.hashCode()+",s1.hashCode():"+s1.hashCode());`

`		objectOutputStream.flush();`

`		objectOutputStream.close();`

`		objectInputStream.close();`

`	}`

`}`

运行结果：

不添加readResolve时test2：

![](/assets/image8_2.png)

添加readResolve时test2：

![](/assets/image8_3.png)

test1运行结果：

![](/assets/image8_4.png)

