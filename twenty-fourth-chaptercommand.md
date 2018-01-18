### 第二十四章 命令\(Command\)模式

### 定义：

将“请求”封装成对象，以便使用不同的请求、队列或者日志来参数化其他对象。命令模式也支持可撤销的操作。

### 角色：

1、Command：所有命令的抽象类，一般需要对外公开一个执行命令的方法execute，如有需要还需提供一个命令的撤销方法undo。

2、ConcreteCommand：命令的实现类。

3、Invoker：调用者，负责命令的调度。

4、Reveiver：接收者，负责命令的接收和执行。

5、Client：客户端，命令的发起者。

案例：

银行存款与撤销\([http://wuhongyu.iteye.com/blog/2027964\](http://wuhongyu.iteye.com/blog/2027964\)\)

`//银行系统Receiver`

`public class Ccb {`

```
public void saveMoney\(long amount\) {

    System.out.println\("向建设银行存入金额"+amount\);

}

public void fetchMoney\(long amount\) {

    System.out.println\("从建设银行取出金额"+amount\);

}
```

`}`

`//银行系统Receiver`

`public class Cmb {`

```
public void quQian\(long amoung\) {

    System.out.println\("从招商银行取出金额"+amoung\);

}

public void cunQian\(long amount\) {

    System.out.println\("向招商银行存入金额"+amount\);

}
```

`}`

`//命令的抽象类`

`public interface Command {`

```
void execute\(\);

void undo\(\);
```

`}`

`public class CcbDepositCommand implements Command{`

```
private Ccb ccb = new Ccb\(\);


@Override

public void execute\(\) {

    ccb.saveMoney\(100\);

}

@Override

public void undo\(\) {

    ccb.fetchMoney\(100\);

}
```

`}`

`public class CcbWithdrawCommand implements Command{`

```
private Ccb ccb = new Ccb\(\);

@Override

public void execute\(\) {

    ccb.fetchMoney\(100\);

}

@Override

public void undo\(\) {

    ccb.saveMoney\(100\);

}
```

`}`

`public class CmbDepositCommand implements Command{`

```
private Cmb cmb = new Cmb\(\);

@Override

public void execute\(\) {

    cmb.cunQian\(100\);

}

@Override

public void undo\(\) {

    cmb.quQian\(100\);

}
```

`}`

`public class CmbWithdrawCommand implements Command{`

```
private Cmb cmb = new Cmb\(\);


@Override

public void execute\(\) {

    cmb.quQian\(100\);

}


@Override

public void undo\(\) {

    cmb.cunQian\(100\);

}
```

`}`

`public class NoCommand implements Command{`

```
@Override

public void execute\(\) {

}


@Override

public void undo\(\) {

}
```

`}`

`//调用者invoker`

`public class ATM {`

```
private Command\[\] commands;


public ATM\(\) {

    this.commands = new Command\[\] {new NoCommand\(\)};

}


public void setCommand\(Command command\[\]\) {

    this.commands = command;

}


public void action\(int i\) {

    this.commands\[i\].execute\(\);

}

public void cancel\(int i\) {

    this.commands\[i\].undo\(\);

}
```

`}`

`//使用ATM的人就是客户端Client`

`public class Test {`

```
public static void main\(String\[\] args\) {

    ATM atm = new ATM\(\);

    Command\[\] commands = new Command\[3\];

    commands\[0\] = new CcbDepositCommand\(\);

    commands\[1\]=new CcbWithdrawCommand\(\);

    commands\[2\]=new CmbWithdrawCommand\(\);

    atm.setCommand\(commands\);

    atm.action\(0\);

    atm.cancel\(0\);

    atm.action\(1\);

    atm.cancel\(1\);

    atm.action\(2\);

    atm.cancel\(2\);

}
```

`}`

运行结果：

![](/assets/image24_1.png)

