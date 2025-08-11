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

```cpp
class Person
{

public:

	static int m_A; //静态成员变量

	//静态成员变量特点：
	//1 在编译阶段分配内存
	//2 类内声明，类外初始化
	//3 所有对象共享同一份数据

	void setm_B(int k) {
		m_B = k;
	}

	void get_B() {
		cout << m_B;
	}

private:
	static int m_B; //静态成员变量也是有访问权限的
};
int Person1::m_A = 10;
int Person1::m_B = 10;

```

```cpp
class game {
public:

	void getAll() {
		getStatus();
		cout << "目前游戏的状态";
	}



private:
	static void getStatus() {//静态成员函数
		cout << status;
	}

	static int status;
};

int game::status = 0;


void test002() {
	game g;
	g.getAll();
}




```

* 静态成员函数只能访问静态成员变量
* 类内其他函数可以直接使用函数名调用类内函数

调用方式	                         非静态成员函数	                                   静态成员函数
非静态成员函数调用	✅ 直接调用（func()）              	✅ 直接调用（staticFunc()）
静态成员函数调用	   ❌ 不能直接调用（需通过对象）	✅ 直接调用（staticFunc()）
类内函数可以直接调用其他函数名，但 静态函数不能直接调用非静态成员函数（因为没有 this 指针）。

推荐：在类内部直接使用函数名调用，保持代码简洁；静态函数调用非静态成员时，必须传入对象实例。

#### (9) C++对象模型和this指针

在C++中，类内的成员变量和成员函数分开存储

只有非静态成员变量才属于类的对象上

* 非静态成员变量占用对象空间
* 静态成员变量不占用对象空间
* 函数也不占用对象空间，所有函数共享一个函数实例

this指针不需要定义，直接使用即可

* this指针的用途：

1.当形参和成员变量同名时，可用this指针来区分
2.在类的非静态成员函数中返回对象本身，可使用return *this
3.this指针是隐含每一个非静态成员函数内的一种指针

* 空指针访问成员函数
  1.C++中空指针也是可以调用成员函数的，但是也要注意有没有用到this指针

2.如果用到this指针，需要加以判断保证代码的健壮性

```cpp
//空指针访问成员函数
class Person {
public:

	void ShowClassName() {
		cout << "我是Person类!" << endl;
	}

	void ShowPerson() {
		if (this == NULL) {
			return;
		}
		cout << mAge << endl;
	}

public:
	int mAge;
};

void test01()
{
	Person * p = NULL;
	p->ShowClassName(); //空指针，可以调用成员函数
	p->ShowPerson();  //但是如果成员函数中用到了this指针，就不可以了
}
```

#### (10) const修饰成员函数

* 常函数：

1.成员函数后加const后我们称为这个函数为常函数
2.常函数内不可以修改成员属性
3.成员属性声明时加关键字mutable后，在常函数中依然可以修改

* 常对象：

1.声明对象前加const称该对象为常对象
2.常对象只能调用常函数

```cpp
class Person {
public:
	Person() {
		m_A = 0;
		m_B = 0;
	}

	//this指针的本质是一个指针常量，指针的指向不可修改
	//如果想让指针指向的值也不可以修改，需要声明常函数
	void ShowPerson() const {
		//const Type* const pointer;
		//this = NULL; //不能修改指针的指向 Person* const this;
		//this->mA = 100; //但是this指针指向的对象的数据是可以修改的

		//const修饰成员函数，表示指针指向的内存空间的数据不能修改，除了mutable修饰的变量
		this->m_B = 100;
	}

	void MyFunc() const {
		//mA = 10000;
	}

public:
	int m_A;
	mutable int m_B; //可修改 可变的
};


```

#### (11)友元

* 友元的目的就是让一个函数或者类 访问**另一个类中**私有成员
* 友元的关键字为 friend

友元的三种实现

1.全局函数作友元
2.类做友元
3.成员函数做友元

1. new Building vs new Building()
   表达式	行为	适用场景
2. new Building	调用默认构造函数（如果存在），不进行值初始化（内置类型如 int、float 等可能包含垃圾值）。	适用于类类型（class/struct），且不需要初始化内置类型成员。

new Building()	调用默认构造函数，并进行值初始化（内置类型会被初始化为 0、nullptr 等）。	适用于需要确保所有成员（包括内置类型）初始化为零值的情况。

* Building b;与Building* b = new Building; 声明对象的区别
  特性	Building b; (栈)	           |    Building* b = new Building; (堆)
  存储位置	栈	                        |  堆
  生命周期	自动（作用域结束销毁）        |	手动（需 delete）
  访问方式	b.method()	                |  b->method()
  内存管理	自动	                    |手动（易内存泄漏）
  推荐使用场景	局部变量、临时对象        |	需长期存活或跨作用域的对象
  现代 C++ 替代方案	无（本身就是最优方式）|	std::unique_ptr/std::shared_ptr

#### (12)运算符重载

* 加号运算符重载

```cpp
class Person {
public:
	Person() {};
	Person(int a, int b)
	{
		this->m_A = a;
		this->m_B = b;
	}
	//成员函数实现 + 号运算符重载
	Person operator+(const Person& p) {
		Person temp;
		temp.m_A = this->m_A + p.m_A;
		temp.m_B = this->m_B + p.m_B;
		return temp;
	}


public:
	int m_A;
	int m_B;
};

//全局函数实现 + 号运算符重载
//Person operator+(const Person& p1, const Person& p2) {
//	Person temp(0, 0);
//	temp.m_A = p1.m_A + p2.m_A;
//	temp.m_B = p1.m_B + p2.m_B;
//	return temp;
//}

//运算符重载 可以发生函数重载 
Person operator+(const Person& p2, int val)  
{
	Person temp;
	temp.m_A = p2.m_A + val;
	temp.m_B = p2.m_B + val;
	return temp;
}

void test() {

	Person p1(10, 10);
	Person p2(20, 20);

	//成员函数方式
	Person p3 = p2 + p1;  //相当于 p2.operaor+(p1)
	cout << "mA:" << p3.m_A << " mB:" << p3.m_B << endl;


	Person p4 = p3 + 10; //相当于 operator+(p3,10)
	cout << "mA:" << p4.m_A << " mB:" << p4.m_B << endl;

}

int main() {

	test();

	system("pause");

	return 0;
}
```

* 左移运算符重载

```cpp
class Person {
	friend ostream& operator<<(ostream& out, Person& p);

public:

	Person(int a, int b)
	{
		this->m_A = a;
		this->m_B = b;
	}

	//成员函数 实现不了  p << cout 不是我们想要的效果
	//void operator<<(Person& p){
	//}

private:
	int m_A;
	int m_B;
};

//全局函数实现左移重载
//ostream对象只能有一个
ostream& operator<<(ostream& out, Person& p) {
	out << "a:" << p.m_A << " b:" << p.m_B;
	return out;
}
```

* 递增运算符重载

```cpp
class MyInteger {

	friend ostream& operator<<(ostream& out, MyInteger myint);

public:
	MyInteger() {
		m_Num = 0;
	}
	//前置++
	MyInteger& operator++() {
		//先++
		m_Num++;
		//再返回
		return *this;
	}

	//后置++
	MyInteger operator++(int) {
		//先返回
		MyInteger temp = *this; //记录当前本身的值，然后让本身的值加1，但是返回的是以前的值，达到先返回后++；
		m_Num++;
		return temp;
	}

private:
	int m_Num;
};


ostream& operator<<(ostream& out, MyInteger myint) {
	out << myint.m_Num;
	return out;
}

```

* 总结： 前置递增返回引用，后置递增返回值
* c++编译器至少给一个类添加4个函数
  1.默认构造函数(无参，函数体为空)
  2.默认析构函数(无参，函数体为空)
  3.默认拷贝构造函数，对属性进行值拷贝
  4.赋值运算符 operator=, 对属性进行值拷贝
  5.如果类中有属性指向堆区，做赋值操作时也会出现深浅拷贝问题

```cpp
class Person
{
public:

	Person(int age)
	{
		//将年龄数据开辟到堆区
		m_Age = new int(age);
	}

	//重载赋值运算符 
	Person& operator=(Person &p)
	{
		if (m_Age != NULL)
		{
			delete m_Age;
			m_Age = NULL;
		}
		//编译器提供的代码是浅拷贝
		//m_Age = p.m_Age;

		//提供深拷贝 解决浅拷贝的问题
		m_Age = new int(*p.m_Age);

		//返回自身
		return *this;
	}


	~Person()
	{
		if (m_Age != NULL)
		{
			delete m_Age;
			m_Age = NULL;
		}
	}

	//年龄的指针
	int *m_Age;

};

```

* 关系运算符重载

```cpp
class Person
{
public:
	Person(string name, int age)
	{
		this->m_Name = name;
		this->m_Age = age;
	};

	bool operator==(Person & p)
	{
		if (this->m_Name == p.m_Name && this->m_Age == p.m_Age)
		{
			return true;
		}
		else
		{
			return false;
		}
	}

	bool operator!=(Person & p)
	{
		if (this->m_Name == p.m_Name && this->m_Age == p.m_Age)
		{
			return false;
		}
		else
		{
			return true;
		}
	}

	string m_Name;
	int m_Age;
};

void test01()
{
	//int a = 0;
	//int b = 0;

	Person a("孙悟空", 18);
	Person b("孙悟空", 18);

	if (a == b)
	{
		cout << "a和b相等" << endl;
	}
	else
	{
		cout << "a和b不相等" << endl;
	}

	if (a != b)
	{
		cout << "a和b不相等" << endl;
	}
	else
	{
		cout << "a和b相等" << endl;
	}
}

```

* 函数调用运算符重载

```cpp
class MyPrint
{
public:
	void operator()(string text)
	{
		cout << text << endl;
	}

};
void test01()
{
	//重载的（）操作符 也称为仿函数
	MyPrint myFunc;
	myFunc("hello world");
}


class MyAdd
{
public:
	int operator()(int v1, int v2)
	{
		return v1 + v2;
	}
};

void test02()
{
	MyAdd add;
	int ret = add(10, 10);
	cout << "ret = " << ret << endl;

	//匿名对象调用  
	cout << "MyAdd()(100,100) = " << MyAdd()(100, 100) << endl;
}

```

#### (13)继承

定义这些类，下级别的成员除了拥有上一级的共性，还有自己的特性。

这个时候我们就可以考虑利用继承的技术，减少重复代码

继承的好处：可以减少重复的代码

* class A : public B;

1.A 类称为子类 或 派生类

2.B 类称为父类 或 基类

3.派生类中的成员，包含两大部分：

4.一类是从基类继承过来的，一类是自己增加的成员。

5.从基类继承过过来的表现其共性，而新增的成员体现了其个性。

继承方式

* 继承的语法：class 子类 : 继承方式 父类
* 继承方式一共有三种：
  1.公共继承
  2.保护继承
  3.私有继承
* public 继承（最常用）
  基类成员的访问权限在派生类中保持不变：

public → public

protected → protected

private → 不可访问（基类的 private 始终仅对基类可见）

```cpp
class Base {
public:
    int x;
protected:
    int y;
private:
    int z;
};

class Derived : public Base {
    // x 仍然是 public
    // y 仍然是 protected
    // z 不可访问
};
```

```cpp
Derived d;
d.x;  // ✅ 可访问（public）
d.y;  // ❌ 不可访问（protected）
d.z;  // ❌ 不可访问（private）
```

* protected 继承
  基类的 public 和 protected 成员在派生类中变为 protected：

public → protected

protected → protected

private → 不可访问

语义：派生类继承基类的实现，但不暴露基类接口（少见用法）。

```cpp
class Derived : protected Base {
    // x 变为 protected
    // y 仍然是 protected
    // z 不可访问
};
```

```cpp
Derived d;
d.x;  // ❌ 不可访问（现在是 protected）
d.y;  // ❌ 不可访问（protected）
d.z;  // ❌ 不可访问（private）
```

* private 继承（极少用）
  基类的所有成员在派生类中变为 private：

public → private

protected → private

private → 不可访问

语义：派生类"按实现继承"基类，但完全隐藏基类接口（通常用组合替代）。

```cpp
class Derived : private Base {
    // x 变为 private
    // y 变为 private
    // z 不可访问
};
```

```cpp
Derived d;
d.x;  // ❌ 不可访问（现在是 private）
d.y;  // ❌ 不可访问（private）
d.z;  // ❌ 不可访问（private）
```

* 优先用 public 继承表示接口继承。
* 优先用组合（成员变量）替代 protected/private 继承。
* 派生类内部：

1.可以访问基类的 public 和 protected 成员（权限可能被继承方式降级）。

2.永远不能直接访问基类的 private 成员（需通过基类的 public/protected 方法间接访问）。

* 外部代码：

1.能否访问继承的成员变量，取决于该成员在派生类中的最终权限（由继承方式决定）。

2.private 成员的访问：

3.若派生类需要访问基类的 private 成员，基类必须提供 public/protected 的接口（如 getter/setter）。

*如何让派生类访问基类的 private 成员？
通过基类的 protected 方法（或 friend 机制，但破坏封装）

* 最佳实践
  1.优先用 public 继承：表示 "is-a" 关系（如 Dog 继承 Animal）。

2.慎用 protected/private 继承：通常用组合（成员对象）替代。

3.封装基类 private 成员：通过接口暴露必要功能，而非直接访问。

* 结论： 父类中私有成员也是被子类继承下去了，只是由编译器给隐藏后访问不到
* 继承中构造和析构顺序
  继承中 先调用父类构造函数，再调用子类构造函数，析构顺序与构造相反

```cpp
class Base 
{
public:
	Base()
	{
		cout << "Base构造函数!" << endl;
	}
	~Base()
	{
		cout << "Base析构函数!" << endl;
	}
};

class Son : public Base
{
public:
	Son()
	{
		cout << "Son构造函数!" << endl;
	}
	~Son()
	{
		cout << "Son析构函数!" << endl;
	}

};


void test01()
{
	//继承中 先调用父类构造函数，再调用子类构造函数，析构顺序与构造相反
	Son s;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

* 继承同名成员处理方式
  问题：当子类与父类出现同名的成员，如何通过子类对象，访问到子类或父类中同名的数据呢？

访问子类同名成员 直接访问即可
访问父类同名成员 需要加作用域

```cpp
class Base {
public:
	Base()
	{
		m_A = 100;
	}

	void func()
	{
		cout << "Base - func()调用" << endl;
	}

	void func(int a)
	{
		cout << "Base - func(int a)调用" << endl;
	}

public:
	int m_A;
};


class Son : public Base {
public:
	Son()
	{
		m_A = 200;
	}

	//当子类与父类拥有同名的成员函数，子类会隐藏父类中所有版本的同名成员函数
	//如果想访问父类中被隐藏的同名成员函数，需要加父类的作用域
	void func()
	{
		cout << "Son - func()调用" << endl;
	}
public:
	int m_A;
};

void test01()
{
	Son s;

	cout << "Son下的m_A = " << s.m_A << endl;
	cout << "Base下的m_A = " << s.Base::m_A << endl;

	s.func();
	s.Base::func();
	s.Base::func(10);

}
```

* 总结：

1.子类对象可以直接访问到子类中同名成员
2.子类对象加作用域可以访问到父类同名成员
3.当子类与父类拥有同名的成员函数，子类会隐藏父类中同名成员函数，加作用域可以访问到父类中同名函数

*继承同名静态成员处理方式
问题：继承中同名的静态成员在子类对象上如何进行访问？
静态成员和非静态成员出现同名，处理方式一致
1.访问子类同名成员 直接访问即可
2.访问父类同名成员 需要加作用域

* 总结：同名静态成员处理方式和非静态处理方式一样，只不过有两种访问的方式（通过对象 和 通过类名）
* 多继承语法
  1.C++允许一个类继承多个类

2.语法： class 子类 ：继承方式 父类1 ， 继承方式 父类2...

3.多继承可能会引发父类中有同名成员出现，需要加作用域区分

4.C++实际开发中不建议用多继承

```cpp
class Base1 {
public:
	Base1()
	{
		m_A = 100;
	}
public:
	int m_A;
};

class Base2 {
public:
	Base2()
	{
		m_A = 200;  //开始是m_B 不会出问题，但是改为mA就会出现不明确
	}
public:
	int m_A;
};

//语法：class 子类：继承方式 父类1 ，继承方式 父类2 
class Son : public Base2, public Base1 
{
public:
	Son()
	{
		m_C = 300;
		m_D = 400;
	}
public:
	int m_C;
	int m_D;
};


//多继承容易产生成员同名的情况
//通过使用类名作用域可以区分调用哪一个基类的成员
void test01()
{
	Son s;
	cout << "sizeof Son = " << sizeof(s) << endl;
	cout << s.Base1::m_A << endl;
	cout << s.Base2::m_A << endl;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

* 总结： 多继承中如果父类中出现了同名情况，子类使用时候要加作用域
* 菱形继承
  菱形继承概念：

 1. 两个派生类继承同一个基类

 2. 又有某个类同时继承者两个派生类

 3. 这种继承被称为菱形继承，或者钻石继承

*菱形继承问题：

羊继承了动物的数据，驼同样继承了动物的数据，当草泥马使用数据时，就会产生二义性。
草泥马继承自动物的数据继承了两份，其实我们应该清楚，这份数据我们只需要一份就可以。

* 总结：

1.菱形继承带来的主要问题是子类继承两份相同的数据，导致资源浪费以及毫无意义
2.利用虚继承可以解决菱形继承问题

#### (14)多态

* 多态分为两类

1.静态多态: 函数重载 和 运算符重载属于静态多态，复用函数名
2.动态多态: 派生类和虚函数实现运行时多态

* 静态多态和动态多态区别：

1.静态多态的函数地址早绑定 - 编译阶段确定函数地址
2.动态多态的函数地址晚绑定 - 运行阶段确定函数地址

```cpp
class Animal
{
public:
	//Speak函数就是虚函数
	//函数前面加上virtual关键字，变成虚函数，那么编译器在编译的时候就不能确定函数调用了。
	virtual void speak()
	{
		cout << "动物在说话" << endl;
	}
};

class Cat :public Animal
{
public:
	void speak()
	{
		cout << "小猫在说话" << endl;
	}
};

class Dog :public Animal
{
public:

	void speak()
	{
		cout << "小狗在说话" << endl;
	}

};
//我们希望传入什么对象，那么就调用什么对象的函数
//如果函数地址在编译阶段就能确定，那么静态联编
//如果函数地址在运行阶段才能确定，就是动态联编

void DoSpeak(Animal & animal)
{
	animal.speak();
}
//
//多态满足条件： 
//1、有继承关系
//2、子类重写父类中的虚函数
//多态使用：
//父类指针或引用指向子类对象

void test01()
{
	Cat cat;
	DoSpeak(cat);


	Dog dog;
	DoSpeak(dog);
}
```

1、什么是虚函数
C++ 对象有三大特性：继承、封装、多态；虚函数就是实现多态的一种方式。

**虚函数是指加了 virtual 修饰词的类的成员函数**，但 virtual 关键字并非强制必须要有的。

对于某些函数，当基类希望**派生类**重新定义合适自己的版本时，基类就把这些函数声明为虚函数。

注意：virtual 关键字只能出现在类内部的函数声明中，不能用于类外部的函数定义。

* 基类初次声明‌	✅ 必须	定义函数为虚函数
* 派生类重写时‌	❌ 可选	建议用override替代virtual
* 存在通过基类指针/引用操作派生类对象的情况（即使当前只有一个派生类），必须声明为virtual ``Base* obj = new Derived();  // 基类指针指向派生类``

```cpp
class Animal {
	virtual void makeSound() {
		std::cout << "Animal makes a sound." << std::endl;
	}
};

class Dog :public Animal {
	void makeSound() override {
		std::cout << "Dog makes a sound." << std::endl;
	}
};
```

* 构造/析构函数可以是虚函数吗？
  1.构造函数不能是虚函数，析构函数可以是虚函数且最好设置为虚函数

2.构造函数不可以是虚函数
3.构造函数是在创建对象时执行的，而虚函数是程序运行时执行的；也就是说在创建对象时虚函数还没确定用那个版本呢，所以构造函数不可以是虚函数。

4.析构函数可以是虚函数且最好写成虚函数
5.如果析构函数不是虚函数，则容易造成内存泄露。原因为：
若有父类指针指向子类对象存在，需要析构的是子类对象；但父类析构函数不是虚函数，则只析构了父类，造成子类对象没有及时释放，引起内存泄漏。

* 基类析构函数不声明为虚函数造成内存泄露的原因？
  1.在C++中，将基类析构函数声明为虚函数能正确析构子类对象，其底层机制依赖于‌动态绑定‌和‌虚函数表（vtable）‌ 的实现原理：
  🔧 核心机制解析
  ‌虚函数表（vtable）的作用‌
  当类声明虚函数时（包括虚析构函数），编译器会为其生成**虚函数表（vtable）**。表中存储该类的虚函数地址（包括析构函数地址）。
  ‌派生类继承基类的vtable‌，并替换其中与自身类型匹配的函数地址（如重写基类虚析构函数时，派生类析构函数地址会覆盖基类析构函数地址）
  ‌动态绑定机制‌
  通过基类指针删除对象时，实际调用哪个析构函数由‌对象实际类型‌决定：

```cpp

Base* obj = new Derived();  // 基类指针指向子类对象
delete obj;
```

// 关键：根据obj实际指向的Derived对象类型查找vtable

* ‌若基类析构为虚函数‌：
  delete obj 会查询 Derived 对象的 vtable，找到 Derived::~Derived() 地址并调用它，再‌自动调用基类析构函数‌，完整销毁对象
* ‌若基类析构非虚‌：
  仅静态绑定 Base::~Base()，跳过 Derived::~Derived() 的调用，导致派生类资源泄漏
* 多态的优点：
  1.代码组织结构清晰
  2.可读性强
  3.利于前期和后期的扩展以及维护
* 纯虚函数和抽象类
  1.在多态中，通常父类中虚函数的实现是毫无意义的，主要都是调用子类重写的内容

2.因此可以将虚函数改为纯虚函数

3.纯虚函数语法：virtual 返回值类型 函数名 （参数列表）= 0 ;``virtual void func() = 0;``

4.当类中有了纯虚函数，这个类也称为抽象类

* 抽象类特点：

1.无法实例化对象
2.子类必须重写抽象类中的纯虚函数，否则也属于抽象类

```cpp
class Base
{
public:
	//纯虚函数
	//类中只要有一个纯虚函数就称为抽象类
	//抽象类无法实例化对象
	//子类必须重写父类中的纯虚函数，否则也属于抽象类
	virtual void func() = 0;
};

class Son :public Base
{
public:
	virtual void func() 
	{
		cout << "func调用" << endl;
	};
};

void test01()
{
	Base * base = NULL;
	//base = new Base; // 错误，抽象类无法实例化对象
	base = new Son;
	base->func();
	delete base;//记得销毁
}

```

* 关于上面代码的一点描述
  0.‌不声明为virtual时的行为‌
  1.派生类即使不显式声明virtual，函数仍为虚函数（因继承基类属性），以下调用完全合法
  2.显式表明多态意图‌,虽然基类virtual属性会自动继承到派生类，但显式声明virtual能明确表达这是对基类虚函数的重写，增强代码可读性。
  3.C++11标准后的override标记兼容性‌
  若使用override关键字（如void func() override {...}），则必须保留基类的virtual特性，显式或隐式均可。
  4.代码规范一致性‌
  某些编码规范要求派生类重写时显式保留virtual，以保持风格统一。
* 虚析构和纯虚析构
  多态使用时，如果子类中有属性开辟到堆区，那么父类指针在释放时无法调用到子类的析构代码

**解决方式：将父类中的析构函数改为虚析构或者纯虚析构**

* 虚析构和纯虚析构共性：

1.可以解决父类指针释放子类对象
2.都需要有具体的函数实现
3.虚析构和纯虚析构区别：

如果是纯虚析构，该类属于抽象类，无法实例化对象
虚析构语法：
``virtual ~类名(){}``
纯虚析构语法：
``virtual ~类名() = 0;``
类名::~类名(){}

```cpp
class Animal {
public:

	Animal()
	{
		cout << "Animal 构造函数调用！" << endl;
	}
	virtual void Speak() = 0;

	//析构函数加上virtual关键字，变成虚析构函数
	//virtual ~Animal()
	//{
	//	cout << "Animal虚析构函数调用！" << endl;
	//}


	virtual ~Animal() = 0;
};

Animal::~Animal()
{
	cout << "Animal 纯虚析构函数调用！" << endl;
}

//和包含普通纯虚函数的类一样，包含了纯虚析构函数的类也是一个抽象类。不能够被实例化。

class Cat : public Animal {
public:
	Cat(string name)
	{
		cout << "Cat构造函数调用！" << endl;
		m_Name = new string(name);
	}
	virtual void Speak()
	{
		cout << *m_Name <<  "小猫在说话!" << endl;
	}
	~Cat()
	{
		cout << "Cat析构函数调用!" << endl;
		if (this->m_Name != NULL) {
			delete m_Name;
			m_Name = NULL;
		}
	}

public:
	string *m_Name;
};

void test01()
{
	Animal *animal = new Cat("Tom");
	animal->Speak();

	//通过父类指针去释放，会导致子类对象可能清理不干净，造成内存泄漏
	//怎么解决？给基类增加一个虚析构函数
	//虚析构函数就是用来解决通过父类指针释放子类对象
	delete animal;
}

int main() {

	test01();

	system("pause");

	return 0;
}
```

* 总结：

 1. 虚析构或纯虚析构就是用来解决通过父类指针释放子类对象

 2. 如果子类中没有堆区数据，可以不写为虚析构或纯虚析构

 3. 拥有纯虚析构函数的类也属于抽象类
