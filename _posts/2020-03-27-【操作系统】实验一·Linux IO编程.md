---
title: 【操作系统】实验一·Linux I/O编程
tags:
 - 操作系统
 - 实验
---
# 任务1.
> 在当前用户目录下创建数据文件student.txt，文件的内部信息存储格式为Sname:S#:Sdept:Sage:Ssex，即“姓名:学号:学院:年龄:性别”，每行一条记录，输入不少于10条学生记录，其中包括学生本人记录。编写程序task41.c，从文件中查找Sdept字段值为“计算机与网络安全学院”的文本行，输出到文件csStudent.txt中，保存时各字段顺序调整为S#:Sname:Sage: Ssex:Sdept。

``` c
#include"wrapper.h"
#define MAXSIZE 101
int main() {	
	FILE *fp1,*fp2;
	if((fp1 = fopen("student.txt","r"))==NULL) {
		perror("Open1 Error");
		return 0;
	}
	if((fp2 = fopen("csStudent.txt","a+"))==NULL) {
		perror("Open2 Error");
		return 0;
	}
	char buf[MAXSIZE];
	char name[] = "计算机与网络安全学院"; 
	while (!feof(fp1))
	{
		fgets(buf, MAXSIZE, fp1);
		if (strstr(buf, name))
		{
			fputs(buf, fp2);
		}
	}
	fclose(fp1);
	fclose(fp2);
	return 0;
}
```

# 任务2.

> 调用Unix I/O库函数，编写程序task42.c，从键盘读入5个学生的成绩信息，包括学号、姓名、语文、数学、英语，成绩允许有一位小数，存入一个结构体数组，结构体定义为：
typedef struct _subject {
char sno[20];	   //学号
char name[20];   //姓名
float chinese;	   //语文成绩
float math;		//数学成绩
float english;	   //英语成绩
   }  subject;
将学生信息，逐条记录写入数据文件data，最后读回第1、3、5条学生成绩记录，显示出来，检查读出结果是否正确。

**实验数据**
> 刘一:383:学生社区知行学院:18:女
陈二:100:计算机与网络安全学院:18:男
肖:201:计算机与网络安全学院:20:男
张三:122:学生社区知行学院:19:男
李四:231:计算机与网络安全学院:18:女
王五:534:计算机与网络安全学院:19:男
赵六:234:电子工程与智能化学院:18:女
孙七:786:计算机与网络安全学院:20:男
周八:347:生态环境与建筑工程学院:20:男
吴九:349:计算机与网络安全学院:18:女
郑十:846:经济与管理学院:19:男

**代码**
``` c
#include"wrapper.h"
typedef struct _subject {
	char sno[20]; //学号
	char name[20]; //姓名
	float chinese; //语文成绩
	float math; //数学成绩
	float english; //英语成绩
} subject;

int main() {
	subject stu[5];
	int fd = open("data",O_WRONLY|O_CREAT,0777);
	int i=0;
	for(;i<5;i++){
		scanf("%s %s %f %f %f",stu[i].sno,stu[i].name,&stu[i].chinese,&stu[i].english,&stu[i].math);
		write(fd,&stu[i],sizeof(subject));
	}
	close(fd);
	fd = open("data",O_RDONLY|O_CREAT,0777);
	subject temp;
	for(i=0;i<5;i++){
	read(fd,&temp,sizeof(subject));
	if(i==0||i==2||i==3)
		printf("%s %s %.1f %.1f %.1f\n",temp.sno,temp.name,temp.chinese,temp.english,temp.math);
}
	close(fd);
}
```

# 任务3.

> 在Linux环境下，可以调用库函数gettimeofday测量一个代码段的执行时间，请写一个程序task43.c，测量read、write、fread、fwrite函数调用所需的执行时间，并与prof/gprof工具测的结果进行对比，看是否基本一致。并对四个函数的运行时间进行对比分析。
提示：由于一次函数调用时间太短，测量误差太多，应测量上述函数多次（如10000次）运行的时间，结果才会准确。

``` c
#include"wrapper.h"
struct  timeval start;
struct  timeval end;

void time_fread(){
	FILE* fp = fopen("data1","r");
	int i=0;
	char arr[100];
	gettimeofday(&start,NULL);
	for(i=0;i<100000;i++)
		fread(arr,1,1,fp);
	gettimeofday(&end,NULL);
	printf("time_fread:%ld\n",end.tv_usec-start.tv_usec);
	fclose(fp);
	} 
void time_fwrite(){
	FILE* fp = fopen("data1","a+");
	int i=0;
	char arr[100];
	gettimeofday(&start,NULL);
	for(i=0;i<100000;i++)
		fwrite(arr,10,1,fp);
	gettimeofday(&end,NULL);
	printf("time_fwrite:%ld\n",end.tv_usec-start.tv_usec);
	fclose(fp);
	} 
void time_read(){
	int fd = open("data1",O_RDONLY);
	int i=0;
	char arr[100];
	gettimeofday(&start,NULL);
	for(i=0;i<100000;i++)
		read(fd,arr,10);
	gettimeofday(&end,NULL);
	printf("time_read:%ld\n",end.tv_usec-start.tv_usec);
	close(fd);
	} 
void time_write(){
	int fd = open("data1",O_WRONLY);
	int i=0;
	char arr[100];
	gettimeofday(&start,NULL);
	for(i=0;i<100000;i++)
		write(fd,arr,10);
	gettimeofday(&end,NULL);
	printf("time_write:%ld\n",end.tv_usec-start.tv_usec);
	close(fd);
	} 
int main(){
	time_fread();
	time_fwrite();
	time_read();
	time_write();
}

```
# 任务4.

> 在Linux系统环境下，编写程序task44.c,对一篇英文文章文件的英文单词词频进行统计。
（1）	以“单词：次数”格式输出所有单词的词频（必做）
（2）	以“单词：次数”格式、按词典序输出各单词的词频（选做）
（3）	以“单词：次数”格式输出出现频度最高的10个单词的词频
例如，若某个输入文件内容为：
     GNU is an operating system that is free software—that is, it respects users' freedom. 
The development of GNU made it possible to use a computer without software that would trample your freedom.
则输出应该是：
      GNU:2
      is:3
      it:2
      ……
提示：可以调用字符串处理函数、二叉树处理函数等库函数



``` c
#include"wrapper.h"
#define MAXSIZE 100
#define MAXWORD 20
int wordnum[MAXSIZE]= {0};
char word[MAXSIZE][MAXWORD],temp[MAXWORD];
int fd,i,j,k,count=0;

void strlowr(char *str)
{
    char *orign=str;
    for (; *str!='\0'; str++)
        *str = tolower(*str);
    return ;
}

void word_sort(){
	for(i=0;i<count;i++){
		int min=i;
		for(j=i;j<count;j++){
			char temp1[MAXWORD],temp2[MAXWORD];//change to lower to aviod ASKII problem
			strcpy(temp1,word[min]);
			strcpy(temp2,word[j]);
			strlowr(temp1);
			strlowr(temp2);
			if(strcmp(temp1,temp2)>0){//compare
				min = j;
			}
		}
		if(min!=j){
				int tempnum = wordnum[i] ;
				char tempword[MAXWORD];
				strcpy(tempword,word[i]);
				wordnum[i] = wordnum[min];
				strcpy(word[i],word[min]);
				wordnum[min] = tempnum;
				strcpy(word[min],tempword);
			}
	}
}
int main() {
	
	
	if((fd=open("4.txt",O_RDONLY))<0) {
		perror("Open Error");
		return 0;
	}
	for(i=0; i<MAXSIZE; i++) {
		char c;
		j=0;
		read(fd,&c,sizeof(char));
		while(c>='a'&&c<='z' || c>='A'&&c<='Z') { //read word
			temp[j++] = c;
			read(fd,&c,sizeof(char));
		}
		int flag=1,k=0;
		temp[j] = '\0';
		for(; k<count; k++) {
			if(strcmp(temp,word[k])==0) { //ident existence
				flag=0;
				wordnum[k]++; //add word number
			}
		}
		if(strcmp(temp,"\0")!=0 && flag) {
			strcpy(word[count],temp);
			wordnum[count++]++;//add new word
		}
	}
	printf("---print with the sequence of origin----\n");
	for(i=0; i<count; i++) {
		printf("%s:%d\n",word[i],wordnum[i]);
	}
	printf("---print with the sequence of dictionary----\n");
	word_sort();
	for(i=0; i<count; i++) {
		printf("%s:%d\n",word[i],wordnum[i]);
	}
	printf("---10 word with the highest frequency----\n");
	for(i=0;i<10;i++)
	{
		j=0;
		for(k=1;k<count;k++)
		{
			if(wordnum[k]>wordnum[j]) j=k;
		}
		printf("%s:%d\n",word[j],wordnum[j]);
		wordnum[j]=0;
	}
	return 0;
}

```

