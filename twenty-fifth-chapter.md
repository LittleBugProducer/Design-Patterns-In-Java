# 第二十五章 解释器\(Interpreter\)模式

定义：

解释器模式是类的行为模式。给定一个语言之后，解释器模式可以定义出其文法的一种表示，并同时提供一个解释器。客户端可以使用这个解释器来解释这个语言中的句子。

角色：

（1）抽象表达式\(Expression\)角色：声明一个所有的具体表达式角色都需要实现的抽象接口。这个接口主要是一个interpret\(\)方法，称做解释操作。

（2）终结符表达式\(Terminal Expression\)角色：实现了抽象表达式角色所要求的接口，主要是一个interpret\(\)方法；文法中的每一个终结符都有一个具体终结表达式与之相对应。比如有一个简单的公式R=R1+R2，在里面R1和R2就是终结符，对应的解析R1和R2的解释器就是终结符表达式。

（3）非终结符表达式\(Nonterminal Expression\)角色：文法中的每一条规则都需要一个具体的非终结符表达式，非终结符表达式一般是文法中的运算符或者其他关键字，比如公式R=R1+R2中，“+"就是非终结符，解析“+”的解释器就是一个非终结符表达式。

（4）环境\(Context\)角色：这个角色的任务一般是用来存放文法中各个终结符所对应的具体值，比如R=R1+R2，我们给R1赋值100，给R2赋值200。这些信息需要存放到环境角色中，很多情况下我们使用Map来充当环境角色就足够了。

案例:

`//抽象表达式角色`

`public abstract class Expression {`

`	public abstract boolean interpret(Context xtc);	`

`	public abstract boolean equals(Object obj);	`

`	public abstract int hashCode();	`

`	public abstract String toString();`

`}`

`//终结符表达式常量`

`public class Constant extends Expression{`

`	private boolean value;	`

`	public Constant(boolean value) {`

`		this.value = value;		`

`	}	`

`	@Override`

`	public boolean interpret(Context xtc) {`

`		return value;`

`	}`

`	@Override`

`	public boolean equals(Object obj) {`

`		if(obj!=null&&obj instanceof Constant){`

`			return this.value ==((Constant)obj).value;`

`		}`

`		return false;`

`	}`

`	@Override`

`	public int hashCode() {`

`		return this.toString().hashCode();`

`	}`

`	@Override`

`	public String toString() {`

`		return new Boolean(value).toString();`

`	}	`

`}`

`//终结符表达式变量`

`public class Variable extends Expression{`

`	private String name;	`

`	public Variable(String name) {`

`		this.name = name;`

`	}`

`	@Override`

`	public boolean interpret(Context xtc) {`

`		return xtc.lookup(this);`

`	}`

`	@Override`

`	public boolean equals(Object obj) {`

`		if(obj!=null&&obj instanceof Variable) {`

`			return this.name.equals(((Variable)obj).name);`

`		}`

`		return false;`

`	}`

`	@Override`

`	public int hashCode() {`

`		return this.toString().hashCode();`

`	}`

`	@Override`

`	public String toString() {`

`		return name;`

`	}`

`}`

`//非终结符表达式与`

`public class And extends Expression{`

`	private Expression left,right;	`

`	public And(Expression left,Expression right) {`

`		this.left = left;`

`		this.right = right;`

`	}	`

`	@Override`

`	public boolean interpret(Context xtc) {		`

`		return left.interpret(xtc)&&right.interpret(xtc);`

`	}`

`	@Override`

`	public boolean equals(Object obj) {`

`		if(obj!=null&&obj instanceof And) {`

`			return left.equals(((And)obj).left)&&right.equals(((And)obj).right);`

`		}`

`		return false;`

`	}`

`	@Override`

`	public int hashCode() {`

`		return this.toString().hashCode();`

`	}`

`	@Override`

`	public String toString() {`

``

`		return "("+left.toString()+" AND "+right.toString()+")";`

`	}	`

`}`

`//非终结符表达式或`

`public class Or extends Expression{`

`	private Expression left,right;	`

`	public Or(Expression left,Expression right) {`

`		this.left = left;`

`		this.right = right;		`

`	}`

`	@Override`

`	public boolean interpret(Context xtc) {`

`		return left.interpret(xtc)||right.interpret(xtc);		`

`	}`

`	@Override`

`	public boolean equals(Object obj) {`

`		if(obj!=null&&obj instanceof Or) {`

`			return this.left.equals(((Or)obj).left)&&this.right.equals(((Or)obj).right);`

`		}`

`		return false;`

`	}`

`	@Override`

`	public int hashCode() {`

`		return this.toString().hashCode();`

`	}`

`	@Override`

`	public String toString() {`

`		return "("+left.toString()+" OR "+right.toString()+")";`

`	}`

`}`

`//非终结符表达式非`

`public class Not extends Expression{`

`	private Expression exp;	`

`	public Not(Expression exp) {`

`		this.exp = exp;`

`	}`

`	@Override`

`	public boolean interpret(Context xtc) {`

`		return !exp.interpret(xtc);`

`	}`

`	@Override`

`	public boolean equals(Object obj) {`

`		if(obj!=null&&obj instanceof Not) {`

`			return exp.equals(((Not)obj).exp);`

`		}`

`		return false;`

`	}`

`	@Override`

`	public int hashCode() {`

`		return this.toString().hashCode();`

`	}`

`	@Override`

`	public String toString() {`

`		return "(Not "+exp.toString()+")";`

`	}	`

`}`

`//环境角色`

`public class Context {	`

`	private Map<Variable, Boolean>map = new HashMap<Variable,Boolean>();	`

`	public void assign(Variable var,boolean value) {`

`		map.put(var, value);`

`	}`

`	public boolean lookup(Variable var)throws IllegalArgumentException{`

`		Boolean value = map.get(var);`

`		if(value==null) {`

`			throw new IllegalArgumentException();`

`		}`

`		return value.booleanValue();`

`	}`

`}`

`public class Test {`

`	public static void main(String[] args) {`

`		Context ctx = new Context();`

`		Variable x = new Variable("x");`

`		Variable y = new Variable("y");`

`		Constant c = new Constant(true);`

`		ctx.assign(x, false);`

`		ctx.assign(y, true);`

`		Expression exp = new Or(new And(c, x), new And(y, new Not(x)));`

`		System.out.println("x="+x.interpret(ctx));`

`		System.out.println("y="+y.interpret(ctx));`

`		System.out.println(exp.toString()+"="+exp.interpret(ctx));`

`	}`

`}`

运行结果：

![](/assets/image25_1.png)



