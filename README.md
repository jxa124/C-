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
