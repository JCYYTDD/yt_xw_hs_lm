//1.函数模板
//2.类模板
//3.STL


//加强巩固：数组 、 友元 、继承

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
