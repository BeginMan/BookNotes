这节的知识点有：

- 数据类型大分类
- 头文件的使用
- 共享代码
- make

# 一. 数据类型
人家[C语言基本数据类型简介](http://www.cnblogs.com/onedime/archive/2012/11/21/2780149.html) 比我总结的好，我就盗一张图：😊

![](https://raw.githubusercontent.com/BeginMan/BookNotes/master/C/media/data.jpg)

摘取《Head First C》里面的一些小技巧：

- `(float) x / y`: 编译器会自动把y的类型转换为float
- 所有的数据类型默认都是有符号(`signed`)的，存储器中符号占用一位， `unsigned`:修饰非负数，由于无需记录负数，无符号数有更多的位可用。

# 二.头文件的使用
下面的例子：

	int main() {
		func();
	}
	void func() {...}

会报错，如果把func放在main()函数前，在main()调用它之前先定义，那么就ok，why? **因为编译器发现一个不认识的函数调用，不知道该函数的返回类型，就假设为`int`类型，等后面编译的时候编译器看到实际的函数，它认为有两个同名的函数，一个是文件中的函数，一个是编译器假设返回int的那个**

如何去避免呢：**有没有办法让编译器一开始就知道函数的返回值类型呢？为了防止编译器假设函数的返回类型,你可以显式地告诉它。告诉编
译器函数会返回什么类型的语句就叫`函数声明`。**

## 1.声明与定义分离

	float func(int age);

函数声明包括：函数名，返回值类型，形参类型，以`;`结束，没有函数体.

**一旦声明了函数,编译器就不需要假设,完全可以先调用函数,再定义函数。**

对于上面的问题，可以这样写：
	
	int main(int argc, char *argv[]) {
	    void func();
	    func();
	    return 0;
	}

	void func(){
	    printf("come on baby!");
	}


如果有多个这样的玩意，我们就需要在main函数中一次次的声明，比较好的方式是可以放在**头文件**

## 2.头文件

创建头文件，func.h, 将函数声明写在此：

	#ifndef intoC_func_h
	#define intoC_func_h

	void func();

	#endif


然后上面程序就可以在main函数中不用再函数声明了.

# 三.共享代码
- 为了共享代码，可以把代码放在一个单独的C文件中，gcc编译`gcc main.c func.c commonFunction.c -o main`把所有东西编译在一起
- 如果想共享变量，可以在头文件中声明，并把变量名前加上`extend`关键字，如：`extend int passwd`; 

# 四.make让你编译智能化
一旦项目大了起来，零零碎碎编辑了一些源文件，不可能每次都gcc a.c b.c ....... -o xxx, 因为编译过程很慢，每次小小的改动就编译整个项目是在很残暴，一个很好的解决方案是：**吃核桃补脑，记住你编辑过的文件，然后`gcc -c 你已经编辑过的文件.c`， 只需要编译你改动过的文件，然后`gcc *.o -o main` 重新链接所有目标文件**， 这个需要在开始之前就要分步处理：

	gcc -c *.c  // 为所有源文件创建对应的目标文件
	gcc *.o -o main // 重新链接所有目标文件

有时候脑瓜总忘了到底特么编辑过哪些文件，这时候`make`该出场了，可以用`make`工具自动化构建，**make是一个可以替你运行编译命令的工具。make会检查源文件
和目标文件的时间􏵘,如果目标文件过期,make就会重新编译它。**

使用make工具要编写Makefile或makefile文件：如：

	a.txt:b.txt c.txt
		cat b.txt c.txt > a.txt

a.txt依赖b.txt,c.txt,通过tab后的命令创建a.txt

对C的构建实例如下：

	main.o: main.c Mencrypt.c Mencrypt.h
		gcc -c main.c
	Mencrypt.o: Mencrypt.c Mencrypt.h
		gcc -c Mencrypt.c
	main: main.o Mencrypt.o
		gcc main.o Mencrypt.o -o main

更多内容参考[Make 命令教程](http://www.ruanyifeng.com/blog/2015/02/make.html)
