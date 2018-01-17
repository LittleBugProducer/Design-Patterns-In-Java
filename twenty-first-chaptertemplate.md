### 第二十一章 模板方法\(Template Method\)模式

### 概述

定义一个操作中的算法的骨架，而将步骤延迟到子类中。模板方法使得子类可以不改变一个算法的结构即可重定义算法的某些特定步骤。

### 角色

抽象类（AbstractClass）：实现了模板方法，定义了算法的骨架。

具体类（ConcreteClass\)：实现抽象类中的抽象方法，已完成完整的算法。

钩子方法:一个钩子方法由一个抽象类或具体类声明并实现，而其子类可能会加以扩展。通常在父类给出的实现是一个空实现（可使用virtual关键字将其定义为虚函数\),并以该空实现作为方法的默认实现，当然钩子方法也可以提供一个非空的默认实现。

案例\(冒泡排序\)：

//抽象类

public abstract class BubbleSorter {



	private int operations=0;

	protected int length=0;

	

	protected int doSort\(\) {

		operations=0;

		if\(length&lt;=1\) {

			return operations;

		}

		for\(int nextToLast=length-1;nextToLast&gt;=0;nextToLast--\) {

			for\(int index=0;index&lt;nextToLast;index++\) {

				if\(outOfOrder\(index\)\) {

					swap\(index\);

				}

				operations++;

			}

		}

		return operations;

	}

	

	protected abstract void swap\(int index\);

	

	public abstract boolean outOfOrder\(int index\) ;

}

//具体类

public class IntBubbleSorter extends BubbleSorter{

	private int\[\] array=null;

	

	public int Sort\(int\[\] theArray\) {

		array = theArray;

		length = array.length;

		return doSort\(\);

	}



	public int\[\] getArray\(\) {

		return array;

	}



	public void setArray\(int\[\] array\) {

		this.array = array;

	}



	@Override

	protected void swap\(int index\) {

		int temp = array\[index\];

		array\[index\] = array\[index+1\];

		array\[index+1\]=temp;

	}



	@Override

	public boolean outOfOrder\(int index\) {

		return \(array\[index\]&gt;array\[index+1\]\);

	}

	



}

//具体类

public class FloatBubbleSorter extends BubbleSorter{



	private float\[\] array=null;

	

	

	

	public float\[\] getArray\(\) {

		return array;

	}



	public void setArray\(float\[\] array\) {

		this.array = array;

	}



	public int sort\(float\[\] theArray\) {

		array = theArray;

		length = array.length;

		return doSort\(\);

	}

	

	@Override

	protected void swap\(int index\) {

		float temp = array\[index\];

		array\[index\]=array\[index+1\];

		array\[index+1\]=temp;

	}



	@Override

	public boolean outOfOrder\(int index\) {

		return \(array\[index\]&gt;array\[index+1\]\);

	}

	



}

public class Test {



	public static void main\(String\[\] args\) {

		int\[\] intArray=new int\[\] {5,3,12,8,10};

		IntBubbleSorter sorter = new IntBubbleSorter\(\);

		sorter.Sort\(intArray\);

		for \(int i : intArray\) {

			System.out.print\(i+" "\);

		}

		System.out.println\(\);

		float\[\] floatArray = new float\[\] {5.0f,3.0f,12.0f,8.0f,10.0f};

		FloatBubbleSorter sorter2 = new FloatBubbleSorter\(\);

		sorter2.sort\(floatArray\);

		for \(float f : floatArray\) {

			System.out.print\(f+" "\);

		}

		System.out.println\(\);

	}

}

