# 第二十九章 访问者\(Visitor\)模式

### 定义：

访问者模式即表示一个作用于某对象结构中的各元素的操作，它使我们可以在不改变各元素的类的前提下定义作用于这些元素的新操作。

角色：

Vistor:

 抽象访问者。为该对象结构中的ConcreteElement的每一个类声明的一个操作。 

ConcreteVisitor:

 具体访问者。实现Visitor申明的每一个操作，每一个操作实现算法的一部分。 

Element:

 抽象元素。定义一个Accept操作，它以一个访问者为参数。 

ConcreteElement:

 具体元素 。实现Accept操作。 

ObjectStructure:

 对象结构。能够枚举它的元素，可以提供一个高层的接口来允许访问者访问它的元素。

案例：

\(http://blog.csdn.net/chenssy/article/details/12029633\)

在医院付费取药：

public abstract class Visitor {

	protected String name;

	

	public void setName\(String name\) {

		this.name = name;

	}

	

	public abstract void visitor\(MedicineA a\);

	

	public abstract void visitor\(MedicineB b\);



}

public class Charger extends Visitor{



	@Override

	public void visitor\(MedicineA a\) {

		System.out.println\("收银员:"+name+"给药"+a.getName\(\)+"扣款:"+a.getPrice\(\)\);

	}



	@Override

	public void visitor\(MedicineB b\) {

		System.out.println\("收银员:"+name+"给药"+b.getName\(\)+"扣款:"+b.getPrice\(\)\);

	}

	



}

public class WorkerOfPharmacy extends Visitor{



	@Override

	public void visitor\(MedicineA a\) {

		System.out.println\("药房工作者:"+name+"拿药:"+a.getName\(\)\);

	}



	@Override

	public void visitor\(MedicineB b\) {

		System.out.println\("药房工作者:"+name+"拿药:"+b.getName\(\)\);

	}

	



}

public abstract class Medicine {

	protected String name;

	protected double price;

	

	public Medicine\(String name,double price\) {

		this.name = name;

		this.price = price;

	}

	

	public String getName\(\) {

		return name;

		

	}

	public void setName\(String name\) {

		this.name = name;

	}

	public double getPrice\(\) {

		return price;

	}

	public void setPrice\(double price\) {

		this.price = price;

	}

	public abstract void accept\(Visitor visitor\);



}

public class MedicineA extends Medicine{



	public MedicineA\(String name, double price\) {

		super\(name, price\);

		

	}



	@Override

	public void accept\(Visitor visitor\) {

		visitor.visitor\(this\);

	}



}

public class MedicineB extends Medicine{



	public MedicineB\(String name, double price\) {

		super\(name, price\);

		

	}



	@Override

	public void accept\(Visitor visitor\) {

		visitor.visitor\(this\);

	}



}

public class Presciption {



	List&lt;Medicine&gt;list = new ArrayList&lt;Medicine&gt;\(\);

	

	public void accept\(Visitor visitor\) {

		Iterator&lt;Medicine&gt; iterator = list.iterator\(\);

		while\(iterator.hasNext\(\)\) {

			iterator.next\(\).accept\(visitor\);

		}

	}

	public void addMedicine\(Medicine medicine\) {

		list.add\(medicine\);

	}

	public void removeMedicine\(Medicine medicine\) {

		list.remove\(medicine\);

	}

}

public class Test {



	public static void main\(String\[\] args\) {

		Medicine a = new MedicineA\("板蓝根", 11.0\);

		Medicine b = new MedicineB\("感康", 14.3\);

		

		Presciption presciption = new Presciption\(\);

		presciption.addMedicine\(a\);

		presciption.addMedicine\(b\);

		

		Visitor charger = new Charger\(\);

		charger.setName\("张三"\);

		Visitor workerOfPharmacy = new WorkerOfPharmacy\(\);

		workerOfPharmacy.setName\("李四"\);

		presciption.accept\(charger\);

		System.out.println\("------------------"\);

		presciption.accept\(workerOfPharmacy\);

	}

}

运行结果：

![](/assets/image29_1.png)





