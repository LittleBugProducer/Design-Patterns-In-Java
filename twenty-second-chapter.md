# 第二十二章 状态\(State\)模式

本章通过一个电梯的描述过程进化来说明，素材取自\([http://blog.csdn.net/u012401711/article/details/52675873\](http://blog.csdn.net/u012401711/article/details/52675873%29\)

demo:

用程序实现电梯的四个动作--&gt;开门、关门、运行、停止

\`public interface ILift {

\`

`void open();`

`void close();`

`void run();`

`void stop();`

`}`

\`public class Lift implements ILift{

\`

`@Override`

`public void open() {`

`System.out.println("电梯门打开");`

\`    }

\`

`@Override`

`public void close() {`

`System.out.println("电梯门关闭");`

\`    }

\`

`@Override`

`public void run() {`

`System.out.println("电梯运行");`

\`    }

\`

`@Override`

`public void stop() {`

`System.out.println("电梯停止");`

`}`

`}`

\`public class Test {

\`

`public static void main(String[] args) {`

`ILift lift = new Lift();`

`lift.open();`

`lift.close();`

`lift.run();`

`lift.stop();`

`}`

`}`

运行结果：

![](/assets/image22_1.png)

demo1:四个状态的执行都有前置条件:

![](/assets/image22_2.png)

`public interface ILift {`

`	final static int OPENING_STATE=1;`

`	final static int CLOSING_STATE=2;`

`	final static int RUNNING_STATE=3;`

`	final static int STOPPING_STATE=4;`

`	void setState(int state);`

`	void open();`

`	void close();`

`	void run();`

`	void stop();`

`}`

`public class Lift implements ILift{	`

`	private int state;`

`	public void setState(int state) {`

`		this.state=state;`

`	}	`

`	private void closeWithoutLogic() {`

`		System.out.println("电梯门关闭");`

`	}	`

`	private void openWithoutLogic() {`

`		System.out.println("电梯门开启");`

`	}	`

`	private void runWithoutLogic() {`

`		System.out.println("电梯上下跑起来");`

`	}`

`	private void stopWithoutLogic() {`

`		System.out.println("电梯停止了");`

`	}`

`	public void open() {`

`		switch (this.state) {`

`		case OPENING_STATE:			`

`			break;`

`		case CLOSING_STATE:`

`			this.openWithoutLogic();`

`			this.setState(OPENING_STATE);`

`			break;`

`		case RUNNING_STATE:`

`			break;`

`		case STOPPING_STATE:`

`			this.openWithoutLogic();`

`			this.setState(OPENING_STATE);`

`			break;`

`		default:`

`			break;`

`		}`

`	}`

`	public void close() {`

`		switch (this.state) {`

`		case OPENING_STATE:`

`			this.closeWithoutLogic();`

`			this.setState(CLOSING_STATE);`

`			break;`

`		case CLOSING_STATE:`

`			break;`

`		case RUNNING_STATE:`

`			break;`

`		case STOPPING_STATE:`

`			break;`

`		default:`

`			break;`

`		}`

`	}`

`	public void run() {`

`		switch (this.state) {`

`		case OPENING_STATE:			`

`			break;`

`		case CLOSING_STATE:`

`			this.runWithoutLogic();`

`			this.setState(RUNNING_STATE);`

`			break;`

`		case RUNNING_STATE:`

`			break;`

`		case STOPPING_STATE:`

`			this.runWithoutLogic();`

`			this.setState(RUNNING_STATE);`

`			break;`

`		default:`

`			break;`

`		}`

`	}`

`	public void stop() {`

`		switch (this.state) {`

`		case OPENING_STATE:			`

`			break;`

`		case CLOSING_STATE:`

`			this.stopWithoutLogic();`

`			this.setState(CLOSING_STATE);`

`			break;`

`		case RUNNING_STATE:`

`			this.stopWithoutLogic();`

`			this.setState(CLOSING_STATE);`

`			break;`

`		case STOPPING_STATE:`

`			break;`

`		default:`

`			break;`

`		}`

`	}	`

`}`

`public class Test {`

`	public static void main(String[] args) {`

`		ILift lift = new Lift();`

`		lift.setState(ILift.STOPPING_STATE);`

`		lift.open();`

`		lift.close();`

`		lift.run();`

`		lift.stop();`

`	}`

`}`

运行结果：

![](/assets/image22_3.png)



