# class 补充

## 类的实例化

类在实例化前，是不占空间的
必须先实例化，才能访问类对象的内部成员

## 类对象大小

计算类大小的时候，**不计算成员函数的大小**，因为函数是放在函数表里的，所以在类里面是通过地址找函数的，所以一个类大小，**只计算成员变量**.

如果这个类是**空**(没有成员变量)的，没有成员，那就是**1个字节**,不存数据，就标识有这个对象存在过

计算类大小的时候可以参考结构体的内存对齐规则

## 类的默认函数

如果一个类，什么成员都没有，那么就称为**空类**，但事实上，并不是完全没有成员，**类默认会有6个成员函数**，构造函数、析构函数、拷贝构造、赋值重载、取地址重载（主要是普通对象和const对象取地址，很少会自己实现）

## 无参构造函数与全缺省构造函数

全缺省构造函数：
构造函数的所有参数**都有默认值**，形式如 ClassName(Type1 param1 = value1, Type2 param2 = value2)。它可以不传参数调用，也可以通过**传递部分或全部参数**调用。

## 拷贝构造函数

拷贝构造函数是构造函数的一个重载形式,**一般得加const**，以免拷贝错了
`NN(const NN&a){}`
拷贝构造函数的**参数只有1个**，且必须是**该类类型对象的引用**（指针也行，但c++规定是引用），因为如果用传值传参，这个时候，拷贝构造函数的形参是接受来自实参的值传递，又相当于调用了一次新的拷贝构造函数，如此，无穷尽也，就会出事，所以，编译器会直接报错。

## 运算符重载

重载操作符必须有一个类类型参数用于内置类型的运算符，其含义不能改变，例如：内置的整型+，不能改变其含义

作为类成员函数重载时，其形参看起来比操作数数目少1，因为成员函数第一个参数为隐形的this指针。

### 赋值运算符重载

格式：参数类型：const 类型名&，**传引用提高效率**

返回值类型：类型名&，返回引用提高效率，有返回值，是为了能够直接拿来赋值检测自己是否给自己赋值

返回*this：返回值

1.**赋值运算符**只能重载成**类的成员函数**，**不能重载成全局函数**，因为编译器会自动生产一个赋值运算符的重载在类里面（如果你不显示定义），这样全局和类里面会冲突，所以不能重载全局。

2.编译器默认生成的赋值运算符，跟拷贝构造很像，对内置类型的变量，直接采用逐字节值覆盖，自定义类型采用该类型的赋值运算符。

3.同样，针对涉及资源管理（在堆上开辟空间等行为），最好是自己写赋值运算符，不涉及的，可以让编译器自己生成。

### 实例

```c++
//date.h
#pragma once
#include<iostream>
using namespace std;
 
class NN
{
public:
	NN(int year=1, int month=1, int day=1);
	int getday(int year, int month1);
 
	NN& operator+= (int day);
	NN operator+ (int day);
	NN& operator=(const NN& a);
	NN& operator-= (int day);
	NN operator- (int day);
	NN& operator++();
	NN operator++(int);
	NN& operator--();
	NN operator--(int);
 
	bool operator==(const NN& a);
	bool operator>(const NN& a);
	bool operator<(const NN& a);
	bool operator>=(const NN& a);
	bool operator<=(const NN& a);
	bool operator!=(const NN& a);
	int operator-(const NN& a);
	void print();
    friend ostream& operator<<(ostream& out, const NN& d);//友元是为了让外面的函数可以调用类私有的成员
	friend istream& operator>>(istream& in,  NN& d);
private:
	int _year;
	int _month;
	int _day;
};

ostream& operator<<(ostream& out, const NN& d);
istream& operator>>(istream& in, NN& d);
```

```c++
date.cpp
#include"date.h"
 
NN::NN(int year, int month, int day)
{
	_year = year;
	_month = month;
	_day = day; 
	if (_year < 1 || _month>12 || _day < 1 || _day>getday(_year, _month))
	{
		this->print();
		cout << "日期非法" << endl;
	}
}
 
int NN::getday(int year, int month1)
{
	int month[13] = { 0,31,28,31,30,31,30,31,31,30,31,30,31 };
	if (month1 == 2 && (year % 4 == 0 && year % 100 != 0 || year % 400 == 0))
	{
		return 29;
	}
	return month[month1];
}
NN& NN::operator+= (int day)
{
	if (day < 0)
	{
		return *this -= (-day);
	}
	_day += day;
	while (_day > getday(_year, _month))
	{
		_day -= getday(_year, _month);
		_month++;
		if (_month > 12)
		{
			_year++;
			_month = 1;
		}
	}
	//* this = *this + day;
	return *this;
}
NN NN::operator+ (int day)
{
	NN T(*this);
	T += day;
	//T._day += day;
	//while (T._day > getday(T._year, T._month))
	//{
	//	T._day -= getday(T._year, T._month);
	//	T._month++;
	//	if (T._month > 12)
	//	{
	//		T._year++;
	//		T._month = 1;
	//	}
	//}
	return T;
}
NN& NN::operator=(const NN& a)
{
	if (this != &a)
	{
		_year = a._year;
		_month = a._month;
		_day = a._day;
	}
	return *this;
}
 
NN& NN::operator++()//前置++,当前类的day++,返回this指针
{
	*this += 1;
	return *this;
}
NN NN::operator++(int)//为了区分前置和后置，c++规定，后置的里面参数要加个int，不用我们传，编译器自己传
{
	NN tmp(*this);
	*this += 1;
	return tmp;
}
 
bool NN::operator==(const NN& a)
{
	if (_year == a._year && _month == a._month && _day == a._day)
	{
		return true;
	}
	else
	{
		return false;
	}
}
bool NN::operator>(const NN& a)
{
	if (_year > a._year)
	{
		return true;
	}
	else if (_year == a._year && _month > a._month)
	{
		return true;
	}
	else if (_year == a._year && _month == a._month && _day > a._day)
	{
		return true;
	}
	else return false;
}
void NN::print()
{
	cout << _year << '/' << _month << '/' << _day;
}
 
bool NN::operator>=(const NN& a)
{
	return *this > a || *this == a;
}
bool NN::operator<(const NN& a)
{
	return !(*this >= a);
}
 
bool NN::operator<=(const NN& a)
{
	return !(*this > a);
}
bool NN::operator!=(const NN& a)
{
	return !(*this == a);
}
 
 
NN& NN::operator-= (int day)
{
	if (day < 0)
	{
		return *this += (-day);
	}
	_day -= day;
	while (_day <= 0)
	{
		--_month;
		if (_month == 0)
		{
			--_year;
			_month = 12;
		}
		_day += getday(_year, _month);
	}
	return *this;
}
NN NN::operator- (int day)
{
	NN a(*this);
	a -= day;
	return a;
}
 
int NN::operator-(const NN& a)
{
	int flag = 1;
	NN max(*this);
	NN min(a);
	if (*this < a)
	{
		max = a;
		min = *this;
		flag = -1;
	}
	int d = 0;
	while (min != max)
	{
		++min;
		d++;
	}
	return d * flag;
 
}
 
NN& NN::operator--()
{
	*this -= 1;
	return *this;
}
NN NN::operator--(int)
{
	NN tmp(*this);
	*this -= 1;
	return tmp;
}
ostream& operator<<(ostream& out, const NN & d)
{
	//这是流提取的运算符重载
	//注意，我们不采用成员函数的形式重载运算符，而是全局
	//因为，双目操作符的规定，第一个参数一定是左操作数，第二个参数是右操作数
	//如果我们用成员函数的形式重载运算符，则会出现d1<<cout这样别扭的形式
	//为了遵从习惯，我们采用全局的方式，
	//这样才能出现cout<<d1这样的形式。
	// 注意，cout是个函数，需要被传参数，实现效果，不能
	//加const，但要输出的类对象，我们可以用const修饰
	//为了实现连续操作比如cout<<d1<<d2,我们要给函数一个返回值，返回cout，从而
	//让cout继续做新一轮的左操作数，<<这个操作符的结合性是从左边开始的，赋值
	//操作符的结合性是从右开始。out是cout的引用，返回out的引用，本质还是返回cout的引用，不影响
	out << d._year << d._month << d._day << endl;
	return out;
}
 
istream& operator>>(istream& in, NN& d)
{
	//跟<<差不多，区别是cin是在istream类里，且这次是流插入，也就是把东西
	//插入类对象里面，所以类对象形参不能加const。
	in >> d._year >> d._month >> d._day;
	return in;
} 
```

### >>和<<重载

我们打印自定义类型不能直接用cout，为什么（我们之前知道cout和cin是自动识别类型，这是因为c++自己写了内置类型的函数重载，从而识别）cout是自ostream类里面的，cin是在istream类里面的。如果我们要用cout和cin输入输出指定的自定义类型，我们要运算符重载。

### const

我们定义成员函数的时候，对于不会修改内容的函数，尽量都加入const修饰，会修改内容的成员函数，不要加const修饰。

### 取地址和const取地址重载

注意，const修饰的函数，如果有重名，本身也构成函数重载，因为接受的参数类型不一样

```c++
	NN* operator&()
	{
		return this;
		
	}
	const NN* operator&()const
	{
		return this;
	}
```

