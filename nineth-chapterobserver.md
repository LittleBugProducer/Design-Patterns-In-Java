# 第九章 观察者\(Observer\)模式

观察者模式又称为发布/订阅\(Publish/Subscribe\)模式,因此我们可以用报纸期刊的订阅来形象的说明:

报社方负责出版报纸.

你订阅了该报社的报纸,那么只要报社发布了新报纸,就会通知你,或发到你手上.

如果你不想再读报纸,可以取消订阅,这样,报社发布了新报纸就不会再通知你.

理解其实以上的概念,就可以理解观察者模式,观察者模式中有主题\(Subject\)和观察者\(Observer\),分别对应报社和订阅用户\(你\).观察者模

式定义了对象之间的一对多的依赖关系,这样,当"一"的一方状态发生变化时,它所依赖的"多"的一方都会收到通知并且自动更新.

\([https://www.cnblogs.com/fingerboy/p/5468994.html\](https://www.cnblogs.com/fingerboy/p/5468994.html%29\)

案例:

`//观察者接口`

`public interface Observer {`

`	public void update(String info);`

`}`

`//主题接口`

`public interface Subject {`

`	void addObserver(Observer observer);`

`	void deleteObserver(Observer observer);`

`	void notifyObserver();`

`}`

`//观察者实现类`

`public class StudentObserver implements Observer{`

`	private TeacherSubject t;`

`	private String name;	`

`	public StudentObserver(String name,TeacherSubject t) {`

`		this.name = name;`

`		this.t = t;`

`		t.addObserver(this);`

`	}	`

`	@Override`

`	public void update(String info) {`

`		System.out.println(name+"得到作业:"+info);`

`	}`

`}`

`//主题实现类`

`public class TeacherSubject implements Subject{`

`	private List<Observer>observers = new ArrayList<Observer>();	`

`	private String info;	`

`	@Override`

`	public void addObserver(Observer observer) {`

`		observers.add(observer);`

`	}`

`	@Override`

`	public void deleteObserver(Observer observer) {`

`		int i = observers.indexOf(observer);`

`		if(i>=0) {`

`			observers.remove(i);`

`		}`

`	}`

`	@Override`

`	public void notifyObserver() {`

`		for(int i = 0;i<observers.size();i++) {`

`			Observer o = (Observer)observers.get(i);`

`			o.update(info);`

`		}`

`	}`

`	public void setHomework(String info) {`

`		this.info = info;`

`		System.out.println("今天的作业是"+info);`

`		notifyObserver();`

`	}	`

`}`

`//测试类`

`public class Test {`

`	public static void main(String[] args) {`

`		TeacherSubject t = new TeacherSubject();`

`		StudentObserver zs = new StudentObserver("张三", t);`

`		StudentObserver ls = new StudentObserver("李四", t);`

`		StudentObserver ww = new StudentObserver("王五", t);`

`		t.setHomework("p2");`

`		t.setHomework("p6");`

`		t.setHomework("p56-p65");`

`	}`

`}`

![](/assets/image9_1.png)

