# c语言笔记

## 存储类

### auto 
只能修饰局部变量，局部变量的默认的存储类型。
```C
auto int a;
```
自动类型转换, 无需声明变量a的类型。
```C
int array[2]={1,2};
auto a = array[0];
```
### register
register 存储类用于定义存储在寄存器中而不是 RAM 中的局部变量。这意味着变量的最大尺寸等于寄存器的大小（通常是一个字），且不能对它应用一元的 '&' 运算符（因为它没有内存位置）。
```C
register int num;
```
### static
static 修饰符也可以应用于全局变量。当 static 修饰全局变量时，会使变量的作用域限制在声明它的文件内。
在函数中声明一个局部变量，用static修饰，则该函数调用多次也不会使变量的值重置。

### extern 
extern 存储类用于定义在其他文件中声明的全局变量或函数。当使用 extern 关键字时，不会为变量分配任何存储空间，而只是指示编译器该变量在其他文件中定义
```C
// 文件1
int count;
extern void countprint();
int main(void){
    count=5;
    countprint();
}
```

```C
// 文件2
extern int count;
void countprint(){
    printf("%d",count);
}
```

## 浮点数

### float 和double
float 和double分别为单精度浮点数和双精度浮点数，输入分别使用占位符%f和%lf；
printf（）只能看到双精度，输出均使用%f,没有%lf。默认输出位数为6，如果想要更多位，使用%.10f。
而在C++中，double类型用%lf输出是正确的。

## 静态和动态数组
### 静态数组
内存分配：在程序编译时分配，存储在栈上或者全局数据区。
大小固定：数组大小在定义是确定，无法在运行时改变
```C
int* a[]={1,2,3,4};
int* b[10];
```
### 动态数组
内存分配：运行时分配，存储在堆上，使用malloc()和calloc(),包含在头文件<stdlib.h>里面
大小可变：运行时调整，可以用realloc()重新分配内存，并改变数组大小
```C
    int* array=(int*)malloc(size*sizeof(int));
    double* darray=(double*)malloc(size*sizeof(double));
```

### 枚举类型
enum枚举是 C 语言中的一种基本数据类型，用于定义一组具有离散值的常量。
```c
enum Day{
    MON, TUE, WED, THU, FRI, SAT, SUN
};
```
第一个枚举类型默认值为零，可修改默认值，后一个枚举类型的值比前一个大1。
```c
enum Day{
    MON,TUE,WED,THU,FRI,SAT,SUN
};
```

枚举类型的遍历
```C
#include <stdio.h>
enum day{
    a,b,c,d,e,f,g
} theday;
int main(void){
    theday=(enum day)1;
    printf("%d\n",theday);

    for(theday=a;theday<g;theday++){
        printf("%d",theday);
    }
}
```

## 指针
指针的运行，能使用++，--，- ，+
指针数组，可以定义一个char类型指针，指向字符串
```C
char *ptr[]={
    "MON",
    "TUS",
    "WEN",
}
printf("%s",ptr[0]);
```
c语言允许指针指向指针
```C
int *ptr1,**ptr2;
int a=100;
ptr1=&a;
ptr2=&ptr1;
printf("%d",**ptr2);//输出a的值
```
### 函数指针
```C
#include <stdio.h>

int max(int a, int b){
   return a>b?a:b;
}
int main(void){
   int a=2,b=4;
   int (*p)(int,int)=&max;
   printf("%d\n",max(a,b));
   printf("%d",p(a,b));
}
```
```c
#include <stdio.h>
int max(int a, int b){
   return a>b?a:b;
}
int maxOfArray(int *array,int size,int (*p)(int,int)){
   int result=0;
   for(int i=0;i<size;i++){
      result=p(array[i],result);
   }
   return result;
}
int main(void){
   int a[]={1,4,6,2,4};
   printf("%d",maxOfArray(a,5,max));
}
```
### 函数返回指针
```C
int* newarray(int size){
    int *p;
    p=(int*)malloc(size*sizeof(int))
    return p;
}
int main(void){
    int* p;
    p=newarray(5);
    p[0]=2;
    printf("%d",p[0]);
}


```
## 字符串
字符串实质上是以'\0'结尾的字符数组。
```C
char a[]={'a','b','c','\0'};
char b[5]={'a','b','c','\0'};
char b[]="abcd";
char b[14]="abcde";
```
### 字符串常用库函数
strlen(str),输出字符串大小，'\0'之前的字符数量，不包含'\0'
strcmp(str1,str2),比较两个字符串，大于则大于0，等于则等于0，小于则输出负数
strcat(str1,str2),str2复制到str1中
strchr(str1,char1),返回指针
strstr(str1,substr),返回指针，substr开始的子字符串
```C
#include <stdio.h>
#include <string.h> 
int main ()
{
   char str1[14] = "runoob";
   char str2[14] = "google";
   char str4[5]={'a','b','c'};
   printf("%s\n",str4);
   char str3[14] = "hello,world";
   char str[]="noob";
   int  len ;
   //比较两个字符串
   printf("%d\n",strcmp(str1,str2));
   //连接两个字符串
   printf("%s\n",strcat(str1,str2));
   //将str2复制到str3
   strcpy(str3,str2);
   printf("strcpy()输出为%s\n",str3);
   // 返回一个char指针，指向str1中以第一个被发现的‘a’开始的子串
   printf("%s\n",strchr(str1,'b'));
   // 返回一个char类型指针，指向str1中以第一个str开始的子串
   printf("%s",strstr(str1, str));
   return 0;
}
```

## union共同体
结构体占用的内存大于等于所有成员占用的内存的总和（成员之间可能会存在缝隙），共用体占用的内存等于最长的成员占用的内存。共用体使用了内存覆盖技术，同一时刻只能保存一个成员的值，如果对新的成员赋值，就会把原来成员的值覆盖掉。
1. 如果同时使用共同体的两个变脸，会导致结果出错。如下所示，data.i和data.j结果可能出现异常，data.ch正常。
```C
#include <stdio.h>
#include <string.h>
union Data
{
   /* data */
   int i;
   float j;
   char ch[20];
};

int main ()
{
   union Data data;
   data.i=10;
   data.j=600.1236;
   printf("%d\n",data.i);
   printf("%f\n",data.j);
   strcpy(data.ch, "hello world");
   printf("%s\n",data.ch);
}
```

## 位域
### 位域的特点和使用方法如下：
C 语言的位域（bit-field）是一种特殊的结构体成员，允许我们按位对成员进行定义，指定其占用的位数。
1. 定义位域时，可以指定成员的位域宽度，即成员所占用的位数。
2. 位域的宽度不能超过其数据类型的大小，因为位域必须适应所使用的整数类型。
3. 位域的数据类型可以是 int、unsigned int、signed int 等整数类型，也可以是枚举类型。
4. 位域可以单独使用，也可以与其他成员一起组成结构体。
5. 位域的访问是通过点运算符（.）来实现的，与普通的结构体成员访问方式相同。
```C
#include <stdio.h>
#include <string.h>

struct S1
{
   int i;
   int j;
}s1;
struct S2
{
   int i:1;
   int j:1;
}s2;
struct S3
{
   int i : 16;
   int j : 16; 
}s3;
struct S4
{
   int i : 16;
   int j : 17;
}s4;
int main ()
{
   printf("%lu\n",sizeof(s1)); //输出8
   printf("%lu\n",sizeof(s2)); //输出4
   printf("%lu\n",sizeof(s3)); //输出4
   printf("%lu\n",sizeof(s4)); //输出8，因为33位，超出4字节（int的大小），还有一位需要用一个4字节的空间来存储
}
```














