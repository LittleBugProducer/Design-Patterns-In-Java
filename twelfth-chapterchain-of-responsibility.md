# 第十二章 职责链\(Chain of Responsibility\)模式

### 职责链模式

使多个对象都有机会处理请求，从而避免请求的发送者和接受者之间的耦合关系，

将这个对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理他为止。  


### 角色

抽象处理者角色\(Handler\)：定义出一个处理请求的接口。如果需要，接口可以定义 出一个方法以设定和返回对下家的引用。这个角色通常由一个Java抽象类或者Java接口实现。

具体处理者角色\(ConcreteHandler\)：具体处理者接到请求后，可以选择将请求处理掉，或者将请求传给下家。由于具体处理者持有对下家的引用，因此，如果需要，具体处理者可以访问下家。

案例：

申请聚餐费用的大致流程一般是，由申请人先填写申请单，然后交给领导审批，如果申请批准下来，领导会通知申请人审批通过，然后申请人去财务领取费用，如果没有批准下来，领导会通知申请人审批未通过，此事也就此作罢。

不同级别的领导，对于审批的额度是不一样的，比如，项目经理只能审批500元以内的申请；部门经理能审批1000元以内的申请；而总经理可以审核任意额度的申请。

`//抽象处理者角色`

`public abstract class Handler {`

`	protected Handler successor = null;`

`	public Handler getSuccessor() {`

`		return successor;`

`	}`

`	public void setSuccessor(Handler successor) {`

`		this.successor = successor;`

`	}	`

`	public abstract String handleFeeRequest(String user,double fee);	`

`}`

`//具体处理者，项目经理`

`public class ProjectManager extends Handler{`

`	@Override`

`	public String handleFeeRequest(String user, double fee) {`

`		String string = "";`

`		if(fee<500) {`

`			if("zs".equals(user)) {`

`				string="成功:项目经理已经批准 "+user+" 的聚餐费用，额度为 "+fee+" 美元";`

`			}else {`

`				string = "失败，项目经理要求你先干活";`

`			}`

`		}else {`

`			if(getSuccessor()!=null) {`

`				return getSuccessor().handleFeeRequest(user, fee);`

`			}`

`		}`

`		return string;`

`	}	`

`}`

`//具体处理者，部门经理`

`public class DepManager extends Handler{`

`	@Override`

`	public String handleFeeRequest(String user, double fee) {`

`		String str = "";`

`		if(fee<1000) {`

`			if("zs".equals(user)) {`

`				str = "部门大大同意了 "+user+" 的聚餐申请 "+fee+" 美元";				`

`			}else {`

`				str="部门大大拒绝了你的聚餐申请";`

`			}`

`		}else {`

`			if(getSuccessor()!=null) {`

`				return getSuccessor().handleFeeRequest(user, fee);`

`			}`

`		}`

`		return str;`

`	}	`

`}`

`//具体处理者，总经理`

`public class GeneralManager extends Handler{`

`	@Override`

`	public String handleFeeRequest(String user, double fee) {`

`		String str = "";`

`		if(fee<10000) {`

`			if("zs".equals(user)) {`

`				str = "总经理同意了 "+user+" 的聚餐申请 "+fee+" 美元";				`

`			}else {`

`				str="boss拒绝了你的聚餐申请";`

`			}`

`		}else {`

`			str = "总经理觉得你花太多不让你去聚餐";`

`		}`

`		return str;`

`	}`

`}`

`//测试类`

`public class Test {`

`	public static void main(String[] args) {`

`		Handler pm = new ProjectManager();`

`		Handler dm = new DepManager();`

`		Handler gm = new GeneralManager();`

`		pm.setSuccessor(dm);`

`		dm.setSuccessor(gm);`

`		System.out.println(pm.handleFeeRequest("zs", 200));`

`		System.out.println(pm.handleFeeRequest("zs", 700));`

`		System.out.println(pm.handleFeeRequest("zs", 1500));`

`		System.out.println(pm.handleFeeRequest("zs", 15000));`

`	}`

`}`

运行结果：

![](/assets/image12_1.png)



