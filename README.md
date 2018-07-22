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
