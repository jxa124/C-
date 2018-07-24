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

## 头文件 fstream
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

**open() 里的filename需为char类型，如果为string,需要转为char,用c_str()**

很多程序中，可能会碰到ofstream out("Hello.txt"), ifstream in("..."),fstream foi("...")这样的的使用，并没有显式的去调用open（）函数就进行文件的操作，直接调用了其默认的打开方式，因为在stream类的构造函数中调用了open()函数,并拥有同样的构造函数，所以在这里可以直接使用流对象进行文件的操作；

当使用默认方式进行对文件的操作时，你可以使用成员函数is_open()对文件是否打开进行验证；

**状态标志符的验证**(Verification of state flags)

除了eof()以外，还有一些验证流的状态的成员函数（所有都返回bool型返回值）：

bad()

如果在读写过程中出错，返回 true 。例如：当我们要对一个不是打开为写状态的文件进行写入时，或者我们要写入的设备没有剩余空间的时候

fail()

除了与bad() 同样的情况下会返回 true 以外，加上格式错误时也返回true ，例如当想要读入一个整数，而获得了一个字母的时候

eof()

如果读文件到达文件末尾，返回true

good()

这是最通用的：如果调用以上任何一个函数返回true 的话，此函数返回 false 

要想重置以上成员函数所检查的状态标志，你可以使用成员函数clear()，没有参数

## getline函数
getline（istream &in, string &s）

功能：
–从输入流中读入字符，存到string变量

–直到出现以下情况为止：

> * 读入了文件结束标志
> * 读到一个新行
> * 达到字符串的最大长度
> * 如果getline没有读入字符，将返回false，可用于判断文件是否结束

## namespace的用法
namespace通常用来给类或者函数做个区间定义，以使编译器能准确定位到适合的类或者函数；
例如：自行实现了一个函数test(void)，而在该项目的库函数内也定义了一个函数test(void);当你调用test()，用namespace编译器就知道调用哪个了；

namespace定义方法
```
namespace namespace_name {
    // code declarations
    // 函数，类名等等
}   
```  
namespace调用方法
```
name::code; //此处code就是对应namespace内定义的类名或者函数名等等   
```
实际调用举例：
```
#include <iostream>
using namespace std;
    
// first name space
namespace first_space{
    void func(){
        cout << "Inside first_space" << endl;
    }
}
    
// second name space
namespace second_space{
    void func(){
        cout << "Inside second_space" << endl;
    }
}
    
int main () {
     
// Calls function from first name space.
first_space::func();
       
// Calls function from second name space.
second_space::func(); 
    
return 0;
} 
```
## 无符号类型
unsinged int，unsigned long，size_t, std::size_t

四种类型都是无符号类型，是用以表示元素个数或者数组索引的最佳类型。在作为函数参数时，不需像有符号类型那样检测值是否小于零;

## 重载运算符和重载函数
C++ 允许在同一作用域中的某个函数和运算符指定多个定义，分别称为函数重载和运算符重载；

重载声明是指一个与之前已经在该作用域内声明过的函数或方法具有相同名称的声明，但是它们的参数列表和定义（实现）不相同；

当您调用一个重载函数或重载运算符时，编译器通过把您所使用的参数类型与定义中的参数类型进行比较，决定选用最合适的定义。选择最合适的重载函数或重载运算符的过程，称为重载决策；

函数重载：

```
#include <iostream>
using namespace std;
 
class printData
{
   public:
      void print(int i) {
        cout << "整数为: " << i << endl;
      }
 
      void print(double  f) {
        cout << "浮点数为: " << f << endl;
      }
 
      void print(char c[]) {
        cout << "字符串为: " << c << endl;
      }
};
 
int main(void)
{
   printData pd;
 
   // 输出整数
   pd.print(5);
   // 输出浮点数
   pd.print(500.263);
   // 输出字符串
   char c[] = "Hello C++";
   pd.print(c);
 
   return 0;
}
```
重载运算符：

重载的运算符是带有特殊名称的函数，函数名是由关键字 operator 和其后要重载的运算符符号构成的。与其他函数一样，重载运算符有一个返回类型和一个参数列表；
```
Box operator+(const Box&);
```
如果我们定义上面的函数为类的非成员函数，那么我们需要为每次操作传递两个参数；
```
Box operator+(const Box&, const Box&);
```
实例：
```
#include <iostream>
using namespace std;
 
class Box
{
   public:
 
      double getVolume(void)
      {
         return length * breadth * height;
      }
      void setLength( double len )
      {
          length = len;
      }
 
      void setBreadth( double bre )
      {
          breadth = bre;
      }
 
      void setHeight( double hei )
      {
          height = hei;
      }
      // 重载 + 运算符，用于把两个 Box 对象相加
      Box operator+(const Box& b)
      {
         Box box;
         box.length = this->length + b.length;
         box.breadth = this->breadth + b.breadth;
         box.height = this->height + b.height;
         return box;
      }
   private:
      double length;      // 长度
      double breadth;     // 宽度
      double height;      // 高度
};
// 程序的主函数
int main( )
{
   Box Box1;                // 声明 Box1，类型为 Box
   Box Box2;                // 声明 Box2，类型为 Box
   Box Box3;                // 声明 Box3，类型为 Box
   double volume = 0.0;     // 把体积存储在该变量中
 
   // Box1 详述
   Box1.setLength(6.0); 
   Box1.setBreadth(7.0); 
   Box1.setHeight(5.0);
 
   // Box2 详述
   Box2.setLength(12.0); 
   Box2.setBreadth(13.0); 
   Box2.setHeight(10.0);
 
   // Box1 的体积
   volume = Box1.getVolume();
   cout << "Volume of Box1 : " << volume <<endl;
 
   // Box2 的体积
   volume = Box2.getVolume();
   cout << "Volume of Box2 : " << volume <<endl;
 
   // 把两个对象相加，得到 Box3
   Box3 = Box1 + Box2;
 
   // Box3 的体积
   volume = Box3.getVolume();
   cout << "Volume of Box3 : " << volume <<endl;
 
   return 0;
}
```
