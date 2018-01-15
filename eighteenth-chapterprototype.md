# 第十八章 原型\(Prototype\)模式

原型模式是一种创建型设计模式,它通过复制一个已经存在的实例来返回新的实例,而不是新建实例.被复制的实例就是我们所称的原型,这个原型是可定制的.  
原型模式多用于创建复杂的或者耗时的实例, 因为这种情况下,复制一个已经存在的实例可以使程序运行更高效,或者创建值相等,只是命名不一样的同类数据.

原型模式中的拷贝分为"浅拷贝"和"深拷贝":  
浅拷贝: 对值类型的成员变量进行值的复制,对引用类型的成员变量只复制引用,不复制引用的对象.  
深拷贝: 对值类型的成员变量进行值的复制,对引用类型的成员变量也进行引用对象的复制.

\([https://www.cnblogs.com/itTeacher/archive/2012/12/02/2797857.html\](https://www.cnblogs.com/itTeacher/archive/2012/12/02/2797857.html%29\)

案例1：浅拷贝

`//具体原型`

`public class Prototype implements Cloneable{`

`private String name;`

`public String getName() {`

`return name;`

`}`

`public void setName(String name) {`

`this.name = name;`

`}`

`public Object clone() {`

`try {`

`return super.clone();`

`}catch (CloneNotSupportedException e) {`

`e.printStackTrace();`

`return null;`

`// TODO: handle exception`

`}`

\`    }

\`

`}`

`//测试类`

\`public class Test {

\`

`public static void main(String[] args) {`

`Prototype prototype = new Prototype();`

`prototype.setName("original object");`

`Prototype prototype2 = (Prototype)prototype.clone();`

\`        prototype2.setName\("changed object1"\);

\`

`System.out.println("original object:"+prototype.getName());`

\`        System.out.println\("cloned object:"+prototype2.getName\(\)\);

\`

\`    }

\`

`}`

运行结果：

![](/assets/image18_1.png)

案例2：\(浅拷贝\)

`//引用对象`

\`public class Prototype {

\`

\`    private String name;

\`

`public String getName() {`

`return name;`

\`    }

\`

`public void setName(String name) {`

`this.name = name;`

\`    }

\`

`}`

`//具体原型`

`public class NewPrototype implements Cloneable{`

\`    private String id;

\`

`public String getId() {`

`return id;`

\`    }

\`

`public void setId(String id) {`

`this.id = id;`

`}`

\`    private Prototype prototype;

\`

`public Prototype getPrototype() {`

`return prototype;`

\`    }

\`

`public void setPrototype(Prototype prototype) {`

`this.prototype = prototype;`

`}`

`public Object clone() {`

`try {`

`return super.clone();`

`}catch (CloneNotSupportedException e) {`

`e.printStackTrace();`

`return null;`

`// TODO: handle exception`

`}`

\`    }

\`

`}`

`//测试类`

`public class Test {`

`public static void main(String[] args) {`

`Prototype prototype = new Prototype();`

`prototype.setName("original object");`

`NewPrototype newobj = new NewPrototype();`

`newobj.setId("test1");`

`newobj.setPrototype(prototype);`

`NewPrototype copyobj = (NewPrototype)newobj.clone();`

`copyobj.setId("testCopy");`

\`        copyobj.getPrototype\(\).setName\("changed object"\);

\`

`System.out.println("original object id:"+newobj.getId());`

`System.out.println("original objcet name:"+newobj.getPrototype().getName());`

`System.out.println("cloned object id:"+copyobj.getId());`

`System.out.println("cloned object name:"+copyobj.getPrototype().getName());`

\`    }

\`

`}`

测试结果：

![](/assets/image18_2.png)

案例3\(深拷贝\)：

//引用类型

public class Prototype implements Cloneable{

	private String name;

	public Object clone\(\) {

		try {

			return super.clone\(\);

		}catch \(CloneNotSupportedException e\) {

			e.printStackTrace\(\);

			return null;

			// TODO: handle exception

		}

	}

	public String getName\(\) {

		return name;

	}

	public void setName\(String name\) {

		this.name = name;

	}

	



}//具体原型

public class NewPrototype implements Cloneable{

	private String id;

	

	private Prototype prototype;

	

	public String getId\(\) {

		return id;

	}



	public Prototype getPrototype\(\) {

		return prototype;

	}



	public void setId\(String id\) {

		this.id = id;

	}



	public void setPrototype\(Prototype prototype\) {

		this.prototype = prototype;

	}



	public Object clone\(\) {

		NewPrototype ret = null;

		try {

			ret = \(NewPrototype\)super.clone\(\);

			ret.prototype = \(Prototype\)this.prototype.clone\(\);

			return ret;

		}catch \(CloneNotSupportedException e\) {

			e.printStackTrace\(\);

			return null;

			// TODO: handle exception

		}

	}



}

//测试类

public class Test {



	public static void main\(String\[\] args\) {

		Prototype prototype = new Prototype\(\);

		prototype.setName\("original object"\);

		NewPrototype newob = new NewPrototype\(\);

		newob.setId\("test1"\);

		newob.setPrototype\(prototype\);

		

		NewPrototype copyObj = \(NewPrototype\)newob.clone\(\);

		copyObj.setId\("testCopy"\);

		copyObj.getPrototype\(\).setName\("change object"\);

		System.out.println\("original object id:"+newob.getId\(\)\);

		System.out.println\("original object name:"+newob.getPrototype\(\).getName\(\)\);

		System.out.println\("cloned object id:"+copyObj.getId\(\)\);

		System.out.println\("cloned object name:"+copyObj.getPrototype\(\).getName\(\)\);

	}

}

运行结果：

![](/assets/image18_3.png)

