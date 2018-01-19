# 第二十八章 迭代器\(Iterator\)模式

### 定义：

提供一种方法顺序访问一个聚合对象中的各种元素，而又不暴露该对象的内部表示

### 角色：

\(1\)迭代器角色（Iterator）:定义遍历元素所需要的方法，一般来说会有这么三个方法：取得下一个元素的方法next\(\)，判断是否遍历结束的方法hasNext\(\)），移出当前对象的方法remove\(\),

\(2\)具体迭代器角色（Concrete Iterator）：实现迭代器接口中定义的方法，完成集合的迭代。

\(3\)容器角色\(Aggregate\):  一般是一个接口，提供一个iterator\(\)方法，例如java中的Collection接口，List接口，Set接口等

\(4\)具体容器角色（ConcreteAggregate）：就是抽象容器的具体实现类，比如List接口的有序列表实现ArrayList，List接口的链表实现LinkList，Set接口的哈希列表的实现HashSet等。

案例：

`//迭代器角色`

`public interface Iterator {`

`	boolean hasNext();`

`	Object next();`

`}`

`//具体迭代器角色`

`public class ConcreteIterator implements Iterator{`

`	private List list = null;`

`	private int index;	`

`	public ConcreteIterator(List list) {`

`		super();`

`		this.list = list;`

`	}	`

`	@Override`

`	public boolean hasNext() {`

`		if(index>=list.getSize()) {`

`			return false;`

`		}`

`		return true;`

`	}`

`	@Override`

`	public Object next() {`

`		Object object = list.get(index);`

`		index++;`

`		return object;`

`	}	`

`}`

`//定义容器角色`

`public interface List {`

`	public void add(Object obj);`

`	public Object get(int index);`

`	public Iterator iterator();`

`	public int getSize();`

`}`

`//定义具体容器角色`

`public class ConcreteAggregate implements List{`

`	private Object[] list;`

`	private int size=0;`

`	private int index=0;	`

`	public ConcreteAggregate() {`

`		index = 0;`

`		size = 0;`

`		list = new Object[100];`

`	}	`

`	@Override`

`	public void add(Object obj) {`

`		list[index++]=obj;`

`		size++;`

`	}`

`	@Override`

`	public Object get(int index) {`

`		return list[index];`

`	}`

`	@Override`

`	public Iterator iterator() {`

`		return new ConcreteIterator(this);`

`	}`

`	@Override`

`	public int getSize() {`

`		return size;`

`	}`

`}`

`public class Test {`

`	public static void main(String[] args) {`

`		List list = new ConcreteAggregate();`

`		list.add("a");`

`		list.add("b");`

`		list.add("c");`

`		list.add("d");`

`		Iterator it = list.iterator();`

`		while(it.hasNext()) {`

`			System.out.println(it.next());`

`		}`

`	}`

`}`

运行结果：

![](/assets/image28_1.png)

### 优点：

* 简化了遍历方式，对于对象集合的遍历，还是比较麻烦的，对于数组或者有序列表，我们尚可以通过游标来取得，但用户需要在对集合了解很清楚的前提下，自行遍历对象，但是对于hash表来说，用户遍历起来就比较麻烦了。而引入了迭代器方法后，用户用起来就简单的多了。
* 可以提供多种遍历方式，比如说对有序列表，我们可以根据需要提供正序遍历，倒序遍历两种迭代器，用户用起来只需要得到我们实现好的迭代器，就可以方便的对集合进行遍历了。
* 封装性良好，用户只需要得到迭代器就可以遍历，而对于遍历算法则不用去关心。

### 缺点：

* 对于比较简单的遍历（像数组或者有序列表），使用迭代器方式遍历较为繁琐，大家可能都有感觉，像ArrayList，我们宁可愿意使用for循环和get方法来遍历集合。







