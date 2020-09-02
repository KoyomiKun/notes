# Concept of Computer

## 1. Questions

***转16进制：*** 

add $s0, $t1, $t2   [012A8020]

sw $t1, 32( $t2 )     [AD490020]

R0:bne $s0, $t0, R0 [1608FFFF]

addi $t1, $t0, 1234 [210904D2]

addi $sp, $sp, -6   [23BDFFFA]

sw $ra, 1234( $sp )   

lh $t0, 567( $t0 )

sltu $t0, $s0, $t0

***反汇编：*** 


02008080

8E10007B

0200F809

AFB00002

***编写与C函数对等的mips汇编程序， 注意子程序调用规范。调用参数、返回参数、主叫保存caller saved， 被叫保存callee saved*** 


```c
int jia(int x, int y){
	return x+y;
}




int equ(int x, int y){
	return x==y?1:0;
}



int min(int x, int y){
	return x<y ? x:y;
}
min:
slt $t0, $a0, $a1
bne $t0, $0, A
j Exit
A:
add $v0, $0, $a0
Exit:
jr $ra

int max(int x, int y){
	return x>y ? x:y;
}



int avg(int x, int y){
	return (x+y) /2;
}
```






已知段表基地址在：GDTR，逻辑地址：段：偏移，求线性地址
1. 实模式
2. 保护模式

## 2. Review

1. 数据表达 小题目 原补移浮点 加减法 无穷大怎么表示 

2. 指令部分： 以给的推荐指令为准

汇编程序段：溢出判断等
```mips
($s1, $s0) = ($s3, $s2) + ($s5, $s4)

addu $s0, $s2, $s4 # 低位相加
sltu $s1, $s0, $s2 # 溢出判断
add  $s1, $s1, $s3
add  $s1, $s1, $s5 # 高位加进位
```





完整的程序：规范， 系统调用 

```mips
la $a0, hi  # 内容参数
addi $v0, 4 # 4号中断
syscall
```

一段完整的程序：
```c
void main()
{
	char hi[20] = "input a char:", ch;
	ch = getch();
	printf("the ASCii is %d", ch);
}


#ch -> $s0
Hi    .zjie    "input a char: ", 0
Hj    .zjie    "ASCii is ", 0
main:push     $s0     #callee  

	 la		  $a0,hi
	 addi     $v0,$0,4
	 syscall

	 la	      $a0,hj
	 addi     $v0,$0,4
	 syscall

	 addi     $v0,$0,12
	 syscall  

	 li       $v0,11
	 syscall

	 pop      $s0
	 jr       $ra

```
***不考察机器码转换什么的*** 



3. CPU  设计图都要了解 要能画出来

每个组建什么意思

设计指令

例：bgezal rs, ofs
if(rs>=0){$ra=PC;PC+=ofs}

控制信号




