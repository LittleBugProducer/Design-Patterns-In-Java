# 第二十二章 状态\(State\)模式

本章通过一个电梯的描述过程进化来说明，素材取自\([http://blog.csdn.net/u012401711/article/details/52675873\](http://blog.csdn.net/u012401711/article/details/52675873\)\)

demo:

用程序实现电梯的四个动作--&gt;开门、关门、运行、停止

`public interface ILift {`

`	void open();`

`	void close();`

`	void run();`

`	void stop();`

`}`

`public class Lift implements ILift{`

`	@Override`

`	public void open() {`

`		System.out.println("电梯门打开");`

`	}`

`	@Override`

`	public void close() {`

`		System.out.println("电梯门关闭");`

`	}`

`	@Override`

`	public void run() {`

`		System.out.println("电梯运行");`

`	}`

`	@Override`

`	public void stop() {`

`		System.out.println("电梯停止");`

`	}	`

`}`

`public class Test {`

`	public static void main(String[] args) {`

`		ILift lift = new Lift();`

`		lift.open();`

`		lift.close();`

`		lift.run();`

`		lift.stop();`

`	}`

`}`

运行结果：

![](/assets/image22_1.png)

demo1:四个状态的执行都有前置条件:

![](/assets/image22_2.png)





