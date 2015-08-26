to know:
# 一.指针
>根据出现的位置不同，操作符 `*` 既可以用来声明一个指针变量，也可以用作指针的取值。**当用在声明一个变量时，`*`表示这里声明了一个指针。其它情况用到`*`表示指针的取值**。

	int *ptr;           // 声明一个int指针
    int val=1;          // 声明一个int值
    ptr = &val;         // 为指针分配一个int值的引用
    int ptrV = *ptr;    // 对指针进行取值，打印存储在指针地址中的内容
    printf("%d\n", ptrV);

`&`是地址操作符，用来引用一个内存地址。通过在变量名字前使用&操作符，我们可以得到该变量的内存地址

>这里可以把指针、引用和值的关系类比为信封、邮箱地址和房子。一个指针就好像是一个信封，我们可以在上面填写邮寄地址。一个引用（地址）就像是一个邮件地址，它是实际的地址。取值就像是地址对应的房子。我们可以把信封上的地址擦掉，写上另外一个我们想要的地址，但这个行为对房子没有任何影响。

## 1.1 void指针、NULL指针和未初始化指针
一个指针可以被声明为void类型，比如void *x。一个指针可以被赋值为NULL。一个指针变量声明之后但没有被赋值，叫做未初始化指针。

NULL指针被初始化为o。NULL是一个特殊的地址，用NULL赋值的指针指向的地址为0而不是随机的地址。只有当你准备使用这个地址时有效。不要对NULL地址取值，否则会产生段错误。

	int *uninit;            // int指针未初始化
    int *nullptr = NULL;    // 初始化为NULL
    void *vptr;             // void指针未初始化
    
    int val = 1;
    int *iptr;
    int *castptr;
    
    // void类型可以存储任意类型的指针或引用
    iptr = &val;
    vptr = iptr;
    printf("iptr=%p, vptr=%p\n", iptr, vptr);       // iptr=0x7fff5fbff814, vptr=0x7fff5fbff814
    
    // 通过显式转换可以将void类型转换成int类型
    castptr=(int *)vptr;
    printf("*castptr=%d\n", *castptr);              //*castptr=1
    
    // 打印NULL和未初始化指针
    printf("nullptr=%p, uninit=%p", nullptr, uninit);//nullptr=0x0, uninit=0x0

## 1.2 指针和数组

C语言的数组表示一段**连续的内存空间**，用来存储多个特定类型的对象。与之相反，**指针用来存储单个内存地址**。数组和指针不是同一种结构因此不可以互相转换。而**数组变量指向了数组的第一个元素的内存地址**。

一个数组变量是一个常量。即使指针变量指向同样的地址或者一个不同的数组，也不能把指针赋值给数组变量。也不可以将一个数组变量赋值给另一个数组。然而，**可以把一个数组变量赋值给指针**，这一点似乎让人感到费解。**把数组变量赋值给指针时，实际上是把指向数组第一个元素的地址赋给指针**。


	int myarray[4] = {1,2,3,0};
	int *ptr = myarray;
	printf("*ptr=%d\n", *ptr);
	 
	// 数组变量是常量，不能做下面的赋值
	// myarray = ptr
	// myarray = myarray2
	// myarray = &myarray2[0]

第1行初始化了一个int数组，第2行用数组变量初始化了一个int指针。由于数组变量实际上是第一个元素的地址，因此我们可以把这个地址赋值给指针。这个赋值与`int *ptr = &myarray[0]`效果相同，显示地把数组的第一个元素地址赋值到了ptr引用。这里需要注意的是，这里指针需要和数组的元素类型保持一致，除非指针类型为void。

## 1.3 指针与结构体
就像数组一样，指向结构体的指针存储了结构体第一个元素的内存地址。与数组指针一样，结构体的指针必须声明和结构体类型保持一致，或者声明为void类型。

	struct person {
        int age;
        char *name;
    }f;
    struct person *spvr;
    f.age = 20;
    char *fullname="beginman";
    f.name=fullname;
    spvr=&f;
    printf("age=%d, name=%s\n", f.age, spvr->name);
    return 0;


这里需要注意两个不同的符号，`.` 和 `->` 。结构体实例可以通过使用 `.` 符号访问age变量。对于结构体实例的指针，我们可以通过 `->` 符号访问name变量。也可以同样通过`(*ptr).name`来访问name变量。



- 指针的类型
- 指针所指向的类型
- 指针的值(指针所指向的内存区)
- 指针本身所占据的内存区

如下例子：

	int *ptr;
	char *ptr;
	int **ptr;
	int (*ptr)[3];
	int *(*ptr)[4];

# 指针的类型
指针本身所具有的类型，

参考：

[1.C语言指针5分钟教程](http://blog.jobbole.com/25409/)


