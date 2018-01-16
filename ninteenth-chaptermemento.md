第十九章 备忘录\(Memento\)模式

### 定义

在不破坏封装性的前提下，捕获一个对象的内部状态，并在该对象之外保存这个状态。这样就可以将该对象恢复到原先保存的状态

### 角色

\([https://www.cnblogs.com/chenssy/p/3341526.html\](https://www.cnblogs.com/chenssy/p/3341526.html%29\)

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

\([http://blog.csdn.net/zhengzhb/article/details/7697549\](http://blog.csdn.net/zhengzhb/article/details/7697549%29\)

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

案例2--&gt;多状态多备份：

\`public class Memento {

\`

`private Map<String, Object>stateMap;`

`public Memento(Map<String, Object>map) {`

`this.stateMap = map;`

`}`

`public Map<String, Object> getStateMap() {`

`return stateMap;`

`}`

`public void setStateMap(Map<String, Object> stateMap) {`

`this.stateMap = stateMap;`

\`    }

\`

`}`

\`public class BeanUtils {

\`

`public static Map<String, Object>backupPro(Object bean){`

`Map<String, Object>result = new HashMap<String,Object>();`

`try {`

`BeanInfo beanInfo = Introspector.getBeanInfo(bean.getClass());`

`PropertyDescriptor[] descriptors = beanInfo.getPropertyDescriptors();`

`for(PropertyDescriptor des:descriptors) {`

`String fieldName=des.getName();`

`Method getter = des.getReadMethod();`

`Object fieldValue=getter.invoke(bean, new Object[] {});`

`if(!fieldName.equalsIgnoreCase("class")) {`

`result.put(fieldName, fieldValue);`

`}`

`}`

`}catch (Exception e) {`

`// TODO: handle exception`

`e.printStackTrace();`

`}`

`return result;`

`}`

`public static void restoreProp(Object bean,Map<String, Object>propMap) {`

`try {`

`BeanInfo beanInfo = Introspector.getBeanInfo(bean.getClass());`

`PropertyDescriptor[] descriptors = beanInfo.getPropertyDescriptors();`

`for(PropertyDescriptor des:descriptors) {`

`String fieldName=des.getName();`

`if(propMap.containsKey(fieldName)) {`

`Method setter = des.getWriteMethod();`

`setter.invoke(bean, new Object[] {propMap.get(fieldName)});`

`}`

`}`

`}catch (Exception e) {`

`// TODO: handle exception`

`e.printStackTrace();`

`}`

`}`

`}`

\`public class Originator {

\`

`private String state1="";`

`private String state2="";`

`private String state3="";`

`public String getState1() {`

`return state1;`

`}`

`public String getState2() {`

`return state2;`

`}`

`public String getState3() {`

`return state3;`

`}`

`public void setState1(String state1) {`

`this.state1 = state1;`

`}`

`public void setState2(String state2) {`

`this.state2 = state2;`

`}`

`public void setState3(String state3) {`

`this.state3 = state3;`

`}`

`  
`

`public Memento createMemento() {`

`return new Memento(BeanUtils.backupPro(this));`

`}`

`public void restoreMemento(Memento memento) {`

`BeanUtils.restoreProp(this, memento.getStateMap());`

`}`

`@Override`

`public String toString() {`

`return "Originator [state1=" + state1 + ", state2=" + state2 + ", state3=" + state3 + "]";`

\`    }

\`

`}`

`public class Caretaker {`

`  
`

`private Map<String, Memento>meMap = new HashMap<String,Memento>();`

`public Memento getMemento(String index) {`

`return meMap.get(index);`

`}`

`public void setMemento(String index,Memento memento) {`

`this.meMap.put(index, memento);`

`}`

`}`

\`public class Test {

\`

`public static void main(String[] args) {`

`Originator originator = new Originator();`

`Caretaker caretaker = new Caretaker();`

`originator.setState1("我是");`

`originator.setState2("一个");`

`originator.setState3("单身汪");`

`System.out.println("初始状态:"+originator);`

`caretaker.setMemento("001", originator.createMemento());`

`originator.setState1("现在");`

`originator.setState2("我是");`

`originator.setState3("汪汪");`

`System.out.println("修改后状态:"+originator);`

`originator.restoreMemento(caretaker.getMemento("001"));`

\`        System.out.println\("恢复后状态:"+originator\);

\`

`}`

`}`

运行结果:

![](/assets/image19_2.png)

### 优点：

 1、 给用户提供了一种可以恢复状态的机制。可以是用户能够比较方便地回到某个历史的状态。

 2、 实现了信息的封装。使得用户不需要关心状态的保存细节。

### 缺点：

消耗资源。如果类的成员变量过多，势必会占用比较大的资源，而且每一次保存都会消耗一定的内存。







