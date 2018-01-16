第十九章 备忘录\(Memento\)模式

### 定义

在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样就可以将该对象恢复到原先保存的状态

### 角色

\([https://www.cnblogs.com/chenssy/p/3341526.html\](https://www.cnblogs.com/chenssy/p/3341526.html\)\)

Originator: 原发器。负责创建一个备忘录，用以记录当前对象的内部状态，通过也可以使用它来利用备忘录恢复内部状态。同时原发器还可以根据需要决定Memento存储Originator的那些内部状态。

Memento: 备忘录。用于存储Originator的内部状态，并且可以防止Originator以外的对象访问Memento。在备忘录Memento中有两个接口，其中Caretaker只能看到备忘录中的窄接口，它只能将备忘录传递给其他对象。Originator可以看到宽接口，允许它访问返回到先前状态的所有数据。

Caretaker: 负责人。负责保存好备忘录，不能对备忘录的内容进行操作和访问，只能够将备忘录传递给其他对象。

在备忘录模式中，最重要的就是备忘录Memento了。我们都是备忘录中存储的就是原发器的部分或者所有的状态信息，而这些状态信息是不能够被其他对象所访问了，也就是说我们是不可能在备忘录之外的对象来存储这些状态信息，如果暴漏了内部状态信息就违反了封装的原则，故备忘录是除了原发器外其他对象都是不可以访问的。

所以为了实现备忘录模式的封装，我们需要对备忘录的访问做些控制：

对原发器：可以访问备忘录里的所有信息。

对负责人：不可以访问备忘录里面的数据，但是他可以保存备忘录并且可以将备忘录传递给其他对象。

其他对象：不可访问也不可以保存，它只负责接收从负责人那里传递过来的备忘录同时恢复原发器的状态。

所以就备忘录模式而言理想的情况就是只允许生成该备忘录的那个原发器访问备忘录的内部状态。

案例：

`//备忘录`

\`public class Memento {

\`

`private String state = "";`

`public String getState() {`

`return state;`

\`    }

\`

`public void setState(String state) {`

`this.state = state;`

\`    }

\`

`public Memento(String state) {`

`this.state = state;`

\`    }

\`

`}`

`//原发器`

\`public class Originator {

\`

\`    private String state="";

\`

`public String getState() {`

`return state;`

\`    }

\`

`public void setState(String state) {`

`this.state = state;`

\`    }

\`

`public Memento createMemento() {`

`return new Memento(this.state);`

\`    }

\`

`public void restoreMemento(Memento memento) {`

`this.setState(memento.getState());`

\`    }

\`

`}`

`//负责人`

\`public class Caretaker {

\`

\`    private Memento memento;

\`

`public Memento getMemento() {`

`return memento;`

\`    }

\`

`public void setMemento(Memento memento) {`

`this.memento = memento;`

\`    }

\`

`}`

`//测试类`

\`public class Test {

\`

`public static void main(String[] args) {`

`Originator originator = new Originator();`

`originator.setState("状态1");`

`System.out.println("初试状态:"+originator.getState());`

`Caretaker caretaker = new Caretaker();`

`caretaker.setMemento(originator.createMemento());`

`originator.setState("状态2");`

`System.out.println("改变后状态:"+originator.getState());`

`originator.restoreMemento(caretaker.getMemento());`

`System.out.println("恢复后状态："+originator.getState());`

`}`

`}`

运行结果：

![](/assets/image19_1.png)

