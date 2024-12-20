
第一篇博客，博客园注册很久却一直没有好好利用，今天把以前的文章都删掉，就当开个好头吧。


希望在以后的时间中，自己能够认真、努力、珍惜时间。


**零基础入门51单片机**


单片机（Microcontroller Unit，MCU）是一种集成电路芯片，它将计算机的CPU、存储器（RAM和ROM）、输入/输出接口（I/O）等集成在一个芯片上，形成一个**完整的微型计算机系统**。单片机广泛应用于嵌入式系统和自动化控制领域。


**核心部件：**


**中央处理器（CPU）**执行程序指令。


**存储器**只读存储器（ROM）和随机存储器（RAM），前者用来存储固件或程序代码，后者存储临时数据「掉电丢失」。


**I/O接口**：输入/输出的作用是与外部设备进行数据交换。


**功能特点：**单片机具有低功耗、体积小、成本低、高性能、灵活性的特点。


总的来说现在的单片机应用面越来越广，可以使用微型电池供电应用在空间受限的地方，此外成本低廉适合企业大规模生产，性能也越来越接近传统的微处理器，而且可以根据需要定制不同的功能。


在这里我会编写第一个实例程序：点亮一个LED灯「过于简单，就当 Hello,World! 吧」


需要使用到工具：普中51单片机开发版、电脑、Keil5软件（关于Keil软件的安装直接B站搜索,开发版可以暂时使用Proteus）


**关于LED灯的介绍：**


LED灯又叫发光二极管，和普通的二极管相同，只允许电流单向导通。


从图中可以看到LED灯的接线方式，VCC表示高电平1，因此如果想让LED点亮只需给单片机P2端口低电平0就可以实现。
![image](https://img2024.cnblogs.com/blog/2636147/202411/2636147-20241125205933564-242032580.png)


**代码编写：**



```
#include //REGX52.H由Keil公司提供，头文件包含了对单片机特殊功能寄存器（SFR）的宏定义
void main()
{
	P2 =0XFE; 	//1111 1110  操作单片机P2 I/O口 点亮第一个LED
	while(1)
	{
	
	}
}

```

51单片机直接操作P2端口需要使用16进制。"0XFE" 中"0X"表示这个数字为16进制的符合，"FE" 转换为2进制为 "0111 1111"，这个如果不熟悉可以自行学习。



```
10进制:	1	2	3	4	5	6	7	8	9	10	11	12	13	14	15
16进制:	1	2	3	4	5	6	7	8	9	A	B	C	D	E	F
02进制:	0001	0010	0011	0100	0101	0110	0111	1000	1001	1010	1011	1100	1101    1110	1111

```

![image](https://img2024.cnblogs.com/blog/2636147/202411/2636147-20241125205959602-97149089.png)


运行实例：由于程序简单，这里使用Proteus仿真软件模拟运行
![image](https://img2024.cnblogs.com/blog/2636147/202411/2636147-20241125210046143-861182354.png)


第二个程序：LED灯闪烁


设计思路：只需要将第一个LED间隔一段时间熄灭、点亮就可以实现闪烁现象。


设计延时函数：



```
void Delay(unsigned int n)
{
    unsigned char j;
    while(n--)
    {
        for(j = 0; j < 113; j++)
    }
}

```

合并代码如下：



```
#include

void Delay(unsigned int n)	// 延时函数，延时大约1ms
{
	unsigned char j;
	while(n--)
	{
		for(j = 0;j < 113; j++);
	}
}

void main()
{
	while(1)
	{
		P2 = 0XFE;	//点亮一个LED灯
		Delay(500);
		P2 = 0XFF;	//全部置高电平,熄灭
		Delay(500);
		
	}	
}

```

实验现象：
![image](https://img2024.cnblogs.com/blog/2636147/202411/2636147-20241125210112560-920482367.gif)


第三个程序：设计LED流水灯


设计思路：由于是初次入门这里只介绍最简单的实现方式，根据前面两个实验知识，依次点亮、熄灭LED灯，就能实现流水灯效果。



```
#include
void Delay(unsigned int n)
{
	unsigned char j;
	while(n--)
	{
		for(j = 0; j < 113; j++);
	}
}
void main()   
{
	while(1)  //  8 4 2 1
	{
		P2 = 0XFE; // 1111 1110
		Delay(500);
		P2 = 0XFD; // 1111 1101
		Delay(500);
		P2 = 0XFB; // 1111 1011
		Delay(500);
		P2 = 0XF7; // 1111 0111
		Delay(500);
		P2 = 0XEF; // 1110 1111
		Delay(500);
		P2 = 0XDF; // 1101 1111
		Delay(500);
		P2 = 0XBF; // 1011 1111
		Delay(500);
		P2 = 0X7F; // 0111 1111
		Delay(500);	
	}
}

```

实验现象：
![image](https://img2024.cnblogs.com/blog/2636147/202411/2636147-20241125210135475-1161634081.gif)


 本博客参考[蓝猫机场加速器](https://dahelaoshi.com)。转载请注明出处！
