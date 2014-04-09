##hello.c##

~~~c
#include <stdio.h>

main(){
	printf("Hello, world!\n");
} 

~~~
##main.c##

~~~c
#include <stdio.h>
#include <stdlib.h>

/* run this program using the console pauser or add your own getch, system("pause") or input loop */

int main(int argc, char *argv[]) {
	return 0;
}

~~~
##temperature.c##

~~~c
#include <stdio.h>

main(){
	float fahr, celsius;
	float lower, upper, step;
	
	lower = 0;
	upper = 300;
	step = 20;
	
	
	fahr = lower;
	while (fahr <= upper) {
		celsius = (fahr-32.0) * (5.0 / 9.0);
		printf ("%3.0f\t%6.1f\n", fahr, celsius);
		fahr = fahr + step;
	}
}

~~~
##temperature2.c##

~~~c
#include <stdio.h>

main(){
	float fahr;
	for (fahr = 0.0; fahr <=300.0; fahr = fahr + 20.0){
		printf("%3.0f %6.1f\n", fahr, (5.0/9.0)*(fahr-32));
	}
		
}

~~~
##getchar.c##

~~~c
#include <stdio.h>

main(){
	int c;
	c = getchar();
	
	while (c != EOF){
		putchar(c);
		
	}
}

~~~
##counter.c##

~~~c
#include <stdio.h>

main(){
	long nc;
	nc = 0;
	while (getchar() != EOF){
		++nc;
	}
	printf("%ld\n", nc);
}

~~~
##countline.c##

~~~c
#include <stdio.h>
/*Ctrl+Z结束输入，练习1-8*/
main(){
	int c, nl;
	nl = 0;
	int t = 0;
	int b = 0;
	while ((c = getchar()) != EOF){
		if (c == '\n'){
			/*行数*/ 
			++nl;
		}
		if (c == ' '){
			/*空格*/
			++b;
		}
		if (c == '\t'){
			/*制表符*/ 
			++t;
		}
	} 
	printf("%d%d%d\n", nl, b, t);
}

~~~
##ex1_9.c##

~~~c
#include <stdio.h>
/* 输入复制到输出，要求连续的空格用一个空格代替 */
main(){
	int out;
	int bc = 0;
	while ((out = getchar()) != EOF){
		if (out == ' ')
			++bc;
		else {
			if (bc != 0){
				putchar(' ');
				putchar(out);
				bc = 0;
			}
			else
				putchar(out);
		}
	}
	return 0;
} 

~~~
##wc.c##

~~~c
#include <stdio.h>

#define IN 1
#define OUT 0

main(){
	int c, nl, nw, nc, state;
	
	state = OUT;
	nl = nw = nc = 0;
	while ((c = getchar()) != EOF){
		++nc;
		if (c == '\n')
			++nl;
		if (c == ' ' || c == '\n' || c == '\t')
			state = OUT;
		else if (state == OUT){
			state = IN;
			++nw;
		}
	}
	printf("%d %d %d \n", nl, nw, nc);
} 

~~~
##digits.c##

~~~c
#include <stdio.h>
/* count digits, white space, others */

main(){
	int c, i, nwhite, nother;
	int ndigit[10];
	
	nwhite = nother = 0;
	for (i=0; i < 10; ++i)
		ndigit[i] = 0;
	while ((c = getchar()) != EOF)
		if (c >= '0' && c <= '9'){
			printf("%d and %d", c, c - '0');
			++ndigit[c-'0'];
		}
			
		else if (c == ' ' || c == '\n' || c == '\t')
			++nwhite;
		else
			++nother;
	printf("digits =");
	for (i = 0; i < 10; i++)
		printf("%d", ndigit[i]);
	printf(", white space = %d, other = %d\n", nwhite, nother);
}


~~~
##ex1_13.c##

~~~c
#include <stdio.h>
/* 打印单词长度的直方图 */ 
/* C语言不支持动态数组，所以。。。lwdigit[10000]，不够大新建数组复制数据 */ 
main(){
	int c, nc, onc, no, i, j;
	nc = onc = no = 0;
	int lwdigit[10000];
	int ldigit[100];
	int mdigit[100];
	int nw = 0;
	int isword = 0; 
	while ((c = getchar()) != EOF){
		++nc;
		/*词汇计数*/
		if (c == ' ' || c == '\t' || c == '\n'){
			isword = 0;	
			++no;
		}
		else if (isword == 0){
			isword = 1;
			++nw;
			
			lwdigit[nw - 2] = nc - onc - no;
			onc = nc;
			no = 0;
		}
	}
	printf("nw %d \n", nw);
	for (i = 0; i < 100; i++){
		ldigit[i] = i;
		mdigit[i] = 0;
	}
	int nx = 0;
	for (i = 0; i <= (nw - 1); i++){
		printf("%2d|", lwdigit[i]);
		for (j = 0; j < 100; j++){
			if (lwdigit[i] == (ldigit[j] + 1)){
				++mdigit[j];
				if (j > nx)
					nx = j; 
				
			}
		}
	}
	printf("\n---开始打印水平直方图---\n");
	for (i = 0; i <= nx; i++){
		if (mdigit[i] == 0){
			printf("%2d(%2d) |\n", i, mdigit[i]);
		}
		else {
			printf("%2d(%2d) |", i, mdigit[i]);
			for (j = 0; j <= mdigit[i]; j++)
				printf("]");
			printf("\n");
		}
	}
	
	printf("\n---开始打印竖直直方图---\n");
	int height = 0;
	for (i = 0; i < nx; i++)
		if (mdigit[i] > height)
			height = mdigit[i];
	
	for (i = 0; i < height; i++){
		for (j = 0; j < nx; j++){
			if ((height - i) > mdigit[j])
				printf("%4s", " ");
			else 
				printf("%4s", "[]");
		}
		printf("\n");
	} 
	for (i = 0; i < nx; i++)
		printf("%4d", i);
	printf("----->\n");
	for (i = 0; i < nx; i++)
		printf("(%2d)", mdigit[i]);
	
}

~~~
##bk1_7.c##

~~~c
#include <stdio.h>
int power(int m, int n);

main(){
	int i;
	for (i = 0; i < 10; ++i){
		printf("%d %d %d \n", i, power(2,i), power(-3,i));
	}
	return 0;
}

int power(int base, int n){
	int i, p;
	p = 1;
	for (i =1; i <= n; ++i){
		p = p * base;
	}
	return p;
}

~~~
##ex1_15.c##

~~~c
#include <stdio.h>
/*摄氏华氏互转函数*/
float convert(float, char);
float convert(float t, char n){

	if (n == 'C')
		return (9.0/5.0) * t + 32;
	else if(n == 'F')
		return (5.0/9.0) * (t-32);
	else
		return 0;
}
main(){
	float f, c;
	for(f = 0; f < 300; f = f + 10)
		printf("F2C: %3.1f, %3.1f \n", f, convert(f, 'F'));
	for(c = 0; c < 100; c = c + 10)
		printf("C2F: %3.1f, %3.1f \n", c, convert(c, 'C'));
	
}

~~~
##bk1_9.c##

~~~c
#include <stdio.h>
#define MAXLINE 1000
/* Bug */
int getline(char line[], int maxline);
void copy(char to[], char from[]);

main(){
	int len;
	int max;
	char line[MAXLINE];
	char longest[MAXLINE];
	max = 0;
	while ((len = getline(line, MAXLINE)) > 0){
		if (len > max){
			max = len;
			copy(longest, line);
		}
	}
	if (max > 0){
		printf("%s", longest);
	}
	return 0;
}

int getline(char s[], int lim){
	int c, i;
	for (i=0; i < lim -1 && (c = getchar()) != EOF && c != '\n'; ++i)
		s[i] = c;
	if (c == '\n'){
		s[i] = c;
		++i;
	}
	s[i] = '\0';
	return i; 
}

void copy(char to[], char from[]){
	int i;
	i = 0;
	while ((to[i] = from[i] != '\0'))
		++i;
}

~~~
##test.c##

~~~c
#include <stdio.h>

main(){
	float i, j;
	for (i = 0; i < 100000000; ++i){
		j = j + i;
	}
	printf("%d", j);
}

~~~
##bk1_10.c##

~~~c
#include <stdio.h>
#define MAXLINE 1000

int max;
char line[MAXLINE];
char longest[MAXLINE];

int getline(void);
void copy(void);

main(){
	int len;
	extern int max;
	extern char longest[];
	max = 0;
	while ((len = getline()) > 0){
		if (len > max){
			max = len;
			copy();
		}
	}
	if (max > 0){
		printf("%s", longest);
	}
	return 0;
}

int getline(void){
	int c, i;
	for (i=0; i < MAXLINE -1 && (c = getchar()) != EOF && c != '\n'; ++i)
		line[i] = c;
	if (c == '\n'){
		line[i] = c;
		++i;
	}
	line[i] = '\0';
	return i; 
}

void copy(void){
	int i;
	extern char line[], longest[];
	i = 0;
	while ((longest[i] = line[i] != '\0'))
		++i;
}

~~~
##ex1_22_AutoNewLine.c##

~~~c
#include <stdio.h>
#define MAXLINE 80 
#define MAXCHAR 1000
/* 英文智能折行 */

main(){
	int i, c;
	i = 0;
	char s[MAXCHAR];
	while ((c = getchar()) != EOF){
		s[i] = c;
	}
	printf("%c%d%f", s[1], s[1], s[1]);
	putchar(s[1]);
	
	return 0;
}

 

~~~
##test2.c##

~~~c
#include <stdio.h>

main(){
	char c[] = "hello, world";
	printf("%s", c);
	
	int d;

	
	
}

~~~
##ex2_1.c##

~~~c
#include <stdio.h>
#include <limits.h>
#include <float.h>
#include <time.h>

main(){
	
	printf("CHAR_BIT:%d\nCHAR_MIN:%d\nCHAR_MAX:%d\n", CHAR_BIT, CHAR_MIN, CHAR_MAX);
	printf("INT_MAX:%d\nLONG_MAX:%d\n", INT_MAX, LONG_MAX);
	printf("FLT_MAX:%e\nDBL_MAX:%e\n", FLT_MAX, DBL_MAX);
	
	int int_max;
	
	clock_t start, finish;
	start = clock();
	for (int_max = 0; int_max < int_max + 1; int_max++){
	}
	finish = clock();
	printf("int_max:%d //time:%.3fs", int_max, (double)(finish - start) / CLOCKS_PER_SEC);
	
}

~~~
##ex2_3.c##

~~~c
#include <stdio.h>
#include <math.h>
/* 将形如0x的16进制转换为10进制 */ 
main(){
	char hex[] = "0X10fD";
	printf("%s%s\nint: %d","hex: ", hex, htoi(hex));
	return 0;
}

int htoi(char hex[]);
int htoi(char hex[]){
	/* 出错返回-1 */ 
	int i, j;
	int len = 0;
	int anser = 0;
	for (i = 0; hex[i] != '\0'; ++i)/* 连获取一个数组长度都要自己for遍历。。。*/
	len = i + 1;
	if (hex[0] == '0' && hex[1] == 'x' || hex[1] == 'X' ){/* 判断是不是0x/0X开头 */
		for (i = 2; i < len; i++){
			if ('0' <=  hex[i] && hex[i] <= '9')/* 连这个都要分开写 */ 
				j = hex[i] - '0';
			else if ('a' <= hex[i] && hex[i] <= 'f')
				j = hex[i] - 'a' + 10;
			else if ('A' <= hex[i] && hex[i] <= 'F')
				j = hex[i] - 'A' + 10;
			else
				return -1;
			anser = anser + pow(16, (len - 1 - i)) * j;
		}
	}
	else 
		return -1;
	return anser;
}

~~~
##test3.c##

~~~c
/*
	FileName: foreachString.cpp
	Author: ACb0y
	Create Time: 2011年3月20日22:20:33
	Last Modify Time: 2011年3月20日22:46:43
 */
#include <iostream>
using namespace std;
void foreachStringOne(char * str)
{
	int len = strlen(str);
	for (int i = 0; i < len; ++i) 
	{
		printf("%c", str[i]);
	}
	printf("/n");
}
void foreachStringTwo(char * str) 
{
	while (*str) 
	{
		printf("%c", *str);
		++str;
	}
	printf("/n");
}
void print(char c)
{
	printf("%c", c);
}
void foreachStringThree(char * str) 
{
	for_each(str, str + strlen(str), print);
	printf("/n");
}
int main()
{
	char str[] = "hello word!";
	foreachStringOne(str);
	foreachStringTwo(str);
	foreachStringThree(str);
	return 0;
}


~~~
