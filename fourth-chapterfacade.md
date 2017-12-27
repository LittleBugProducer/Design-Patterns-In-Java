# 第四章 外观\(Facade\)模式\(门面模式\)

1.概述

外观模式（Facade）,他隐藏了系统的复杂性，并向客户端提供了一个可以访问系统的接口。这种类型的设计模式属于结构性模式。为子系统中的一组接口提供了一个统一的访问接口，这个接口使得子系统更容易被访问或者使用。\([https://www.cnblogs.com/lthIU/p/5860607.html\](https://www.cnblogs.com/lthIU/p/5860607.html%29\)

2.模式中的角色

1）门面角色：外观模式的核心。它被客户角色调用，它熟悉子系统的功能。内部根据客户角色的需求预定了几种功能的组合。

2）子系统角色:实现了子系统的功能。它对客户角色和Facade是未知的。它内部可以有系统内的相互交互，也可以由供外界调用的接口。

3）客户角色:通过调用Facede来完成要实现的功能。

\([https://www.cnblogs.com/lthIU/p/5860607.html\](https://www.cnblogs.com/lthIU/p/5860607.html%29\)

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

