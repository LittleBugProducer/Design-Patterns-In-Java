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

\`public interface ILift {

\`

`final static int OPENING_STATE=1;`

`final static int CLOSING_STATE=2;`

`final static int RUNNING_STATE=3;`

`final static int STOPPING_STATE=4;`

`void setState(int state);`

`void open();`

`void close();`

`void run();`

`void stop();`

`}`

\`public class Lift implements ILift{

\`

\`    private int state;

\`

`public void setState(int state) {`

`this.state=state;`

\`    }

\`

`private void closeWithoutLogic() {`

`System.out.println("电梯门关闭");`

\`    }

\`

`private void openWithoutLogic() {`

`System.out.println("电梯门开启");`

\`    }

\`

`private void runWithoutLogic() {`

`System.out.println("电梯上下跑起来");`

`}`

`private void stopWithoutLogic() {`

`System.out.println("电梯停止了");`

\`    }

\`

`public void open() {`

`switch (this.state) {`

`case OPENING_STATE:`

`break;`

`case CLOSING_STATE:`

`this.openWithoutLogic();`

`this.setState(OPENING_STATE);`

`break;`

`case RUNNING_STATE:`

`break;`

`case STOPPING_STATE:`

`this.openWithoutLogic();`

`this.setState(OPENING_STATE);`

`break;`

`default:`

`break;`

`}`

\`    }

\`

`public void close() {`

`switch (this.state) {`

`case OPENING_STATE:`

`this.closeWithoutLogic();`

`this.setState(CLOSING_STATE);`

`break;`

`case CLOSING_STATE:`

`break;`

`case RUNNING_STATE:`

`break;`

`case STOPPING_STATE:`

`break;`

`default:`

`break;`

`}`

\`    }

\`

`public void run() {`

`switch (this.state) {`

\`        case OPENING\_STATE:

\`

`break;`

`case CLOSING_STATE:`

`this.runWithoutLogic();`

`this.setState(RUNNING_STATE);`

`break;`

`case RUNNING_STATE:`

`break;`

`case STOPPING_STATE:`

`this.runWithoutLogic();`

`this.setState(RUNNING_STATE);`

`break;`

`default:`

`break;`

`}`

\`    }

\`

`public void stop() {`

`switch (this.state) {`

\`        case OPENING\_STATE:

\`

`break;`

`case CLOSING_STATE:`

`this.stopWithoutLogic();`

`this.setState(CLOSING_STATE);`

`break;`

`case RUNNING_STATE:`

`this.stopWithoutLogic();`

`this.setState(CLOSING_STATE);`

`break;`

`case STOPPING_STATE:`

`break;`

`default:`

`break;`

`}`

`}`

`}`

\`public class Test {

\`

`public static void main(String[] args) {`

`ILift lift = new Lift();`

`lift.setState(ILift.STOPPING_STATE);`

`lift.open();`

`lift.close();`

`lift.run();`

`lift.stop();`

`}`

`}`

运行结果：

![](/assets/image22_3.png)

这段程序有什么问题，首先Lift.java 这个文件有点长，长的原因是我们在程序中使用了大量的switch…case 这样的判断（if…else 也是一样）；其次，扩展性非常的不好，电梯还有两个状态没有加，通电状态和断电状态；再其次，我们来思考我们的业务，电梯在门敞开状态下就不能上下跑了吗？电梯有没有发生过只有运行没有停止状态呢（从40 层直接坠到1 层嘛）？电梯故障嘛，还有电梯在检修的时候，可以在stop状态下不开门，这也是正常的业务需求呀，你想想看，如果加上这些判断条件，上面的程序有多少需要修改？业务上的任务一个小小增加或改动都对我们的这个电梯类产生了修改。  
         刚刚我们是从电梯的有哪些方法以及这些方法执行的条件去分析，现在我们换个角度来看问题，我们来想电梯在具有这些状态的时候，能够做什么事情，也就是说在电梯处于一个具体状态时，我们来思考这个状态是由什么动作触发而产生以及在这个状态下电梯还能做什么事情，举个例子来说，电梯在停止状态时，我们来思考两个问题：

        第一、这个停止状态时怎么来的，那当然是由于电梯执行了stop 方法而来的；  
         第二、在停止状态下，电梯还能做什么动作?继续运行？开门？那当然都可以了。  
        我们再来分析其他三个状态，也都是一样的结果，我们只要实现电梯在一个状态下的两个任务模型就可以了：这个状态是如何产生的以及在这个状态下还能做什么其他动作（也就是这个状态怎么过渡到其他状态）

demo2:

`public abstract class LiftState {`

`	protected Context context;`

`	public void setContext(Context context) {`

`		this.context = context;`

`	}`

`	public abstract void open();`

`	public abstract void close();`

`	public abstract void run();`

`	public abstract void stop();`

`}`

`public class Context {`

`	public final static OpenningState openningState=new OpenningState();`

`	public final static ClosingState closingState = new ClosingState();`

`	public final static RunningState runningState = new RunningState();`

`	public final static StoppingState stoppingState = new StoppingState();`

`	private LiftState liftState;`

`	public LiftState getLiftState() {`

`		return liftState;`

`	}`

`	public void setLiftState(LiftState liftState) {`

`		this.liftState = liftState;`

`		this.liftState.setContext(this);`

`	}`

`	public void open() {`

`		this.liftState.open();		`

`	}`

`	public void close() {`

`		this.liftState.close();`

`	}`

`	public void run() {`

`		this.liftState.run();`

`	}`

`	public void stop() {`

`		this.liftState.stop();`

`	}	`

`}`

`public class OpenningState extends LiftState{`

`	@Override`

`	public void open() {`

`		System.out.println("电梯门开启");`

`	}`

`	@Override`

`	public void close() {`

`		super.context.setLiftState(Context.closingState);`

`		super.context.getLiftState().close();`

`	}`

`	@Override`

`	public void run() {`

`	}`

`	@Override`

`	public void stop() {`

`	}	`

`}`

`public class ClosingState extends LiftState{`

`	@Override`

`	public void open() {`

`		super.context.setLiftState(Context.openningState);`

`		super.context.getLiftState().open();`

`	}`

`	@Override`

`	public void close() {`

`		System.out.println("电梯门关闭");`

`	}`

`	@Override`

`	public void run() {`

`		super.context.setLiftState(Context.runningState);`

`		super.context.getLiftState().run();`

`	}`

`	@Override`

`	public void stop() {`

`		super.context.setLiftState(Context.stoppingState);`

`		super.context.getLiftState().stop();`

`	}	`

`}`

`public class RunningState extends LiftState{`

`	@Override`

`	public void open() {`

`	}`

`	@Override`

`	public void close() {`

`	}`

`	@Override`

`	public void run() {`

`		System.out.println("电梯上下跑");`

`	}`

`	@Override`

`	public void stop() {`

`		super.context.setLiftState(Context.stoppingState);`

`		super.context.getLiftState().stop();`

`	}	`

`}`

`public class StoppingState extends LiftState{`

`	@Override`

`	public void open() {`

`		super.context.setLiftState(Context.openningState);`

`		super.context.getLiftState().open();`

`	}`

`	@Override`

`	public void close() {`

`	}`

`	@Override`

`	public void run() {`

`		super.context.setLiftState(Context.runningState);`

`		super.context.getLiftState().run();`

`	}`

`	@Override`

`	public void stop() {`

`		System.out.println("电梯停止了");`

`	}	`

`}`

`public class Test {`

`	public static void main(String[] args) {`

`		Context context = new Context();`

`		context.setLiftState(new ClosingState());`

`		context.open();`

`		context.close();`

`		context.run();`

`		context.stop();`

`	}`

`}`

运行结果：

![](/assets/image22_4.png)



