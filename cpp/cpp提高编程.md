//1.函数模板
//2.类模板
//3.STL

//加强巩固：数组 、 友元 、继承

//cpp什么数据会在堆区创建对象，需要手动释放，具体声明形式

# 1.函数模板

```cpp
//语法
//参数模板化
//使用时需要显示指明
//案例
//局限性
在上述代码中提供的赋值操作，如果传入的a和b是一个  .. 数组 ..  ，就无法实现了
在上述代码中，如果T的数据类型传入的是像Person这样的..自定义数据类型.. ，也无法正常运行
因此C++为了解决这种问题，提供模板的重载，可以为这些特定的类型提供具体化的模板
解决案例
#include<iostream>
using namespace std;

#include <string>

class Person
{
public:
	Person(string name, int age)
	{
		this->m_Name = name;
		this->m_Age = age;
	}
	string m_Name;
	int m_Age;
};

//普通函数模板
template<class T>
bool myCompare(T& a, T& b)
{
	if (a == b)
	{
		return true;
	}
	else
	{
		return false;
	}
}


//具体化，显示具体化的原型和定意思以template<>开头，并通过名称来指出类型
//具体化优先于常规模板
template<> bool myCompare(Person &p1, Person &p2)
{
	if ( p1.m_Name  == p2.m_Name && p1.m_Age == p2.m_Age)
	{
		return true;
	}
	else
	{
		return false;
	}
}

void test01()
{
	int a = 10;
	int b = 20;
	//内置数据类型可以直接使用通用的函数模板
	bool ret = myCompare(a, b);
	if (ret)
	{
		cout << "a == b " << endl;
	}
	else
	{
		cout << "a != b " << endl;
	}
}

void test02()
{
	Person p1("Tom", 10);
	Person p2("Tom", 10);
	//自定义数据类型，不会调用普通的函数模板
	//可以创建具体化的Person数据类型的模板，用于特殊处理这个类型
	bool ret = myCompare(p1, p2);
	if (ret)
	{
		cout << "p1 == p2 " << endl;
	}
	else
	{
		cout << "p1 != p2 " << endl;
	}
}

int main() {

	test01();

	test02();

	system("pause");

	return 0;
}

// :>  函数模板很简单，最关键的是他的局限性
```

# 2.类模板

```cpp

//语法
//参数模板化
//使用时需要显示指明
//类模板传入参数的方式
//  //1、指定传入的类型
void printPerson1(Person<string, int> &p)
// //2、参数模板化
template <class T1, class T2>
void printPerson2(Person<T1, T2>&p)
//  //3、整个类模板化
template<class T>
void printPerson3(T & p)
//类模板的继承
类模板与继承
当类模板碰到继承时，需要注意一下几点：

1.当子类继承的父类是一个类模板时，子类在声明的时候，要指定出父类中T的类型
2.如果不指定，编译器无法给子类分配内存
3.如果想灵活指定出父类中T的类型，子类也需变为类模板

//模板创建时机的问题，创建时机为使用时有可能会链接不到

掌握类模板成员函数分文件编写产生的问题以及解决方式
问题：

类模板中成员函数创建时机是在调用阶段，导致分文件编写时链接不到
解决：

解决方式1：直接包含.cpp源文件
解决方式2：将声明和实现写到同一个文件中，并更改后缀名为.hpp，hpp是约定的名称，并不是强制

文件 xx.hpp  抽象类声明与实现写在一起

#pragma once
#include <iostream>
using namespace std;
#include <string>

template<class T1, class T2>
class Person {
public:
	Person(T1 name, T2 age);
	void showPerson();
public:
	T1 m_Name;
	T2 m_Age;
};

//构造函数 类外实现
template<class T1, class T2>
Person<T1, T2>::Person(T1 name, T2 age) {
	this->m_Name = name;
	this->m_Age = age;
}

//成员函数 类外实现
template<class T1, class T2>
void Person<T1, T2>::showPerson() {
	cout << "姓名: " << this->m_Name << " 年龄:" << this->m_Age << endl;
}

//友元
//案例
```

# 3.类与对象

#### （1）封装

意义：

* 将属性和行为作为一个整体，表现生活中的事物
* 将属性和行为加以权限控制
* class默认权限为私有
* struct 默认权限为公共
* class 默认权限为私有

#### （2）成员属性设置为私有的优点

* 优点1：将所有成员属性设置为私有，可以自己控制读写权限
* 优点2：对于写权限，我们可以检测数据的有效性

#### （3）对象的初始化和清理  构造函数  析构函数

* 编译器提供的构造函数和析构函数是空实现。
* 构造函数：主要作用在于创建对象时为对象的成员属性赋值，构造函数由编译器自动调用，无须手动调用。
* 析构函数：主要作用在于对象**销毁前**系统自动调用，执行一些清理工作。

 **构造函数语法：** `类名(){}`

1. 构造函数，没有返回值也不写void
2. 函数名称与类名相同
3. 构造函数可以有参数，因此可以发生重载
4. 程序在调用对象时候会自动调用构造，无须手动调用,而且只会调用一次

**析构函数语法：** `~类名(){}`

1. 析构函数，没有返回值也不写void
2. 函数名称与类名相同,在名称前加上符号 ~
3. **析构函数不可以有参数，因此不可以发生重载**
4. 程序在对象销毁前会自动调用析构，无须手动调用,而且只会调用一次

#### (4)构造函数的分类及调用

* 两种分类方式：

 按参数分为： 有参构造和无参构造

 按类型分为： 普通构造和拷贝构造

* 三种调用方式：

1. 括号法
2. 显示法
3. 隐式转换法

```cpp
//1、构造函数分类
// 按照参数分类分为 有参和无参构造   无参又称为默认构造函数
// 按照类型分类分为 普通构造和拷贝构造

class Person {
public:
	//无参（默认）构造函数
	Person() {
		cout << "无参构造函数!" << endl;
	}
	//有参构造函数
	Person(int a) {
		age = a;
		cout << "有参构造函数!" << endl;
	}
	//拷贝构造函数
	Person(const Person& p) {
		age = p.age;
		cout << "拷贝构造函数!" << endl;
	}
	//析构函数
	~Person() {
		cout << "析构函数!" << endl;
	}
public:
	int age;
};

//2、构造函数的调用
//调用无参构造函数
void test01() {
	Person p; //调用无参构造函数
}

//调用有参的构造函数
void test02() {

	//2.1  括号法，常用
	Person p1(10);
	//注意1：调用无参构造函数不能加括号，如果加了编译器认为这是一个函数声明
	//Person p2();

	//2.2 显式法
	Person p2 = Person(10); 
	Person p3 = Person(p2);
	//Person(10)单独写就是匿名对象  当前行结束之后，马上析构

	//2.3 隐式转换法
	Person p4 = 10; // Person p4 = Person(10); 
	Person p5 = p4; // Person p5 = Person(p4); 

	//注意2：不能利用 拷贝构造函数 初始化匿名对象 编译器认为是对象声明
	//Person p5(p4);
}

int main() {

	test01();
	//test02();

	system("pause");

	return 0;
}
```

#### （5）构造函数调用规则

默认情况下，c++编译器至少给一个类添加3个函数

1．默认构造函数(无参，函数体为空)

2．默认析构函数(无参，函数体为空)

3．默认拷贝构造函数，对属性进行值拷贝

构造函数调用规则如下：

* 如果用户定义有参构造函数，c++不在提供默认无参构造，但是会提供默认拷贝构造
* 如果用户定义拷贝构造函数，c++不会再提供其他构造函数
* 如果不写构造函数，编译器会自动添加拷贝构造，并且做浅拷贝操作

**浅拷贝，深拷贝注意事项：**

* 如果不利用深拷贝在堆区创建新内存，会导致浅拷贝带来的**重复释放堆区**问题
* 如果属性有在**堆区**开辟的，一定要自己提供拷贝构造函数，防止浅拷贝带来的问题

#### (6)初始化列表

`Person(int a, int b, int c) :m_A(a), m_B(b), m_C(c) {}`
#### (7)类对象作为类成员
C++类中的成员可以是另一个类的对象，我们称该成员为 对象成员
#### (8)静态对象
静态成员
静态成员就是在成员变量和成员函数前加上关键字static，称为静态成员
**共享共用**
静态成员分为：

静态成员变量
* 所有对象共享同一份数据
* 在编译阶段分配内存
* 类内声明，类外初始化
静态成员函数
* 所有对象共享同一个函数
* 静态成员函数只能访问静态成员变量