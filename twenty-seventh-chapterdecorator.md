# 第二十七章 装饰器模式\(Decorator\)

定义：

允许向一个现有的对象添加新的功能，同时又不改变其结构。装饰着可以在所委托被装饰的行为之前或之后加上自己的行为，以达到特定的目的。

角色：

抽象组件（Component）：需要装饰的抽象对象。

具体组件（ConcreteComponent）：是我们需要装饰的对象

抽象装饰类（Decorator）：内含指向抽象组件的引用及装饰者共有的方法。

具体装饰类（ConcreteDecorator）：被装饰的对象。

案例：

咖啡+奶+糖\([http://blog.csdn.net/wwh578867817/article/details/51480441\](http://blog.csdn.net/wwh578867817/article/details/51480441\)\)

\`public interface Drink {

\`

`public float cost();`

`public String getDescription();`

`}`

\`public class Coffee implements Drink{

\`

\`    final private String description="coffee";

\`

`@Override`

`public float cost() {`

`return 10;`

\`    }

\`

`@Override`

`public String getDescription() {`

`return description;`

\`    }

\`

`}`

\`public class CondimentDecorator implements Drink{

\`

`protected Drink decoratorDrink;`

`public CondimentDecorator(Drink decoratorDrink) {`

`this.decoratorDrink = decoratorDrink;`

\`    }

\`

`@Override`

`public float cost() {`

`return decoratorDrink.cost();`

\`    }

\`

`@Override`

`public String getDescription() {`

`return decoratorDrink.getDescription();`

\`    }

\`

`}`

`public class Milk extends CondimentDecorator{`

`public Milk(Drink decoratorDrink) {`

`super(decoratorDrink);`

\`    }

\`

`@Override`

`public float cost() {`

`return super.cost()+2;`

\`    }

\`

`@Override`

`public String getDescription() {`

`return super.getDescription()+" milk";`

\`    }

\`

`}`

\`public class Sugar extends CondimentDecorator{

\`

`public Sugar(Drink decoratorDrink) {`

\`        super\(decoratorDrink\);

\`

`}`

`@Override`

`public float cost() {`

`return super.cost()+1;`

`}`

`@Override`

`public String getDescription() {`

`return super.getDescription()+" sugar";`

\`    }

\`

`}`

\`public class Test {

\`

`public static void main(String[] args) {`

`Drink drink = new Coffee();`

`System.out.println(drink.getDescription()+":"+drink.cost());`

`drink = new Milk(drink);`

`System.out.println(drink.getDescription()+":"+drink.cost());`

`drink = new Sugar(drink);`

`System.out.println(drink.getDescription()+":"+drink.cost());`

`drink = new Sugar(drink);`

\`        System.out.println\(drink.getDescription\(\)+":"+drink.cost\(\)\);

\`

`}`

`}`

运行结果：

![](/assets/image27_2.png)

运行过程分析，通过设置断点分析得，第二次drink.getDescription\(\)过程如下：

milk.getDescription-&gt;CondimentDecorator.getDescription-&gt;Coffee.getDescription-&gt;CondimentDecorator.getDescription-&gt;Milk.getDescription\(\)。

### 优点：

* 装饰类和被装饰类可以独立发展，不会相互耦合
* 动态的将责任附加到对象身上。

### 缺点：

* 多层装饰比较复杂。



