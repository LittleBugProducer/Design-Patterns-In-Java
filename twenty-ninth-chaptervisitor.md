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

\([http://blog.csdn.net/chenssy/article/details/12029633\](http://blog.csdn.net/chenssy/article/details/12029633%29\)

在医院付费取药：

`public abstract class Visitor {`

```
protected String name;

public void setName\(String name\) {

    this.name = name;

}

public abstract void visitor\(MedicineA a\);

public abstract void visitor\(MedicineB b\);
```

`}`

`public class Charger extends Visitor{`

```
@Override

public void visitor\(MedicineA a\) {

    System.out.println\("收银员:"+name+"给药"+a.getName\(\)+"扣款:"+a.getPrice\(\)\);

}

@Override

public void visitor\(MedicineB b\) {

    System.out.println\("收银员:"+name+"给药"+b.getName\(\)+"扣款:"+b.getPrice\(\)\);

}
```

`}`

`public class WorkerOfPharmacy extends Visitor{`

```
@Override

public void visitor\(MedicineA a\) {

    System.out.println\("药房工作者:"+name+"拿药:"+a.getName\(\)\);

}

@Override

public void visitor\(MedicineB b\) {

    System.out.println\("药房工作者:"+name+"拿药:"+b.getName\(\)\);

}
```

`}`

`public abstract class Medicine {`

```
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
```

`}`

`public class MedicineA extends Medicine{`

```
public MedicineA\(String name, double price\) {

    super\(name, price\);
}



@Override

public void accept\(Visitor visitor\) {

    visitor.visitor\(this\);

}
```

`}`

`public class MedicineB extends Medicine{`

```
public MedicineB\(String name, double price\) {

    super\(name, price\);

}



@Override

public void accept\(Visitor visitor\) {

    visitor.visitor\(this\);

}
```

`}`

`public class Presciption {`

```
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
```

`}`

`public class Test {`

```
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
```

`}`

运行结果：

![](/assets/image29_1.png)

### 优点

1、使得新增新的访问操作变得更加简单。

2、能够使得用户在不修改现有类的层次结构下，定义该类层次结构的操作。

3、将有关元素对象的访问行为集中到一个访问者对象中，而不是分散搞一个个的元素类中。

### 缺点

1、增加新的元素类很困难。在访问者模式中，每增加一个新的元素类都意味着要在抽象访问者角色中增加一个新的抽象操作，并在每一个具体访问者类中增加相应的具体操作，违背了“开闭原则”的要求。  
2、破坏封装。当采用访问者模式的时候，就会打破组合类的封装。

3、比较难理解。貌似是最难的设计模式了。

另一个案例：

\([https://www.jianshu.com/p/80b9cd7c0da5\](https://www.jianshu.com/p/80b9cd7c0da5\)\)

老板和会计查看账单

`public interface Bill {`

`	void accept(AccountBookView viewer);`

`}`

`public class ConsumerBill implements Bill{`

`	private String item;`

`	private double amount;	`

`	public ConsumerBill(String item,double amount) {`

`		this.item = item;`

`		this.amount = amount;`

`	}	`

`	public String getItem() {`

`		return item;`

`	}	`

`	public double getAmount() {`

`		return amount;`

`	}		`

`	@Override`

`	public void accept(AccountBookView viewer) {`

`		viewer.view(this);`

`	}	`

`}`

`public class IncomeBill implements Bill{`

`	private String item;`

`	private double amount;	`

`	public IncomeBill(String item,double amount) {`

`		this.item = item;`

`		this.amount = amount;`

`	}	`

`	public String getItem() {`

`		return item;`

`	}	`

`	public double getAmount() {`

`		return amount;`

`	}	`

`	@Override`

`	public void accept(AccountBookView viewer) {`

`		viewer.view(this);`

`	}`

`}`

`public interface AccountBookView {`

`	void view(ConsumerBill oConsumerBill);	`

`	void view(IncomeBill incomeBill);`

`}`

`public class Boss implements AccountBookView{`

`	private double totalConsume;`

`	private double totalIncome;	`

`	@Override`

`	public void view(ConsumerBill oConsumerBill) {`

`		totalConsume = totalConsume+oConsumerBill.getAmount();`

`	}`

`	@Override`

`	public void view(IncomeBill incomeBill) {`

`		totalIncome+=incomeBill.getAmount();`

`	}`

`	public void getTotalConsume() {`

`		System.out.println("老板一共消费了"+totalConsume);`

`	}`

`	public void getTotalIncome() {`

`		System.out.println("老板一共收入了"+totalIncome);`

`	}	`

`}`

`public class CPA implements AccountBookView{`

`	int count = 0;	`

`	public void view(ConsumerBill consumerBill) {`

`		count++;`

`		if(consumerBill.getItem().equals("消费")) {`

`			System.out.println("第"+count+"个单子消费了:"+consumerBill.getAmount());`

`		}`

`	}`

`	public void view(IncomeBill incomeBill) {`

`		count++;`

`		if(incomeBill.getItem().equals("收入")) {`

`			System.out.println("第"+count+"个单子收入了:"+incomeBill.getAmount());`

`		}`

`	}`

`}`

`public class AccountBook {`

`	private List<Bill>listBill = new ArrayList<Bill>();`

`	public void add(Bill bill) {`

`		listBill.add(bill);`

`	}`

`	public void show(AccountBookView viewer) {`

`		for(Bill b:listBill) {`

`			b.accept(viewer);`

`		}`

`	}`

`}`

`public class Test {`

`	public static void main(String[] args) {`

`		Bill consumerBill = new ConsumerBill("消费", 3000);`

`		Bill incomeBill = new IncomeBill("收入", 4000);`

`		Bill consumerBill2 = new ConsumerBill("消费", 5000);`

`		Bill incomeBill2 = new IncomeBill("收入", 10000);`

`		AccountBook accountBook = new AccountBook();`

`		accountBook.add(consumerBill);`

`		accountBook.add(incomeBill);`

`		accountBook.add(consumerBill2);`

`		accountBook.add(incomeBill2);`

`		AccountBookView boss = new Boss();`

`		AccountBookView cpa = new CPA();`

`		accountBook.show(boss);`

`		accountBook.show(cpa);`

`		((Boss)boss).getTotalConsume();`

`		((Boss)boss).getTotalIncome();`

`	}`

`}`

运行结果：

![](/assets/image29_2.png)

理解帮助，用面向对象的思想，将账单看作主体。



