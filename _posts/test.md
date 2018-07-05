---
layout:     post
title:      "系统级程序设计笔记(2)"
subtitle:   "C Programming Model"
date:       2018-1-6 17:40:00
author:     "Snow"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
- Notes
---

# C Programming Model

---

## 前言

本篇博客整理了**系统级程序设计课程**中的`lecture3`.

相关材料见：[SLP-Notes](https://github.com/RMSnow/SLP-Notes.git)

### 待更新

- Code Example
- **misleadings**
- gdb常用命令
- Variables and Addresses: P29 代码
- 数组的初始化
- Arrays and Strings：P46 extern

---

### The Wonder of Program Execution

#### Code Example

```c
int accum = 0;

int sum(int x, int y){
    int t = x + y;
    accum += t;
    return t;
}

int main(int argc, char **argv)
{
    sum(1, 2);
    return 0;
}
```

函数的调用过程



对如上所示的代码，我们分别观察其在32位、64位gcc编译器下生成的汇编代码文件：

```shell
snow:notes2 snow$ ls
accum.c
snow:notes2 snow$ gcc -m32 -S *.c -o accum-32.s
snow:notes2 snow$ gcc -m64 -S *.c -o accum-64.s
snow:notes2 snow$ ls
accum-32.s	accum-64.s	accum.c
snow:notes2 snow$ 
```

列出`main()`函数与其调用的`sum()`函数部分：

```c
/* accum-32.s */

_sum:                                   ## @sum
## BB#0:
	pushl	%ebp
	movl	%esp, %ebp
	subl	$12, %esp
	calll	L0$pb
L0$pb:
	popl	%eax
	movl	12(%ebp), %ecx
	movl	8(%ebp), %edx
	movl	%edx, -4(%ebp)
	movl	%ecx, -8(%ebp)
	movl	-4(%ebp), %ecx
	addl	-8(%ebp), %ecx
	movl	%ecx, -12(%ebp)
	movl	-12(%ebp), %ecx
	addl	_accum-L0$pb(%eax), %ecx
	movl	%ecx, _accum-L0$pb(%eax)
	movl	-12(%ebp), %eax
	addl	$12, %esp
	popl	%ebp
	retl

	.globl	_main
	.p2align	4, 0x90
_main:                                  ## @main
## BB#0:
	pushl	%ebp
	movl	%esp, %ebp
	pushl	%esi
	subl	$36, %esp
	movl	12(%ebp), %eax
	movl	8(%ebp), %ecx
	movl	$1, %edx
	movl	$2, %esi
	movl	$0, -8(%ebp)
	movl	%ecx, -12(%ebp)
	movl	%eax, -16(%ebp)
	movl	$1, (%esp)
	movl	$2, 4(%esp)
	movl	%edx, -20(%ebp)         ## 4-byte Spill
	movl	%esi, -24(%ebp)         ## 4-byte Spill
	calll	_sum
	xorl	%ecx, %ecx
	movl	%eax, -28(%ebp)         ## 4-byte Spill
	movl	%ecx, %eax
	addl	$36, %esp
	popl	%esi
	popl	%ebp
	retl
	
/* accum-64.s */

_sum:                                   ## @sum
	.cfi_startproc
## BB#0:
	pushq	%rbp
Lcfi0:
	.cfi_def_cfa_offset 16
Lcfi1:
	.cfi_offset %rbp, -16
	movq	%rsp, %rbp
Lcfi2:
	.cfi_def_cfa_register %rbp
	movl	%edi, -4(%rbp)
	movl	%esi, -8(%rbp)
	movl	-4(%rbp), %esi
	addl	-8(%rbp), %esi
	movl	%esi, -12(%rbp)
	movl	-12(%rbp), %esi
	addl	_accum(%rip), %esi
	movl	%esi, _accum(%rip)
	movl	-12(%rbp), %eax
	popq	%rbp
	retq
	.cfi_endproc

	.globl	_main
	.p2align	4, 0x90
_main:                                  ## @main
	.cfi_startproc
## BB#0:
	pushq	%rbp
Lcfi3:
	.cfi_def_cfa_offset 16
Lcfi4:
	.cfi_offset %rbp, -16
	movq	%rsp, %rbp
Lcfi5:
	.cfi_def_cfa_register %rbp
	subq	$32, %rsp
	movl	$1, %eax
	movl	$2, %ecx
	movl	$0, -4(%rbp)
	movl	%edi, -8(%rbp)
	movq	%rsi, -16(%rbp)
	movl	%eax, %edi
	movl	%ecx, %esi
	callq	_sum
	xorl	%ecx, %ecx
	movl	%eax, -20(%rbp)         ## 4-byte Spill
	movl	%ecx, %eax
	addq	$32, %rsp
	popq	%rbp
	retq
	.cfi_endproc
```

- Integer Registers

  %esp,%ebp

  %rsp,%rbp

- ​

#### Some misleadings

Eg1:

```c
#define ARRAY_SIZE 10
void natural_numbers (void) {
    int i;
    int array[ARRAY_SIZE];
    
    i = 1;
    while ( i <= ARRAY_SIZE) {
        array[i] = i - 1;
        printf("array[%d] = %d\n", i, i - 1);
        i = i + 1;
    }
}
```

Eg2:

```c
#define ARRAY_SIZE 10
void natural_numbers (void) {
    int i;
    int array[ARRAY_SIZE];

    i = 0;
    while ( i < ARRAY_SIZE + 2) {
        array[i] = i - 1;
        printf("array[%d] = %d\n", i, i - 1);
        i = i + 1;
    }
}
```

Memory layout and allocation

### The Compiling and Debugging Environment

gdb常用命令

### Variables and Addresses

P29代码，有关`strncmp()`

#### Addresses and Naming

```c
#include <stdio.h>

char globalchar1;
char globalchar2 = 'g';
int globalint1;
int globalint2 = 9;
char globalchar3;

int main(int argc, char* argv[]){
    char localchar1;
    char localchar2 = 'l';
    int localint1;
    int localint2 = 1;
    char localchar3;

    printf("Globals: '%c' (%d)\t'%c' (%d)\t%d\t%d\t'%c' %d\n",
           globalchar1, globalchar1,
           globalchar2, globalchar2,
           globalint1, globalint2,
           globalchar3, globalchar3);

    printf("Locals: '%c' (%d)\t'%c' (%d)\t%d\t%d\t'%c' %d\n",
           localchar1, localchar1,
           localchar2, localchar2,
           localint1, localint2,
           localchar3, localchar3);

    return 0;
}
```

在`lldb`编译器（64位）环境下，观察各全局变量和局部变量在内存中的地址：

```shell
(lldb) p &globalchar1
(char *) $0 = 0x0000000100001030 <no value available>
(lldb) p &globalchar2
(char *) $1 = 0x0000000100001028 "g"
(lldb) p &globalint1
(int *) $2 = 0x0000000100001034
(lldb) p &globalint2
(int *) $3 = 0x000000010000102c
(lldb) p &globalchar3
(char *) $4 = 0x0000000100001038 <no value available>
(lldb) p &localchar1
(char *) $5 = 0x00007ffeefbff607 <no value available>
(lldb) p &localchar2
(char *) $6 = 0x00007ffeefbff606 "l"
(lldb) p &localint1
(int *) $7 = 0x00007ffeefbff600
(lldb) p &localint2
(int *) $8 = 0x00007ffeefbff5fc
(lldb) p &localchar3
(char *) $9 = 0x00007ffeefbff5fb <no value available>
```

可以发现如下规律：

1. 全局变量的地址，远低于局部变量的地址。
2. 在全局变量中，地址从低到高分别为：`globalchar2` < `globalint2` < `globalchar1` < `globalint1` < `globalchar3` ,且有：已初始化的变量地址，低于未初始化的变量地址。
3. 在局部变量中，地址从低到高分别为：`localchar3` < `localint2` < `localint1` < `localchar2` < `localchar1` ，且有：变量之间的地址顺序，与是否初始化无关，而只与声明的顺序有关（在64位lldb编译器环境下，先声明的局部变量地址高。由于堆栈的内存空间向下生长，即：先声明的局部变量，先分配内存空间）。



C语言模型所申请的内存空间地址如下图所示：

![2](http://img.my.csdn.net/uploads/201801/06/1515224068_1766.png)

- Addresses are clustered in three groups:
  - all the **local variables** are clustered together.
  - of the **global variables**, those that are **initialized** when declared are in one cluster.
  - and that those that are **not initialized** are in another.
- Uninitialized global variables seem to be somehow initialized to **ZERO**
- Local variables start up with arbitrary values.
- Padding

注：上述结论均在32位编译环境下。在64位环境下，如未声明的局部变量初始值等，或稍有不同。



除上图所示之外，内存地址自下而上亦可分为如下几个区域：

- 文本区
- 数据区
  - 初始化的全局变量数据区
  - 未初始化的全局变量数据区
- 堆区
- ...
- 共享库区(shared libraries)
- ...
- 堆栈区
- 内核区

#### Pointers: Stored Addresses

- C allows us to declare variables that hold theaddresses of data, rather than holding datadirectly.
- In a real machine, outside the programming language abstraction, data of any type is represented with nothing but bits.
- It is the **compiler** that interprets those bits as integers or as characters.

#### Arrays and Strings

How to Initialize Arrays?

```c
#include <stdio.h>

int main(int argc, char* argv[]){
    float banana [5] = { 0.0, 1.0, 2.72, 3.14, 25.625 };
    int cantaloupe[2][5] = { {10, 12, 3, 4, -5}, {31, 22, 6, 0, -5},
    };
    int rhubarb[ ][3] ={ {0,0,0}, {1,1,1}, };
    
    char vegetables[ ][9] = { "beet", "barley",
        "basil", "broccoli", "beans" };
    
    char *vegetables_2[ ] = { "carrot", "celery",
        "corn",
        "cilantro",
        "crispy fried potatoes" };
    
    int *weights[ ] = {
        { 1, 2, 3, 4, 5 }, { 6, 7 },
        { 8, 9, 10 }
    };
    
    int row_1[ ] = {1,2,3,4,5,-1}; /* -1 is end-of-row marker */
    int row_2[ ] = {6,7,-1};
    int row_3[ ] = {8,9,10,-1};
    
    int *weights_2[ ] = {
        row_1, row_2, row_3
    };
    
    return 0;
}
```

在上述初始化的过程中，需注意：

```shell
(lldb) p vegetables
(char [5][9]) $3 = {
  [0] = {
    [0] = 'b'
    [1] = 'e'
    [2] = 'e'
    [3] = 't'
    [4] = '\0'
    [5] = '\0'
    [6] = '\0'
    [7] = '\0'
    [8] = '\0'
  }
  ...
  [4] = {
    [0] = 'b'
    [1] = 'e'
    [2] = 'a'
    [3] = 'n'
    [4] = 's'
    [5] = '\0'
    [6] = '\0'
    [7] = '\0'
    [8] = '\0'
  }
}
(lldb) p vegetables_2
(char *[5]) $4 = ([0] = "carrot", [1] = "celery", [2] = "corn", [3] = "cilantro", [4] = "crispy fried potatoes")

```

…...



Advanced about Point and Array

- P46 extern

>  As **formal parameters** in a function definition `char s[ ];` and `char *s;` are equivalent.



- 左值与右值

![2](http://img.my.csdn.net/uploads/201801/06/1515227900_2686.png)

> A “**modifiable L-value**” is a term introduced by C. Hence, an arrayname **is an L-value but not a modifiable L-value**. The standard stipulates that an assignment operator MUST have a modifiable L-value as its left operand.

即：赋值语句的左侧必须为一个**可更改的左值**。

如：对数组`int a[5];`，则`a = 1;`为不合法的语句。因为`a`虽然为左值，但其为不可更改的左值。



**[Expanded]**

- extern & long
- an un-declared function return type




Allocation and Reference

```c
#include "stdio.h"

void Initialize(char *x, char *y){
    x[0] = 'T';
    x[1] = 'h';
    x[2] = 'i';
    x[3] = 's';
    x[4] = ' ';
    x[5] = 'i';
    x[6] = 's';
    x[7] = ' ';
    x[8] = 'A';
    x[9] = '\0';
    
    y = x;
    y[8] = 'B';
}

#define ARRAY_SIZE 10
char a[ARRAY_SIZE];
char b[ARRAY_SIZE];

int main(int argc, char ** argv){
    Initialize(a, b);
    printf("%s\n%s\n", a, b);
    return 0;
}
```

输出结果：

```shell
This is B

Program ended with exit code: 0
```

### Data and Function Calls

- Scoping & Formal Parameters

  按值传递 & 按地址（引用）传递

  Eg：

  ```c
  #include <stdio.h>

  int first;
  int second;

  void callee(int * first){
      int second;
      second = 1;
      *first = 2;

      printf("callee: first = %d, second = %d\n", *first, second);
  }

  int main(int argc, char ** argv){
      first = 1;
      second = 2;
      callee(&first);
      printf("caller: first = %d, second = %d\n", first, second);
      return 0;
  }
  ```

  输出结果：

  ```
  callee: first = 2, second = 1
  caller: first = 2, second = 2
  Program ended with exit code: 0
  ```

- Recursion

  ```c
  #include <stdio.h>
  #include <stdlib.h>

  void callee(int n){
      if(n == 0) return;
      printf("%d (0x%08x)\n", n, (int)&n);

      callee(n-1);
      printf("%d (0x%08x)\n", n, (int)&n);

  }

  int main(int argc, char ** argv){
      int n;

      if(argc < 2){
          printf("USAGE: %s <integer>\n", argv[0]);
          return 1;
      }

      n = atoi(argv[1]);

      callee(n);
      return 0;
  }
  ```

  输出结果为：

  ```
  10 (0xefbff5ec)
  9 (0xefbff5cc)
  8 (0xefbff5ac)
  7 (0xefbff58c)
  6 (0xefbff56c)
  5 (0xefbff54c)
  4 (0xefbff52c)
  3 (0xefbff50c)
  2 (0xefbff4ec)
  1 (0xefbff4cc)
  1 (0xefbff4cc)
  2 (0xefbff4ec)
  3 (0xefbff50c)
  4 (0xefbff52c)
  5 (0xefbff54c)
  6 (0xefbff56c)
  7 (0xefbff58c)
  8 (0xefbff5ac)
  9 (0xefbff5cc)
  10 (0xefbff5ec)
  Program ended with exit code: 0
  ```

  Tips：

  - The compiler inserts additional code for every function call and every function return.
  - This is called **dynamic allocation**, because the **local variables** are allocated **at runtime**, as needed.
  - **Global variables** can be **allocated statically**: the compiler can fix specific addresses for global variables before the program executes.
  - The compiler does not know how many variables it will need to allocate dynamically, it **reserves a lot of space** for expansion.
  - This is why the local variables are allocated to addresses that are **very far away from** the addresses that hold the global variables.





- **[概念题]** Activation Records:

  - The chunk of memory **allocated for each function invocation** is called an activation record.
  - An activation record for a function invocation is created when the function is called, and it is destroyed when the function returns.

- Stacks

  - Activation records are organized in a stack. They are often also called **stack frames**, the activation record for the callee is pushed on top of that **of the caller**.

  - Access data from an activation record that is at the top of the stack.

  - **[概念题]** Stack Pointer & Frame Pointer

    The **stack pointer** holds the address where the stack ends-it is here that a **new** activation record will be allocated.

    The **frame pointer** holds the address where the previous activation record ends-it is to this value that the stack pointer will return when the current function **returns**.

- 回顾上方的Code Example：

  When a function is called, compiler and hardware:

  - Pushes the return address (the current program counter) into the stack.
  - Pushes the frame pointer into the stack.
  - Sets the frame pointer equal to the stack pointer.
  - Decrements the stack pointer by as many memory addresses as are required to store the local state of the callee.

  When a function returns, compiler and hardware:

  - Sets the stack pointer equal to the frame pointer.
  - Pops the value of the old frame pointer from the stack.
  - Pops the value of the return address from the stack.
  - Jumps to the return address.

  ​


- Conclusions

  ![2](http://img.my.csdn.net/uploads/201801/06/1515230755_4269.png)

  ​

  32位：

  ![2](http://img.my.csdn.net/uploads/201801/06/1515230756_3708.png)

  ​

  64位：

  ![2](http://img.my.csdn.net/uploads/201801/06/1515230757_1413.png)



### The Code Itself

**[Expanded]**

- A fetch-decode-execute circle
- Function Pointers
  - C doesn't provide a general way of manipulating the addresses of code, but the addresses of data.
  - **Changing the function pointer would also change the method invoked**.



- Register Allocation and Compiler Optimization

  - The Compiler: The CPU memory that is managed explicitly by the compiler is usually called a register bank, and the individual memory cells that store each data item are called registers.

    The Hardware: the hardware can create and destroy surrogates faster than the compiler, but it often creates surrogates for the wrong things; These memories are typically called caches

  - Optimization Level

    - Use rules of thumb to allocate registers to variables in ways that typically result in **pretty good, though not the best**, performance.
    - Compilers do not always use their most aggressive optimizations because:
      - Compilation with aggressive optimizations takes a long time.
      - Aggressive optimizations change the code substantially: It may be difficult, both for people and for debuggers, to relate the resulting machine code to the original source code.
      - Debugging, in particular, becomes almost impossible at high optimization levels.

---