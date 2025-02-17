# C++ 体系结构和内核分析

[toc]

# 写在前面的 —— 编程规范

## 常规

1. 每行不得超过一个表达式

2. 不能将函数写在一行上

3. 单行不得超过 120 个字符 ，并且始终不得超过 140 个字符

4. 行不应以空格或制表符结尾

5. 文件必须以空行结尾

6. 所有命名都必须由正确的英语单词或其缩写构成

7. 变量 的定义应尽可能的接近使⽤点 应有尽可能⼩的作⽤域

8. 所有 变量 都 应 在 定义时 初始化

9. 所有的常量都应用 const 标记

10. 如果通过引用传递的函数参数在函数中无更改 ，则其也应用 const 标记

11. 每单位的缩进为两个字符

12. 缩进反映了程序的逻辑结构

    - 在循环中的语句的缩进值必须大于循环标头
    - 在 if 和 else 中的语句必须具有⼤于 if 和 else 标头的缩进
    - switch 中的 case 关键字必须与 switch 对齐
    - 在 switch 中包含的语句 ，其缩进应至少大于一个一个单位 性对于 switch 的缩进

13. 循环和条件判断中的所有语句都应用打括号括起来 ，及时它为单条语句

14. 打括号必须 规范放置 ，有以下两种选择

    1. ![image-20210804154958445](Readme.assets/image-20210804154958445.png)
    2. ![image-20210804155004724](Readme.assets/image-20210804155004724.png)

    函数的括号放置只允许一种情况：

    ![image-20210804155025280](Readme.assets/image-20210804155025280.png)

    在程序中括号的放置只应遵循一种 ，不得多种方法混用

15. 关键字 if 和 else 可以组合在一起 ，以便减少缩进

    ![image-20210804155104112](Readme.assets/image-20210804155104112.png)

16. 所有的命名不得以下划线开头

17. 宏定义必须为大写

18. 较长语句的分行应向如下所示：

    1. 在下一行的开头，必须有一个连字符运算符（逗号除外）
    2. 边距的缩进必须大于 **2** 个单位
    3. 如果在带括号的复杂表达式中使⽤连字符，则缩进应反映括号嵌套，每个嵌套级别 在下⼀⾏添加1个缩进

    例如：

    ![image-20210804160235035](Readme.assets/image-20210804160235035.png)

19. 在表达式内部，应使⽤括号以避免歧义。 除了明显的算数优先级外，所有表达式都应经过设计，因此在阅读时不需要知道运算符优先级

20. 不允许在头文件中的顶部使用命名空间

21. 头文件必须包含 “#include 防范”，也就是必须防止头文件被重复编译

22. 包含头⽂件的顺序⾮常重要：

    1. 包含应有其定义的头文件，例如：circle.cpp 首先应包含 circle.hpp
    2. 标注库
    3. 头文件使用的库
    4. 自定义头文件

23. 对头文件的依赖应最小化，如果头文件中包含了过多的声明，则不应该在此头文件中进行其实现

## 类的编写

1. 类名必须由**大写字母开头** ，如果名称有多个单词组成 ，则每个单词首字母都应大写 CapsCase 。例如 Queue 和 RoundedRectangle；此外另一种可能的命名规则为每个单词为小写且用下划线分割（不推荐）

2. 类名必须是带有**明确含义的完整单词** ，另外 ，函数名必须由动词开头 ，因为它代表一个动作

3. 如果为 struct 结构体 （无需构造和析构函数 ），结构体的命名可用小写字母加下划线分割 ，且必须在名称后面添加 `_t`

4. 类内的部分 应按照他们的作用排序

   1. public ：对外接口
   2. protected ：继承接口
   3. private ：私有部分

5. 类内在每个部分的信息应安排如下：

   1. 类型
   2. 类字段
   3. 无参默认构造函数
   4. 复制和移动构造
   5. 所有其他构造
   6. 析构函数
   7. 重载运算符
   8. 成员方法

6. 函数和成员方法 应使用驼峰命名法 （camelCase），即 小写字母开头紧随其后的单词首字母应为大写 ，例如 `draw() `、 `getArea()`

7. 函数名应为动词开头 因为他们代表一个动作

8. 应在所有未改变变量值的成员方法加上 `const` 限定符

9. 类字段的命名必须拥有独特的字段属性

   - 前缀为 `m_`

   - 在最后添加下划线 `_` 推荐

     ```c++
     class Demo
     {
         public:
         	...
         
         private:
         	double aurmOne_;
         	char aurmTwo_;
     }
     ```

10. 构造函数中的初始化列表必须每⾏最多初始化⼀个变量或基类。 初始化列表前⾯的冒号必须保留在参数列表的⾏中，不允许将其转移到下⼀⾏

    ```c++
    Circle::Circle(double r_circle, const point_t& center):
      circle_center_(center),
      r_circle_(r_circle)
    {
      assert(r_circle > 0);
    }
    ```

11. 在实现⽂件中，所有函数必须具有完全限定的名称，其中名称空间和类名称使⽤“::”与函数名称分隔。 例如，对于**lab::detail** 名称空间中名为 **foo()** 的函数，实现应如下所示：

    ![image-20210804160428577](Readme.assets/image-20210804160428577.png)

12. 

# 一. 流

## 1.1 std
- **`std::getline(输入流，string，终止符[可选])   `** 

  从输入流中获取一整行，可以是单个普通字符、**空字符、制表符**和**换行符**

  ```cpp
    std::string str;
    std::getline(std::cin, str);	// 将输入流作为参数传入
    std::getline(std::cin, str, ',');	// 最后一个参数是终止字符，即读取到这个字符后结束
  ```

- **`std::skipws`** 和 **`std::noskipws`**

  定义于头文件 `<ios>`

  启用或禁用有格式输入函数所做的跳过前导空白符**（默认启用）**。在输出上无效果。
  
  1) 如同用调用 `str.setf([std::ios_base::skipws] ` **启用**流 `str` 中的 `skipws` 标志
  
  2) 如同用调用 `str.unsetf([std::ios_base::skipws]` **禁用**流 `str` 中的 `skipws` 标志**(会对整个流生效知道使用 `std::skipws` 取消)**
  
  ```cpp
  #include <iostream>
  #include <sstream>
  int main()
  {
      char c1, c2, c3;
      std::istringstream("a b c") >> c1 >> c2 >> c3;
      std::cout << "Default  behavior: c1 = " << c1 << " c2 = " << c2 << " c3 = " << c3 << '\n';
    std::istringstream("a b c") >> std::noskipws >> c1 >> c2 >> c3;
      std::cout << "noskipws behavior: c1 = " << c1 << " c2 = " << c2 << " c3 = " << c3 << '\n';
  }
  =================================================================
  输出：
  Default  behavior: c1 = a c2 = b c3 = c
  noskipws behavior: c1 = a c2 =   c3 = b
  ```
  
  
  
- **`std::ws`**

  剔除流中的**空字符、制表符**和**换行符**，直到匹配到非空字符，可使用 **`std::noskipws`** 将此禁用

  ```cpp
  int main()
  {
      std::string str;
      std::cin >> std::ws >> str;	// 剔除str前面的所有空字符，当然在这里不用也行
      
      // 读取一串数字中的一个字母（使用空格分隔）
      // 比如：>>>    16468   f     23541
      int num1 = 0;
      int num2 = 0;
      char c_sign = '\0';
      std::cin >> std::ws >> num1 >> std::ws >>  c_sign >> std::ws >> num2;
  }
  ```

  

## 1.2 iostream

### 1.2.1 常用函数

**所有流中未处理或读取的字符都会在缓冲区等待**

头文件：`#include <iostream>`

> **输入和输出流都能够作为参数传递，除了在 main 函数（主进程中），其他的函数中的输入输出流都应该作为参数传入，而不应该直接使用 `std::cin` 或者 `std::cout` ，因为在引发异常时主线程必须能够将子线程中的流覆盖**

比如：

```cpp
#include <iostream>

// 在函数中 istr 和 ostr 可以像 std::cin 和 std::cout 一样自由使用
void foo(int num, istream& istr, ostream& ostr)
{
    ostr << "Please input one number: ";
    istr >> num;
    
    if (!istr.good() || !ostr.good())
    {
        // 抛出异常
	}
}

int main(int args, char * argv[])
{
    int num;
    
    // 将 std::cin 和 std::cout 从 main 函数中作为参数传入
    foo(num, std::cin, std::cout);
}
```

- **`std::cin <<`** 

  输入时自动跳过前置**空字符**及**空行**
  
- **`std::cin.get()`**	

  从输入流中获取一个字符，这个字符可以是普通字符、**空字符、制表符**和**换行符**
  
- **`std::cin.peek()`**    

  查看输入流中的下一个字符，但是不读取，这个字符可以是普通字符、**空字符、制表符**和**换行符**
  
  ```cpp
  // 从输入流中读取一个字符，如果是字母 A 则读取读取并输出，如果不是则返回
  void foo(istream& istr, ostream& ostr)
  {
      char c_sign = '\0';
      if (istr.peek() == 'A')
      {
          ostr << istr.get();
      }
  }
  ```

- **`std::cin.ignore`**
  - `std::cin.ignore()`

    从输入流中忽略一个字符

  - `std::cin.ignore(num, 终止字符)`

    跳过输入流中n个字符，或在遇到指定的终止字符时提前结束（此时跳过包括终止字符在内的若干字符）

  - `std::cin.ignore(numeric_limits<std::streamsize>::max(), ‘\n’)`

    清除当前行

  - `std::cin.ignore(numeric_limits<std::streamsize>::max())` 

    清除 cin 里所有内容

  - `ignore(std::numeric_limits<std::streamsize>::max(), ',')`

    忽略逗号前面的所有内容

---

### 1.2.1 流状态标志

> std::ios_base::iostate
>
> https://zh.cppreference.com/w/cpp/io/ios_base/iostate

​    

- **`goodbit`**无错误

- **`badbit`**不可恢复的流错误

- **`failbit`**输入/输出操作失败（格式化或提取错误）

- **`eofbit`**关联的输出序列已抵达文件尾

相关函数：

- **`std::cin.fail()`**

  输入/输出操作失败（格式化或提取错误）。比如 `int` 类型的却读取了一个 `char`

- **`std::cin.good()`** 

  输入流无错误

- **`std::cin.setstate(std::istream::failbit)`**

  将输入流设置为 `failbit`，也可以是上面其他的 **`goodbit`**，**`badbit`**，**`eofbit`**

---

## 1.3 string stream

### 1.3.1 常用方法

例如：

将 std::cin 输入流中的内容导入到一个 stringstream 中，再处理这个 stringstream 而不是处理 std::cin 流

```cpp
int main()
{
  std::string str_line;
  std::getline(std::cin, str_line);
  std::stringstream str_stream(str_line);

  int num;
  str_stream >> num;	// 会像 std::cin 一样自动互忽略流中的空字符
}
```

---

# 二. C++标准库

> 重要的网站：
>
> www.cplusplus.com
>
> www.en.cppreference.com/w/
>
> www.gcc.gnu.org

## 2.1 前置知识

Q: 什么是`泛型编程`？

A: 所谓泛型编程，就是使用`template（模板）`为主要工具来编程，STL就是泛型编程最成功的作品

---

Q：什么是STL

A：STL - Standard Template Library - 标准模板库

---

标准库以头文件的形式呈现，而不是编译好的，所以直接看到源代码

- C++的标准库的头文件不带拓展名`.h`，例如 `#include <vector>`
- 新的C文件也不带`.h`了，例如 `#include <cstdio>`
- 新的头文件里的组件都被封装在了 `namespace std`
  - `using namespace std`
  - `using std::cout`
-  旧的头文件中的组件**没有**封装在 `namespace std`



STL六大部件（Components）：

- 容器（Containers）
- 分配器（Allocators） - 为容器分配内存
- 算法（Algorithms）
- 迭代器（Iterators） - 类似于一个泛化指针
- 适配器（Adapters）
- 仿函数（Functors）

![image-20201205185708070](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201205185708070.png)

实例程序：

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <functional>

int main()
{
	int arr[6] = { 21, 210, 100, 40, 56, 6 };
    //    ↓container      ↓allocator
    std::vector<int, allocator<int> > vec(arr, arr + 6);
    
    //           ↓algorithm     ↓iterator↓        ↓function adapter(negator)
    std::cout << count_if(vec.begin(), vec.end(), not1(bind2nd(less<int>(), 40)));
    //                                            function object↑
    // 整个 not1(bind2nd(less<int>(), 40)) 是一个 谓词（predicate）
    return 0;
}
```

## 2.2 算法复杂度

1. O(1) or O(N)：常数时间（constant time）
2. O(N)：线性时间（linear time）
3. O(log2N)：二次线性时间（sub-liner time）
4. O(N^2)：平方时间（quadratic time）
5. O(N^3)：立方时间（cubic time）
6. O(2^N)：指数时间（exponential time）
7. O(Nlog2N)：介于线性及二次方的中间行为模式

## 2.3 OOP vs. GP

### 2.3.1 OOP

![image-20201220235225378](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201220235225378.png)

`OOP` - Object-Oriented programming

**OOP 企图将 数据(datas) 和 方法 (methods) 联系在一起**

比如：

`list` 容器将许多方法和保存的数据保存到了一起

```cpp
template<typename T>
class list
{
public:
    push_back(T data);
    push_frount(T data);
    ...
    ...
    sort();
	// Q: 考虑一下为什么 list 提供了自己的 sort 方法，而不是像其他容器一样可以随意的使用标准库提供的 			::sort 方法？
    // A: 因为 list 是一个双向链表，元素在内存上不是连续的，所以泛化指针不能随意的执行++、--等操作
private:
    T data_;
}
```

---

### 2.3.2 GP

![image-20201220235418794](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201220235418794.png)

`GP` - Generic Programming

**GP 欲将  数据(datas) 和 方法 (methods) 分离**

这么做的好处有：

- **容器(Containers) **和 **算法(Algorithms)** 可以**分开设计，各尽其职**。然后**之间使用迭代器(Iterator)沟通/链接**
- 算法(Algorithms) 通过 迭代器(Iterator) 确定操作范围，并通过 迭代器(Iterator) 取用容器(Containers) 中的元素

比如：

![image-20201221000016970](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221000016970.png)

## 2.4 操作符重载

以下操作符不能重载：`::`，`.`， `.*`，`?:`



# 三. 容器

## 3.1 容器的基本使用

- **前闭后开 [ )**

  `.begin()` 指向容器中的第一个元素

  `.end()` 指向容器所有元素的下一个元素（不是容器的一部分）

  ```cpp
  Container<T> c;
  ...
  Container<T>::iterator it = c.begin();
  for ( ; it < c.end(), ++it)
  {
      ...
  }
  ```

- C++ 11 新的迭代方法

  **range-based `for` statement**

  ```cpp
  for ( 声明 : 容器 )
  {
      ...
  }
  ```

  ```cpp
  for ( int i : { 21, 210, 100, 40, 56, 6 })
  {
      std::cout << i << endl;
  }
  ```

  ```cpp
  std::vector<double> vec;
  ...
  for (auto elem : vec) // 在这里 auto 自动适配为 std::vector<double>::iterator
  {
  	std::cout << elem << std::endl; 
  }
  
  for (auto& elem : vec)
  {
      elem *= 3;
  }
  ```

- C++11 `auto` 关键字

  `auto` 为C++中的自动类型推导

  例如**以往**的代码这么写：

  ```cpp
  list<string> c;
  ...
  list<string>::iterator it;
  it = ::find(c.begin(), c.end(), target);
  ```

  **如今可以更改为**

  ```cpp
  list<string> c;
  ...
  // 自动适配为 list<string>::iterator it
  auto it = ::find(c.begin(), c.end(), target);
  ```

  

- 容器不一定是连续的空间

## 3.2 容器的基本使用

![ ](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201206151434712.png)

- `map`和`set`的底层都是使用红黑树实现
  - `map`中单个节点包含 `key`和`value`，之后使用`key`找`value`
  - `set` 的`key`和`value`是不分的，是一个东西
- 容器中带有**`multi`** 的代表元素可以重复**（multi - 代表多）**，比如：`multiset`和`multimap`中的元素（`key`）可以重复
- 容器中带有**`unordered`** 的代表底部使用了**哈希表**
- **所有的容器中，所有模板都有第二个参数 （vector<参数一，参数二>），第二个参数是一个`分配器`**。如果不填写第二个参数，则使用默认的分配器

---

![image-20201221150122003](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221150122003.png)

---

### 3.2.1 array

![image-20201215185545057](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201215185545057.png)

头文件：`<array>`

初始化：`array<类型, 数组大小> arr;`

方法：

- `arr.size()` 返回数组的大小
- `arr.front() ` 第一个元素的内容
- `arr.back()` 最后一个元素的内容
- `arr.data()` 第一个元素的**地址**

---

### 3.2.2 vector

![image-20201215190852597](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201215190852597.png)

描述：**一端**开口的容器，会自动为容器提前分配空间（大约是1.5倍）。**容器的大小/容量始终大于或等于容器中元素的个数**。注意，这个容器“成长”的过程是缓慢的，因为要在另一个区域重新开辟一块内存，然后将当前内存中的**所有**元素一一拷贝过去

头文件：`<vector>`

初始化：`std::vector<类型> vec;`

方法：

- `vec.push_back()` 将元素从尾部放入
- `vec.size()` 容器中真正的元素个数
- `vec.capacity()` 容器的大小
- `vec.front()` 返回容器中的第一个元素
- `vec.back()` 返回容器中最后一个元素
- `vec.data()` 容器的起始地址

---

### 3.2.3 list

![image-20201218091054364](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201218091054364.png)

描述：双向链表

头文件：`<list>`

初始化：`list<类型> l;`

方法：

- `l.size()` 返回双向链表的大小
- `l.max_size()` 返回双向链表的最大大小
- `l.push_frount` 头插
- `l.push_back` 尾插
- `l.pop_frount` 移除首元素
- `l.pop_back` 移除末元素
- `l.front() ` 返回双向链表中第一个元素的内容
- `l.back()` 返回双向链表中最后一个元素的内容
- **`l.sort()` list 自己提供的排序算法**

---

### 3.2.4 forward_list

![image-20201218210703251](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201218210703251.png)

描述：单向链表

头文件：`<forward_list>`

初始化：`list<类型> f_l;`

方法：

- **无此方法：** *`f_l.size()`* 
- `f_l.max_size()` 返回单向链表的最大大小
- `l.push_frount` 头插
- `f_l.pop_frount` 移除首元素
- **无此方法：** *`l.push_back`*
- `f_l.front() ` 返回双向链表中第一个元素的内容
- **无此方法：** *`f_l.back()`* 
- **`f_l.sort()` forward_list 自己提供的排序算法**

---

### 3.2.5 slist

`slist` 是GUN标准下的（非标准库）一个单向链表，功能完全和标准库中的forward_list完全一样

---

### 3.2.6 deque

描述：duque是一个**双向开口，可进可出**的容器。duque在内存中并不是连续的，连续只是一个假象。duque是有许多内存段中组合而成，**一个内存段称为一个 `buffer`** 。术语上称为：**分段连续**

![image-20201218211839131](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201218211839131.png)

头文件：`<deque>`

初始化：`std::deque<类型> deq;`

方法：

- `deq.size()` 目前deque容器的大小
- `deq.max_size()` deque容器的最大大小
- `deq.resize(N)` 初始化N个元素到deque中
- `deq.at(N)` 返回第N个元素
- `deq.frount()` 返回首元素
- `deq.back()` 返回末元素
- `deq.pop_frount()` 移除首元素
- `deq.pop_back()` 移除末元素
- deque 没有提供自己的 sort

---

### 3.2.7 stack

![image-20201218221718925](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201218221718925.png)

描述：stack是栈 - **先进后出**。因为容器的特性，所以不提供 `iterator` 的操作

头文件：`<stack>`

初始化：`std::stack<类型> s;`

方法：

- `s.size()` 返回栈的大小
- `s.top()` 返回栈顶元素
- `s.push()` 将元素推入栈中
- `s.pop()` 将一个元素弹出栈

---

### 3.2.8 queue

![image-20201218222040350](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201218222040350.png)

描述：queue 是**队列**，**先进先出**。因为容器的特性，所以不提供 `iterator` 的操作

头文件：`<queue>`

初始化：`std::queue<类型> q;`

方法：

- `q.size()` 返回队列的大小
- `q.push()` 将元素推入栈中
- `q.front()` 返回队手元素
- `q.back()` 返回队尾元素
- `q.pop()` 将一个元素弹出栈

---

### 3.2.9 multiset

![image-20201219173103348](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201219173103348.png)

描述：小型的关联数据库，以**红黑树（高度平衡树）**形成的底层结构。**允许出现重复元素**

头文件：`<multiset>`

初始化：`multiset<类型> mul_set;`

方法：

- `mul_set.insert()` 插入一个元素
- `mul_set.size()` multiset 的大小
- `mul_set.max_size()` multiset 能容纳的最多元素
- `mul_set.find()` 查找一个元素，并返回一个迭代器

---

> set 和 map 的区别：
>
> set 容器的一个节点只能存储一个 **value**，
>
> map 可以的一个节点可以存储**一个 key 和一个 value，并通过 key 来查找 value**

---

### 3.2.10 multimap

![image-20201219182902723](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201219182902723.png)

描述：底层是一个红黑树,**第一个参数是key，第二个参数是value**。multimap 不能使用 [] 做 insertion

头文件：`<multimap>`

**初始化：`multimap<key的类型，value的类型> mul_map;`**

方法：

- `mul_map.insert()` 插入一个元素
- **`mul_map.insert(std::pair<key的类型，value的类型>(key值，value值))` 插入一个元素**
- `mul_map.size()` multimap 的大小
- `mul_map.max_size()` multimap 能容纳的最多元素
- `mul_map.find()` 查找一个元素，并返回一个迭代器

---

### 3.2.11 unordered_multiset

![image-20201219182923279](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201219182923279.png)

描述：是一个关联式容器，**底层是一个哈希表**

头文件：`<unordered_multiset>`

初始化：`unordered_multiset<类型> unor_mul_set;`

方法：

- `unor_mul_set.insert()` 插入一个元素
- `unor_mul_set.size()` multiset 的大小
- `unor_mul_set.max_size()` multiset 能容纳的最多元素
- `unor_mul_set.bucker_count()` 因为底层是哈希表，所以此方法取得的是**当前哈希表中篮子的数量**，并且**篮子的数量一定比元素数量多**
- `unor_mul_set.max_bucker_count()` 因为底层是哈希表，所以此方法取得的是**哈希表中篮子的最大数量**
- `unor_mul_set.load_factor()` 载重因子
- `unor_mul_set.max_load_factor()` 最大载重因子（一定是1）
- `unor_mul_set.find()` 查找一个元素，并返回一个迭代器

---

### 3.2.12 unordered_multimap

![image-20201219182934958](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201219182934958.png)

描述：是一个关联式容器，底层是一个哈希表。multimap 不能使用 [] 做 insertion

头文件：`<unordered_multimap>`

**初始化：`unordered_multimap<key的类型，value的类型> unord_mul_map;`**

方法：

- `unord_mul_map.insert()` 插入一个元素
- **`unord_mul_map.insert(std::pair<key的类型，value的类型>(key值，value值))` 插入一个元素**
- `unord_mul_map.size()` multiset 的大小
- `unord_mul_map.max_size()` multiset 能容纳的最多元素
- `unord_mul_map.find()` 查找一个元素，并返回一个迭代器

---

### 3.2.13 set

![image-20201219185139673](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201219185139673.png)

描述：小型的关联数据库，以**红黑树（高度平衡树）**形成的底层结构。**不允许出现重复元素**

头文件：`<set>`

初始化：`set<类型> set;`

方法：

- `set.insert()` 插入一个元素
- `set.size()` set 的大小
- `set.max_size()` set 能容纳的最多元素
- `set.find()` 查找一个元素，并返回一个迭代器

---

### 3.2.14 map

![image-20201219185147583](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201219185147583.png)

描述：底层是一个红黑树，**key 不能重复，value 可以重复**

头文件：`<map>`

**初始化：`map<key的类型，value的类型> map;`**

方法：

- `map.insert()` 插入一个元素
- **`map.insert(std::pair<key的类型，value的类型>(key值，value值))` 插入一个元素**
- **`map[key] = value` 在 key 处插入一个 value ** 重点！！！！！
- `map.size()` map 的大小
- `map.max_size()` map 能容纳的最多元素
- `mul_map.find()` 查找一个元素，并返回一个迭代器

---

### 3.2.16 unordered_set

![image-20201219181928071](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201219181928071.png)

描述：**底层是哈希表支撑的 set 容器**

头文件：`<unordered_set>`

初始化：`unordered_set<类型> unord_set;`

方法：

- `unord_set.insert()` 插入一个元素
- `unord_set.size()` set 的大小
- `unord_set.max_size()` set 能容纳的最多元素
- `unor_set.bucker_count()` 因为底层是哈希表，所以此方法取得的是**当前哈希表中篮子的数量**，并且**篮子的数量一定比元素数量多**
- `unor_set.max_bucker_count()` 因为底层是哈希表，所以此方法取得的是**哈希表中篮子的最大数量**
- `unor_set.load_factor()` 载重因子
- `unor_set.max_load_factor()` 最大载重因子（一定是1）
- `set.find()` 查找一个元素，并返回一个迭代器

---

### 3.2.15 unordered_map

![image-20201219182440457](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201219182440457.png)

描述：**底层是哈希表支撑的 map 容器**

头文件：`<unordered_map>`

**初始化：`unordered_map<key的类型，value的类型> unord_map;`**

方法：

- `unord_map.insert()` 插入一个元素
- **`unord_map.insert(std::pair<key的类型，value的类型>(key值，value值))` 插入一个元素**
- **`unord_map[key] = value` 在 key 处插入一个 value ** 重点！！！！！
- `unord_map.size()` map 的大小
- `unord_map.max_size()` map 能容纳的最多元素
- `unord_map.find()` 查找一个元素，并返回一个迭代器

## 3.3 容器原码分析

### 3.3.1 array



### 3.3.2 vector



### 3.3.3 list

<img src="https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201222095725501.png" alt="image-20201222095725501" style="zoom:80%;" />

![image-20201222101632401](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201222101632401.png)

![image-20201222103816861](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201222103816861.png)

![image-20201222104710288](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201222104710288.png)

![image-20201222104950584](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201222104950584.png)

![image-20201222105056295](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201222105056295.png)

### 3.3. 



### 3.3.



### 3.3.



### 3.3.



### 3.3.



### 3.3.



### 3.3.



### 3.3.



### 3.3.

---

# 四. 模板

## 4.1 类模板

![image-20201221034734013](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221034734013.png)

## 4.2 函数模板

![image-20201221034747579](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221034747579.png)

**函数模板不需要使用 `<>` 告诉编译器使用的类型**，因为有自动类型推导。但是类模板不行，如果不提供模板参数，编译器无法确定类型

## 4.3 成员模板

后续补充

![image-20201221050407143](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221050407143.png)

## 4.4 模板的泛化与特化

解释什么是泛化与特化：

​	想象一下，在STL中是提供了模板，模板用于匹配参数类型。但是基础的数据类型中有 `int`，`float`，`double`，`char` 等类型，在模板自动匹配类型后如何对这些不同的类型进行操作和处理呢；其实在STL的内部已经把各种基础类型的功能或方法一一手动实现了（按各个类型最优的方式，时间或空间效率高），这些一一实现就称之为**模板的特化**。

​	泛化 - 顾名思义，就指的是一个总体的大的分类。

​	特化 - 就是对这个总的大的分类进行一一具体的实现

​	比如：`猫` 是一个总称，对于会捉老鼠，喵喵叫的我们都可以称之为`猫`。这是一个大体的分类，所以称之为**`泛化`**；但是猫又分为橘猫、蓝猫、中华田园猫、俄罗斯无毛猫 等等，假如我现在要写文章介绍这各个品种的猫的特点和生活习性，那么，因为我们对这些猫各自的特点和生活习惯的描述具体到了某一品种，所以就称之为**`特化`**

---

![image-20201221050644237](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221050644237.png)

关于特化和泛化写法的记忆：

- 未特化的模板写法和平时一样
- 对于特化的模板就像：把原本首行 `template<typename T>` 中的 `T` 拿出来了，放在了类名的后方(橘色方框的部分)，以表示对具体类型进行特化。所以说，原本的 `template<typename T>` 的 `<>` 为空了，变成了 `template<>` (黄色方框部分)

---

以下是STL库中的几种泛化和特化的实现：

**其中 `__STL_TEMPLATE_NULL` 是一个 `#define`，其实就是 `template<>`**

![image-20201221053125192](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221053125192.png)

![image-20201221053155758](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221053155758.png)



## 4.5 局部特化（偏特化）

- 对于拥有**多个模板参数**的类或者函数可以进行**局部特化**，即对某一个模板参数进行特化
- 对于拥有**一个模板参数**的类或者函数可以进行**局部特化**，即对模板参数的 *`<T*>` (指针类型)* 或者 *`<const T*>` (const指针)* 进行特化

![image-20201221053827841](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221053827841.png)



# 五. 分配器 - allocator

## 5.1 基本说明

分配器支持了容器对内存的使用

**所有内存的分配动作，最后都会涉及到 `malloc` 操作。然后 `malloc` 根据不同的操作系统来申请内存**

---

![image-20201219185923398](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201219185923398.png)

![image-20201220204737370](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201220204737370.png)

## 5.2 基本使用

头文件 `<memory>` 里包含了 allocator

但是如果使用 std::allocator 以外的 allocator，需要自行 `#include <ext\...>`

注意：其他的分配器是在 **`__gnu_cxx::`** 下的

比如：

```cpp
#include <ext\array_allocator.h>
#include <ext\mt_allocator.h>
#include <ext\debug_allocator.h>
#include <ext\pool_allocator.h>		// 内存池
#include <ext\bitmap_allocator.h>
#include <ext\malloc_allocator.h>
#include <ext\new_allocator.h>

void test_list_with_special_allocator()
{
	list<std::string, allocator<std::string> > c1;
    list<std::string, __gnu_cxx::malloc_allocator<std::string> > c2;
    list<std::string, __gnu_cxx::new_allocator<std::string> > c3;
    list<std::string, __gnu_cxx::__pool_allocator<std::string> > c4;
    list<std::string, __gnu_cxx::__mt_allocator<std::string> > c5;
    list<std::string, __gnu_cxx::bitmap_allocator<std::string> > c6;
}
```

![image-20201220204913799](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201220204913799.png)

**使用分配器分配并归还内存**

```cpp
int* p;		// 注意：p 是一个 int 类型的**指针**

allocator<int> alloc1;
p = alloc1.allocator(1);	// 1 代表申请一份内存
alloc1.deallocator(p, 1);	// 1 代表归还一份内存, p 代表归还的对象是谁。
							// 可以通过 new 和 delete 来理解

__gnu_cxx::malloc_allocator<int> alloc2;
p = alloc2.allocator(1);
alloc2.deallocate(p, 1);

__gnu_cxx::new_allocator<int> alloc3;
p = alloc3.allocator(1);
alloc3.allocator(p, 1);

__gnu_cxx::__pool_allocator<int> alloc4;
p = alloc4.allocator(1);
alloc4.allocator(p, 1);

__gnu_cxx::__mt_allocator<int> alloc5;
p = alloc5.allocator(1);
alloc5.allocator(p, 1);

__gnu_cxx::bitmap_allocator<int> alloc6;
p = alloc6.allocator(1);
alloc6.allocator(p, 1);
```

## 5.3 解析

![image-20201221081026072](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221081026072.png)

### 5.3.1 VC6 STL 对 allocator 的使用

![image-20201221140244647](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221140244647.png)

![image-20201221140403407](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221140403407.png)

### 5.3.2 BC5 STL 对 allocator 的使用

![image-20201221140519806](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221140519806.png)

### 5.3.3 G2.9 对 allocator 的使用

一下虽然实现了 allocator，但是没有包含在任何容器中。也不建议使用

![image-20201221140707011](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221140707011.png)

---

以下是 G2.9 STL 中使用内存池对分配器(allocator)的实现，推荐使用。因为能节省大量在申请内存时不必要浪费的空间

在 G2.9 STL 中称之为 **`alloc`**

![image-20201221141106421](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221141106421.png)

![image-20201221141139759](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221141139759.png)

![image-20201221142010844](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221142010844.png)

![image-20201221142043934](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221142043934.png)

### 5.3.4 sizeof(迭代器)

![image-20201221142238414](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201221142238414.png)

# 六. 萃取机 - trait



![image-20201226201348707](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201226201348707.png)

- `iterator_category` iterator 的类型
- `difference_type` 两个 iterator 之间的距离
- `value_type` iterator (迭代器)指向的那个东西的类型
- 剩余的两种 `reference` 和 `pointer` 未在标准库中使用过

![image-20201226202806858](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201226202806858.png)

**指针是一种退化的Iterator，Iterator 是一种泛化的指针**

![image-20201227190003769](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201227190003769.png)

![image-20201227190019885](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201227190019885.png)

![image-20201227190031977](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201227190031977.png)

![image-20201227190044824](https://github.com/NekoSilverFox/CPP/blob/master/images/image-20201227190044824.png)

# 标准库算法

头文件：`<algorithm>`

功能：

复杂度：

定义：

描述：

返回值：

## 查找

### find

头文件：`<algorithm>`

功能：查找元素，循序查找法

复杂度：

定义：find(起始迭代器, 结束迭代器, 谓词)

描述：对容器内的元素进行**线性**查找

返回值：指向该元素的**迭代器**

### bsearch

头文件：`<algorithm>`

功能：二分查找（C标准库）

复杂度：

定义：bseach(谓词，容器起始地址，容器大小，单位元素大小);

描述：对容器内的元素进行**二分**查找，但是注意，**只能对有序容器进行查找**，所以说使用前要对容器进行排序

返回值：指向该元素的**迭代器**

## 排序

### sort

头文件：`<algorithm>`

功能：对容器进行排序

复杂度：*O(N·log(N))*

定义：sort(起始迭代器, 结束迭代器)

描述：默认进行升序排序

返回值：

# 其他函数

## 程序终止

1. [`std::_Exit` ](http://en.cppreference.com/w/cpp/utility/program/_Exit)导致正常程序终止，仅此而已。
2. [`std::quick_exit` ](http://en.cppreference.com/w/cpp/utility/program/quick_exit)导致正常程序终止并调用[ `std::at_quick_exit` ](http://en.cppreference.com/w/cpp/utility/program/at_quick_exit)处理程序，不执行其他任何清除操作。
3. [`std::exit` ](http://en.cppreference.com/w/cpp/utility/program/exit)导致正常程序终止，然后调用[ `std::atexit` ](http://en.cppreference.com/w/cpp/utility/program/atexit)处理程序。执行其他类型的清除，例如调用静态对象析构函数。
4. [`std::abort` ](http://en.cppreference.com/w/cpp/utility/program/abort)导致程序异常终止，不执行任何清理。如果程序以非常非常意外的方式终止，则应调用此方法。它只会通知操作系统有关异常终止的任何信息。在这种情况下，某些系统会执行核心转储。
5. [`std::terminate` ](http://en.cppreference.com/w/cpp/error/terminate)调用[ `std::terminate_handler` ](http://en.cppreference.com/w/cpp/error/terminate_handler)，默认情况下调用[ `std::abort` ](http://en.cppreference.com/w/cpp/utility/program/abort)。

### std::abort

定义于头文件 `<cstdlib> ` - void abort();

导致不正常程序终止，除非传递给 [std::signal](https://zh.cppreference.com/w/cpp/utility/program/signal) 的信号处理函数正在捕捉 [SIGABRT](https://zh.cppreference.com/w/cpp/utility/program/SIG_types) ，且该处理函数不返回。

不调用拥有自动、线程局域 (C++11 起)和静态[存储期](https://zh.cppreference.com/w/cpp/language/storage_duration)的对象的析构函数。亦不调用以 [std::atexit()](https://zh.cppreference.com/w/cpp/utility/program/atexit) 和 [std::at_quick_exit](https://zh.cppreference.com/w/cpp/utility/program/at_quick_exit) (C++11 起) 注册的函数。是否关闭打开的资源，例如文件是实现定义的。返回给宿主环境指示不成功执行的实现定义状态。