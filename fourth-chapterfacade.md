# 第四章 外观\(Facade\)模式\(门面模式\)

1.概述

外观模式（Facade）,他隐藏了系统的复杂性，并向客户端提供了一个可以访问系统的接口。这种类型的设计模式属于结构性模式。为子系统中的一组接口提供了一个统一的访问接口，这个接口使得子系统更容易被访问或者使用。\([https://www.cnblogs.com/lthIU/p/5860607.html\](https://www.cnblogs.com/lthIU/p/5860607.html%29\)

2.模式中的角色

1）门面角色：外观模式的核心。它被客户角色调用，它熟悉子系统的功能。内部根据客户角色的需求预定了几种功能的组合。

2）子系统角色:实现了子系统的功能。它对客户角色和Facade是未知的。它内部可以有系统内的相互交互，也可以由供外界调用的接口。

3）客户角色:通过调用Facede来完成要实现的功能。

\([https://www.cnblogs.com/lthIU/p/5860607.html\](https://www.cnblogs.com/lthIU/p/5860607.html%29\)

3.外观模式的使用场景\(https://www.cnblogs.com/liumu/p/5245691.html\):

1\)首先，在设计初期阶段，应该要有意识的将不同的两个层分离，比如经典的三层架构，就需要考虑在数据访问层和业务逻辑，业务逻辑层和表示层的层与层之间建立外观Facade\(外观\)，这样可以为复杂的子系统提供应该简单的接口，使得耦合大大降低。

2\)其次，在开发阶段，子系统往往因为不断的重构演化而变得越来越复杂，大多数的模式使用时也都会产生很多很小的类，这本是好事，但也给外部调用它们的用户程序带来使用上的困难，增加外观Facade可以提供一个简单接口，减少它们之间的依赖。

3\)在维护一个遗留的大型系统时，可能这个系统已经非常难以维护和扩展了，但因为它包含非常重要的功能，新的需求开发必须依赖它。此时用外观模式Facade也是非常合适的。你可以为新系统开发一个外观Facade类，来提供设计粗糙或高度复杂的遗留代码的比较清晰简单的接口，让新系统与Facade对象进行交互，Facade与遗留代码交互所有复杂的工作。

案例：

`public class Employee {`

```
private String name;

private int age;

private Salary salary;
```

`//getter and settter...`

`}`

`public class Salary {`

```
private String from;

private String to;

private double Amount;
```

`//getter and settter...`

`}`

`//一个子系统`

`public class EmployeeDataAccess {`

```
public void saveEmployee\(Employee employee\) {

    System.out.println\("Save employee to database."\);

}

public void deleteEmployee\(Employee employee\)

{

    System.out.println\("Remove employee from database"\);

}
```

`}`

`//门面类facade`

`public class DataAccess {`

```
private EmployeeDataAccess employeeDataAccess = new EmployeeDataAccess\(\);

private SalaryDataAccess salaryDataAccess = new SalaryDataAccess\(\);



public void saveEmployee\(Employee employee\) {

    employeeDataAccess.saveEmployee\(employee\);

    salaryDataAccess.saveSalary\(employee.getSalary\(\)\);

}

public void removeEmployee\(Employee employee\) {

    employeeDataAccess.deleteEmployee\(employee\);

    salaryDataAccess.deleteSalary\(employee.getSalary\(\)\);

}
```

`}`

`//另一个子系统`

`public class SalaryDataAccess {`

```
public void saveSalary\(Salary salary\) {

    System.out.println\("Save salary to database"\);

}

public void deleteSalary\(Salary salary\) {

    System.out.println\("Remove salary from database."\);

}
```

`}`

`//测试类`

`public class Test {`

```
public static void main\(String\[\] args\) {

    DataAccess dataAccess = new DataAccess\(\);

    Employee employee = new Employee\(\);

    employee.setName\("xiao hong"\);

    employee.setAge\(22\);

    employee.setSalary\(new Salary\(\) {

        @Override

        public void setFrom\(String from\) {

            super.setFrom\("2017-12-27"\);

        };

        @Override

        public void setTo\(String to\) {

            super.setTo\("2018-12-27"\);

        }

        @Override

        public void setAmount\(double amount\) {

            super.setTo\("2018-12-27"\);

        }

    }\);

    dataAccess.saveEmployee\(employee\);

    dataAccess.removeEmployee\(employee\);

}
```

`}`

测试结果：

![](/assets/image4_1.png)

