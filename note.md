# C++基础知识

## c和c++的区别

**C面向过程，C++面向对象**

- 面向过程语言：性能比面向对象高，因为类调用时需要实例化，比较消耗资源，但是没有面向对象易维护，易复用，易扩展。
- 面向对象语言：易维护，易复用，易扩展，由于具有面向对象的特性，可以设计出低耦合的系统使得系统更加灵活。但是性能低。
- 我对面向对象的理解：**面向对象的编程方式使得每一个类都只做一件事**。**面向过程会让一个类越来越全能，就像一个管家一样做了所有的事**。而面向对象像是雇佣了一群职员，每个人做一件小事，各司其职，最终合作共赢！
  - 对大话设计模式种的一个故事印象很深，曹操写诗句喝酒唱歌，人生很爽。交给工匠，刻一个雕版印刷。诗句印刷好了之后，曹丞相觉得不满意，改成了对酒当歌，人生很爽，工匠又只能重新刻板硬刷。当工匠把样张再给曹操看的时候，曹操又改成了对酒当歌，人生几何，工匠哭晕在厕所。
  - 二活字印刷就像是面向对象编程，要改只要改一个子，要用可以重复使用，要扩展新刻一个字就可以了。

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

## 指针引用

#### 为什么使用指针

- 能够动态分配内存，实现内存自由管理
- 方便使用数组和字符串
- 值传递不如指针传递高效

#### 指针和引用的区别

- 指针是地址引用是别名
- 指针本身也是一个对象占有一块内存；引用不占有内存，指向的对象可以更改，引用初始化时必须指向一个已经存在的对象

## const&static

#### const和define区别

- define定义的常量没有任何类型，不做类型检查；const常量具有类型，在编译阶段会做类型检查

- define有多少地方用就展开多少次不分配内存；const分配内存
- define由预处理器处理，定义的常量不会记录在记号表中，如果在其他编译单元定义的常量编译报错很难找到错误位置

#### const 作用

- 修饰变量，说明该变量不可以被改变；
- 修饰指针，分为指向常量的指针（pointer to const）和自身是常量的指针（常量指针，const pointer）；
- 修饰引用，指向常量的引用（reference to const），用于形参类型，即避免了拷贝，又避免了函数对值的修改；
- 修饰成员函数，说明该成员函数内不能修改成员变量。

#### const成员函数和static成员函数的区别

- const成员函数：由于传入函数内部的this指针是一个const，所以在函数内部不能通过this指针访问类的非静态成员变量，当然如果硬要访问也可以，在类成员变量前加mutable修饰就行了。
- static成员函数：对于这个类和他的所有对象来说只有一个static成员函数实体。由于static成员函数不在内部传入this指针，所以它也无法访问类的非静态成员变量。另外，static成员函数可以直接由类名或者对象名调用。

#### const与指针

- 常量指针：const修饰的是指针，指针本身是一个常量地址不可以被改变，但是指针所指向的对象是可以改变的。
- 指向常量的指针：const修饰的是指针指向的对象，对象不可以被改变，但是指针可以改变指向的对象。
- 指向常量的常量指针：不论是指针本身还是指向的对象都不可以被改变。

#### static的作用

- 对于全局变量：改变全局变量的作用域，使其他的编译单元不可见
- 对于局部变量：改变他的存储位置（由栈到静态存储区）和生命周期（函数结束到程序结束）
- 成员变量：成为类的全局变量，需要在类外初始化，只有唯一的实例，被所有的类对象共享。
- 成员函数：成为类的静态成员函数被所有类对象共享，可以直接用类名调用，没有隐含this指针

## this指针

- `this` 指针是一个隐含于每一个非静态成员函数中的特殊指针。它指向调用该成员函数的那个对象。
- 当对一个对象调用成员函数时，编译程序先将对象的地址赋给 `this` 指针，然后调用成员函数，每次成员函数存取数据成员时，都隐式使用 `this` 指针。
- 当一个成员函数被调用时，自动向它传递一个隐含的参数，该参数是一个指向这个成员函数所在的对象的指针。
- `this` 指针被隐含地声明为: `ClassName *const this`，这意味着不能给 `this` 指针赋值；在 `ClassName` 类的 `const` 成员函数中，`this` 指针的类型为：`const ClassName* const`，这说明不能对 `this` 指针所指向的这种对象是不可修改的（即不能对这种对象的数据成员进行赋值操作）；
- `this` 并不是一个常规变量，而是个右值，所以不能取得 `this` 的地址（不能 `&this`）。
- 在以下场景中，经常需要显式引用指针：
  1. 为实现对象的链式引用；
  2. 为避免对同一对象进行赋值操作；
  3. 在实现一些数据结构时，如 `list`。

## inline 内联函数

#### 特征

- 相当于把内联函数里面的内容写在调用内联函数处；
- 相当于不用执行进入函数的步骤，直接执行函数体；
- 相当于宏，却比宏多了类型检查，真正具有函数特性；
- 编译器一般不内联包含循环、递归、switch 等复杂操作的内联函数；
- 在类声明中定义的函数，除了虚函数的其他函数都会自动隐式地当成内联函数。

#### 使用

inline 使用

```c++
// 声明1（加 inline，建议使用）
inline int functionName(int first, int second,...);

// 声明2（不加 inline）
int functionName(int first, int second,...);

// 定义
inline int functionName(int first, int second,...) {/****/};

// 类内定义，隐式内联
class A {
    int doA() { return 0; }         // 隐式内联
}

// 类外定义，需要显式内联
class A {
    int doA();
}
inline int A::doA() { return 0; }   // 需要显式内联

```

#### 编译器对 inline 函数的处理步骤

1. 将 inline 函数体复制到 inline 函数调用点处；
2. 为所用 inline 函数中的局部变量分配内存空间；
3. 将 inline 函数的的输入参数和返回值映射到调用方法的局部变量空间中；
4. 如果 inline 函数有多个返回点，将其转变为 inline 函数代码块末尾的分支（使用 GOTO）。

#### inline优缺点

优点

1. 内联函数同宏函数一样将在被调用处进行代码展开，省去了参数压栈、栈帧开辟与回收，结果返回等，从而提高程序运行速度。
2. 内联函数相比宏函数来说，在代码展开时，会做安全检查或自动类型转换（同普通函数），而宏定义则不会。
3. 在类中声明同时定义的成员函数，自动转化为内联函数，因此内联函数可以访问类的成员变量，宏定义则不能。
4. 内联函数在运行时可调试，而宏定义不可以。

缺点

1. 代码膨胀。内联是以代码膨胀（复制）为代价，消除函数调用带来的开销。如果执行函数体内代码的时间，相比于函数调用的开销较大，那么效率的收获会很少。另一方面，每一处内联函数的调用都要复制代码，将使程序的总代码量增大，消耗更多的内存空间。
2. inline 函数无法随着函数库升级而升级。inline函数的改变需要重新编译，不像 non-inline 可以直接链接。
3. 是否内联，程序员不可控。内联函数只是对编译器的建议，是否对函数内联，决定权在于编译器。

## assert()

断言，是宏，而非函数。assert 宏的原型定义在 `<assert.h>`（C）、`<cassert>`（C++）中，其作用是如果它的条件返回错误，则终止程序执行。可以通过定义 `NDEBUG` 来关闭 assert，但是需要在源代码的开头，`include <assert.h>` 之前。

assert() 使用

```c++
#define NDEBUG          // 加上这行，则 assert 不可用
#include <assert.h>

assert( p != NULL );    // assert 不可用
```

## C++ 中 struct 和 class

总的来说，struct 更适合看成是一个数据结构的实现体，class 更适合看成是一个对象的实现体。

#### 区别

- 最本质的一个区别就是默认的访问控制
  1. 默认的继承访问权限。struct 是 public 的，class 是 private 的。
  2. struct 作为数据结构的实现体，它默认的数据访问控制是 public 的，而 class 作为对象的实现体，它默认的成员变量访问控制是 private 的。

## 什么是右值引用，跟左值又有什么区别？

- 左值和右值的概念
  - 左值：能对表达式取地址、或具名对象/变量。一般指表达式结束后依然存在的持久对象。
  - 右值：不能对表达式取地址，或匿名对象。一般指表达式结束就不再存在的临时对象。
    - C++11:右值是由两个概念构成，将亡值和纯右值。纯右值是用于识别临时变量和一些不跟对象关联的值，比如1+3产生的临时变量值，2、true等，而将亡值通常是指具有转移语义的对象，比如返回右值引用T&&的函数返回值等

- 它实现了转移语义和精确传递。它的主要目的有两个方面：
  - 消除两个对象交互时不必要的对象拷贝，节省运算存储资源，提高效率
  - 能够更简洁明确地定义泛型函数
- 总结一个类型T
  - 左值引用， 使用 `T&`, 只能绑定**左值**
  - 右值引用， 使用 `T&&`， 只能绑定**右值**
  - 常量左值， 使用 `const T&`, 既可以绑定**左值**又可以绑定**右值**
  - 已命名的**右值引用**，编译器会认为是个**左值**
  - `编译器有返回值优化`，但不要过于依赖

## extern的作用

- extern c ：告诉编译器在编译的时候按着c的规则去编译
- extern ：所声明的函数和变量可以在本模块和其他模块一起使用

### 野指针

- 访问一个已销毁或者访问受限的内存区域的指针

- 产生的原因
  - 指针定义时未被初始化
  - 指针被释放时没有置空
  - 指针操作超越变量作用域：不要返回指向栈内存的指针或者引用，因为栈内存在函数结束的时候会被释放

- 

### 函数指针

- 定义：函数指针是指向函数的指针变量，c在编译的时候每一个函数都会有一个入口地址，函数指针就是指向这个地址。
- 用途：调用函数和做函数的参数，比如回调函数

## 重载和覆盖

- 重载：两个函数名相同，但是参数列表不同的函数，在同一个作用域中
- 覆盖：子类继承父类，重写父类中的虚函数

## strcpy和strlen

- strlen计算字符串长度，返回长度
- sltcpy逐个拷贝字符，因为没有指定长度所以会导致拷贝越界，安全版本为strncpy

## volatile关键字

- 指出 变量是随时可能发生变化的，每次使用它的时候必须从 i的地址中读取，因而编译器生成的汇编代码会重新从i的地址读取数据放在 b 中

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
  - `Random Access Iterator`：前四种迭代器都只提供一部分指针算术能力（前三支持`opertor++`，第四还支持`operatr--`），第五种则涵盖所有指针算术能力（p+n,p-n,p[n],p1-p2,p1<p2）

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

# C++ 智能指针

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

# C++ 强制转换

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

# C++ 面向对象

面向对象程序设计（Object-Oriented Programming, OOP）是种具有对象概念的程序编程典范，同时也是一种程序开发的抽象方针。

![image-20200816092406117](C:\Users\free\AppData\Roaming\Typora\typora-user-images\image-20200816092406117.png)

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
    - 虚函数和继承
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
- 过程
  - 通过类的继承和虚函数机制在运行期间判断选择调用哪一个子类的对应函数（动态联编）举个例子：基类指针指向一个子类对象的时候，当父类调用的子类函数是虚函数的时候，就会调用子类重写过后的虚函数。

### 虚析构函数

- 析构函数可以是虚函数，是为了解决基类的指针指向派生类对象，并用基类的指针删除派生类对象（删除基类指针时会调用派生类析构函数，防止基类指针无法释放派生类内存资源）
- 用来做基类的类的析构函数一般都是虚析构函

## 构造函数是否可以为虚函数

不可以，因为虚函数的调用需要调用vptr，而vptr是存放在对象内存中的，如果构造函数可以是虚函数那么构造函数的vptr是没有地方存放的。

## 虚函数，纯虚函数

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

## 虚函数指针、虚函数表

- 虚函数指针（Virtual Function Pointer, `vfptr`）：在含有虚函数类的对象中，指向虚函数表，在运行时确定
- 虚函数表（Virtual Function Table）：在程序只读数据段（`.rodata section`）即内存常量区存放虚函数指针，如果派生类实现了基类的某个虚函数，则在虚表中**覆盖原本基类的那个虚函数指针**，在编译时根据类的声明创建

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



## 虚继承

- 虚继承用于解决多继承条件下的**菱形继承问题**（浪费存储空间、存在二义性）
- 底层实现原理与编译器相关，一般通过**虚基类指针**和**虚基类表**实现，每个虚继承的子类都有一个虚基类指针（占用一个指针的存储空间，4字节）和虚基类表（不占用类对象的存储空间）（需要强调的是，虚基类依旧会在子类里面存在拷贝，只是仅仅最多存在一份而已，并不是不在子类里面了）；当虚继承的子类被当做父类继承时，虚基类指针也会被继承
- 实际上，`vbptr` 指的是虚基类表指针（virtual base table pointer），该指针指向了一个虚基类表（virtual table），虚表中记录了虚基类与本类的偏移地址；通过偏移地址，这样就找到了虚基类成员，而虚继承也不用像普通多继承那样维持着公共基类（虚基类）的两份同样的拷贝，节省了存储空间。

## 虚继承和虚函数区别

- 相同之处：都使用了虚指针（均占用类的存储空间）和虚表（均不占用类的存储空间）
- 不同之处：
  - 虚继承：
    - 虚基类依旧存在派生类中，只占用存储空间，只有一份。
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

# C++ 内存管理

## C++内存分配

- 计算机中的程序内存分布图如下图所示

  ![img](https://uploadfiles.nowcoder.com/images/20190313/311436_1552470920741_7D40CEF3951A10F626301148E06D89DA)

- 栈（Stack）：由编译器自动分配和释放，存放函数运行时分配的**局部变量（包括局部常量，不可修改性由编译器保证，可以`const_cast`）、函数参数值、返回数据、返回地址**等。操作类似于数据结构中的栈 
- 动态链接库：用于在程序运行期间加载和卸载动态链接库
- 堆（Heap）：一般**由程序员分配**，如果程序员没有释放，程序结束时可能由操作系统回收。`malloc(),calloc(),free()`操作的就是这块内存
- 全局数据区（global data）：存放**全局变量、静态变量（包括静态局部和静态全局）**等，这块内存有读写权限，因此他们的值在程序运行期间可以任意改变。程序结束之后由系统释放
- 常量区（文字常量区）：存放**一般的常量、常量字符串**，这块内存只有读权限，因此在运行期间不能改变。程序结束后由系统释放
- 代码区：存放函数体的二进制代码

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

## 内存管理

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

- malloc内部实现：将可用的内存块连接为一个空闲链表，调用malloc的时候就遍历这个空闲链表找到一个合适的内存块，将内存块一分为二，大小合适的那一块返回给用户，剩下的内存块再连进空闲链表；free的时候再把释放的内存块连入空闲链表，如果有太多的小的内存分块，那么malloc函数会请求延时并对空闲链表进行整理，将相邻的空闲块合并成大的空闲块

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

## malloc和new、free和delete区别

- 申请的位置不同：
  - malloc在堆上动态分配内存
  - new在自由存储区上为对象动态分配内存空间
- 类型不同
  - malloc是一个函数，只能分配内置类型，不能调用构造和析构
  - new是操作符，可以为类分配内存空间
- 类型检查
  - malloc不关心创建的对象类型，只负责开辟指定大小的内存然后把起始地址返回给调用者，调用者需要自己进行类型转化
  - new需要指定类型不需要指定大小，并会初始化。
- 能否重载
  - malloc不能重载
  - new可以

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

![图6-1  固定内存池](C:/Users/free/Desktop/面试/interviewmd/assets/post/others/memorypool.png)

在内存池初次生成时，只向系统申请了一个内存块，返回的指针作为整个内存池的头指针。之后随着应用程序对内存的不断需求，内存池判断需要动态扩大时，才再次向系统申请新的内存块，并把所**有这些内存块通过指针链接起来**。

当应用程序需要通过该内存池分配一个单元大小的内存时，**只需要简单遍历所有的内存池块头信息**，快速定位到还有空闲单元的那个内存池块。然后根据该块的块头信息直接定位到第1个空闲的单元地址，把这个地址返回，并且标记下一个空闲单元即可；当应用程序释放某一个内存池单元时，直接在对应的内存池块头信息中标记该内存单元为空闲单元即可。

相当于系统管理内存相比，内存池主要由以下优点：

- 针对特殊情况，例如需要频繁释放固定大小的内存对象时，不需要复杂的分配算法和多线程保护。也不需要维护内存空闲表的额外开销，从而获得较高的性能
- 由于开辟一定数量的连续内存空间作为内存吃块，因而从一定程度上提高了程序局部性，提升了系统性能
- 比较容易控制页边界对齐和内存字节对齐，没有内存碎片的问题

### 内存池实现

![图6-2  内存池的数据结构](C:/Users/free/Desktop/面试/interviewmd/assets/post/others/memorypoolstruct.png)

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

![image-20200818131431353](C:\Users\free\AppData\Roaming\Typora\typora-user-images\image-20200818131431353.png)




- 预处理阶段：处理以`#`开头的预处理命令，生成`.i/.ii`文件

- ```shell
  g++ -E hello.cpp -o hello.i
  ```

- 编译阶段：编译器进行词法分析、语法分析、语义分析、中间代码生成、优化、目标代码生成等，生成`.s`汇编文件

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

![image-20200816093642433](C:\Users\free\AppData\Roaming\Typora\typora-user-images\image-20200816093642433.png)

### 动态链接

- 静态链接有以下两个问题：
  - 当静态库更新时那么整个程序都要重新进行链接；
  - 对于`printf`这种标准函数库，如果每个程序都要有代码，这会极大浪费资源。
- 共享库是为了解决静态库的这两个问题而设计的，在 Linux 系统中通常用 .so 后缀来表示，Windows 系统上它们被称为 DLL。它具有以下特点：
  - 在给定的文件系统中一个库只有一个文件，所有引用该库的可执行目标文件都共享这个文件，它不会被复制到引用它的可执行文件中；
  - 在内存中，一个共享库的 .text 节（已编译程序的机器代码）的一个副本可以被不同的正在运行的进程共享。
  - ![image-20200816093702187](C:\Users\free\AppData\Roaming\Typora\typora-user-images\image-20200816093702187.png)

### 链接的接口-符号

- 在链接中，目标文件之间相互拼合实际上是目标文件之间对地址的引用，即对函数和变量的地址的引用。函数和变量统称为符号（Symbol），函数名或变量名就是符号名（Symbol Name）。

## CoreDump

Core Dump也叫做核心转储，当程序运行过程中发生异常，程序异常退出时，由操作系统把程序当前的内存状况存储到一个core文件中，叫做core dump。

使用`ulimit -c unlimited`可以开启核心转储功能。

使用`gdb [exe_path] [core_path]`打开core文件查看出错信息

## 函数的调用过程

使用到了三个常用的寄存器，`EIP`为指令指针，即指向下一条即将执行的指令的地址；`EBP`为基址地址，常用来指向栈底；`ESP`为栈顶指针，常用来指向栈顶。

- 函数调用
  - 将调用函数的参数压入帧栈中，call指令跳转到子函数起始地址
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

## c++11新标准线程库

- 以往windows：CreateThread（），linux：pthread_create（）；不能跨平台
- c++语言本身增加了多线程支持，可移植性（跨平台）；

  - 库 #include <thread>

    - **thread**
      - 标准库里的类；myprint：是一个可调用对象
      - thread mytobj(myprint);//创建了线程，从myprint开始执行
    - **join()**
      - join阻塞，阻塞主线程，让主线程等待子线程执行完毕，子线程执行完毕后主线程再开始运行。如果主线程执行完毕但是子线程没执行完毕，报异常。
    - **detach()分离**
      - 传统多线程主线程需要等待子线程，然后自己退出；detach:主线程和子线程分离，主线程不必等
      - 为什么引入detach：创建很多子线程，让主线程逐个等待子线程结束，不太好，引入了detach
      - 一旦detach后，与这个主线程关联地thread对象就会失去与这个主线程的关联，此时这个子线程就会驻留在后台（守护进程）运行了，子线程相当于被系统接管了，子进程执行完毕后由运行时库负责清理该线程相关的资源。
    - **joinable()**
      - 判断是否已经join或者detach过

## 创建线程的不同手法

- 用类以及一个问题范例

  ```c++
  class TA {
  public:
  	int &m_i;
  	TA(int &i) :m_i(i) {
  		cout << "ta的构造函数" << endl;
  	}
  
  	TA(const TA&ta) :m_i(ta.m_i)
  	{
  		cout << "ta的拷贝构造函数" << endl;
  	}
  	~TA()
  	{
  		cout << "ta的析构函数" << endl;
  	}
  	void operator()()
  	{
  // 		cout << "我的线程开始执行了" << endl;
  // 
  // 
  // 		cout << "我得线程执行完毕了" << endl;
  		cout << "m_i的值为" << m_i << endl;
  		cout << "m_i的值为" << m_i << endl;
  		cout << "m_i的值为" << m_i << endl;
  		cout << "m_i的值为" << m_i << endl;
  		cout << "m_i的值为" << m_i << endl;
  		cout << "m_i的值为" << m_i << endl;
  		cout << "m_i的值为" << m_i << endl;
  
  	}
  };
  
  int main()
  {
  		int myi = 7;
  		TA ta(myi);//传入的是引用，所以一旦主线程执行完毕之后回收内存，那么分离执行的子线程中就失去了myi的对象
  		thread mytobj2(ta);	
  	
  		mytobj2.join();
  		//mytobj2.detach();
  		cout << "i love china" << endl;
      	return 0;
  }
  ```

- 用类成员函数

  ```c++
  class A
  {
  public:
  	int m_i;
  	//类型转换构造函数，可以直接传入一个整形构造
  	A(int a) :m_i(a) {
  		cout << "[A::A(int a)构造函数执行]" <<this<<"thread id="<<std::this_thread::get_id()<< endl;
  	}
  	A(const A & a) :m_i(a.m_i) {
  		cout << "[A::A(cosnt int a)构造函数执行]" <<this << "thread id=" << std::this_thread::get_id() << endl;
  	}
  	~A() {
  		cout << "[A::~A()析构函数执行]" <<this << "thread id=" << std::this_thread::get_id() << endl;
  	}
  
  	void thread_work(int num)
  	{
  		cout<<"子线程work函数执行"<<this<< "thread id=" << std::this_thread::get_id() << endl;
  	}
  };
  
  int main()
  {
  	A myobj(1);
  	std::thread mytobj(&A::thread_work,myobj,16);//&成员函数函数名，this指针，变量
  	mytobj.join();
  	cout << "i love china" << endl;
      return 0;
  }
  ```

  

- 用lambda表达式

  ```c++
  int main()
  {
  	auto mylamthread = [] {
  		cout << "我的线程3开始了" << endl;
  		cout << "我的线程3结束了" << endl;
  	};
  	thread mytobj4(mylamthread);
  	mytobj4.join();
  
  	
  	cout << "i love china" << endl;
  
  	cout << "i love china" << endl;
  	cout << "i love china" << endl;
  	cout << "i love china" << endl;
  	cout << "i love china" << endl;
      return 0;
  }
  
  ```

- 传递临时对象作为线程参数

  - 不用引用和指针传递，字符串指针会出现野指针，所以我们想将char[]隐式转换成string引用，但隐式类型转换对象在子线程中构造，会导致主线程中的对象已经被析构了子线程中的构造函数还没运行。
    - 事实：只要用临时构造的A类对象作为参数传递给线程，那么一定能够在主线程执行完毕前把线程参数构建出来从而确保即便detach了子线程也会安全运行。如果不传入临时对象，那么会出现
  - 总结
    - 若传递int这种简单的类型，直接值传递
    - 若传递类对象，避免隐式类型转换,全部都在创建线程的过程中就构建出临时对象来，然后在函数参数里用引用来接收，否则系统还会多构造一次对象。

- 线程id

  - 线程id概念，每一个线程都对应这一个线程id；获取线程号：std::this_thread::get_id()



```c++
class A
{
public:
	int m_i;
	//类型转换构造函数，可以直接传入一个整形构造
	A(int a) :m_i(a) {
		cout << "[A::A(int a)构造函数执行]" <<this<<"thread id="<<std::this_thread::get_id()<< endl;
	}
	A(const A & a) :m_i(a.m_i) {
		cout << "[A::A(cosnt int a)构造函数执行]" <<this << "thread id=" << std::this_thread::get_id() << endl;
	}
	~A() {
		cout << "[A::~A()析构函数执行]" <<this << "thread id=" << std::this_thread::get_id() << endl;
	}
};

void myprint(const int i, const A & pmybug)
{
	cout << "son thread parameter add ="<<&pmybug << endl;//打印pmybug地址
	cout << "son thread id =" << std::this_thread::get_id() << endl;
	return;
}

int main()
{
	int mvar = 1;
	int mysecondpar = 12;
	cout << "father thread id="<<std::this_thread::get_id() << endl;
	thread mytobj(myprint, mvar, A(mysecondpar));//将mysecondpar转成A类型对象传递给myprint
												//在创建线程的同时临时构造对象的方法传递参数是可行的
	mytobj.join();
	cout << "i love china" << endl;


    return 0;
}

```

- 向线程传递主线程中类对象，还可以修改主线程中的对象
  - 用std::ref(对象)

```c++
class A
{
public:
	int m_i;
	//类型转换构造函数，可以直接传入一个整形构造
	A(int a) :m_i(a) {
		cout << "[A::A(int a)构造函数执行]" <<this<<"thread id="<<std::this_thread::get_id()<< endl;
	}
	A(const A & a) :m_i(a.m_i) {
		cout << "[A::A(cosnt int a)构造函数执行]" <<this << "thread id=" << std::this_thread::get_id() << endl;
	}
	~A() {
		cout << "[A::~A()析构函数执行]" <<this << "thread id=" << std::this_thread::get_id() << endl;
	}
};

void myprint(const int i, A & pmybug)
{
	cout << "son thread parameter add ="<<&pmybug << endl;//打印pmybug地址
	cout << "son thread id =" << std::this_thread::get_id() << endl;
	pmybug.m_i = 199;
	return;
}

int main()
{
	int mvar = 1;
	int mysecondpar = 12;
	cout << "father thread id="<<std::this_thread::get_id() << endl;
	A myobj(10);
	thread mytobj(myprint, mvar, std::ref(myobj));//将mysecondpar转成A类型对象传递给myprint
												//在创建线程的同时临时构造对象的方法传递参数是可行的
	mytobj.join();
	cout << "i love china" << endl;


    return 0;
}
```

## 创建多个线程、数据共享问题

### 创建和等待多个线程

- 创建十个线程，十个线程开始执行

  - a.多个线程执行的顺序是乱的，跟操作系统内部对线程的运行调度机制有关
  - b.主线程阻塞等待所有子线程运行结束后再结束
  - c.用容器管理大量线程

  ```c++
  int main()
  {
  		for (int i = 0; i < 10; i++)
  		{
  			mythreads.push_back(thread(myprint, i));//创建并开始执行线程
  		}
  		for (auto iter = mythreads.begin(); iter != mythreads.end(); ++iter)
  		{
  			iter->join();
  		}
  		cout << "i love china" << endl;
      return 0;
  }
  ```

  

### 数据共享问题分析

- 只读数据是安全稳定的，不需要特别的处理手段

  ```c++
  vector<int>g_v = { 1,2,3 };//共享数据
  
  void myprint(int inum)
  {
  	cout << "id为" << std::this_thread::get_id() << "的线程 打印G_V值" << g_v[0] << g_v[1] << g_v[2] << endl;
  	return;
  }
  
  int main()
  {
  		for (int i = 0; i < 10; i++)
  		{
  			mythreads.push_back(thread(myprint, i));//创建并开始执行线程
  	
  		}
  	
  		for (auto iter = mythreads.begin(); iter != mythreads.end(); ++iter)
  		{
  			iter->join();
  		}
  	
  		cout << "i love china" << endl;
  
      return 0;
  }
  ```

- 有读有写的数据：2个线程写，8个线程读，如果代码没有特别的处理，那程序肯定崩溃

  - 最简单的不崩溃处理，读的时候不写，写的时候不读
  - 写的步骤分10小步，由于任务切换导致各种诡异事件发生

- 其他案例

  - 北京-深圳 火车 ，10个窗口卖票 ，其中两个窗口同时都要订99号座位

### 共享数据的保护代码案例

- 网络游戏服务器。两个自己创建的线程
  - 一个线程收集玩家命令（用一个数字代表玩家发来的命令），并把命令数据写到一个队列中。
  - 一个线程从队列中去除玩家发送的命令，解析然后执行玩家需要的动作
  - 准备用成员函数作为线程函数
  - 代码解决问题：引入一个c++解决多线程保存共享数据问题的第一个概念”互斥量“

```c++
class A {
public:
	//把收到的消息入到一个队列的线程
	void inMsgRecvQueue()
	{
		for (int i = 0; i < 100000; i++)
		{
			cout << "inMsgRecvQueue，插入一个元素" << i << endl;
			msgRevcQuere.push_back(i);//假设数字i就是收到的命令，插入消息队列

		}
	}

	//把数据从消息队列中取走并处理的线程
	void outMsgRecvQueue()
	{
		for (int i = 0; i < 100000; i++)
		{
			if (!msgRevcQuere.empty())
			{
				//消息不为空
				int command = msgRevcQuere.front();//返回第一个元素
				msgRevcQuere.pop_front();//移除第一个元素，但不返回
				//处理数据
				//。。。。
			}
			else {
				cout << "outMsgRecvQueue执行，但目前消息队列为空" << i << endl;
			}
		}
		cout << "end" << endl;
	}

private:
	std::list<int>msgRevcQuere;
};

int main()
{
    A myobj;//创建对象
	std::thread myOutMsgobj(&A::outMsgRecvQueue, &myobj);//成员函数创建线程
	std::thread myInMsgobj(&A::inMsgRecvQueue, &myobj);

	myOutMsgobj.join();//阻塞
	myInMsgobj.join();

		return 0;
}
```

### 互斥量mutex

- 互斥量基本概念

  - 互斥量就是一个类对象，理解成一把锁，多个线程尝试用lock（）成员函数来加锁这把锁头，只有一个线程可以锁定成功（成功的标志是lock（）函数返回了，如果没有成功，那么阻塞在lock（）不断尝试加锁）
  - 互斥量使用要小心，保护数据不多也不少

- 互斥量的用法

  -  lock(),unlock()
     - 操作步骤：先lock()操作共享数据，再unlock();
     - 两者需要成对使用
     - 由lock忘记unlock的问题，有时候很难排查，为了防止大家忘记unlock（），引入了一个叫std::lock_guard的类模板：你忘了unlock不要紧，我替你unlock();

  ```c++
  class A {
  public:
  	//把收到的消息入到一个队列的线程
  	void inMsgRecvQueue()
  	{
  		for (int i = 0; i < 100000; i++)
  		{
  			cout << "inMsgRecvQueue，插入一个元素" << i << endl;
  			my_mutex.lock();//锁住写操作
  			msgRevcQuere.push_back(i);//假设数字i就是收到的命令，插入消息队列
  			my_mutex.unlock();
  		}
  	}
  
  	bool outMsgLULProc(int &command)//将读数据和删除数据提取到一个函数中去
  	{
  		my_mutex.lock();//lock
  		if (!msgRevcQuere.empty())
  		{
  			//消息不为空
  			command = msgRevcQuere.front();//返回第一个元素
  			msgRevcQuere.pop_front();//移除第一个元素，但不返回
  			my_mutex.unlock();							 //处理数据
  			return true;						 //。。。。
  		}
  		my_mutex.unlock();//每一个分支都需要unlock
  		return false;
  	}
  
  	//把数据从消息队列中去除的线程
  	void outMsgRecvQueue()
  	{
  		int command = 0;
  		for (int i = 0; i < 100000; i++)
  		{
  			bool result = outMsgLULProc(command);
  			if (result == true)
  			{
  				cout << "outMsgRecvQueue()执行，取出一个元素" << command << endl;
  				//可以考虑可疑进行命令处理
  			}
  			else {
  				cout << "outMsgRecvQueue执行，但目前消息队列为空" << i << endl;
  			}
  		}
  		cout << "end" << endl;
  	}
  
  private:
  	std::list<int>msgRevcQuere;
  	std::mutex my_mutex;
  };
  ```

  - std::lock_guard类模板：直接取代lock()和unlcok();也就是说，用了lock_guard后，可以不用unlock了

    - lock_guard再构造函数的时候调用lock(),在生命周期结束后析构函数调用unlock();

    ```c++
    bool outMsgLULProc(int &command)
    	{
    		std::lock_guard<std::mutex> sbguard(my_mutex);//lock_guard构造函数里执行了mutex::lock(),由于lock_gurad是一个局部对象，所以在函数结束的时候执行析构函数的时候调用mutex::unlock()
    		if (!msgRevcQuere.empty())
    		{
    			command = msgRevcQuere.front();//返回第一个元素
    			msgRevcQuere.pop_front();//移除第一个元素，但不返回
    			return true;						 //。。。。
    		}
    		return false;
    	}
    ```

### 死锁

- 概念：有两把互斥锁（死锁产生的前提条件，至少两个互斥量）：金锁（jinlock）银锁（yinlock）

  - //两个线程A，B	
  - 1.线程A执行的时候，这个线程先锁金锁，成功，然后去lock银锁。。。这时候出现了线程切换，线程B开始执行，先锁银锁，成功，然后去lock金锁。
    此时此刻，死锁产生
  - 2.A拿不到银锁，B拿不到金锁，都阻塞，也不将自己的锁解开，产生死锁

- 死锁演示

  ```c++
  class A {
  public:
  	void inMsgRecvQueue()
  	{
  		for (int i = 0; i < 100000; i++)
  		{
  			cout << "inMsgRecvQueue，插入一个元素" << i << endl;
  				my_mutex1.lock();//实际代码中，两个lock不一定挨着
  				my_mutex2.lock();
  				msgRevcQuere.push_back(i);//假设数字i就是收到的命令，插入消息队列
  				my_mutex2.unlock();
  				my_mutex1.unlock();
  			//...很长一段处理代码
  		}
  	}
  
  	bool outMsgLULProc(int &command)
  	{
  		my_mutex2.lock();
  		my_mutex1.lock();
  		if (!msgRevcQuere.empty())
  		{
  			command = msgRevcQuere.front();//返回第一个元素
  			msgRevcQuere.pop_front();//移除第一个元素，但不返回
  			my_mutex1.unlock();
  			my_mutex2.unlock();
  			return true;						 //。。。。
  		}
  		my_mutex1.unlock();
  		my_mutex2.unlock();
  		return false;
  	}
  	
  	void outMsgRecvQueue()
  	{
  		int command = 0;
  		for (int i = 0; i < 100000; i++)
  		{
  			bool result = outMsgLULProc(command);
  			if (result == true)
  			{
  				cout << "outMsgRecvQueue()执行，取出一个元素" << command << endl;
  				//可以考虑可疑进行命令处理
  			}
  			else {
  				cout << "outMsgRecvQueue执行，但目前消息队列为空" << i << endl;
  			}
  		}
  		cout << "end" << endl;
  	}
  
  private:
  	std::list<int>msgRevcQuere;
  	std::mutex my_mutex1;
  	std::mutex my_mutex2;
  };
  
  int main()
  {
      A myobj;
      std::thread myOutMsgobj(&A::outMsgRecvQueue,&myobj);
      std::thread myInMsgobj(&A::inMsgrecvQueue,&myobj);
      
      myOutMsgobj.join();
      myInMsgobj.join();
      
      reutrn 0;
  }
  ```

  - 死锁简单例子

  ```c++
  #include <iostream>
  #include <thread>//线程头文件
  #include <mutex>
  using namespace std;
  
  std::mutex lock1;
  std::mutex lock2;
  
  void mythread1(int mypar)
  {
  	for (int i = 0; i < mypar; i++)
  	{
  		lock1.lock();
  		lock2.lock();
  		cout <<"1:"<< i << endl;
  		lock1.unlock();
  		lock2.unlock();
  	}
  }
  
  void mythread2(int mypar)
  {
  	for (int i = 0; i < mypar; i++)
  	{
  		lock1.lock();
  		lock2.lock();
  		cout << "2:" << i << endl;
  		lock1.unlock();
  		lock2.unlock();
  	}
  }
  
  int main()
  {
  	std::thread t1(mythread1,10000);
  	std::thread t2(mythread2,10000);
  
  	t1.join();
  	t2.join();
  
  	return 0;
  }
  
  ```

  

- 死锁的一般解决方案：

  - 只要保证两个互斥量上锁的顺序一致，就不会死锁
  - std::lock()函数模板
    - 能力：一次锁住两个或者两个以上的互斥量（至少两个，多个不行，1个不行）；
    - 不存在因为锁头的顺序问题导致的死锁风险问题
    - 原理，std::lock()：要么两个互斥量都缩住，要么两个互斥量都没锁柱，一旦有一个没锁住就会解锁另一个已经锁住的互斥量。

  ```c++
  		std::lock(my_mutex1, my_mutex2);
  // 		my_mutex2.lock();
  // 		my_mutex1.lock();
  ```

- std::lock_guard的std::adopt_lock参数 

  - adopt_lock是一个结构体对象，起标记作用，标记已经此锁已经lock

  ```c++
  std::lock(my_mutex1, my_mutex2);
  std::lock_guard<std::mutex>sbgurad1(my_mutex1,std::adopt_lock); //用一个大括号包含需要加锁的代码段，提前结束lock_guard的生命周期
  std::lock_guard<std::mutex>sbgurad2(my_mutex2,std::adopt_lock); 
  ```

### unique取代lock_guard

- unique_lock取代lock_guard

  - unique_lock是类模板，工作中一般用lock_guard(推荐使用)；
  - unique_lock比lock_guard灵活很多

- unique_lock第二个参数

  - std::adopt_lock:表视互斥量已经被lock了（必须把互斥量提前lock了），假设调用方已经lock成功才能使用
  - std::try_to_lock:尝试用mutex的lock去锁定这个mutex，但如果没有锁定成功，我也会立即返回，并不会阻塞在那里
  - std::defer_lock:不能提前lock，否则会报异常，初始化一个没有加锁的mutex

- unique_lock的成员函数

  - lock()，加锁

  - unlock()，解锁

  - try_lock()，尝试给互斥量加锁，如果拿不到锁，则返回false，如果拿到了锁，返回true，这个函数不阻塞，可以执行其他任务

  - release(),返回它所管理的mutex对象指针，并释放所有权；也就是说，这个unique_lock和mutex不在有联系

    - 		//不要和unlock混淆，unique_lock初始化时候绑定，用release释放绑定
            		//一旦释放绑定，如果mutex处于锁定状态，需要手动解锁
            		//有时候需要unlock(),因为lock锁住的代码段越少，执行越快，整个效率运行程序越高。

- 所有权传递 std::unique_lock<std::mutex>sbguard1(std::move(sbgurad));//移动语义

### std::call_once

- c++11引入的函数，该函数第二个参数是一个函数名
- 功能是能够保证函数a只被调用一次。
- 具备互斥量这种能力，而且效率上比互斥量消耗的资源更少；
- 需要和一个标集结合使用，std::once_flag;

```c++
std::once_flag g_flag;//系统定义的标记

class MyCAS {

	static void CreateInstance()//只被调用一次的函数
	{
		m_instance = new MyCAS();
		static CGAEhuishou cl;
	}
private:
	MyCAS()
	{
	}

	static MyCAS * m_instance;//静态成员变量

public:
	static MyCAS *GetInstance()
	{
		std::call_once(g_flag, CreateInstance);//假设两个线程同时开始执行到这一行，其中一个线程要等另一个线程执行完createinstance后才能决定是否调用createinstance
		cout << "call_once执行完毕" << endl;
		return m_instance;
	}

	class CGAEhuishou {
	public:
		~CGAEhuishou()
		{
			if (MyCAS::m_instance)
			{
				delete MyCAS::m_instance;
				MyCAS::m_instance = NULL;
			}
		}
	};//类中套类，用来释放对象

	void func()
	{
		cout << "测试" << endl;
	}
};

```

## 条件变量

### condition_variable、wait()、notify_one()

- 实际上是一个类，等待一个条件达成，需要和互斥量来配合工作
- wait()用来等一个东西，
  - 当其他线程用notify_one（）将阻塞的wait叫醒后，wait就开始恢复干活了
  - wait()不断尝试获得互斥锁，如果获取不到，那么流程就卡住还等着获取锁 ，如果获取到那么wait继续执行b
  - 如果第二个参数lambda表达式返回的是true，那么wait直接返回，往下运行
  - 如果第二个参数lambda表达式返回的是false，那么wait()将解锁互斥量，并堵塞在本行，堵到其他某个线程调用notify_one()成员函数为止
- notify_all()
  - 同时唤醒两个wait



```c++
#include "stdafx.h"
#include <iostream>
#include <thread>//线程头文件
#include <vector>
#include <list>
#include <mutex>

using namespace std;

std::mutex resource_mutex;
std::once_flag g_flag;//系统定义的标记


class A {
public:
	//把收到的消息入到一个队列的线程
	void inMsgRecvQueue()
	{
		for (int i = 0; i < 100000; i++)
		{			
			std::unique_lock<std::mutex>sbguard(my_mutex);
			cout << "inMsgRecvQueue，插入一个元素" << i << endl;
			msgRevcQuere.push_back(i);//假设数字i就是收到的命令，插入消息队列
			my_cond.notify_all();//我们尝试把wait（）线程唤醒，执行完这行，那么outMsgRecvQueue()里面的wait就会被唤醒
		}
	}
	//把数据从消息队列中去除的线程
	void outMsgRecvQueue()
	{
		int command = 0;
		while (true)
		{
			std::unique_lock<std::mutex> sbguard(my_mutex);
			my_cond.wait(sbguard, [this] {//一个lambda表达式就是一个可调用对象
				if (!msgRevcQuere.empty())
					return true;
				return false;
			});

			command = msgRevcQuere.front();
			msgRevcQuere.pop_front();
			cout << "outMsgRecvQueue()执行，取出一个元素" << command <<"thread id"<<std::this_thread::get_id() << endl;
			sbguard.unlock();//

		}
	}

private:
	std::list<int>msgRevcQuere;
	std::mutex my_mutex;
	std::condition_variable my_cond;//生成一个条件变量
};




int main()
{
	A myobj;//创建对象
	std::thread myOutMsgobj(&A::outMsgRecvQueue, &myobj);//成员函数创建线程
	std::thread myOutMsgobj2(&A::outMsgRecvQueue, &myobj);
	std::thread myInMsgobj(&A::inMsgRecvQueue, &myobj);

	myOutMsgobj.join();//阻塞
	myOutMsgobj2.join();
	myInMsgobj.join();

	return 0;
}
```

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
- *开闭原则*。 向应用程序中引入新产品变体时， 你无需修改客户端代码。

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
  -  客户端不必根据需求对子类进行实例化， 只需找到合适的原型并对其进行克隆即可。

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
- *开闭原则*。 只要客户端代码通过客户端接口与适配器进行交互， 你就能在不修改现有客户端代码的情况下在程序中添加新类型的适配器。

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
-  对于功能差异较大的类，提供公共接口或许会有困难。在特定情况下，你需要过度一般化组件接口，使其变得令人难以理解。

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
- *单一职责原则*。 你可以将实现了许多不同行为的一个大类拆分为多个较小的类。

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
- *开闭原则*。 你可以在不更改现有代码的情况下在程序中新增处理者。

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
- *开闭原则*。 你无需修改实际组件就能增加新的中介者。
- 你可以减轻应用中多个组件间的耦合情况。
- 你可以更方便地复用各个组件。

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
- 比如说在一个二维地图中有主角和各种npc怪物，主角移动到怪物附近怪物需要攻击主角，就可以用观察者模式。

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
- 你可以在运行时建立对象之间的联系。

缺点：

- 订阅者的通知顺序是随机的。

### 迭代器模式（Iterator）

迭代器模式是一种行为设计模式，让你能在不暴露集合底层表现形式（列表、栈和树等）的情况下遍历集合中所有的元素。

#### 解决方案

迭代器模式的主要思想就是将集合的遍历行为抽取为单独的迭代器模式。

#### 迭代器模式结构

![迭代器设计模式的结构](C:/Users/free/Desktop/面试/interviewmd/assets/post/others/iterator.png)

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

# 算法与数据结构

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

## 一致性哈希

一致性哈希（Distribute Hash Table, DHT）是一种哈希分布方式，其目的是为了客服传统哈希分布在服务器节点数量变化时大量数据迁移的问题。

#### 基本原理

将哈希空间`[0,2^n-1], n=32`看成一个哈希环，每个服务器节点都配知道哈希环上，每个数据对象通过哈希取模得到哈希值之后，存放到哈希环中**顺时针方向第一个大于该哈希值的服务器节点**上。如下图将数据对象C插入到节点中，计算`Hash()%2^32`会得到一个节点，此时他的顺时针方向第一个服务器节点`Node C`，则将它放入节点`Node C`中。

一致性哈希在增加（集群扩容）或者删除服务器节点（某节点宕机）时只会影响到哈希环中**相邻的节点**。例如下图中新增服务器节点X，只需要将在节点`Node B`和节点`Node X`之间的原本属于`Node C`的数据对象C重新分配即可，对于其他数据对象A,B,D均没有影响。

#### 一致性哈希中的数据倾斜问题

当一致性Hash算法中的服务器节点过少时，容易因为分布不均匀而造成数据倾斜（被缓存的对象大部分集中缓存在某一台服务器上）的问题。如图中，只有两台机器`D0, D1`，这两台机器离的很近，那么顺时针第一个机器节点上将存有大量的数据；第二台机器上的数据很少。

- 使用**虚拟节点**解决数据倾斜问题，也就是每台机器节点会进行多次哈希，最终每个机器节点在哈喜欢上会有多个虚拟节点存在，使用这种方式来大大削弱甚至避免数据倾斜问题。同时数据定位算法不变，只是多了一步**虚拟节点到实际节点的映射**，例如定位到“D1#1”、“D1#2”、“D1#3”三个虚拟节点的数据均定位到 D1 上。

## 排序算法

### 算法分类

十种常见排序算法可以分为两大类：

- **比较类排序**：通过比较来决定元素间的相对次序，由于其时间复杂度不能突破O(nlogn)，因此也称为非线性时间比较类排序。

- **非比较类排序**：不通过比较来决定元素间的相对次序，它可以突破基于比较排序的时间下界，以线性时间运行，因此也称为线性时间非比较类排序。 

  ![img](https://img2018.cnblogs.com/blog/849589/201903/849589-20190306165258970-1789860540.png)

### 算法复杂度

![img](https://images2018.cnblogs.com/blog/849589/201804/849589-20180402133438219-1946132192.png)

## 归并排序（Merge Sort）

归并排序是建立在归并操作上的一种有效的排序算法。该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。若将两个有序表合并成一个有序表，称为2-路归并。 

### 算法描述

- 把长度为n的输入序列分成两个长度为n/2的子序列；
- 对这两个子序列分别采用归并排序；
- 将两个排序好的子序列合并成一个最终的排序序列。

### 动图演示

![img](https://images2017.cnblogs.com/blog/849589/201710/849589-20171015230557043-37375010.gif)

### 算法分析

归并排序是一种稳定的排序方法。和选择排序一样，归并排序的性能不受输入数据的影响，但表现比选择排序好的多，因为始终都是O(nlogn）的时间复杂度。代价是需要额外的内存空间。

## 快速排序（Quick Sort）

快速排序的基本思想：通过一趟排序将待排记录分隔成独立的两部分，其中一部分记录的关键字均比另一部分的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序。

### 算法描述

快速排序使用分治法来把一个串（list）分为两个子串（sub-lists）。具体算法描述如下：

- 从数列中挑出一个元素，称为 “基准”（pivot）；
- 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
- 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。

### 动图演示

![img](https://images2017.cnblogs.com/blog/849589/201710/849589-20171015230936371-1413523412.gif)

## 堆排序（Heap Sort）

堆排序（Heapsort）是指利用堆这种数据结构所设计的一种排序算法。堆积是一个近似完全二叉树的结构，并同时满足堆积的性质：即子结点的键值或索引总是小于（或者大于）它的父节点。

### 算法描述

- 将初始待排序关键字序列(R1,R2….Rn)构建成大顶堆，此堆为初始的无序区；
- 将堆顶元素R[1]与最后一个元素R[n]交换，此时得到新的无序区(R1,R2,……Rn-1)和新的有序区(Rn),且满足R[1,2…n-1]<=R[n]；
- 由于交换后新的堆顶R[1]可能违反堆的性质，因此需要对当前无序区(R1,R2,……Rn-1)调整为新堆，然后再次将R[1]与无序区最后一个元素交换，得到新的无序区(R1,R2….Rn-2)和新的有序区(Rn-1,Rn)。不断重复此过程直到有序区的元素个数为n-1，则整个排序过程完成。

### 动图演示

![img](https://images2017.cnblogs.com/blog/849589/201710/849589-20171015231308699-356134237.gif)

## 计数排序（Counting Sort）

计数排序不是基于比较的排序算法，其核心在于将输入的数据值转化为键存储在额外开辟的数组空间中。 作为一种线性时间复杂度的排序，计数排序要求输入的数据必须是有确定范围的整数。

### 算法描述

- 找出待排序的数组中最大和最小的元素；
- 统计数组中每个值为i的元素出现的次数，存入数组C的第i项；
- 对所有的计数累加（从C中的第一个元素开始，每一项和前一项相加）；
- 反向填充目标数组：将每个元素i放在新数组的第C(i)项，每放一个元素就将C(i)减去1。

### 动图演示

![img](https://images2017.cnblogs.com/blog/849589/201710/849589-20171015231740840-6968181.gif)

## 桶排序（Bucket Sort）

桶排序是计数排序的升级版。它利用了函数的映射关系，高效与否的关键就在于这个映射函数的确定。桶排序 (Bucket sort)的工作的原理：假设输入数据服从均匀分布，将数据分到有限数量的桶里，每个桶再分别排序（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行排）。

### 算法描述

- 设置一个定量的数组当作空桶；
- 遍历输入数据，并且把数据一个一个放到对应的桶里去；
- 对每个不是空的桶进行排序；
- 从不是空的桶里把排好序的数据拼接起来。 

### 图片演示

![img](https://images2017.cnblogs.com/blog/849589/201710/849589-20171015232107090-1920702011.png)

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

## 系统调用

如果一个进程在用户态需要使用内核态功能，就进行系统调用从而陷入内核态，之后由操作系统代为完成。

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
- 切换的三种方式：系统调用（用户进程主动）、中断（被动）、外围设备中断（被动）
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

## 软链接和硬链接区别

- 建立软链接和硬链接的语法
  - 软链接：ln -s 源文件 目标文件
  - 硬链接：ln 源文件 目标文件
- 软硬连接的理解
  - 软连接类似于快捷方式，指向源文件的地址
  - 硬连接类似于cp -p加上同步更新
- 删除原文件对软硬链接的影响
  - 软链接失效
  - 硬链接还可以查看

## 字节序（大端小端）

- 大端字节序：高位字节在前，低位字节在后，这是人类读写数值的方法。
- 小端字节序：低位字节在前，高位字节在后

为什么要有大端小段？

- 计算机电路先处理低位字节，效率比较高，因为计算都是从低位开始的。所以，计算机的内部处理都是小端字节序。

- 但是，人类还是习惯读写大端字节序。所以，除了计算机的内部处理，其他的场合几乎都是大端字节序，比如网络传输和文件储存。

- 一般只有读取外部数据的时候才需要考虑字节序 

常用系统的大小端

- x86字节序：小端 

- macos：大端 

- 网络字节序大端：

  ```c++
  //将主机字节序转换为网络字节序
   unit32_t htonl (unit32_t hostlong);
   unit16_t htons (unit16_t hostshort);
   //将网络字节序转换为主机字节序
   unit32_t ntohl (unit32_t netlong);
   unit16_t ntohs (unit16_t netshort);
  ```

如何判断

- **判断的思路是：**确定一个多字节的值（下面使用的是4字节的整数），将其写入内存（即赋值给一个变量），然后用指针取其首地址所对应的字节（即低地址的一个字节），判断该字节存放的是高位还是低位，高位说明是Big endian，低位说明是Little endian。

## linux查看端口、进程(常用命令)

- netstat -tunlp
- ps aux
- cat /proc/cpuinfo 显示CPU info的信息 
- df -h 显示已经挂载的分区列表 
- chmod ugo+rwx directory1 设置目录的所有人(u)、群组(g)以及其他人(o)以读（r ）、
- tar -cvfj archive.tar.bz2 dir1 创建一个bzip2格式的压缩包 
  tar -jxvf archive.tar.bz2 解压一个bzip2格式的压缩包 
  tar -cvfz archive.tar.gz dir1 创建一个gzip格式的压缩包 
  tar -zxvf archive.tar.gz 解压一个gzip格式的压缩包 

# 操作系统进程与线程

## 进程、线程和协程的概念

- 进程是对运行时程序的封装，是系统进行资源调度和分配的的基本单位，实现了操作系统的并发。

- 线程是进程内的一个执行实体或执行单元，是CPU调度和分派的基本单位，实现进程内部的并发。

- 区别：
  - 调度
    - 一个线程只能属于一个进程，而一个进程至少有一个线程。
    - 进程不会相互影响，而线程挂掉一个就会导致整个进程挂掉
  - 资源角度：
    - 进程在执行过程中拥有独立的内存单元，而同一进程的多个线程共享进程的内存。每个线程都由自己独立的栈段。
    - 进程是资源分配的最小单位，线程是CPU调度的最小单位
  - 系统开销
    - 进程的创造销毁切换所花费的系统开销要与远大于线程的开销
  - 进程间通信依靠IPC，线程间通信直接读取共享数据段
  
  - 协程和线程区别
  
    - 和多线程比，协程最大的优势就是协程极高的执行效率。因为子程序切换不是线程切换，而是由程序自身控制，因此，没有线程切换的开销，和多线程比，线程数量越多，协程的性能优势就越明显。
    - 第二大优势就是不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在协程中控制共享资源不加锁，只需要判断状态就好了，所以执行效率比多线程高很多。

## 阻塞，非阻塞，同步，异步

- 同步：在发出一个功能调用的时候，在没有得到结果之前，该调用就不返回。（该调用还处于激活状态）
- 异步：当一个异步调用发出后，调用这并不能立刻得到结果。实际处理调用的部件在完成后通过状态、通知和回调来通知调用者。
- 阻塞：阻塞调用在调用结果返回之前，线程会被挂起。只有在得到结果之后才会返回。
- 非阻塞：调用再不能立刻得到结果之前，函数不会阻塞当前进程，而会立刻返回。（recv接收数据）

## 进程状态转换图

![img](https://uploadfiles.nowcoder.com/images/20190313/311436_1552470678794_F9BF116BD97A95A5E655DF9E1672186F)



1）创建状态：进程正在被创建

2）就绪状态：进程被加入到就绪队列中等待CPU调度运行

3）执行状态：进程正在被运行

4）等待阻塞状态：进程因为某种原因，比如等待I/O，等待设备，而暂时不能运行。

5）终止状态：进程运行完毕

- 交换技术

  当多个进程竞争内存资源时，会造成内存资源紧张，并且，如果此时没有就绪进程，处理机会空闲，I/0速度比处理机速度慢得多，可能出现全部进程阻塞等待I/O。

  针对以上问题，提出了两种解决方法：

  - 1）交换技术：换出一部分进程到外存，腾出内存空间。
  - 2）虚拟存储技术：每个进程只能装入一部分程序和数据。

  在交换技术上，将内存暂时不能运行的进程，或者暂时不用的数据和程序，换出到外存，来腾出足够的内存空间，把已经具备运行条件的进程，或进程所需的数据和程序换入到内存。从而出现了进程的挂起状态：进程被交换到外存，进程状态就成为了挂起状态。

- 活动阻塞，静止阻塞，活动就绪，静止就绪

  - 1）活动阻塞：进程在内存，但是由于某种原因被阻塞了。
  - 2）静止阻塞：进程在外存，同时被某种原因阻塞了。
  - 3）活动就绪：进程在内存，处于就绪状态，只要给CPU和调度就可以直接运行。
  - 4）静止就绪：进程在外存，处于就绪状态，只要调度到内存，给CPU和调度就可以运行。

### 哪些情况进程会由运行转化为阻塞

- 进程缺少相应io资源
- 访问正在被其他进程访问的临界资源，等待解锁
- 进程睡眠

## 进程之间的通信方式以及优缺点

- 管道（PIPE）
  - 有名管道：一种半双工的通信方式，它允许无亲缘关系进程间的通信
    - 优点：可以实现任意关系的进程间的通信
    - 缺点：
      1. 长期存于系统中，使用不当容易出错
      2. 缓冲区有限
    - 使用：
      1. **int** **mkfifo**(**const** **char** *path, **mode_t** mode);
      2. **int** **mkfifoat**(**int** fd, **const** **char** *path, **mode_t** mode);
  - 无名管道：一种半双工的通信方式，只能在具有亲缘关系的进程间使用（父子进程）
    - 优点：简单方便
    - 缺点：
      1. 局限于单向通信
      2. 只能创建在它的进程以及其有亲缘关系的进程之间
      3. 缓冲区有限
    - 使用：
      1. 父进程创建一个管道，创建一个数组作为索引。（int pipe（int fd[2]））
      2. fork一个子进程，子进程会复制父进程的管道文件。父子进程根据需要各自关闭读写端。
- 信号量（Semaphore）：一个计数器，可以用来控制多个线程对共享资源的访问
  - 优点：可以同步进程
  - 缺点：信号量有限
  - 使用（SIGHUP）子进程监视父进程是否存在，接收父进程死亡的信号
  - 只有当管道所有的读端都被关闭时，才会产生SIGPIPE
- 信号（Signal）：一种比较复杂的通信方式，用于通知接收进程某个事件已经发生
- 消息队列（Message Queue）：是消息的链表，存放在内核中并由消息队列标识符标识
  - 优点：可以实现任意进程间的通信，并通过系统调用函数来实现消息发送和接收之间的同步，无需考虑同步问题，方便
  - 缺点：信息的复制需要额外消耗 CPU 的时间，不适宜于信息量大或操作频繁的场合
- 共享内存（Shared Memory）：映射一段能被其他进程所访问的内存，这段共享内存由一个进程创建，但多个进程都可以访问
  - 优点：无须复制，快捷，信息量大
  - 缺点：
    1. 通信是通过将共享空间缓冲区直接附加到进程的虚拟地址空间中来实现的，因此进程间的读写操作的同步问题
    2. 利用内存缓冲区直接交换信息，内存的实体存在于计算机中，只能同一个计算机系统中的诸多进程共享，不方便网络通信
- 套接字（Socket）：可用于不同计算机间的进程通信
  - 优点：
    1. 传输数据为字节级，传输数据可自定义，数据量小效率高
    2. 传输数据时间短，性能高
    3. 适合于客户端和服务器端之间信息实时交互
    4. 可以加密,数据安全性强
  - 缺点：需对传输的数据进行解析，转化成应用级的数据。

## 进程间的调度算法

- 批处理系统：先来先服务、短作业优先、最短剩余时间优先
- 交互式系统：时间片轮转、优先级调度，多级反馈队列

## 线程之间的通信方式

- 锁机制：包括互斥锁/量（mutex）、读写锁（reader-writer lock）、自旋锁（spin lock）、条件变量（condition）
  - 互斥锁/量（mutex）：提供了以排他方式防止数据结构被并发修改的方法。
  - 读写锁（reader-writer lock）：允许多个线程同时读共享数据，而对写操作是互斥的。
  - 自旋锁（spin lock）与互斥锁类似，都是为了保护共享资源。互斥锁是当资源被占用，申请者进入睡眠状态；而自旋锁则循环检测保持者是否已经释放锁。
  - 条件变量（condition）：可以以原子的方式阻塞进程，直到某个特定条件为真为止。对条件的测试是在互斥锁的保护下进行的。条件变量始终与互斥锁一起使用。
- 信号量机制(Semaphore)
  - 无名线程信号量
  - 命名线程信号量
- 信号机制(Signal)：类似进程间的信号处理
- 屏障（barrier）：屏障允许每个线程等待，直到所有的合作线程都达到某一点，然后从该点继续执行。

线程间的通信目的主要是用于线程同步，所以线程没有像进程通信中的用于数据交换的通信机制

## 进程、线程之间私有和共享的资源

### 进程之间私有和共享的资源

- 私有：地址空间、堆、全局变量、栈、寄存器
- 共享：代码段，公共数据，进程目录，进程 ID

### 线程之间私有和共享的资源

- 私有：线程栈，寄存器，程序计数器
- 共享：堆，地址空间，全局变量，静态变量

## 多进程和多线程对比

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

## fork函数（pid_t fork( void);）

- 调用后执行的功能

  - 向系统申请一个新PID
  - 创建子进程，复制父进程的PCB，获得父进程的数据空间、堆、栈等资源的副本
  - 在父进程中返回子进程的PID，在子进程中返回0
  - 执行完以上动作后，父进程和子进程便开始并发执行了。

- fork()返回值

  - 父进程中的fork()结束后返回子进程的pid
  - 子进程中的fork()结束后返回0
  - 错误返回负值

- 写时拷贝

  - 如果每一次fork()都要拷贝很浪费内存，linux中就在fork()后让父子进程共享内存，当进行写操作时再进行拷贝

- fork和vfork的区别：

  \1. fork( )的子进程拷贝父进程的数据段和代码段；vfork( )的子进程与父进程共享数据段

  \2. fork( )的父子进程的执行次序不确定；vfork( )保证子进程先运行，在调用exec或exit之前与父进程数据是共享的，在它调用exec或exit之后父进程才可能被调度运行。

  \3. vfork( )保证子进程先运行，在它调用exec或exit之后父进程才可能被调度运行。如果在调用这两个函数之前子进程依赖于父进程的进一步动作，则会导致死锁。

  4.当需要改变共享数据段中变量的值，则拷贝父进程。

- fork实例

```c
int main(void)
{
	pid_t pid;
	signal(SIGCHLD, SIG_IGN);
	printf("before fork pid:%d\n", getpid());
	int abc = 10;
	pid = fork();
    if(pid==-1){
        perror("tile");
		return -1;
    }
    if(pid==0){
        abc++;
		printf("child:%d,parent: %d\n", getpid(), getppid());
		printf("abc:%d", abc);
	}
    if(pid>0){
        abc++;
		printf("parent:pid:%d \n", getpid());
		printf("abc:%d \n", abc);
		sleep(20);
    }
    printf("fork after...\n");
}
```

### 

## 线程池原理

- 多线程技术主要解决处理器单元内多个线程执行的问题，它可以显著减少处理器单元的闲置时间，增加处理器单元的吞吐能力，假设一个服务器完成一项任务所需时间为：T1 创建线程时间，T2 在线程中执行任务的时间，T3 销毁线程时间。
- 如果：T1 + T3 远大于 T2，则可以采用线程池，以提高服务器性能。
- 一个线程池包括以下四个基本组成部分：
      1、线程池管理器（ThreadPool）：用于创建并管理线程池，包括 创建线程池，销毁线程池，添加新任务；
      2、工作线程（PoolWorker）：线程池中线程，在没有任务时处于等待状态，可以循环的执行任务；
      3、任务接口（Task）：每个任务必须实现的接口，以供工作线程调度任务的执行，它主要规定了任务的入口，任务执行完后的收尾工作，任务的执行状态等；
      4、任务队列（taskQueue）：用于存放没有处理的任务。提供一种缓冲机制。
- 线程池技术正是关注如何缩短或调整T1,T3时间的技术，从而提高服务器程序性能的。它把T1，T3分别安排在服务器程序的启动和结束的时间段或者一些空闲的时间段，这样在服务器程序处理客户请求时，不会有T1，T3的开销了。
  线程池不仅调整T1,T3产生的时间段，而且它还显著减少了创建线程的数目，看一个例子：
  假设一个服务器一天要处理50000个请求，并且每个请求需要一个单独的线程完成。在线程池中，线程数一般是固定的，所以产生线程总数不会超过线程池中线程的数目，而如果服务器不利用线程池来处理这些请求则线程总数为50000。一般线程池大小是远小于50000。所以利用线程池的服务器程序不会为了创建50000而在处理请求时浪费时间，从而提高效率。
- 怎么实现线程池
  - 1.设置一个生产者消费者队列，作为临界资源
  - 2.初始化n个线程，并让其运行起来，加锁去队列取任务运行
  - 3.当任务队列为空的时候，所有线程阻塞
  - 4.当生产者队列来了一个任务后，先对队列加锁，把任务挂在到队列上，然后使用条件变量去通知阻塞中的一个线程

### 正常进程、僵尸进程和孤儿进程

- 正常进程

  - 正常情况下，子进程是通过父进程创建的，子进程再创建新的进程。子进程的结束和父进程的运行是一个异步过程，即父进程永远无法预测子进程到底什么时候结束。 当一个进程完成它的工作终止之后，它的父进程需要调用wait()或者waitpid()系统调用取得子进程的终止状态。

    unix提供了一种机制可以保证只要父进程想知道子进程结束时的状态信息， 就可以得到：在每个进程退出的时候，内核释放该进程所有的资源，包括打开的文件，占用的内存等。 但是仍然为其保留一定的信息，直到父进程通过wait / waitpid来取时才释放。保存信息包括：

    1进程号the process ID

    2退出状态the termination status of the process

    3运行时间the amount of CPU time taken by the process等

- 孤儿进程

  - 一个父进程退出，而它的一个或多个子进程还在运行，那么那些子进程将成为孤儿进程。孤儿进程将被init进程(进程号为1)所收养，并由init进程对它们完成状态收集工作。

- 僵尸进程

  - 一个进程使用fork创建子进程，如果子进程退出，而父进程并没有调用wait或waitpid获取子进程的状态信息，那么子进程的进程描述符仍然保存在系统中。这种进程称之为僵尸进程。

    僵尸进程是一个进程必然会经过的过程：这是每个子进程在结束时都要经过的阶段。

    如果子进程在exit()之后，父进程没有来得及处理，这时用ps命令就能看到子进程的状态是“Z”。如果父进程能及时 处理，可能用ps命令就来不及看到子进程的僵尸状态，但这并不等于子进程不经过僵尸状态。

    如果父进程在子进程结束之前退出，则子进程将由init接管。init将会以父进程的身份对僵尸状态的子进程进行处理。

- 危害：

  - 如果进程不调用wait / waitpid的话， 那么保留的那段信息就不会释放，其进程号就会一直被占用，但是系统所能使用的进程号是有限的，如果大量的产生僵死进程，将因为没有可用的进程号而导致系统不能产生新的进程。

- 外部消灭：

  - 通过kill发送SIGTERM或者SIGKILL信号消灭产生僵尸进程的进程，它产生的僵死进程就变成了孤儿进程，这些孤儿进程会被init进程接管，init进程会wait()这些孤儿进程，释放它们占用的系统进程表中的资源

- 内部解决：

  - 1、子进程退出时向父进程发送SIGCHILD信号，父进程处理SIGCHILD信号。在信号处理函数中调用wait进行处理僵尸进程。
  - 2、fork两次，原理是将子进程成为孤儿进程，从而其的父进程变为init进程，通过init进程可以处理僵尸进程。

## 死锁

#### 原因

- 系统资源不足
- 资源分配不当
- 进程运行推进顺序不合适

#### 产生条件

- 互斥
- 请求和保持
- 不剥夺
- 环路

#### 编码时解决死锁

- 死锁的一般解决方案：

  - 只要保证两个互斥量上锁的顺序一致，就不会死锁
  - std::lock()函数模板
    - 能力：一次锁住两个或者两个以上的互斥量（至少两个，多个不行，1个不行）；
    - 不存在因为锁头的顺序问题导致的死锁风险问题
    - 原理，std::lock()：要么两个互斥量都缩住，要么两个互斥量都没锁柱，一旦有一个没锁住就会解锁另一个已经锁住的互斥量。

  ```c++
  		std::lock(my_mutex1, my_mutex2);
  // 		my_mutex2.lock();
  // 		my_mutex1.lock();
  ```

- std::lock_guard的std::adopt_lock参数 

  - adopt_lock是一个结构体对象，起标记作用，标记已经此锁已经lock

  ```c++
  std::lock(my_mutex1, my_mutex2);
  std::lock_guard<std::mutex>sbgurad1(my_mutex1,std::adopt_lock); //用一个大括号包含需要加锁的代码段，提前结束lock_guard的生命周期
  std::lock_guard<std::mutex>sbgurad2(my_mutex2,std::adopt_lock); 
  ```

#### 处理方法

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

## Linux的4种锁机制：

- 互斥锁：互斥锁：mutex，用于保证在任何时刻，都只能有一个线程访问该对象。当获取锁操作失败时，线程会进入睡眠，等待锁释放时被唤醒
- 读写锁：rwlock，分为读锁和写锁。处于读操作时，可以允许多个线程同时获得读操作。但是同一时刻只能有一个线程可以获得写锁。其它获取写锁失败的线程都会进入睡眠状态，直到写锁释放时被唤醒。 注意：写锁会阻塞其它读写锁。当有一个线程获得写锁在写时，读锁也不能被其它线程获取；写者优先于读者（一旦有写者，则后续读者必须等待，唤醒时优先考虑写者）。适用于读取数据的频率远远大于写数据的频率的场合。
- 自旋锁：spinlock，在任何时刻同样只能有一个线程访问对象。但是当获取锁操作失败时，不会进入睡眠，而是会在原地自旋，直到锁被释放。这样节省了线程从睡眠状态到被唤醒期间的消耗，在加锁时间短暂的环境下会极大的提高效率。但如果加锁时间过长，则会非常浪费CPU资源。
- RCU：即read-copy-update，在修改数据时，首先需要读取数据，然后生成一个副本，对副本进行修改。修改完成后，再将老数据update成新的数据。使用RCU时，读者几乎不需要同步开销，既不需要获得锁，也不使用原子指令，不会导致锁竞争，因此就不用考虑死锁问题了。而对于写者的同步开销较大，它需要复制被修改的数据，还必须使用锁机制同步并行其它写者的修改操作。在有大量读操作，少量写操作的情况下效率非常高。

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

## 进程线程常见面试题

### 设计一下如何采用单线程的方式处理高并发

- 在单线程模型中，可以采用I/O复用来提高单线程处理多个请求的能力，然后再采用事件驱动模型，基于异步回调来处理事件来

### 如何设计server，使得能够接收多个客户端的请求

- 单线程+io复用
- 线程池
- 多线程

### 死循环+来连接时新建线程的方法效率有点低，怎么改进？

- 提前创建好一个线程池，用生产者消费者模型，创建一个任务队列，队列作为临界资源，有了新连接，就挂在到任务队列上，队列为空所有线程睡眠。

- 改进死循环：使用select epoll这样的技术

### 怎么唤醒被阻塞的socket线程？

当socket接受到数据，中断程序调用回调函数唤醒线程

### 有了进程，为什么还要有线程？

- 线程产生的原因：如果没有线程，那么一个进程在同一时间只能干一件事情。如果进程在执行过程中因为缺少资源而被阻塞，即使有些任务不需要当前缺少的资源，整个进程也会被挂起。

- 线程的优势

  - 从资源上来讲，线程是一种非常"节俭"的多任务操作方式。在linux系统下，启动一个新的进程必须分配给它独立的地址空间，建立众多的数据表来维护它的代码段、堆栈段和数据段，这是一种"昂贵"的多任务工作方式。而线程可以共享进程的内存空间。
  - 从切换效率上来讲，运行于一个进程中的多个线程，它们之间使用相同的地址空间，而且线程间彼此切换所需时间也远远小于进程间切换所需要的时间。据统计，一个进程的开销大约是一个线程开销的30倍左右。
  - 从通信机制上来讲，线程间方便的通信机制。对不同进程来说，它们具有独立的数据空间，要进行数据的传递只能通过进程间通信的方式进行，这种方式不仅费时，而且很不方便。线程则不然，由于同一进城下的线程之间贡献数据空间，所以一个线程的数据可以直接为其他线程所用，这不仅快捷，而且方便。

- 多线程程序作为一种多任务、并发的工作方式，还有如下优点：

  1、使多CPU系统更加有效。操作系统会保证当线程数不大于CPU数目时，不同的线程运行于不同的CPU上。

  2、改善程序结构。一个既长又复杂的进程可以考虑分为多个线程，成为几个独立或半独立的运行部分，这样的程序才会利于理解和修改。

### 单核机器上写多线程程序，是否需要考虑加锁，为什么？

- 在单核机器上写多线程程序，仍然需要线程锁。因为线程锁通常用来实现线程的同步和通信。在单核机器上的多线程程序，仍然存在线程同步的问题。因为在抢占式操作系统中，通常为每个线程分配一个时间片，当某个线程时间片耗尽时，操作系统会将其挂起，然后运行另一个线程。如果这两个线程共享某些数据，不使用线程锁的前提下，可能会导致共享数据修改引起冲突。

# 操作系统内存管理

## 虚拟内存

- 目的是为了让物理内存扩充成更大的逻辑内存，从而让程序获得更多的可用内存

- 为了更好的管理内存，系统将内存抽象成地址空间。
- 每个程序拥有自己的地址空间，这个地址空间被分为多个块，每一块为一页。这些页被映射到物理内存，但不需要映射到连续的物理内存，也不需要所有页都在物理内存中。
- 当引用到不再物理内存中的页时，由硬件执行必要的映射，将缺失的部分装入物理内存并重新执行失败的命令
- 虚拟内存允许内存不用将地址空间中的每一页都映射到物理内存，也就是说一个程序不需要全部调入内存就可以运行。16位地址可以映射64KB地址，32位可以映射4GB地址。

![image-20200816131820778](C:\Users\free\AppData\Roaming\Typora\typora-user-images\image-20200816131820778.png)

### 分页系统地址映射

内存管理单元（`Memory Management Unit, MMU`）管理着地址空间和物理内存的转换，其中的页表（`Page table`）存储着页（程序地址空间）和页框（物理内存空间）的映射表。

一个虚拟地址分成两个部分，一部分存储页面号，一部分存储偏移量。即（存储页面号+页内偏移量）

下图的页表存放着 16 个页，这 16 个页需要用 4 个比特位来进行索引定位。例如对于虚拟地址（`0010 0000 0000 0100`），前 4 位是存储页面号 2，读取表项内容为（`110 1`），页表项最后一位表示是否存在于内存中，1 表示存在，0表示不存在。后 12 位存储偏移量。这个页对应的页框的地址为 （`110 0000 0000 0100`）。

![image-20200816131902810](C:\Users\free\AppData\Roaming\Typora\typora-user-images\image-20200816131902810.png)

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

#### 时钟（Clock）

第二次机会算法需要在链表中移动页面，降低了效率。时钟算法使用环形链表将页面连接起来，再使用一个指针指向最老的页面。

## 分段和分页

- 分段通俗解释：linux中就把一个程序分成代码段，数据段和堆栈段等。
- 分页通俗解释：将这些段，例如代码段分成均匀的小块，然后这些给这些小块编号，然后就可以放到内存中去，由于编号了的，所以也不怕顺序乱
- 然后我们就可以通过段号，页号和页内偏移找到程序的地址

虚拟内存采用的是分页技术，也就是将地址空间划分成固定大小的页，每一页再与内存进行映射。

下图为一个编译器在编译过程中建立的多个表，有 4 个表是动态增长的，如果使用分页系统的一维地址空间，动态增长的特点会导致覆盖问题的出现。

分段的做法是把每个表分成段，**一个段构成一个独立的地址空间**。每个段的长度可以不同，并且可以动态增长。

## 段页式

程序的地址空间划分成多个拥有独立地址空间的段，每个段上的地址空间划分成大小相同的页。这样既拥有分段系统的共享和保护，又拥有分页系统的虚拟内存功能。

### 分页和分段的比较

- 对程序员：分页透明，分段需要程序员显式划分每个段
- 地址空间维度：分页地址是一维的，分段地址是二维（段名+段内地址）的
- 大小是否可以改变：分页不可变，分段可变
- 出现的原因：分页主要用于虚拟内存，从而获得更大的地址空间；分段是为了使程序和数据可以被划分为逻辑上独立的地址空间并且有助于共享和保护

# 操作系统linux

## Linux文件系统

- `superblock`：记录文件系统的整体信息，包括inode和block的总量，使用量和剩余量，以及文件系统的格式及相关信息等；
- `block bitmap`：记录block是否被使用的位图
- `inode`：一个文件占用一个inode，记录文件的属性，同时记录此文件的内容所在的block编号；
- `block`：记录文件的内容，文件太大时，会占用多个block

![img](C:/Users/free/Desktop/面试/interviewmd/assets/post/2018-04-02/BSD_disk.png)

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

# 计算机网络层级协议

- 1、DNS协议，http协议，https协议属于应用层

应用层是体系结构中的最高层。应用层确定进程之间通信的性质以满足用户的需要。这里的进程就是指正在运行的程序。应用层不仅要提供应用进程所需要的信息交换和远地操作，而且还要作为互相作用的应用进程的用户代理，来完成一些为进行语义上有意义的信息交换所必须的功能。应用层直接为用户的应用进程提供服务。

- 2、TCP/UDP属于传输层

传输层的任务就是负责主机中两个进程之间的通信。因特网的传输层可使用两种不同协议：即面向连接的传输控制协议[TCP](https://www.baidu.com/s?wd=TCP&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)，和无连接的用户数据报协议[UDP](https://www.baidu.com/s?wd=UDP&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)。面向连接的服务能够提供可靠的交付，但无连接服务则不保证提供可靠的交付，它只是“尽最大努力交付”。这两种服务方式都很有用，备有其优缺点。在分组交换网内的各个交换结点机都没有传输层。

- 3、IP协议，ARP协议属于网络层

网络层负责为分组交换网上的不同主机提供通信。在发送数据时，网络层将运输层产生的报文段或用户数据报封装成分组或包进行传送。在[TCP](https://www.baidu.com/s?wd=TCP&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)/IP体系中，分组也叫作IP数据报，或简称为数据报。网络层的另一个任务就是要选择合适的路由，使源主机运输层所传下来的分组能够交付到目的主机。

- 4、数据链路层

当发送数据时，数据链路层的任务是将在网络层交下来的IP数据报组装成帧，在两个相邻结点间的链路上传送以帧为单位的数据。每一帧包括数据和必要的控制信息（如同步信息、地址信息、差错控制、以及[流量控制](https://www.baidu.com/s?wd=流量控制&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)信息等）。控制信息使接收端能够知道—个帧从哪个比特开始和到哪个比特结束。控制信息还使接收端能够检测到所收到的帧中有无差错。

- 5、物理层

物理层的任务就是透明地传送比特流。在物理层上所传数据的单位是比特。传递信息所利用的一些物理媒体，如[双绞线](https://www.baidu.com/s?wd=双绞线&tn=SE_PcZhidaonwhc_ngpagmjz&rsv_dl=gh_pc_zhidao)、同轴电缆、光缆等，并不在物理层之内而是在物理层的下面。因此也有人把物理媒体当做第0层。

## 七层协议各层作用及协议

| 分层       | 作用                                                | 协议                                                |
| ---------- | --------------------------------------------------- | --------------------------------------------------- |
| 物理层     | 通过媒介传输比特，确定机械及电气规范（比特 Bit）    | RJ45、CLOCK、IEEE802.3（中继器，集线器）            |
| 数据链路层 | 将比特组装成帧和点到点的传递（帧 Frame）            | PPP、FR、HDLC、VLAN、MAC（网桥，交换机）            |
| 网络层     | 负责数据包从源到宿的传递和网际互连（包 Packet）     | IP、ICMP、ARP、RARP、OSPF、IPX、RIP、IGRP（路由器） |
| 运输层     | 提供端到端的可靠报文传递和错误恢复（ 段Segment）    | TCP、UDP、SPX                                       |
| 会话层     | 建立、管理和终止会话（会话协议数据单元 SPDU）       | NFS、SQL、NETBIOS、RPC                              |
| 表示层     | 对数据进行翻译、加密和压缩（表示协议数据单元 PPDU） | JPEG、MPEG、ASII                                    |
| 应用层     | 允许访问OSI环境的手段（应用协议数据单元 APDU）      | FTP、DNS、Telnet、SMTP、HTTP、WWW、NFS              |

### 物理层

- 传输数据的单位：比特
- 数据传输系统：源系统（源点、发送器） --> 传输系统 --> 目的系统（接收器、终点）

通道：

- 单向通道（单工通道）：只有一个方向通信，没有反方向交互，如广播
- 双向交替通信（半双工通信）：通信双方都可发消息，但不能同时发送或接收
- 双向同时通信（全双工通信）：通信双方可以同时发送和接收信息

通道复用技术：

- 频分复用（FDM，Frequency Division Multiplexing）：不同用户在不同频带，所用用户在同样时间占用不同带宽资源
- 时分复用（TDM，Time Division Multiplexing）：不同用户在同一时间段的不同时间片，所有用户在不同时间占用同样的频带宽度
- 波分复用（WDM，Wavelength Division Multiplexing）：光的频分复用
- 码分复用（CDM，Code Division Multiplexing）：不同用户使用不同的码，可以在同样时间使用同样频带通信

### 数据链路层

主要信道：

- 点对点信道
- 广播信道

#### 点对点信道

- 数据单元：帧

三个基本问题：

- 封装成帧：把网络层的 IP 数据报封装成帧，`SOH - 数据部分 - EOT`
- 透明传输：不管数据部分什么字符，都能传输出去；可以通过字节填充方法解决（冲突字符前加转义字符）
- 差错检测：降低误码率（BER，Bit Error Rate），广泛使用循环冗余检测（CRC，Cyclic Redundancy Check）

点对点协议（Point-to-Point Protocol）：

- 点对点协议（Point-to-Point Protocol）：用户计算机和 ISP 通信时所使用的协议

#### 广播信道

广播通信：

- 硬件地址（物理地址、MAC 地址）
- 单播（unicast）帧（一对一）：收到的帧的 MAC 地址与本站的硬件地址相同
- 广播（broadcast）帧（一对全体）：发送给本局域网上所有站点的帧
- 多播（multicast）帧（一对多）：发送给本局域网上一部分站点的帧

### 网络层

- IP（Internet Protocol，网际协议）是为计算机网络相互连接进行通信而设计的协议。
- ARP（Address Resolution Protocol，地址解析协议）
- ICMP（Internet Control Message Protocol，网际控制报文协议）
- IGMP（Internet Group Management Protocol，网际组管理协议）

#### IP 网际协议

IP 地址分类：

- `IP 地址 ::= {<网络号>,<主机号>}`

| IP 地址类别 | 网络号                                 | 网络范围               | 主机号 | IP 地址范围                  |
| ----------- | -------------------------------------- | ---------------------- | ------ | ---------------------------- |
| A 类        | 8bit，第一位固定为 0                   | 0 —— 127               | 24bit  | 1.0.0.0 —— 127.255.255.255   |
| B 类        | 16bit，前两位固定为 10                 | 128.0 —— 191.255       | 16bit  | 128.0.0.0 —— 191.255.255.255 |
| C 类        | 24bit，前三位固定为 110                | 192.0.0 —— 223.255.255 | 8bit   | 192.0.0.0 —— 223.255.255.255 |
| D 类        | 前四位固定为 1110，后面为多播地址      |                        |        |                              |
| E 类        | 前五位固定为 11110，后面保留为今后所用 |                        |        |                              |

IP 数据报格式：

[![IP 数据报格式](https://raw.githubusercontent.com/huihut/interview/master/images/IP%E6%95%B0%E6%8D%AE%E6%8A%A5%E6%A0%BC%E5%BC%8F.png)](https://raw.githubusercontent.com/huihut/interview/master/images/IP数据报格式.png)

#### ICMP 网际控制报文协议

ICMP 报文格式：

[![ICMP 报文格式](https://raw.githubusercontent.com/huihut/interview/master/images/ICMP%E6%8A%A5%E6%96%87%E6%A0%BC%E5%BC%8F.png)](https://raw.githubusercontent.com/huihut/interview/master/images/ICMP报文格式.png)

应用：

- PING（Packet InterNet Groper，分组网间探测）测试两个主机之间的连通性
- TTL（Time To Live，生存时间）该字段指定 IP 包被路由器丢弃之前允许通过的最大网段数量

#### 内部网关协议

- RIP（Routing Information Protocol，路由信息协议）
- OSPF（Open Sortest Path First，开放最短路径优先）

#### 外部网关协议

- BGP（Border Gateway Protocol，边界网关协议）

#### IP多播

- IGMP（Internet Group Management Protocol，网际组管理协议）
- 多播路由选择协议

#### VPN 和 NAT

- VPN（Virtual Private Network，虚拟专用网）
- NAT（Network Address Translation，网络地址转换）

#### 路由表包含什么？

1. 网络 ID（Network ID, Network number）：就是目标地址的网络 ID。
2. 子网掩码（subnet mask）：用来判断 IP 所属网络
3. 下一跳地址/接口（Next hop / interface）：就是数据在发送到目标地址的旅途中下一站的地址。其中 interface 指向 next hop（即为下一个 route）。一个自治系统（AS, Autonomous system）中的 route 应该包含区域内所有的子网络，而默认网关（Network id: `0.0.0.0`, Netmask: `0.0.0.0`）指向自治系统的出口。

根据应用和执行的不同，路由表可能含有如下附加信息：

1. 花费（Cost）：就是数据发送过程中通过路径所需要的花费。
2. 路由的服务质量
3. 路由中需要过滤的出/入连接列表

### 运输层

协议：

- TCP（Transmission Control Protocol，传输控制协议）
- UDP（User Datagram Protocol，用户数据报协议）

端口：

| 应用程序 | FTP  | TELNET | SMTP | DNS  | TFTP | HTTP | HTTPS      | SNMP |
| -------- | ---- | ------ | ---- | ---- | ---- | ---- | ---------- | ---- |
| 端口号   | 21   | 23     | 25   | 53   | 69   | 80   | 334（443） | 161  |

# 计算机网络SOKCET、io复用

## 介绍一下5种IO模型

- 1.阻塞IO:调用者调用了某个函数，等待这个函数返回，期间什么也不做，不停的去检查这个函数有没有返回，必须等这个函数返回才能进行下一步动作
- 2.非阻塞IO:非阻塞等待，每隔一段时间就去检测IO事件是否就绪。没有就绪就可以做其他事。
- 3.信号驱动IO:信号驱动IO:linux用套接口进行信号驱动IO，安装一个信号处理函数，进程继续运行并不阻塞，当IO时间就绪，进程收到SIGIO信号。然后处理IO事件。
- 4.IO复用/多路转接IO:linux用select/poll函数实现IO复用模型，这两个函数也会使进程阻塞，但是和阻塞IO所不同的是这两个函数可以同时阻塞多个IO操作。而且可以同时对多个读操作、写操作的IO函数进行检测。知道有数据可读或可写时，才真正调用IO操作函数
- 5.异步IO:linux中，可以调用aio_read函数告诉内核描述字缓冲区指针和缓冲区的大小、文件偏移及通知的方式，然后立即返回，当内核将数据拷贝到缓冲区后，再通知应用程序。

## epoll原理

- 调用顺序
  - int epoll_create(int size);
  - int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);
  - int epoll_wait(int epfd, struct epoll_event *events,int maxevents, int timeout);
- 工作流程
  - 当某个进程调用 epoll_create 方法时，内核会创建一个 eventpoll 对象
  - 创建 Epoll 对象后，可以用 epoll_ctl 添加或删除所要监听的 Socket，维护一个监视列表，内核会将eventpoll加入到需要监视的socket的等待队列中。
  - 当程序执行到 epoll_wait 时，如果 Rdlist 已经引用了 Socket，那么 epoll_wait 直接返回，如果 Rdlist 为空将进程索引放入eventpoll的等待队列中，阻塞进程。
  - 当 Socket 收到数据后，中断程序会操作socket等待队列中eventpoll ，向“就绪列表”添加 Socket 引用并且唤醒eventpoll等待队列中的进程。
  - 就绪列表：双向链表，epoll_ctl需要随时向监视列表中插入和删除socket，删除监视列表中的socket同时就要删除就绪列表中的socket。所以需要快速插入快速删除
  - 监视列表：保存监视的socket，至少要方便添加和移除，还要方便搜索避免重复添加。所以用红黑树的结构
  - 列表中存放的都是socket的文件描述符，通过item间接引用。

## IO多路复用

- 基本原理：IO多路复用就是我们说的select，poll，epoll。select/epoll的好处就在于单个process就可以同时处理多个网络连接的IO。它的基本原理就是select，poll，epoll这个function会不断的轮询所负责的所有socket，当某个socket有数据到达了，就通知用户进程。
- 和阻塞io的区别：I/O多路复用和阻塞I/O其实并没有太大的不同，事实上，还更差一些。因为这里需要使用两个system call (select 和 recvfrom)，而blocking IO只调用了一个system call (recvfrom)。但是，用select的优势在于它可以同时处理多个connection。
- 所以，如果处理的连接数不是很高的话，使用select/epoll的web server不一定比使用multi-threading + blocking IO的web server性能更好，可能延迟还更大。select/epoll的优势并不是对于单个连接能处理得更快，而是在于能处理更多的连接。

## select，epoll的区别，原理，性能，限制都说一说

- select：是最初解决IO阻塞问题的方法。用结构体fd_set来告诉内核监听多个文件描述符，该结构体被称为描述符集。由数组来维持哪些描述符被置位了。对结构体的操作封装在三个宏定义中。通过轮寻来查找是否有描述符要被处理
  - 存在的问题：
    - 内置数组的形式使得select的最大文件数受限与FD_SIZE；
    - 每次调用select前都要重新初始化描述符集，将fd从用户态拷贝到内核态，每次调用select后，都需要将fd从内核态拷贝到用户态；
    - 轮寻排查当文件描述符个数很多时，效率很低；
- poll：通过一个可变长度的数组解决了select文件描述符受限的问题。数组中元素是结构体，该结构体保存描述符的信息，每增加一个文件描述符就向数组中加入一个结构体，结构体只需要拷贝一次到内核态。poll解决了select重复初始化的问题。轮寻排查的问题未解决。
- epoll：轮寻排查所有文件描述符的效率不高，使服务器并发能力受限。因此，epoll采用只返回状态发生变化的文件描述符，便解决了轮寻的瓶颈。
  - LT模式与ET模式的区别如下：
    - LT模式：当epoll_wait检测到描述符事件发生并将此事件通知应用程序，应用程序可以不立即处理该事件。下次调用epoll_wait时，会再次响应应用程序并通知此事件。
    - ET模式：当epoll_wait检测到描述符事件发生并将此事件通知应用程序，应用程序必须立即处理该事件。如果不处理，下次调用epoll_wait时，不会再次响应应用程序并通知此事件。
  - **从上面可以看出，LT模式相比于ET模式：**
    - 优点在于：事件循环处理比较简单，无需关注应用层是否有缓冲或缓冲区是 否满，只管上报事件
    - 缺点在于：可能经常上报，可能影响性能。

## reactor模式

- 概念：
  - 反应器设计模式(Reactor pattern)是一种为处理并发服务请求，并将请求提交到一个或者多个服务处理程序的事件设计模式。当客户端请求抵达后，服务处理程序使用多路分配策略，由一个非阻塞的线程来接收所有的请求，然后派发这些请求至相关的工作线程进行处理。
- 什么场景下使用Reactor模式
  - 对于高并发系统，常会使用Reactor模式，其代替了常用的多线程处理方式，节省系统的资源，提高系统的吞吐量
- 通俗理解：
  - 一个餐厅，一个客人就餐就是一个socket，服务员就是一个线程。服务网络会有很多请求，服务器每收到一个请求就会指派工作线程去处理。
    - 多线程：有几个人来就餐，就派几个服务员去服务，一个服务员只服务一个客人。但是当同一时刻人数变多之后，需要的服务员的数量也会非常庞大，开销也就很大。
    - 线程池：只用10个服务员的线程池，服务完一个客人就服务另一个。但有时候客人点菜会很慢，这时候服务员需要一直等这个客人点完菜才可以服务别人，另一些等待服务的客人等不及。
    - reactor模式：当客人在点菜的时候，服务员可以去招呼其他客人，客人点好了菜，直接喊一声服务员，马上有空闲的服务员去服务。就是把给菜单、点菜过程、取走菜单交给后台这三步离散处理。
- 工作流程
  - 处理一个TcpConnection连接分为如下步骤，receive - process - send。如果正常处理，要求当前线程必须处理完这么一整个过程才能处理下个连接。而Reactor可以将其拆分成独立的。即处理receive的时候和process和send可以不挂钩。当前线程可以处理完receive，然后将process注册到事件中，然后处理下个连接。

## socket编程中常用的Api

- **int socket(int domain, int type, int protocol)** client:创建socket文件描述符,client:同（ip协议、数据类型、传输协议）
- **int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen)**client:可选，一般不绑定  server：绑定用于接收客户端连接的网络接口（必选远程设置为0000，局域网可以设置为内网地址）
  - client ：系统会给客户端随机给客户端分配端口。客户端可以绑定bind，指定接收socket的端口号，但是如果这个进程被开了多次，那么端口就重复了造成端口的冲突
- **int listen(int sockfd, int backlog)**client：可选 server：使socket处于监听网络状态（第二个参数，设置最大可以等待多少人链接）
- **int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);**client:用于连接server 
- **int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);**server:服务端接受链接请求,接受链接后，使用一个新的socket描述符，使服务端可接受多个链接
- ssize_t send(int sockfd, const void *buf, size_t len, int flags);send 向客户端发送一条数据
- ssize_t recv(int sockfd, void *buf, size_t len, int flags);数据包发送和接收
- ![img](https://uploadfiles.nowcoder.com/images/20190315/308571_1552654678444_69CF8398BCC9F204991E623723D022E7)

## Socket网络编程代码

> [Linux Socket 编程（不限 Linux）](https://www.cnblogs.com/skynet/archive/2010/12/12/1903949.html)

[![Socket 客户端服务器通讯](https://raw.githubusercontent.com/huihut/interview/master/images/socket%E5%AE%A2%E6%88%B7%E7%AB%AF%E6%9C%8D%E5%8A%A1%E5%99%A8%E9%80%9A%E8%AE%AF.jpg)](https://raw.githubusercontent.com/huihut/interview/master/images/socket客户端服务器通讯.jpg)

```c++
client
#define WIN32_LEAN_AND_MEAN//避免引入早期的依赖库的引用
#include <windows.h>
#include <WinSock2.h>
#include <stdio.h>
//1.引入了window头文件之后要调用他的函数需要加入他的静态链接库的文件,不然会报无法解析
//#pragma comment(lib,"ws2_32.lib")
//2.在win下是没有问题，但是在其它系统是不支持的，需要在工程配置属性里，链接器-输入-附加项-加入ws2_32.lib
int main()
{
	//启动windows socket 2.x 环境
	WORD ver = MAKEWORD(2, 2);
	WSADATA dat;
	WSAStartup(ver, &dat);//启动网络环境
	//-------------------------
	//--用socket API建立简单的TCP客户端
	//1.建立一个socket
	SOCKET _sock=socket(AF_INET, SOCK_STREAM, 0);
	if (INVALID_SOCKET == _sock)
	{
		printf("ERROR,建立套接字失败\n");
	}else{
		printf("SUCCESS,建立套接字成功\n");
	}
	//2.链接服务器connect
	sockaddr_in _sin = {};
	_sin.sin_family = AF_INET;
	_sin.sin_port = htons(4567);
	_sin.sin_addr.S_un.S_addr = inet_addr("127.0.0.1");//本机测试，可以用本机ip
	int ret = connect(_sock, (sockaddr*)&_sin, sizeof(sockaddr_in));
	if (SOCKET_ERROR == ret)
	{
		printf("ERROR,连接服务器失败\n");
	}
	else {
		printf("SUCCESS,连接服务器成功\n");
	}

	//3.接受服务器消息recv
	char recvBuf[256] = {};
	int nlen = recv(_sock, recvBuf, 256, 0);
	if (nlen > 0)
	{
		printf("客户端接收到的数据：%s\n", recvBuf);
	}
	//4.关闭套接字closesocket
	closesocket(_sock);

	//-------------------------
	//清除windows socket环境
	WSACleanup();//关闭网路环境
	getchar();
	return 0;
}
```

```c++
server
#define WIN32_LEAN_AND_MEAN//避免引入早期的依赖库的引用
#define _WINSOCK_DEPRECATED_NO_WARNINGS//line63 inet_ntoa 错误需要此宏
#include <windows.h>
#include <WinSock2.h>
#include <stdio.h>
//1.引入了window头文件之后要调用他的函数需要加入他的静态链接库的文件,不然会报无法解析
//#pragma comment(lib,"ws2_32.lib")
//2.在win下是没有问题，但是在其它系统是不支持的，需要在工程配置属性里，链接器-输入-附加项-加入ws2_32.lib

int main()
{
	//启动windows socket 2.x 环境
	WORD ver = MAKEWORD(2, 2);
	WSADATA dat;
	WSAStartup(ver, &dat);//启动网络环境
	//-------------------------
	//--用socket API建立简单的TCP服务器
	
	//1.建立一个socket
	SOCKET _sock=socket(AF_INET, SOCK_STREAM, IPPROTO_TCP);//ip协议、数据类型、传输协议
	
	//2.bind 绑定用于接收客户端连接的网络接口
	sockaddr_in _sin = {};
	_sin.sin_family = AF_INET;//网络ip
	_sin.sin_port = htons(4567);//端口号主机字节序和网络字节序不匹配 host to net unsigned short
	_sin.sin_addr.S_un.S_addr = INADDR_ANY;//本机任意地址  s_un联合体
											//inet_addr("127.0.0.1");//ip本机地址
	if (bind(_sock, (sockaddr*)&_sin, sizeof(_sin)) == SOCKET_ERROR)//判断某个端口是否被绑定
	{
		printf("ERROR，绑定用于接收客户端连接的网络接口\n");
	}
	else
	{
		printf("SUCCESS，绑定端口成功\n");
	}
	
	//3.listen 监听网络端口
	//第二个参数，最大可以等待多少人链接
	if (SOCKET_ERROR == listen(_sock, 5))
	{
		printf("ERROR,监听网络端口失败\n");
	}
	else
	{
		printf("SUCCESS,监听网络端口成功\n");
	}

	//4.accept 等待接收客户端连接
	sockaddr_in clientAddr = {};
	int nAddrLen = sizeof(sockaddr_in);
	SOCKET _cSock = INVALID_SOCKET;
	while (true)//循环重复执行
	{
		_cSock = accept(_sock, (sockaddr*)&clientAddr, &nAddrLen);
		if (_cSock == INVALID_SOCKET)
		{
			printf("ERROR,接收到无效的客户端SOCKET\n");
		}
		printf("新客户端家属：ip=%s \n", inet_ntoa(clientAddr.sin_addr));
		//5.send 向客户端发送一条数据
		char msgBug[] = "hello,i'm server.";
		send(_cSock, msgBug, strlen(msgBug) + 1, 0);
	}
	
	//6.关闭套接字closesocket
	closesocket(_sock);
	//-------------------------
	//清除windows socket环境
	WSACleanup();//关闭网路环境
	return 0;
}
```



## Socket 中 TCP 的三次握手建立连接

我们知道 TCP 建立连接要进行 “三次握手”，即交换三个分组。大致流程如下：

1. 客户端向服务器发送一个 SYN J
2. 服务器向客户端响应一个 SYN K，并对 SYN J 进行确认 ACK J+1
3. 客户端再想服务器发一个确认 ACK K+1

只有就完了三次握手，但是这个三次握手发生在 Socket 的那几个函数中呢？请看下图：

[![socket 中发送的 TCP 三次握手](https://camo.githubusercontent.com/564d582cc74a6293ce8c3231d779467c4525d450/687474703a2f2f696d616765732e636e626c6f67732e636f6d2f636e626c6f67735f636f6d2f736b796e65742f3230313031322f3230313031323132323135373436373235382e706e67)](https://camo.githubusercontent.com/564d582cc74a6293ce8c3231d779467c4525d450/687474703a2f2f696d616765732e636e626c6f67732e636f6d2f636e626c6f67735f636f6d2f736b796e65742f3230313031322f3230313031323132323135373436373235382e706e67)

从图中可以看出：

1. 当客户端调用 connect 时，触发了连接请求，向服务器发送了 SYN J 包，这时 connect 进入阻塞状态；
2. 服务器监听到连接请求，即收到 SYN J 包，调用 accept 函数接收请求向客户端发送 SYN K ，ACK J+1，这时 accept 进入阻塞状态；
3. 客户端收到服务器的 SYN K ，ACK J+1 之后，这时 connect 返回，并对 SYN K 进行确认；
4. 服务器收到 ACK K+1 时，accept 返回，至此三次握手完毕，连接建立。

## Socket 中 TCP 的四次握手释放连接

上面介绍了 socket 中 TCP 的三次握手建立过程，及其涉及的 socket 函数。现在我们介绍 socket 中的四次握手释放连接的过程，请看下图：

[![socket 中发送的 TCP 四次握手](https://camo.githubusercontent.com/d84659d4de694c4fc4ad2b039fe5df626a2e95f3/687474703a2f2f696d616765732e636e626c6f67732e636f6d2f636e626c6f67735f636f6d2f736b796e65742f3230313031322f3230313031323132323135373438373631362e706e67)](https://camo.githubusercontent.com/d84659d4de694c4fc4ad2b039fe5df626a2e95f3/687474703a2f2f696d616765732e636e626c6f67732e636f6d2f636e626c6f67735f636f6d2f736b796e65742f3230313031322f3230313031323132323135373438373631362e706e67)

图示过程如下：

1. 某个应用进程首先调用 close 主动关闭连接，这时 TCP 发送一个 FIN M；
2. 另一端接收到 FIN M 之后，执行被动关闭，对这个 FIN 进行确认。它的接收也作为文件结束符传递给应用进程，因为 FIN 的接收意味着应用进程在相应的连接上再也接收不到额外数据；
3. 一段时间之后，接收到文件结束符的应用进程调用 close 关闭它的 socket。这导致它的 TCP 也发送一个 FIN N；
4. 接收到这个 FIN 的源发送端 TCP 对它进行确认。

这样每个方向上都有一个 FIN 和 ACK。

### 操作系统将网络数据写入相对应的socket的接受缓存区，如何知道网络数据对应于哪个 Socket?

- 每一个socket都对应一个端口号，而网络数据包中包含了ip和端口信息，内核就可以通过端口号找到相对应的socket。为了快速读取，操作系统也会维护端口号到socket的索引结构。

# 计算机网络TCP/UDP

## TCP和UDP的区别

1） 连接

TCP是面向连接的传输层协议，即传输数据之前必须先建立好连接。

UDP无连接。

2） 服务对象

TCP是点对点的两点间服务，即一条TCP连接只能有两个端点；

UDP支持一对一，一对多，多对一，多对多的交互通信。

3） 可靠性

TCP是可靠交付：无差错，不丢失，不重复，按序到达。

UDP是尽最大努力交付，不保证可靠交付。

4）拥塞控制，流量控制

TCP有拥塞控制和流量控制保证数据传输的安全性。

UDP没有拥塞控制，网络拥塞不会影响源主机的发送效率。

5） 报文长度

TCP是动态报文长度，即TCP报文长度是根据接收方的窗口大小和当前网络拥塞情况决定的。

UDP面向报文，不合并，不拆分，保留上面传下来报文的边界。

6)  首部开销

TCP首部开销大，首部20个字节。

UDP首部开销小，8字节。（源端口，目的端口，数据长度，校验和）

2）TCP和UDP适用场景

从特点上我们已经知道，TCP 是可靠的但传输速度慢，UDP 是不可靠的但传输速度快。因此在选用具体协议通信时，应该根据通信数据的要求而决定。

若通信数据完整性需让位与通信实时性，则应该选用TCP 协议（如文件传输、重要状态的更新等）；反之，则使用 UDP 协议（如视频传输、实时通信等）。

## TCP和UDP适用场景

### 什么时候使用TCP？

- **使用TCP的场景有：**
  - 万维网（HTTP等）
  - 文件传输（FTP等）
  - 电子邮件（SMTP等）
  - 远程终端接入等（TELNET等）
- **原因如下：**
  - 对于文件传输、远程终端这些应用来说，是不允许数据丢失的，因为TCP提供了超时与重传等可靠机制，因此采用TCP

### 什么时候使用UDP？

- **使用UDP的场景有：**
  - 包量比较少的通信（DNS、SNMP等）
  - 音视频通话（即时通信等）
  - 广播通信（广播、多播）
- **原因如下：**
  - 因为UDP是不可靠传输，所以当包量比较少的时候使用UDP是比较适合的，因为这样可以避免丢失大量的重要数据
  - 对于音视频通话来说，因为UDP是非连接的，所以占用资源比较少，并且对于音视频来说，丢掉一些数据包也是可以接受的

## TCP收到的攻击

洪泛攻击，大量无意义的tcp连接请求，导致服务器一直处于半连接状态。

对于同一ip发来大量无意义连接请求，拉入黑名单

## TCP怎么保证可靠性

- 序列号、确认应答、超时重传
  - 数据到达接收方，接收方需要发出一个确认应答，表示已经收到该数据段，并且确认序号会说明了它下一次需要接收的数据序列号。如果发送方迟迟未收到确认应答，那么可能是发送的数据丢失，也可能是确认应答丢失，这时发送方在等待一定时间后会进行重传。这个时间一般是2*RTT(报文段往返时间）+一个偏差值。
- 窗口控制与高速重发控制/快速重传（累积确认）
  - TCP会利用窗口控制来提高传输速度，意思是在一个窗口大小内，不用一定要等到应答才能发送下一段数据，窗口大小就是无需等待确认而可以继续发送数据的最大值。如果不使用窗口控制，每一个没收到确认应答的数据都要重发。
    - 例子：使用窗口控制，如果数据段n-n+m丢失，后面数据每次传输，确认应答都会不停地发送序号为n的应答，表示我要接收n开始的数据，发送端如果收到3次相同应答，就会立刻进行重发；但还有种情况有可能是数据都收到了，但是有的应答丢失了，这种情况不会进行重发，因为发送端知道，如果是数据段丢失，接收端不会放过它的，会疯狂向它提醒......
- 拥塞控制
  - 慢启动：定义拥塞窗口，一开始将该窗口大小设为1，之后每次收到确认应答（经过一个rtt），将拥塞窗口大小*2。
  - 拥塞避免：设置慢启动阈值，一般开始都设为65536。拥塞避免是指当拥塞窗口大小达到这个阈值，拥塞窗口的值不再指数上升，而是加法增加（每次确认应答/每个rtt，拥塞窗口大小+1），以此来避免拥塞。将报文段的超时重传看做拥塞，则一旦发生超时重传，我们需要先将阈值设为当前窗口大小的一半，并且将窗口大小设为初值1，然后重新进入慢启动过程。
  - 快速重传：在遇到3次重复确认应答（高速重发控制）时，便对它进行立即重传。
  - 快恢复：快速重传后执行快恢复。慢启动阈值设置为拥塞窗口的一半，拥塞窗口等于慢启动阈值后进入拥塞避免，拥塞窗口每次加法加一。

## TCP三次握手和四次挥手

![img](https://uploadfiles.nowcoder.com/images/20190313/311436_1552471554293_3A87D0457A6EE404083BBF3CB192C358)

- 三次握手

  - 服务器首先要处于监听状态
  - 客户端将请求连接数据包的SYN置1，seq置为随机值n，并将该数据包发送给服务器
  - 服务器收到这个连接请求后，如果同意建立连接就返回一个确认报文，SYN置1，ACK置1，seq置随机数m,确认号ack置n+1
  - 客户端收到服务器返回的确认报文后，还需要向服务器发送一个确认报文，ACK置1，seq置n+1，确认号ack置m+1

- 三次握手原因

  - 第三次握手是为了防止失效的连接请求到达服务器，让服务器错误打开连接。
  - 客户端发送的连接请求如果在网络中滞留，那么就会隔很长一段时间才能收到服务器端发回的连接确认。客户端等待一个超时重传时间之后，就会重新请求连接。但是这个滞留的连接请求最后还是会到达服务器，如果不进行三次握手，那么服务器就会打开两个连接。如果有第三次握手，客户端会忽略服务器之后发送的对滞留连接请求的连接确认，不进行第三次握手，因此就不会再次打开连接。

- 四次挥手

  - A 发送连接释放报文，FIN=1。
  - B 收到之后发出确认，此时 TCP 属于半关闭状态，B 能向 A 发送数据但是 A 不能向 B 发送数据。
  - 当 B 不再需要连接时，发送连接释放报文，FIN=1。
  - A 收到后发出确认，进入 TIME-WAIT 状态，等待 2 MSL（最大报文存活时间）后释放连接。
  - B 收到 A 的确认后释放连接。

- **四次挥手的原因**

  - 客户端发送了 FIN 连接释放报文之后，服务器收到了这个报文，就进入了 CLOSE-WAIT 状态。这个状态是为了让服务器端发送还未传送完毕的数据，传送完毕之后，服务器会发送 FIN 连接释放报文。

- **TIME_WAIT**

  客户端接收到服务器端的 FIN 报文后进入此状态，此时并不是直接进入 CLOSED 状态，还需要等待一个时间计时器设置的时间 2MSL。这么做有两个理由：

  - 确保最后一个确认报文能够到达。如果 B 没收到 A 发送来的确认报文，那么就会重新发送连接释放请求报文，A 等待一段时间就是为了处理这种情况的发生
  - 等待一段时间是为了让本连接持续时间内所产生的所有报文都从网络中消失，使得下一个新的连接不会出现旧的连接请求报文

## TCP粘包问题

- TCP是一个基于字节流的传输服务，"流"意味着TCP所传输的数据是没有边界的。这不同于UDP提供基于消息的传输服务，其传输的数据是有边界的。TCP的发送方无法保证对等方每次接收到的是一个完整的数据包。

- 产生粘包问题的原因有以下几个：

  - 第一 。应用层调用write方法，将应用层的缓冲区中的数据拷贝到套接字的发送缓冲区。而发送缓冲区有一个SO_SNDBUF的限制，如果应用层的缓冲区数据大小大于套接字发送缓冲区的大小，则数据需要进行多次的发送。
  - 第二种情况是，TCP所传输的报文段有MSS的限制，如果套接字缓冲区的大小大于MSS，也会导致消息的分割发送。
  - 第三种情况由于链路层最大发送单元MTU，在IP层会进行数据的分片。

  这些情况都会导致一个完整的应用层数据被分割成多次发送，导致接收对等方不是按完整数据包的方式来接收数据。

- **粘包的问题的解决思路**

  - 粘包问题的最本质原因在与接收对等方无法分辨消息与消息之间的边界在哪。我们通过使用某种方案给出边界，例如：
    - 发送定长包。如果每个消息的大小都是一样的，那么在接收对等方只要累计接收数据，直到数据等于一个定长的数值就将它作为一个消息。
    - 包尾加上\r\n标记。FTP协议正是这么做的。但问题在于如果数据正文中也含有\r\n，则会误判为消息的边界。
    - 包头加上包体长度。包头是定长的4个字节，说明了包体的长度。接收对等方先接收包体长度，依据包体长度来接收包体。
    - 使用更加复杂的应用层协议。

## UDP实现可靠传输

- 自己设计数据包的格式和通信流程
  - 可以在每一个数据包里面插入一个唯一的id，可以是timestamp或者递增的int
  - 在进行udp通信的时候，发送方记录id，接收方接收到之后也要发送id作为回应
  - 如果发送方收到回应，则发送下一个，如果没收到，就重传
- 我记得github上有一个开源的KCP协议，可以实现UDP的可靠传输

# 计算机网络HTTP

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

### 令牌token

某个客户端议程登陆了服务器，服务器通过

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


在通信过程中，**只会有一个 TCP 连接存在，它承载了任意数量的双向数据流**（Stream）。

- 一个数据流（Stream）都有一个唯一标识符和可选的优先级信息，用于承载双向信息。
- 消息（Message）是与逻辑请求或响应对应的完整的一系列帧。
- 帧（Frame）是最小的通信单位，来自不同数据流的帧可以交错发送，然后再根据每个帧头的数据流标识符重新组装。


### 多路复用

就是一个TCP连接中可以存在多条流，也就是可以发送多个请求，对端可以通过帧中的标识知道属于哪个请求。

### 服务端推送

HTTP/2.0 在客户端请求一个资源时，会把相关的资源一起发送给客户端，客户端就不需要再次发起请求了。

例如客户端请求 page.html 页面，服务端就把 script.js 和 style.css 等与之相关的资源一起发给客户端。


### 首部压缩

HTTP/1.1 的首部带有大量信息，而且每次都要重复发送。

HTTP/2.0 要求客户端和服务器同时维护和更新一个包含之前见过的首部字段表，从而避免了重复传输。

不仅如此，HTTP/2.0 也使用 Huffman 编码对首部字段进行压缩。


## HTTPS

HTTP有以下安全性问题：

- 使用明文进行通信，内容可能被窃听；
- 不验证通信方的身份，通信方的身份有可能遭遇伪装；
- 无法证明报文的完整性，报文有可能遭篡改

HTTPS 并不是新协议，而是让 `HTTP `先和 `SSL（Secure Sockets Layer）`通信，再由 SSL 和 TCP 通信，也就是说 HTTPS 使用了隧道进行通信。

通过使用 SSL，HTTPS 具有了**加密（防窃听）**、**认证（防伪装）**和**完整性保护（防篡改）**。


### 加密

#### 对称密钥加密

对称密钥加密（Symmetric-Key Encryption），加密和解密使用同一密钥。

- 优点：运算速度快；
- 缺点：无法安全地将密钥传输给通信方。


#### 非对称密钥加密

非对称密钥加密，又称公开密钥加密（Public-Key Encryption），加密和解密使用不同的密钥。

公开密钥所有人都可以获得，通信发送方获得接收方的公开密钥之后，就可以使用公开密钥进行加密，接收方收到通信内容后使用私有密钥解密。

非对称密钥除了用来加密，还可以用来进行签名。因为私有密钥无法被其他人获取，因此通信发送方使用其私有密钥进行签名，通信接收方使用发送方的公开密钥对签名进行解密，就能判断这个签名是否正确。

- 优点：可以更安全地将公开密钥传输给通信发送方；
- 缺点：运算速度慢。


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

# 计算机网络常见面试题

## MTU和MSS

- `MTU: maximum transmission unit`，最大传输单元，由硬件规定，即物理接口（数据链路层）提供给上层最大一次传输数据的大小，比如以太网的MTU为1500字节
- `MSS: maximum segment size`，最大分节大小，为TCP数据包每次传输的最大数据分段大小，一般由发送端向对端TCP通知对端在每个分节中能发送的TCP数据。一般`MSS=MTU-IPv4Header(20Bytes)-TCPHeader(20Bytes)`

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

# 数据库索引

## 索引概念

- 是什么：索引（Index）是帮助MySQL高效获取数据的数据结构。

- 作用：DB在执行一条Sql语句的时候，默认的方式是根据搜索条件进行全表扫描，遇到匹配条件的就加入搜索结果集合。如果我们对某一字段增加索引，查询时就会先去索引列表中一次定位到特定值的行数，大大减少遍历匹配的行数，所以能明显增加查询的速度。

- 优点：

  通过创建唯一性索引，可以保证数据库表中每一行数据的唯一性。

  可以大大加快数据的检索速度，这也是创建索引的最主要的原因。

  可以加速表和表之间的连接，特别是在实现数据的参考完整性方面特别有意义。

  在使用分组和排序子句进行数据检索时，同样可以显著减少查询中分组和排序的时间。

  通过使用索引，可以在查询的过程中，使用优化隐藏器，提高系统的性能。

- 缺点：

  创建索引和维护索引要耗费时间，这种时间随着数据量的增加而增加。

  索引需要占物理空间，除了数据表占数据空间之外，每一个索引还要占一定的物理空间，如果要建立聚簇索引，那么需要的空间就会更大。

  当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，这样就降低了数据的维护速度。

- 分类：

  - 聚集索引（主键索引）：在数据库里面，所有行数都会按照主键索引进行排序。
  - 非聚集索引：就是给普通字段加上索引。
  - 联合索引：就是好几个字段组成的索引，称为联合索引。遵循左前缀原则。
    - 通过字段顺序依次排序
    - 最左前缀法则：

## 索引不适合的场景

> ### 情景①
>
> - ***\*数据唯一性差（一个字段的取值只有几种时）的字段\****不要使用索引
> - 比如性别，只有两种可能数据。意味着索引的二叉树级别少，多是平级。这样的二叉树查找无异于全表扫描

> ### 情景②
>
> - **频繁更新的字段不要使用索引**
> - 比如logincount登录次数，频繁变化导致索引也频繁变化，增大数据库工作量，降低效率

> ### 情景③
>
> - ***\*字段不在where语句出现时不要添加索引，\****只有在where语句出现，mysql才会去使用索引
> - 如果where***\*后含IS NULL /IS NOT NULL/ like ‘%输入符%’等条件\****，不建议使用索引

> ### 情景④
>
> -  **where子句里对索引列使用不等于（<>）**，使用索引效果一般

## 索引失效的场景

### 条件中带有or

- or语句两侧只有一个使用了索引，那么索引就会失效
- 只有当or语句前后都有索引，那么才能根据索引进行查询

### 复合(组合)索引没有完全使用

- 对于复合索引（组合索引），***\*如果没有使用左边的列，那么组合索引也失效\****

### 在索引字段上使用not、<>、!=

- **不等于操作符是永远不会用到索引的，**因此对它的处理只会产生全表扫描
- 优化方法： key<>0 改为 key>0 or key<0

### 对索引字段进行计算操作

- 如果where语句中对字段***\*进行了计算操作，那么也不会使用索引\****

## B-Tree

- 满足以下条件
  - 出度d大于一的整数，一个节点有几个子节点
  - h为正整数为树的高度
  - 每个非叶子节点由n-1个key和n个指针组成，其中d<=n<=2d（指针和key互相间隔，节点两端是指针）
  - 每个叶子节点最少包含一个key和两个指针，最多包含2d-1个key和2d个指针，叶子节点的指针均为null
  - 所有叶子节点具有相同的深度，等于树高h
  - 一个节点中的key从左到右非递增排列
  - 每个指针要么指向另外一个节点，要么指向null

![img](http://blog.codinglabs.org/uploads/pictures/theory-of-mysql-index/2.png)

- 查找：由于B-Tree的特性，在B-Tree中按key检索数据的算法非常直观：首先从根节点进行二分查找，如果找到则返回对应节点的data，否则对相应区间的指针指向的节点递归进行查找，直到找到节点或找到null指针，前者查找成功，后者查找失败。B-Tree上查找算法的伪代码如下：

```c++
BTree_Search(node, key) {
    if(node == null) return null;
    foreach(node.key)
    {
        if(node.key[i] == key) return node.data[i];
            if(node.key[i] > key) return BTree_Search(point[i]->node);
    }
    return BTree_Search(point[i+1]->node);
}
data = BTree_Search(root, my_key);
```

- 复杂度：关于B-Tree有一系列有趣的性质，例如一个度为d的B-Tree，设其索引N个key，则其树高h的上限为logd((N+1)/2)logd((N+1)/2)，检索一个key，其查找节点个数的渐进复杂度为O(logdN)O(logdN)。从这点可以看出，B-Tree是一个非常有效率的索引数据结构。另外，由于插入删除新的数据记录会破坏B-Tree的性质，因此在插入删除时，需要对树进行一个分裂、合并、转移等操作以保持B-Tree性质

## B+Tree

- 相比于B-Tree的区别
  - 每个节点的指针上限为2d而不是2d+1(少了最左侧的指针)
  - 内节点不再存储data，只存储key；叶子节点也不存储指针，只存放data
  - 基于上一条不同点，B+Tree中叶节点和内节点所分配的空间一般不同；而B-Tree由于每个节点存放的指针和key值上限是一致的，所以在实现B-Tree往往对每个节点申请同等大小的空间。
  - B+Tree叶子节点带有顺序访问指针，提高区间访问的能力；如果需要查询跨越连续叶子节点范围内的数据，不必再次从根节点出发，而是直接通过顺序访问指针遍历

![](http://blog.codinglabs.org/uploads/pictures/theory-of-mysql-index/3.png)

## 索引为什么使用B-Tree（B+Tree）

- 一般来说，索引本身也很大，不可能全部存储在内存中，因此索引往往以索引文件的形式存储的磁盘上。这样的话，索引查找过程中就要产生磁盘I/O消耗，相对于内存存取，I/O存取的消耗要高几个数量级，所以评价一个数据结构作为索引的优劣最重要的指标就是在查找过程中磁盘I/O操作次数的渐进复杂度。换句话说，索引的结构组织要尽量减少查找过程中磁盘I/O的存取次数。
  - 主存存储：主存存储时间仅仅与存储次数呈现线性相关性
  - 外存存储：外存磁盘io时间消耗巨大，通常外存会进行预读（页的整数倍）
- B+/-树索引性能分析
  - 红黑树：h很深，逻辑上很近的父子兄弟节点在物理上可能很远，无法利用局部性，所以红黑树的渐进复杂度也为O(h)，但是h要大很多。
  - hash：查询单个数据很快，但是不常用主要是因为不支持范围查找
  - B-Tree：
    1. 检索一次最多需要访问h（高度）个节点，将一个节点大小设置为一个页可以让每个节点只需要一次io就可以完全读入,所以在新建节点的时候直接申请一个页的空间。
    2. B-Tree中一次检索最多需要h-1次io（根节点常驻内存），渐进复杂度为O（h）=O(logdN)。实际应用中，出度d是一个非常大的数字，通常超过100，所以h一般很小，不超过3。一亿个，四层
  - B+Tree:
    1. 比B-Tree更适合外存索引，原因和出度d有关，d越大那么h越小，那么渐进复杂度就越小。而出度取决于节点内key和data的大小：dmax=floor(pagesize/(keysize+datasize+pointsize)) 最大出度=页大小/（键值大小+数据大小+指针大小）
    2. 因为B+Tree中内节点只有key和指针两项，所以空间大小下就可以比B-数拥有更多的出度，拥有更好的性能；
    3. B+Tree更支持范围查找，叶子节点键具有相连的双向指针。
- 问题
  - 既然磁盘io的时间远大于在内存中查找的时间，为什么不直接把所有数据一次读入内存。浪费空间
  - 根节点16kb  16384  ，一个key值用big int（8字节）一个指针（6字节），根节点出度16kb/14b=1170，那么叶子节点可以放两千万个1k数据 
  - 索引结构存储位置，根目录下的data文件
    - MYISAM：1.frame字段框架,2.data数据,3.index索引三个文件
    - InnoDB：1.frame，2ibd数据和索引
  - 存储引擎是形容数据库表的
  - 

## MySql索引实现

##### MylSAM索引实现（非聚集索引）

- MyISAM引擎使用B+Tree作为索引结构，叶节点的data域存放的是数据记录的地址。下图是MyISAM索引的原理图：

![img](http://blog.codinglabs.org/uploads/pictures/theory-of-mysql-index/8.png)

- 在MyISAM中，主索引和辅助索引（Secondary key）在结构上没有任何区别，只是主索引要求key是唯一的，而辅助索引的key可以重复。如果我们在Col2上建立一个辅助索引，则此索引的结构如下图所示：

![img](http://blog.codinglabs.org/uploads/pictures/theory-of-mysql-index/9.png)

##### InnoDB索引实现（聚集索引，叶子节点包括所有的数据）

- 和MylSAM不同，叶子节点本身不再是数据的地址而是数据本身，节点中的key就是数据表的主键。

![img](http://blog.codinglabs.org/uploads/pictures/theory-of-mysql-index/10.png)

- 这种索引叫做聚集索引。因为InnoDB的数据文件本身要按主键聚集，所以InnoDB要求表必须有主键（MyISAM可以没有），如果没有显式指定，则MySQL系统会自动选择一个可以唯一标识数据记录的列作为主键，如果不存在这种列，则MySQL自动为InnoDB表生成一个隐含字段作为主键，这个字段长度为6个字节，类型为长整形。
- 第二点和MylSAM不同的是辅助索引叶子节点记录的不再是data的地址而是记录的主键key。也就是说InnoDB通过主键搜索十分高效，但是通过辅助索引却需要检索两次索引：找到主键，再通过主键找到相应的数据块。这就是为什么InnoDB为什么不建议使用过长的字段作为主键的原因，过长的主键会导致辅助索引变得很大。

## InnoDB的主键选择与插入优化

- 在使用InnoDB存储引擎时，如果没有特别的需要，请永远使用一个与业务无关的自增字段作为主键

- 如果不设置主键，mysql会自己设置主键或者添加一列隐藏主键列

- 从数据库索引优化角度看，使用InnoDB引擎而不使用自增主键绝对是一个糟糕的主意（比如UUID 的目的是让[分布式系统](https://baike.baidu.com/item/分布式系统)中的所有元素，都能有唯一的辨识资讯）

  - 每一个叶子节点都有固定的大小，如果存储的data将要溢出，那么Mysql就会开辟一个新的节点来存储。

    1. 如果用自增主键，那么每次插入新的记录，记录就会顺序添加到后续的位置，形成一个紧凑的索引结构，每次插入不用移动现有数据不会增加很多维护索引的开销。

       ![img](http://blog.codinglabs.org/uploads/pictures/theory-of-mysql-index/13.png)

    2. 如果使用非自增主键，那么每次插入新的记录都是近乎于随机，那么就会破坏已有的索引结构需要Mysql频繁移动记录位置，造成很多碎片。后续不得不通过OPTIMIZE TABLE来重建表并优化填充页面

       ![img](http://blog.codinglabs.org/uploads/pictures/theory-of-mysql-index/14.png)

- 自增主键和uuid优缺点对比

  - 自增主键：
    - 优点：
      1. 数据库自动编号，速度快，而且是增量增长，按顺序存放，对于检索非常有利；
      2. 数字型，占用空间小，易排序，在程序中传递也方便；
      3. 如果通过非系统增加记录时，可以不用指定该字段，不用担心主键重复问题。
    - 缺点
      1. 当需要手动导入数据或者与其他或者新旧系统双向同步的时候会导致大范围冲突。
  - uuid
    - 优点：
      1. 出现数据拆分、合并存储的时候，能达到全局的唯一性
    - 缺点：
      1. 影响插入速度， 并且造成硬盘使用率低
      2. 作为索引键值，uuid（字符串）之间比较大小相对数字慢不少， 影响查询速度。
      3. uuid占空间大， 如果你建的索引越多， 影响越严重

# Mysql原理

## 事务

### 概念

事务指的是满足 `ACID` 特性的一组操作，可以通过 `Commit` 提交一个事务，也可以使用 `Rollback` 进行回滚。

![image-20200816164909513](C:\Users\free\AppData\Roaming\Typora\typora-user-images\image-20200816164909513.png)

### ACID

#### 原子性（Atomicity）

事务被视为不可分割的最小单元，事务的所有操作要么全部提交成功，要么全部失败回滚。

回滚可以用回滚日志（`Undo Log`）来实现，回滚日志记录着事务所执行的修改操作，在回滚时反向执行这些修改操作即可。

#### 一致性（Consistency）

数据库在事务执行前后都保持一致性（不是一样）状态。在一致性状态下，所有事务对同一个数据的读取结果都是相同的。（指一个系统从一个正确的状态，转移到另一个正确的状态，可以说C是目的，AID是手段）

#### 隔离性（Isolation）

一个事务所做的修改在最终提交以前，对其它事务是不可见的。

#### 持久性（Durability）

一旦事务提交，则其所做的修改将会永远保存到数据库中。即使系统发生崩溃，事务执行的结果也不能丢失。

系统发生奔溃可以用重做日志（`Redo Log`）进行恢复，从而实现持久性。与回滚日志记录数据的逻辑修改不同，重做日志记录的是数据页的物理修改。

#### ACID的关系

- 只有满足一致性，事务的执行结果才是正确的。
- 在无并发的情况下，事务串行执行，隔离性一定能够满足。此时只要能满足原子性，就一定能满足一致性。
- 在并发的情况下，多个事务并行执行，事务不仅要满足原子性，还需要满足隔离性，才能满足一致性。
- 事务满足持久化是为了能应对系统崩溃的情况。

![image-20200816164951714](C:\Users\free\AppData\Roaming\Typora\typora-user-images\image-20200816164951714.png)

### AUTOCOMMIT

MySQL 默认采用自动提交模式。也就是说，如果不显式使用`START TRANSACTION`语句来开始一个事务，那么每个查询操作都会被当做一个事务并自动提交。

## 分布式事务

分布式事务就是指事务的参与者、支持事务的服务器、资源服务器以及事务管理器分别位于不同的**分布式系统的不同节点**之上。

简单的说就是一次大的操作由不同的小操作组成，这些小的操作分布在不同的服务器上，且属于不同的应用，分布式事务需要保证这些小操作要么全部成功，要么全部失败。本质上来说，分布式事务就是为了保证不同数据库的数据一致性。

### CAP原则

CAP原则又称为CAP定理，指的是在一个分布式系统中，web服务无法同时满足以下3个特性：

- 一致性（Consistency）：在分布式系统中数据一更新，所有数据变动都是同步的
- 可用性（Availability）：好的响应性能，每个操作都必须有预期的响应结束
- 分区容错性（Partition Tolerance）：在网络分区的情况下，即使出现单个节点无法使用，系统依然正常对外提供服务。

**单机版的Redis中，牺牲的是A可用性，保证了C一致性和P分区容错性。因为宕掉的一台Master节点无法服务了。**

**Cluster功能的Redis牺牲的是C，保证了A可用性和C分区容错性。因为某个节点宕机，那么会有其他节点顶替上来，保证了A，但是无法保证C了。**

### BASE理论

BASE理论是对CAP中的一致性和可用性进行一个权衡的结果，理论的核心思想是：我们无法做到强一致，但每个应用都可以根据自身的业务特点，采用适当的方式来使系统达到最终一致性。

- 基本可用（Basically Available）
  - 指分布式系统在出现不可预知的故障的时候，允许损失部分可用性（不等价于系统不可用）
- 软状态（Soft State）
  - 和硬状态对应，是指允许系统中的数据存在中间状态，并认为该中间状态的存在不会影响系统的整体可用性，即允许系统的不同节点的数据副本之间进行数据同步的过程存在延时
- 最终一致性（Eventually Consistent）
  - 指的是系统所有的数据副本，经过一段时间的同步之后，最终能够达到一个一致的状态。因此，最终一致性的本质是需要系统保证最终数据能够达到一致，而不需要试试保证系统数据的强一致性。

### 两阶段提交（2PC）

两阶段提交需要有一个协调者，来协调两个操作之间的操作流程。当参与方很多时，逻辑会变得比较复杂。

参与方需要实现两阶段提交协议。

#### 阶段1

- 协调者向所有参与者发送事务内容，询问是否可以提交事务并等待答复
- 各参与者执行事务操作，将undo和redo信息记入日志文件中
- 如果参与者执行成功，给协调者返回yes，否则返回no

#### 阶段2

- 如果协调者收到参与者的失败消息或者超时，直接给每个参与者发送回滚rollback消息，否则发送提交commit消息。

可能遇到的情况：

- 当所有参与者都反馈yes就提交事务
- 当有一个参与者反馈no，回滚事务

#### 问题

- 性能问题：所有参与者在事务提交阶段处于同步阻塞状态，占用系统资源，容易导致性能瓶颈
- 可靠性问题：如果协调者存在单点故障问题，或者出现故障，提供者将一直处于锁定状态
- 数据一致性问题：在第二个阶段中，如果出现协调者和参与者都挂了的情况，有可能导致数据不一致。

#### 优点

- 尽量保证了数据的强一致，适合对数据强一致要求很高的关键领域。

#### 缺点

- 实现复杂，牺牲了可用性，对性能影响大，不适合高并发场景

### 三阶段提交（3PC）

二阶段提交的改进版本，要解决的是协调者和参与者同时挂的问题，3PC把2PC的准备阶段再次一分为二，变成三阶段提交。

#### 阶段1

- 协调者向所有参与者发出包含事务内容的canCommit请求，询问是否可以提交事务，并等待所有参与者答复。
- 参与者收到canCommit请求，如果认为可以执行事务操作，则反馈yes并进入预备状态，否则反馈no

#### 阶段2

- 如果所有参与者返回yes，协调者预执行事务
  - 协调者向所有参与者发出preCommit请求，进入准备阶段
  - 参与者收到preCommit请求后，执行事务操作，将undo和redo信息记入事务日志中（但不提交事务）
  - 各参与者向协调者反馈ack响应或者no响应，并等待最终指令
- 如果有参与者反馈no，或者等待超时后协调者无法收到所有参与者的反馈，即中断事务
  - 协调者向所有参与者发出abort请求
  - 无论收到协调者的abort请求，或者在等待协调者请求过程中出现超时，参与者都会中断事务

#### 阶段3

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

#### 优点：

- 相比二阶段提交，三阶段提交降低了阻塞范围，在等待超时后协调者或参与者会中断事务。避免了协调者单点问题。阶段3中协调者出现问题时，参与者会继续提交事务。

#### 缺点：

- 数据不一致的问题依然存在，当在参与者收到preCommit请求后等待doCommit指令时，此时如果协调者请求中断事务，而协调者无法与参与者正常通信，会导致参与者继续提交事务，造成数据不一致。

### 补偿事务（TCC）

TCC是服务化的二阶段编程模型，采用的补偿机制。核心思想是：针对每个操作，都要注册一个与其对应的确认和补偿操作。分为三个阶段：

- Try阶段主要是对业务系统做检测和资源预留
  - 完成所有业务检查（一致性）
  - 预留必须业务资源（准隔离性）
  - Try尝试执行业务
- Confirm阶段主要对业务系统做确认提交。
  - Try阶段执行成功并开始执行Confirm阶段时，默认Confirm阶段是不会出错的。即只要Try成功，Confirm一定成功。
- Cancel阶段主要是在业务执行错误，需要回滚的状态下执行的业务取消，预留资源释放。

#### 优点：

- 性能提升：具体业务来实现控制资源锁的粒度变小，不会锁定整个资源
- 数据最终一致性：基于Confirm和Cancel的幂等性，保证事务最终完成确认或者取消，保证数据的一致性
- 可靠性：解决了XA协议的协调者单点故障问题，由主业务发起并控制整个业务活动，业务活动管理器也变成多点，引入集群。

#### 缺点：

- TCC的三个阶段操作功能要按具体业务来实现，业务耦合度较高，提高了开发成本。

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

例如：T1 读取某个范围的数据，T2 在这个范围内**插入**新的数据，T1 再次读取这个范围的数据，此时读取的结果和和第一次读取的结果不同。

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

在封锁一节中提到，加锁能解决多个事务同时执行时出现的并发一致性问题。在实际场景中读操作往往多于写操作，因此又引入了读写锁来避免不必要的加锁操作，例如读和读没有互斥关系。**读写锁中读和写操作仍然是互斥的，而 MVCC 利用了多版本的思想，写操作更新最新的版本快照，而读操作去读旧版本快照，没有互斥关系**，这一点和` CopyOnWrite` 类似。

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

## 范式

- 第一范式就是属性不可分割，每个字段都应该是不可再拆分的。比如一个字段是姓名（NAME），在国内的话通常理解都是姓名是一个不可再拆分的单位，这时候就符合第一范式；但是在国外的话还要分为FIRST NAME和LAST NAME，这时候姓名这个字段就是还可以拆分为更小的单位的字段，就不符合第一范式了。
- 第二范式就是要求表中要有主键，表中其他其他字段都依赖于主键，因此第二范式只要记住主键约束就好了。比如说有一个表是学生表，学生表中有一个值唯一的字段学号，那么学生表中的其他所有字段都可以根据这个学号字段去获取，依赖主键的意思也就是相关的意思，因为学号的值是唯一的，因此就不会造成存储的信息对不上的问题，即学生001的姓名不会存到学生002那里去。
- 第三范式即非主属性不传递，只依赖于键码。比如说有一个表是学生表，学生表中有学号，姓名等字段，那如果要把他的系编号，系主任，系主任也存到这个学生表中，那就会造成数据大量的冗余，一是这些信息在系信息表中已存在，二是系中有1000个学生的话这些信息就要存1000遍。因此第三范式的做法是在学生表中增加一个系编号的字段（外键），与系信息表做关联。

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

# Mysql存储引擎

## 主要引擎

| **功能**     | **MyISAM** | **Memory** | **InnoDB** | **Archive** |
| ------------ | ---------- | ---------- | ---------- | ----------- |
| 存储限制     | 256TB      | RAM        | 64TB       | NO          |
| 支持事务     | NO         | NO         | YES        | NO          |
| 支持全文索引 | YES        | NO         | NO         | NO          |
| 支持数索引   | YES        | YES        | YES        | NO          |
| 支持哈希索引 | NO         | YES        | NO         | NO          |
| 支持数据缓存 | NO         | N/A        | YES        | NO          |
| 支持外键     | NO         | NO         | YES        | NO          |

- **InnoDB：**如果想要提供**提交、回滚、崩溃恢复能力**的事务安全（ACID兼容）能力，并实现并发控制
- **MyISAM：**如果数据表**主要用来插入和查询**记录，则此引擎**效率快**
- **Memory：**
  - **数据存放在内存中，数据量小，速度快**
  - 如果只是**临时存放数据，数据量不大**，并且**不需要较高的数据安全性**，可以选择将数据保存在内存中的Memory引擎，MySQL中使用该引擎作为临时表，存放查询的中间结果
- **Archive：**如果**只有insert和select**操作，可以选择此引擎。Archive**支持高并发的插入操作**，但**本身并不是事务安全**的。Archive非常适合**存储归档数据（有很好的压缩机制）**，如记录日志信息

## InnoDB与MyISAM的区别

### 存储结构

> - MyISAM引擎创建数据库，将产生3个文件，文件的名字以表的名字开始，扩展名指出文件类型：
>   - **frm**文件**存储表定义**
>   - **数据文件**的扩展名为**.MYD**（MYData）
>   - **索引文件**的扩展名为**.MYI**（MYIndex）

### 底层结构

- myisam的b+树叶子节点存储的时数据的地址，而innodb存储的时具体的数据

### 存储空间

> - **InnoDB：**需要更多的内存和存储，它会在主内存中建立其专用的缓冲池用 于高速缓冲数据和索引。
> - **MyISAM：**MyISAM索引和数据是分开的，并且索引是有压缩的，内存使用率就对应提高了不少。能加载更多索引，而 Innodb 是索引和数据是紧密捆绑的，没有使用压缩从而会造成 Innodb 比 MyISAM 体积庞大不小

### 事务处理

> - **MyISAM：**MyISAM类型的表强调的是性能，其执行数度比 InnoDB 类型更快，但是***\*不支持外键、不提供事务支持\****
> - **InnoDB：**InnoDB 提供事务***\*支持事务，外部键（foreign key）\****等高级数据库功能

### 表的具体行数

> - **MyISAM：**保存有表的总行数，如果 select count(*) from table;会***\*直接取出该值\****
> - **InnoDB：**没有保存表的总行数，如果使用 select count(*) from table； ***\*就会遍历整个表\****，消耗相当大，但是在加了 where 后，myisam 和 innodb 处 理的方式都一样

### 表锁差异

> - **MyISAM：*****\*只支持表级锁\****，用户在操作 myisam 表时， select，update，delete，insert 语句都会***\*给表自动加锁\****
> - **InnoDB：*****\*支持事务和行级锁，\****是 innodb 的最大特色。行锁大幅度***\*提高了多用户并发操作的新能\****。但是 InnoDB 的行锁也不是绝对的，如果在执行一个 SQL 语句时 MySQL 不能确定要扫描的范围，InnoDB 表同样会锁全表， 例如 update table set num=1 where name like “%aaa%”

## 选取适合的存储引擎

- 对于如何选择存储引擎，可以简单地归纳为一句话：***\*“除非需要用到某些InnoDB不具备的特性，并且没有其他办法可以替代，否则都应该优先选择InnoDB引擎”\****
- **如果应用需要不同的存储引擎，请先考虑以下几个因素：**
  - **事务：**如果应用需要**事务支持**，那么InnoDB（或者XtraDB）是目前最稳定并且经过验证的选择。如果不需要事务，并且主要是SELECT 和INSERT操作，那么MyISAM是不错的选择。一般日志型的应用比较符合这一特性
  - **备份：**备份的需求也会影响存储引擎的选择。如果可以定期地关闭服务器来执行备份，那么备份的因素可以忽略。反之，**如果需要在线热备份，那么选择InnoDB就是基本的要求**
  - **崩溃恢复：**数据量比较大的时候，系统崩溃后如何快速地恢复是一个需要考虑的问题。相对而言，**MyISAM崩溃后发生损坏的概率比InnoDB要高很多，而且恢复速度也要慢**。因此，**即使不需要事务支持，很多人也选择InnoDB引擎，**这是一个非常重要的因素
  - **特有的特性：**最后，有些应用可能依赖一些存储引擎所独有的特性或者优化，比如很多应用依赖聚簇索引的优化。另外，MySQL中也只有 MyISAM支持地理空间搜索。如果一个存储引擎拥有一些关键的特 性，同时却又缺乏一些必要的特性，那么有时候不得不做折中的考 虑，或者在架构设计上做一些取舍。某些存储引擎无法直接支持的 特性，有时候通过变通也可以满足需求

# MySQL查询优化与主从复制

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

使用EXPLAIN可以分析select查询语句的性能，可以通过分析EXPLAIN结果来优化查询语句。

其中EXPLAIN结果中比较重要的字段有：

- `select_type:`查询类型，有简单查询、联合查询、自查询等
- `key`：使用的索引
- `rows`：扫描的行数

## 优化数据访问

#### 减少请求的数据量

- 只返回必要的列：最好不要使用select *语句
  - 会查询不需要的列或者大文本的字段，增加数据库解析的工作量，网络开销和磁盘io操作
  - 失去了覆盖索引优化的可能性，比如某一个查询只需要通过辅助索引就可以得到结果，但是用了select*就必须要再去通过聚簇索引去查询所有列，多了一次b+树查询。
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

## Redis概述

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

## Redis连接方式

### unix scoket（本机通信）

- 基于网络协议栈的，是网络中不同主机之间的通讯，需要明确IP和端口。 

### tcp（网络通信）

- 同一台主机内不同应用不同进程间的通讯，不需要基于网络协议，不需要打包拆包、计算校验和、维护序号和应
  答等，只是将应用层数据从一个进程拷贝到另一个进程，主要是基于文件系统的，它可以用于同一台主机上两个
  没有亲缘关系的进程，并且是全双工的，提供可靠消息传递（消息不丢失、不重复、不错乱）的IPC机制，效率
  会远高于tcp短连接。与Internet domain socket类似，需要知道是基于哪一个文件（相同的文件路径）来通
  信的。
  unix domain socket有2种工作模式一种是SOCK_STREAM，类似于TCP，可靠的字节流。另一种是SOCK_DGRAM，
  类似于UDP，不可靠的字节流。 

如果您想尽快回答并且您的负载低于redis-server峰值性能,那么避免流水线操作可能是最佳选择.但是,如果您希望能够处理更高的吞吐量,那么您可以处理请求的管道.响应可能需要更长时间,但您可以在某些硬件上处理更多请求.

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

![image-20200813105440586](C:/Users/free/Desktop/面试/interviewmd/assets/post/others/rax.png)



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

![image-20200804153945342](C:/Users/free/Desktop/面试/interviewmd/assets/post/others/embstr+raw.png)

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

## Redis 数据类型操作

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

## Redis数据库设计

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

### RDB持久化

将某个时间点的所有数据（RDB文件）都存放在硬盘上。

可以将快照复制到其他服务器从而创建具有相同数据的服务器副本。

如果系统发生故障，将会丢失最后一次创建快照之后的数据。

如果数据量很大，保存快照的时间很长。

#### RDB使用命令

- SAVE命令：阻塞Redis服务器进程，直到RDB文件创建完毕为止，在服务器进程阻塞期间，服务器不处理任何命令请求。
- BGSAVE命令：派生一个子进程，由子进程负责创建RDB文件。`BGSAVE`和`SAVE、BGSAVE、BGREWRITEAOF`命令冲突。

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

### RDB和AOF对比

- 如果redis设置了AOF持久化那么就会优先使用AOF来还原数据库状态，没有设置AOF才会采用RDB载入RDB文件
- Redis加载RDB恢复数据远远快于AOF的方式
  - RDB是一个紧凑压缩的-`+-二进制文件，适用于备份，每一段时间拷贝到远程文件系统中，用于灾难恢复
- RDB无法做到秒级的持久化，因为bgsave每次运行都要执行fork操作创建子进程，属于重量级的操作，执行成本很高。AOF可以设置成每一秒记录到缓冲区，在从缓冲区型硬盘同步。
- AOF需要定期对AOF文件进行重写，达到压缩的目的。

## Redis事件

Redis服务器是一个事件驱动程序。

### 文件事件

服务器通过套接字和客户端（或者其他服务器）进行通信，文件事件就是对套接字操作的抽象。

Redis通过Reactor模式开发了自己的**网络事件处理器**，被称为文件事件处理器。

- 使用I/O多路复用程序同时监听多个套接字，并将到达的事件传送给文件事件分派器，分派器会根据套接字产生的事件类型调用响应的事件处理器。
- 当被监听的套接字准备好执行连接应答（accept），读取（read），写入（write），关闭（close）等操作时，与操作相对应的文件事件就会产生，这时文件事件处理器就会调用套接字之前关联好的事件处理器来处理这些事件。

![img](C:/Users/free/Desktop/面试/interviewmd/assets/post/others/fileevent.png)

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

![image-20200802120117282](C:/Users/free/Desktop/面试/interviewmd/assets/post/others/chijiuhua.png)

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
- 从服务器每次收到主服务器传播来的N个字节的数据时，九江自己的复制偏移量的值加N

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

![image-20200803142244142](C:/Users/free/Desktop/面试/interviewmd/assets/post/others/psync.png)

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

![image-20200803145247549](C:/Users/free/Desktop/面试/interviewmd/assets/post/others/copyping.png)

- 身份验证
  - 如果从服务器设置了`masterauth`选项，就进行身份验证；反之不进行身份验证
  - 在需要进行身份验证的情况下，从服务器向主服务器发送一条AUTH命令，命令的参数为从服务器`masterauth`选项的值

![image-20200803145722958](C:/Users/free/Desktop/面试/interviewmd/assets/post/others/copyauth.png)

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

![image-20200803153109982](C:/Users/free/Desktop/面试/interviewmd/assets/post/others/sentinelfunc.png)

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

### （开始）接收来自主从服务器的频道信息

**当Sentinel与一个主服务器或者从服务器建立起订阅连接之后，Sentinel就会通过订阅连接**，向服务器发送以下命令：

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

每个Sentinel节点会每隔1秒对主节点、从节点、其他Sentinel节点发送ping命令做心跳检测，当这些节点超过down-after-milliseconds没有进行有效回复，Sentinel节点就会对该节点做失败判定，这个行为叫做主观下线

### 检查客观下线状态

当Sentinel将一个主服务器判断为主观下线后，为了确认这个主服务器是否真的下线，他会同样监视这个主服务器的其他Sentinel进行询问，当Sentinel从其他Sentinel那里接收到足够数量（启动时设置的参数）的已下线判断之后，Sentinel就会判定为客观下线，并进行故障转移操作。

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

## Redis集群

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

## Redis高频问题

### Redis常见性能问题和解决方案

1.Master写内存快照，save命令调度rdbSave函数，会阻塞主线程的工作，当快照比较大时对性能影响是非常大的，会间断性暂停服务，所以Master最好不要写内存快照。

2.Master AOF持久化，如果不重写AOF文件，这个持久化方式对性能的影响是最小的，但是AOF文件会不断增大，AOF文件过大会影响Master重启的恢复速度。Master最好不要做任何持久化工作，包括内存快照和AOF日志文件，特别是不要启用内存快照做持久化,如果数据比较关键，某个Slave开启AOF备份数据，策略为每秒同步一次。

3.Master调用BGREWRITEAOF重写AOF文件，AOF在重写的时候会占大量的CPU和内存资源，导致服务load过高，出现短暂服务暂停现象。

4.Redis主从复制的性能问题，为了主从复制的速度和连接的稳定性，Slave和Master最好在同一个局域网内。

### 海量redis数据

**假如Redis里面有1亿个key，其中有10w个key是以某个固定的已知的前缀开头的，如果将它们全部找出来？**

使用keys指令可以扫出指定模式的key列表。

**对方接着追问：如果这个redis正在给线上的业务提供服务，那使用keys指令会有什么问题？**

这个时候你要回答redis关键的一个特性：redis的单线程的。keys指令会导致线程阻塞一段时间，线上服务会停顿，直到指令执行完毕，服务才能恢复。这个时候可以使用scan指令，scan指令可以无阻塞的提取出指定模式的key列表，但是会有一定的重复概率，在客户端做一次去重就可以了，但是整体所花费的时间会比直接用keys指令长。

### 使用Redis做异步队列

一般使用list结构作为队列，rpush生产消息，lpop消费消息。当lpop没有消息的时候，要适当sleep一会再重试。

**如果对方追问可不可以不用sleep呢？**list还有个指令叫blpop，在没有消息的时候，它会阻塞住直到消息到来。

**如果对方追问能不能生产一次消费多次呢？**使用pub/sub主题订阅者模式，可以实现1:N的消息队列。

**如果对方追问pub/sub有什么缺点？**在消费者下线的情况下，生产的消息会丢失，得使用专业的消息队列如rabbitmq等。

**如果对方追问redis如何实现延时队列？**我估计现在你很想把面试官一棒打死如果你手上有一根棒球棍的话，怎么问的这么详细。但是你很克制，然后神态自若的回答道：使用sortedset，拿时间戳作为score，消息内容作为key调用zadd来生产消息，消费者用zrangebyscore指令获取N秒之前的数据轮询进行处理。

到这里，面试官暗地里已经对你竖起了大拇指。但是他不知道的是此刻你却竖起了中指，在椅子背后。

### 如果有大量的key需要设置同一时间过期，一般需要注意什么？

如果大量的key过期时间设置的过于集中，到过期的那个时间点，redis可能会出现短暂的卡顿现象。一般需要在时间上加一个随机值，使得过期时间分散一些。

### Redis如何做持久化的（bgsave原理）

bgsave做镜像全量持久化，aof做增量持久化。因为bgsave会耗费较长时间，不够实时，在停机的时候会导致大量丢失数据，所以需要aof来配合使用。在redis实例重启时，会使用bgsave持久化文件重新构建内存，再使用aof重放近期的操作指令来实现完整恢复重启之前的状态。

对方追问那如果突然机器掉电会怎样？取决于aof日志sync属性的配置，如果不要求性能，在每条写指令时都sync一下磁盘，就不会丢失数据。但是在高性能的要求下每次都sync是不现实的，一般都使用定时sync，比如1s1次，这个时候最多就会丢失1s的数据。

对方追问bgsave的原理是什么？你给出两个词汇就可以了，fork和cow。fork是指redis通过创建子进程来进行bgsave操作，cow指的是copy on write，子进程创建后，父子进程共享数据段，父进程继续提供读写服务，写脏的页面数据会逐渐和子进程分离开来。

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

## 一条sql执行很慢的原因

- 当MySQL执行的任务比较多的时候，例如IO操作、脏页较多等，那么可能导致MySQL的这些后台线程执行缓慢
- 当缓冲池、重做日志缓冲不够用时，或者存储空间变小时，也会是导致MySQL执行慢的原因
- MySQL对数据的访问实惠加锁的，因此当你的SQL语句设计到一张表时，如果这条数据被别的事务加锁了，那你就就必须等到锁的释放，因此这也可能是一种因素
- 没有加索引或者mysql没有使用你的索引  
  - 下面我们想要查询id为999的学生信息，但是由于=运算符的左边是999了，而不是id了，因此索引不会被使用到
  - **select** * **from** student **where** **id** - 1 = 1000;

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

![image-20200730172148589](C:/Users/free/Desktop/面试/interviewmd/assets/post/others/docker.png)

## 底层原理

### Docker是怎么工作的？

Docker 是一个Client-Server结构的系统，Docker的守护进程运行在主机上。通过Socket从客户端访问。

Docker相对于VM有更少的抽象层。他利用的是宿主机的内核，VM需要guest OS

![image-20200730104701884](C:/Users/free/Desktop/面试/interviewmd/assets/post/others/vmdocker.png)

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



# 海量数据应用场景

## 找共同url

- **有10个文件，每个文件1G， 每个文件的每一行都存放的是用户的query，每个文件的query都可能重复。要你按照query的频度排序** 

  1.所有数据有100亿条数据，大小一共为640G，可以考虑把这些文件分成一个个小文件来处理。

  2.用hash对每一个url求取他的哈希值，然后存放到对应的小文件里，这样的话a和b重复的url只会出现在哈希值相同的两个文件里

  3.在分别对哈希值相同的两个小文件求相同url，可以将一个文件中的url全部用bitmap存储，然后再遍历另一个，如果bitmap中存在就存放到文件里，如果不存在就跳过。继续遍历。

## 按照query的出现频率排序

- **有一个1G大小的一个文件，里面每一行是一个词，词的大小不超过16个字节，内存限制大小是1M。返回频数最高的100个词**

  1. 顺序读取每一个文件，对于每一个词语对5000取模，分成5000分小文件，平均每一份文件200k左右。
  2. 然后对每一个小文件都用hash_map统计每个文件中出现的词以及相应的频率
  3. 从每一个文件里面取出出现最多的100个词语在分别存入5000个文件里面，然后对这5000个文件中出现的词进行归并排序。得到出现频率最大的100个词语。

  - hash_map存储query和出现的次数，维护一个存放100个query的数组，一次遍历就可以了

##  统计用户在线分布情况

- 有一个论坛，其注册ID有两亿个，每个ID登陆和退出都会向日志中写入登陆和退出的时间

- **现要求：**写一个算法统计一天中论坛的用户在线分布(取样粒度为秒)，也就是在指定秒

  - **第一步：**一天总共有 3600*24=86400 秒

  - **第二步：**定义一个长度为 86400 的整数数组int delta[86400]，每个索引对应这一秒的人数变化值，也就是该秒登陆人数与退出人数的差值（因此可能为正也可能为负）

  - **第三步：**

    开始时将数组元素都初始化为 0，然后依次读入每个用户的登录时间和退出时间

    - 找到这个用户的登陆时间对应在一天中的秒数，然后将delta数组对应的索引处值加1
    - 找到这个用户的退出时间对应在一天中的秒数，然后将delta数组对应的索引处值减1

  - **第四步：**这样处理一遍后delta数组中存储了每秒中的人数变化情况（可为正可为负）

  - **第五步：**定义另外一个长度为 86400 的整数数组 int online_num[86400]，每个整数对应这一秒的论坛在线人数

  - **第六步：**

    假设一天开始时论坛在线人数为 0，则：

    - 第1秒的人数online_num[0] = delta[0]
    - 第2秒的人数online_num[1] = online_num[0] + delta[1]（如果delta[1]为负，则分布线下降(在线人数变少)；如果delta[1]为正，则分布线上升(在线人数变多)）
    - ......以此类推
    - 第n+1秒的人数online_num[n] = online_num[n-1] + delta[n]

  - 这样我们就获得了一天中任意时间的在线人数

## 找出被删除的数

- 现在有1到10W这10W个数字，现在我们从中去除两个数组，然后剩余的数字全部打乱
- **现问：**如何找出哪两个数从这10W个数字中被去除了
- 解法一：bitmap
  - 用bitmap，每一个bitmap表示一个数字是否出现过
  - 遍历一遍之后那几个位为0那么就是缺失的数字
- 解法二：用二元方程
  - 计算所有数字的和以及所有数字的和以及平方和
  - 计算去除后所有数字的和以及平方和
  - 列二元方程求解

## 判断数是否出现过

- 现在有40亿个不重复的unsigned int的整数，并且是没有排过序的。然后现在再随机给出几个数字
- **现问：**如何快速判断这几个数字是否在这40亿个数中？
- 解法一：
  - bitmap存储所有出现过的数
  - 找目标数对应的位
- 解法二：
  - 先根据第一位将所有数分成两部分，找到目标数所在的那个文件
  - 将找到地文件继续迭代二分，直到找到目标数为止

# 源码分析

## tinyxml

![img](https://pic002.cnblogs.com/images/2012/344510/2012110913043821.png)

### TiXmlBase（虚基类）

- readname：读取element中的tagname和属性值
- readtext：读取<></>中的文本
- parse：纯虚函数，解析xml

### TiXmlNode（所有子类的基类）

- 提供了遍历子节点、访问兄弟节点、插入和删除等等的基本操作。
- iteratechildren：遍历子节点
- insert（end）child：插入子节点
- removechild：移除节点
- getdocument：返回根节点
  - TiXmlDocument:文档中的根
  - TiXmlNode: node为文档中所有结点的父类型.其可以转化为其他的结点类型.
  - TiXmlElement: element结点.即我们平常所使用的,具有属性,tagName的结点.
  - TiXmlComment: 注释
  - TiXmlText: 文字结点.
  - TiXmlDeclaration: xml的声明(?xml version="1.0" standalone="yes"?>)
  - TiXmlUnknown: 任何tinyXml不认的结点都将归结为unknown,在重新写回文件时,按照原样输出.

### TiXmlDocument（根节点）

- loadfile：
- savefile：
- printfile：多叉树遍历的方式打印输出



### 有趣的、印象深刻的点

- 再TiXmlBase基类里面，对于TixmlNode等一些子类它申明了friend友元类，刚开始看的时候很困惑，因为tixmlnode是tixmlbase的子类，应该不需要用友元类。看到后面发现因为成员函数里面调用了子类的保护和私有类型的成员函数，所以需要友元
  - 这里使用friend让人比较困惑,因为这三个都是子类,应该不用才是.在查看了tinyxmlparser.cpp的679行,主要是因为在函数中调用了做为参数的TiXmlNode的streamIn函数,而该函数是protected的,所以需要friend.当然肯定不止这么一个函数,应该用到了很多protected内的方法和参数
- 在tixmlnode中的转化函数，他把所有的转化函数全部写成虚函数并且返回null，然后不同的派生类继承并且重写相应的转换方法。比如对于每一个派生类，都要重写转换成其他每一种派生类的方法，包括他自己和一些不能转换得类，不能转换的就直接返回null。

## cJosn

### JSON语法规则

- 数据在名称/值对中
- 数据由括号分隔
- 大括号保存对象
- 中括号保存数组

### JSON值

- 数字（整数或浮点数）

```javascript
{ "age":30 }
```

- 字符串（在双引号中）

```javascript
{"name" : “brook”}
```

- 逻辑值（true 或 false）

```javascript
{ “flag” : true }
```

- 对象（在大括号中）

```javascript
对象可以包含多个 key/value（键/值）对。
{ "name”:"brook" , “age”:24 }
```

- 数组（在中括号中)

```javascript
数组可包含多个对象：
{
"sites": [
{ "name":"菜鸟教程" , "url":"www.runoob.com" }, 
{ "name":"google" , "url":"www.google.com" }, 
{ "name":"微博" , "url":"www.weibo.com" }
]
}
在上面的例子中，对象 "sites" 是包含三个对象的数组。每个对象代表一条关于某个网站（name、url）的记录。
```

- null

```javascript
{ "runoob":null }
```

### cJosn结构体

```c++
/* The cJSON structure: */
typedef struct cJSON {
	struct cJSON *next,*prev;	/* next/prev allow you to walk array/object chains. Alternatively, use GetArraySize/GetArrayItem/GetObjectItem */
	struct cJSON *child;		/* An array or object item will have a child pointer pointing to a chain of the items in the array/object. */
 
	int type;					/* The type of the item, as above. */
 
	char *valuestring;			/* The item's string, if type==cJSON_String */
	int valueint;				/* The item's number, if type==cJSON_Number */
	double valuedouble;			/* The item's number, if type==cJSON_Number */
 
	char *string;				/* The item's name string, if this item is the child of, or is in the list of subitems of an object. */
} cJSON;
```

- cJSON数据结构为多级双向链表，逗号之间都是用前驱和后继指针连接的，然后上层和下层之间用孩子指针连接的

- 数据指针：包括指向双向链表前驱和后继的指针prev和next，指向孩子节点的child指针。
- 数据项类型：有七种数据项类型，False，True，NULL，数字，字符串，数组，对象。
- 数据：这里需要注意的是，当数据项类型为数字时，数据将在valueint和valuedouble中同时保存。
- 节点名称可能为空

![img](https://img-blog.csdnimg.cn/20181117164146412.PNG?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzE1MzkxODg5,size_16,color_FFFFFF,t_70)****

- 遇到大括号或中括号，如果没有形成键值对关系的，一般节点名称为空。
- 逗号分隔的为属于同一层的节点，存在前驱和后继的关系。
- 分级的层次关系通过child指针表示。

### 内存管理

- cJSON_Hooks：通过Hooks初始化函数可以看出，如果hooks没有自定义的内存分配和释放函数，默认使用malloc和free函数。

- 创建节点：先创建一个cJSON_New_Item对象item，然后再把item类型改为需要的类型，并给一些数据单元赋值
- 删除节点：cJSON_Delete
  - 递归删除子节点
  - 对于字符串节点要先释放字符串的内存
- 添加孩子节点
  - cJSON_AddItemToArray：数组添加孩子节点
  - cJSON_AddItemToObjec：对象添加孩子接待你

- 查找节点：因为是链表，所以采用了暴力遍历
  - cJSON_GetArrayItem
  - cJSON_GetObjectItem

### json序列化

JSON序列化主要就是为了传输方便，将要传输的对象序列化为二进制的数据流，效率极高。一般有两种输出：格式化输出，压缩输出

#### 整体序列化部分

- cJson_print():留给用户的接口 根据item->type类型来判断调用哪一个序列化函数

- print_value
- print_number
- print_string_ptr
- print_array
- print_object

#### 数据解析

- cJSON_Parse：留给用户的接口

### 附加函数说明

- cJSON_strcasecmp：比较字符串
- parse_hex4：将字符串转化为16进制hex格式

### 有趣的、印象深刻的点

- cJSON数据结构为多级双向链表

- 数据指针：包括指向双向链表前驱和后继的指针prev和next，指向孩子节点的child指针。
- 数据项类型：有七种数据项类型，False，True，NULL，数字，字符串，数组，对象。
- 数据：这里需要注意的是，当数据项类型为数字时，数据将在valueint和valuedouble中同时保存。
- 节点名称可能为空

```
{
"sites": [
{ "name":"菜鸟教程" , "url":"www.runoob.com" }, 
{ "name":"google" , "url":"www.google.com" }, 
{ "name":"微博" , "url":"www.weibo.com" }
]
}
```

## sort源码分析 

sort用了一种内省式的排序算法，把快排、堆排和插入排序的优点都集合起来

- 在数据量很大时采用正常的快速排序，此时效率为O(logN)。
- 一旦分段后的数据量小于某个阈值，就改用插入排序（insertion_sort），应该有一个最小分段阈值好像是16，因为此时这个分段是基本有序的，这时效率可达O(N)。
- 在快排递归的时候，如果递归层次太深超过最深的递归阈值的话，sort就使用堆排序来处理，在此情况下，使其效率维持在堆排序的O(N logN)，但这又比一开始使用堆排序好。
- 一般情况下堆排比快排慢了2~5倍，但是因为快速排序会出现最坏的情况，所以在某些情况下堆排比快排有效。

## vector源码分析

- vector用myfirst、myend和mylast来表示的。



![这里写图片描述](https://img-blog.csdn.net/20160223191226316)

- 扩容机制
  - grow()方法，如果不自定义，则默认扩容为原来的2倍大小

- 线程安全性
  - 在Vector中大多的方法使用了synchronized来进行修饰，以确保该方法是同步的，因为多了加锁释放锁的时间。也正是因为同步，所以才导致了Vector中的Iterator在被创建使用时，如果来了另一个线程要来添加或者修改元素，那么Vector中的Iterator就会抛出ConcurrentModificationException异常。



# 设计一个高并发系统

## 负载均衡和反向代理（nginx）

- Nginx可以充当代理的角色，对后端的所有服务器进行反向代理，服务端连接到Nginx，Nginx再将所有请求转发到后端的服务器
- Nginx还可以实现负载均衡，负载均衡就是Nginx将客户端的请求按照一定的规则分配给后端服务器进行处理

## 中间件的使用

**中间件有：**消息队列（ZeroMQ、RocketMQ、ActiveMQ、Kafka）、分布式协同发现（ZooKeepr）、分布式文件系统等等

### 消息队列

- 我们知道队列是先进先出的规则，一端向消息队列存入数据，另一端从消息队列中取出数据
- 并且消息队列还有很多种方式，例如：请求-响应模式、发布-订阅模式等
- 例如，一些服务器获取数据之后向MQ写入数据，另外一些服务器需要获取这些数据，那么就可以从MQ中取出数据

![img](https://img-blog.csdnimg.cn/2020080311213036.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDUzMjg1,size_16,color_FFFFFF,t_70)

### FastDfs

- FastDFS可以充当分布式的文件系统，其内部的数据是直接存储在硬盘上的
- Nginx有一个fastdfs-nginx-module模块，可以配置在Nginx中，直接通过URL进行访问
- **主要优点有：**
  - 备Tracker服务，增强系统的可用性
  - 系统不需要支持POSIX，这样的话就降低了系统的复杂度，使得处理的速度会更高
  - 支持主从文件，支持自定义扩展名
  - 支持在线扩容机制，增强了系统的可扩展性
  - 实现了软RAID，增强了系统的并发处理能力和数据容错恢复能力

![img](https://img-blog.csdnimg.cn/20200803113007861.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDUzMjg1,size_16,color_FFFFFF,t_70)

### zookeeper

- 提供数据的强一致性，强调cp，忽略a的分布式系统

![img](https://img-blog.csdnimg.cn/2020080311455553.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNDUzMjg1,size_16,color_FFFFFF,t_70)

## 数据库的设计

### MySQL

- 对于高并发下的数据的读写与存取，单台MySQL显然是无法满足需求的，因此***\*此时就需要搭建MySQL的集群\****
- 集群的方式有很多，下面以主从模式为例，Master用来提供写服务，多台Slave用来提供读服务，***\*从而实现读写分离\****
- 当然，***\*对于MySQL内部的细节还有很多是需要优化的，\****比如MySQL索引的设置、MySQL的分库分表

### Redis

- Redis作为当今最火的缓存系统，高并发的系统当然少不了
- Redis作为服务端数据读写的缓存，不仅仅可以减轻后端数据库MySQL的压力，而且还提高了服务端数据的读写速度，Redis与MySQL的搭配是非常好的解决方案

# 分布式锁的三种实现原理

## 为什么要使用分布式锁

使用分布式锁的目的，无外乎就是保证同一时间只有一个客户端可以对共享资源进行操作。

**(1)允许多个客户端操作共享资源**
这种情况下，对共享资源的操作**一定是**幂等性操作，无论你操作多少次都不会出现不同结果。在这里使用锁，无外乎就是为了避免重复操作共享资源从而提高效率。
**(2)只允许一个客户端操作共享资源**
这种情况下，对共享资源的操作**一般是**非幂等性操作。在这种情况下，如果出现多个客户端操作共享资源，就可能意味着数据不一致，数据丢失。

## 分布式锁应该具备哪些条件

1、在分布式系统环境下，一个方法在同一时间只能被一个机器的一个线程执行； 
2、高可用的获取锁与释放锁； 
3、高性能的获取锁与释放锁； 
4、具备可重入特性； 
5、具备锁失效机制，防止死锁； 
6、具备非阻塞锁特性，即没有获取到锁将直接返回获取锁失败。

## 单机情形比较

### redis

加锁：set 资源的名字 客户端生成的随机字符串 NX EX 30000

解锁：防止客户端一的锁被客户端二释放，可以采用下面的lua脚本来释放锁（lua脚本具有原子性）

```lua
if redis.call("get",KEYS[1]) == ARGV[1] then
    return redis.call("del",KEYS[1])
else
    return 0
end
```

将redis中保存的随机字符串和客户端的随机字符串作比较，如果相等确认是释放自己的锁，就可以执行，避免锁被他人释放。

注意1：如果释放锁不是原子性操作，在客户端一判断是自己的锁后发出解锁命令，但是此时网络延时了，del命令延迟到达，而客户一的锁已经过期释放了，那么这个延迟的del命令就会将下一个客户的锁释放，造成混乱。

![img](https://img2018.cnblogs.com/blog/905473/201912/905473-20191226132439737-459974196.png)

注意2：如果因为延时等原因，导致客户一的业务还在执行，但是锁过期失效了，那么客户二就可以获得锁，导致两个客户端共同操作共享资源的情况。可以开一个子线程实施监控主线程是否还持有锁，如果持有则更新锁过期时间，如果不持有就结束线程。

###  Zookeeper

原理是利用了临时顺序节点的特性

- **获取锁**

  - 首先，在Zookeeper当中创建一个持久节点ParentLock。当第一个客户端想要获得锁时，需要在ParentLock这个节点下面创建一个**临时顺序节点** Lock1。
  - 之后，Client1查找ParentLock下面所有的临时顺序节点并排序，判断自己所创建的节点Lock1是不是顺序最靠前的一个。如果是第一个节点，则成功获得锁。
  - 这时候，如果再有一个客户端 Client2 前来获取锁，则在ParentLock下载再创建一个临时顺序节点Lock2。
  - Client2查找ParentLock下面所有的临时顺序节点并排序，判断自己所创建的节点Lock2是不是顺序最靠前的一个，结果发现节点Lock2并不是最小的。于是，Client2向排序仅比它靠前的节点Lock1注册**Watcher**，用于监听Lock1节点是否存在。这意味着Client2抢锁失败，进入了等待状态。
  - 这时候，如果又有一个客户端Client3前来获取锁，则在ParentLock下载再创建一个临时顺序节点Lock3。
  - 这恰恰形成了一个等待队列

- **释放锁**

  - **1.任务完成，客户端显示释放**

    当任务完成时，Client1会显示调用删除节点Lock1的指令。

  - **2.任务执行过程中，客户端崩溃**

    获得锁的Client1在任务执行过程中，如果Duang的一声崩溃，则会断开与Zookeeper服务端的链接。根据临时节点的特性，相关联的节点Lock1会随之自动删除。

  - 前一个节点被删除，那么监视他的client就会获得锁。



注意1：这种情况下，虽然避免了设置了有效时间问题，然而还是有可能出现多个客户端操作共享资源的。
大家应该知道，zookeeper如果长时间检测不到客户端的心跳的时候(Session时间)，就会认为Session过期了，那么这个Session所创建的所有的ephemeral类型的znode节点都会被自动删除。这种时候会有如下情形出现：如上图所示，客户端1发生GC停顿的时候，zookeeper检测不到心跳，也是有可能出现多个客户端同时操作共享资源的情形。

![img](https://img2018.cnblogs.com/blog/905473/201912/905473-20191226132604515-950534991.png)

## 集群情形比较

### redis

问题：为了redis的高可用，一般都会给redis的节点挂一个slave,然后采用哨兵模式进行主备切换。但由于Redis的主从复制（replication）是异步的，这可能会出现在数据同步过程中，**master宕机，slave来不及同步数据就被选为master，从而数据丢失**。

具体情形：

- (1)客户端1从Master获取了锁。
- (2)Master宕机了，存储锁的key还没有来得及同步到Slave上。
- (3)Slave升级为Master。
- (4)客户端2从新的Master获取到了对应同一个资源的锁。

解决：

**RedLock算法**，步骤如下(该流程出自官方文档)，假设我们有N个master节点(官方文档里将N设置成5，其实大等于3就行)

- (1)获取当前时间（单位是毫秒）。
- (2)轮流用相同的key和随机值在N个节点上请求锁，在这一步里，客户端在每个master上请求锁时，会有一个和总的锁释放时间相比小的多的超时时间。比如如果锁自动释放时间是10秒钟，那每个节点锁请求的超时时间可能是5-50毫秒的范围，这个可以防止一个客户端在某个宕掉的master节点上阻塞过长时间，如果一个master节点不可用了，我们应该尽快尝试下一个master节点。
- (3)客户端计算第二步中获取锁所花的时间，只有当客户端在大多数master节点上成功获取了锁（在这里是3个），而且总共消耗的时间不超过锁释放时间，这个锁就认为是获取成功了。
- (4)如果锁获取成功了，那现在锁自动释放时间就是最初的锁释放时间减去之前获取锁所消耗的时间。
- (5)如果锁获取失败了，不管是因为获取成功的锁不超过一半（N/2+1)还是因为总消耗时间超过了锁释放时间，客户端都会到每个master节点上释放锁，即便是那些他认为没有获取成功的锁。

**RedLock**算法细想一下还存在下面的问题
**节点崩溃重启，会出现多个客户端持有锁**
假设一共有5个Redis节点：A, B, C, D, E。设想发生了如下的事件序列：
(1)客户端1成功锁住了A, B, C，获取锁成功（但D和E没有锁住）。
(2)节点C崩溃重启了，但客户端1在C上加的锁没有持久化下来，丢失了。
(3)节点C重启后，客户端2锁住了C, D, E，获取锁成功。
这样，客户端1和客户端2同时获得了锁（针对同一资源）。

为了应对节点重启引发的锁失效问题，redis的作者antirez提出了**延迟重启**的概念，即一个节点崩溃后，先不立即重启它，而是等待一段时间再重启，等待的时间大于锁的有效时间。采用这种方式，这个节点在重启前所参与的锁都会过期，它在重启后就不会对现有的锁造成影响。这其实也是通过人为补偿措施，降低不一致发生的概率。

**时间跳跃问题**
(1)假设一共有5个Redis节点：A, B, C, D, E。设想发生了如下的事件序列：
(2)客户端1从Redis节点A, B, C成功获取了锁（多数节点）。由于网络问题，与D和E通信失败。
(3)节点C上的时钟发生了向前跳跃，导致它上面维护的锁快速过期。
客户端2从Redis节点C, D, E成功获取了同一个资源的锁（多数节点）。
客户端1和客户端2现在都认为自己持有了锁。

为了应对始终跳跃引发的锁失效问题，redis的作者antirez提出了应该禁止人为修改系统时间，使用一个不会进行“跳跃”式调整系统时钟的ntpd程序。这也是通过人为补偿措施，降低不一致发生的概率。

### zookeeper

zookeeper在集群部署中，zookeeper节点数量一般是奇数，且一定大等于3

那么写数据流程步骤如下
1.在Client向Follwer发出一个写的请求
2.Follwer把请求发送给Leader
3.Leader接收到以后开始发起投票并通知Follwer进行投票
4.Follwer把投票结果发送给Leader，**只要半数以上返回了ACK信息，就认为通过**
5.Leader将结果汇总后如果需要写入，则开始写入同时把写入操作通知给Leader，然后commit;
6.Follwer把请求结果返回给Client
还有一点，zookeeper采取的是**全局串行化操作**

**集群同步**
client给Follwer写数据，可是Follwer却宕机了，会出现数据不一致问题么？不可能，这种时候，client建立节点失败，根本获取不到锁。
client给Follwer写数据，Follwer将请求转发给Leader,Leader宕机了，会出现不一致的问题么？不可能，这种时候，zookeeper会选取新的leader,继续上面的提到的写流程。
总之，采用zookeeper作为分布式锁，你要么就获取不到锁，一旦获取到了，必定节点的数据是一致的，不会出现redis那种异步同步导致数据丢失的问题。

**时间跳跃问题**
不依赖全局时间，怎么会存在这种问题
**超时导致锁失效问题**
不依赖有效时间，怎么会存在这种问题

## 第三回合，锁的其他特性比较

(1)redis的读写性能比zookeeper强太多，如果在高并发场景中，使用zookeeper作为分布式锁，那么会出现获取锁失败的情况，存在性能瓶颈。
(2)zookeeper可以实现读写锁，redis不行。
(3)zookeeper的watch机制,客户端试图创建znode的时候，发现它已经存在了，这时候创建失败,那么进入一种等待状态，当znode节点被删除的时候，zookeeper通过watch机制通知它，这样它就可以继续完成创建操作（获取锁）。这可以让分布式锁在客户端用起来就像一个本地的锁一样：加锁失败就阻塞住，直到获取到锁为止。这套机制，redis无法实现

## 总结

无论是redis还是zookeeper，其实可靠性都存在一点问题。但是，zookeeper的分布式锁的可靠性比redis强太多！但是,zookeeper读写性能不如redis,存在着性能瓶颈。大家在生产上使用，可自行进行评估使用。