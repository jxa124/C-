# C++
## 头文件
.h——**declaration** 

.cpp——**definition**

.h文件只能包含declaration:

> * extern variables
> * function prototypes
> * class/struct declaration

## 宏定义
3种预处理方式：宏定义、文件包含、条件编译

#ifndefine、#define、#endif：

条件编译：最主要目的是防止头文件的重复包含和编译

```c++
#ifndef x                 //先测试x是否被宏定义过
#define x
   程序段1                //如果x没有被宏定义过，定义x，并编译程序段 1
#endif   
　　程序段2 　　          //如果x已经定义过了则编译程序段2的语句，“忽视”程序段 1
```
## using名令 vs using编译命令
头文件中不要使用 using namespace std，避免名称冲突

using命令：只导入了制定的名称。如果该名称与局部名称发生冲突，编译器将发出指示
```
using std::cout;
```
using编译命令：如果与局部名称发生冲突，则局部名称将覆盖名称空间版本，而编译器并不会发出警告
```
using namespace std;
```
## 类：public、private、protected的作用
如果在类的定义中既不指定private，也不指定public，则系统就默认为是私有的

private: 被声明为私有的（private）成员，只能被本类中的成员函数引用，类外不能调用（友元类除外）

public: 被声明为公用的（public）成员，既可以被本类中的成员函数所引用，也可以被类的作用域内的其他函数引用。

protected: 被声明为公用的（protected）成员，它不能被类外访问（这点与私有成员类似），但可以被派生类的成员函数访问

> 假如有个基类叫Father，代表父亲，它有个protected接口叫money()，Father有个子类叫Son，即儿子，儿子有个接口叫buyBook()，buyBook()中可以调用到Father中的money()，除了这个写好的函数以外，Son都是不能调用到Father的money()接口的，即爸爸给儿子规定好了，你只有买书来学习可以找我要钱，其他任何理由都不能找我要钱，这样就可以把爸爸的钱给“保护”起来了

## 类：构造函数 vs 析构函数
**构造函数**：会在每次创建类的新对象时执行；构造函数的名称与类的名称是完全相同的，并且不会返回任何类型，也不会返回 void；构造函数可用于为某些成员变量设置初始值

**析构函数**：会在每次删除所创建的对象时执行；析构函数的名称与类的名称是完全相同的，只是在前面加了个波浪号（~）作为前缀，它不会返回任何值，也不能带有任何参数。析构函数有助于在跳出程序（比如关闭文件、释放内存等）前释放资源

## 头文件 <fstream>
![stream png](https://github.com/jxa124/C-/blob/master/stream.gif)
```
#include <fstream>
ofstream         //文件写操作 内存写入存储设备 
ifstream         //文件读操作，存储设备读区到内存中
fstream          //读写操作，对打开的文件可进行读写操作   
```  
#### 打开文件
在fstream类中，成员函数open（）实现打开文件的操作，从而将数据流和文件进行关联，通过ofstream,ifstream,fstream对象进行对文件的读写操作
```
public member function
 
void open ( const char * filename,
            ios_base::openmode mode = ios_base::in | ios_base::out );
 
void open(const wchar_t *_Filename,
        ios_base::openmode mode= ios_base::in | ios_base::out,
        int prot = ios_base::_Openprot)；   
```  
参数：
filename   操作文件名

mode       打开文件的方式

prot       打开文件的属性

很多程序中，可能会碰到ofstream out("Hello.txt"), ifstream in("..."),fstream foi("...")这样的的使用，并没有显式的去调用open（）函数就进行文件的操作，直接调用了其默认的打开方式，因为在stream类的构造函数中调用了open()函数,并拥有同样的构造函数，所以在这里可以直接使用流对象进行文件的操作；

当使用默认方式进行对文件的操作时，你可以使用成员函数is_open()对文件是否打开进行验证；

**状态标志符的验证**(Verification of state flags)
除了eof()以外，还有一些验证流的状态的成员函数（所有都返回bool型返回值）：

bad()
如果在读写过程中出错，返回 true 。例如：当我们要对一个不是打开为写状态的文件进行写入时，或者我们要写入的设备没有剩余空间的时候。

fail()
除了与bad() 同样的情况下会返回 true 以外，加上格式错误时也返回true ，例如当想要读入一个整数，而获得了一个字母的时候。

eof()
如果读文件到达文件末尾，返回true。

good()
这是最通用的：如果调用以上任何一个函数返回true 的话，此函数返回 false 。

要想重置以上成员函数所检查的状态标志，你可以使用成员函数clear()，没有参数。

