---
title: matlab保存矩阵到txt或dat文件中
date: 2020-12-15 21:08:38
tags:
---

**将matlab中数据输出保存为txt或dat格式**

**总结网上各大论坛，主要有三种方法。**

<!-- more -->

### **第一种方法：save（最简单基本的）**

具体的命令是：用save *.txt -ascii x
x为变量
*.txt为文件名,该文件存储于当前工作目录下，再打开就可以打开后,数据有可能是以指数形式保存的.

例子：

a =[17 24 1 8 15;23 5 7 14 16 ;4 6 13 20 22 ;10 12 19 21 3 ;11 18 25 2 9 ]；
save afile.txt -ascii a
afile.txt打开之后，是这样的：
1.7000000e+001 2.4000000e+001 1.0000000e+000 8.0000000e+000 1.5000000e+001
2.3000000e+001 5.0000000e+000 7.0000000e+000 1.4000000e+001 1.6000000e+001
4.0000000e+000 6.0000000e+000 1.3000000e+001 2.0000000e+001 2.2000000e+001
1.0000000e+001 1.2000000e+001 1.9000000e+001 2.1000000e+001 3.0000000e+000
1.1000000e+001 1.8000000e+001 2.5000000e+001 2.0000000e+000 9.0000000e+000

### **第二种方法：dlmwrite**

dlmwrite('a.txt',a,'precision','%10.0f')

或者是dlmwrite('a.txt',a,'delimiter', '\t')

对于只有一行或者一列的数据，很适用，但是多行的，就乱了

网上有很多这一类似的问题，但是都不是很理想

### **第三种方法：fopen+fprintf**

下面主要介绍这种方法，可以解决以上问题：用fprintf命令：以上面的例子为例：

#### **第一种情况：**

\>> a=[17 24 1 8 15;23 5 7 14 16 ;4 6 13 20 22 ;10 12 19 21 3 ;11 1825 2 9 ];
\>> fid = fopen('b.txt','wt');
fprintf(fid,'%g\n',a);       # \n 换行
fclose(fid);

然后用写字板打开b.txt，内容如下：为列向量

17
23
4
10
11
24
5
6
12
18
1
7
13
19
25
8
14
20
21
2
15
16
22
3
9

#### **第二种情况：**

对上面的命令做一下改动：# \n 换行改为\t，table键

\>> fid = fopen('b.txt','w');
fprintf(fid,'%g\t',a);
fclose(fid);

然后用写字板打开b.txt，内容如下：为行向量：

17 23 4 10 11 24 5 6 12 18 1 7 13 19 25 8 14 20 21 2 15 16 22 3 9 

#### **第三种情况：**

综合上面的两个结果，我们编写以下命令：

fid=fopen('b.txt','wt');%写入文件路径
[m,n]=size(a);
 for i=1:1:m
    for j=1:1:n
       if j==n
         fprintf(fid,'%g\n',a(i,j));
      else
        fprintf(fid,'%g\t',a(i,j));
       end
    end
end
fclose(fid);

然后用写字板打开b.txt，内容如下：矩阵

17 24 1 8 15
23 5 7 14 16
4 6 13 20 22
10 12 19 21 3
11 18 25 2 9

**说明：以上操作都是在当前的工作目录下完成！下面给出最一般的模型，大家可以试着自己操作,如果需要dat格式，直接把txt换为dat就可以**

fid=fopen('C:\Documents and Settings\cleantotal.ped','wt');%写入文件路径
matrix=input_mattrix                        %input_matrix为待输出矩阵
[m,n]=size(matrix);
 for i=1:1:m
   for j=1:1:n
      if j==n
        fprintf(fid,'%g\n',matrix(i,j));
     else
       fprintf(fid,'%g\t',matrix(i,j));
      end
   end
end
fclose(fid);

×××××××××××××××××××××××××××××××××××××××××××××××××××××××××

下面附了具体的matlab的fopen和fprintf函数具体解释，当然help一下是可以知道的，只是为了方便大家

matlab中fopen函数在指定文件打开的实例如下：

*1)“fopen”打开文件，赋予文件代号。
    语法1：FID= FOPEN（filename,permission）
用指定的方式打开文件
FID=+N(N是正整数)：表示文件打开成功，文件代号是N.
FID=-1            : 表示文件打开不成功。
FID在此次文件关闭前总是有效的。
如以读方式打开，matlab首先搜索工作目录，其次搜索matlab的其他目录，“permission”是打开方式参数。
打开方式参数由以下字符串确定：
r             读出
w             写入（文件若不存在，自动创建）
a             后续写入（文件若不存在，自动创建）
r+            读出和写入（文件应已存在）
w+            重新刷新写入，（文件若不存在，自动创建）
a+            后续写入（文件若不存在，自动创建））
w             重新写入，但不自动刷新
a             后续写入，但不自动刷新
文件的存储格式：文件打开的默认方式是：二进制。以文本方式打开，可以在方式参
数“permission”中加入“t”文件将，如“rt”，“wt+”

matlab中fprintf函数的具体使用方法实例如下：

fprintf函数可以将数据按指定格式写入到文本文件中。其调用格式为：

数据的格式化输出：fprintf(fid, format, variables)

  按指定的格式将变量的值输出到屏幕或指定文件

  fid为文件句柄，若缺省，则输出到屏幕

​    1 for standard output (the screen) or 2 for standarderror. If FID is omitted, output goes to the screen.

  format用来指定数据输出时采用的格式

​    %d 整数

​    %e 实数：科学计算法形式

​    %f 实数：小数形式

​    %g 由系统自动选取上述两种格式之一

​    %s 输出字符串

fprintf（fid，format，A）
说明：fid为文件句柄，指定要写入数据的文件，format是用来控制所写数据格式的格式符，与fscanf函数相同，A是用来存放数据的矩阵。
例6.9 创建一个字符矩阵并存入磁盘，再读出赋值给另一个矩阵。
\>> a='string';
\>> fid=fopen('d:\char1.txt','w');
\>> fprintf(fid,'%s',a);
\>> fclose(fid);
\>> fid1=fopen('d:\char1.txt','rt');
\>> fid1=fopen('d:\char1.txt','rt');
\>> b=fscanf(fid1,'%s')
b =
string

matlab读txt文件

fid=fopen('fx.txt','r');
%得到文件号
[f,count]=fscanf(fid,'%f %f',[12,90]);
%把文件号1的数据读到f中。其中f是[12 90]的矩阵
%这里'%f %f'表示读取数据的形势，他是按原始数据型读出
fclose(fid);
%关闭文件
另外有的txt文件还可以用load来打开
其语句为
f=load('fx.txt)

字符串操作函数

\1.        函数eval可以用来执行用字符串表示的表达式

\2.        函数deblank可以去掉字符串末尾的所有空格

\3.        函数findstr可以用来在长字符串中查找一个短的字符串，并返回相应的位置

\4.        函数isstr可以用来判断变量是否为字符串

\5.        函数isletter可以用来判断字符串中各个元素是否为字母

\6.        函数isspace可以用来判断字符串元素是否为空格符

\7.        函数lower和upper可以把字符串中的字母转为小写格式和大写格式

\8.        函数strcat可以把多个字符串在水平方向上依次连接起来

\9.        函数strvcat可以把多个字符串按竖直方向连接起来

\10.     函数strcmp可以用来进行字符串的比较，返回1或0

\11.     函数strcmpi可以用来忽略英文字母的大小写方式比较字符串

\12.     函数strncmp可以用来比较字符串前N个字符是否相同

\13.     函数strjust可以用来调整字符串矩阵的对齐方式

\14.     函数strmatch可以用来寻找和目标字符串匹配的行

\15.     函数strrep可以实现字符串的查找和替代功能

\16.     函数strtok可以找出字符串第一个空格符前的字符串

\17.     函数texlabel可以把字符串转换成tex软件的格式

\18.     不同进制间的转换，bin2hex，bin2dec（‘字符串’）；dec2hex（数）=字符串；即十进制数不为字符串，      其它进制为字符串

\19.     函数bitget可以用来获取二进制的数位   C=bitget（A，bit），A为一个无符号整形数据

\20.     函数bitset可以用来设定某个二进制数位的值     C=bitset（A，bit） 指定数位的数值取反

​                                                 C=bitset（A，bit，V）指定数位的数值被V替换

\21.     函数bitand，bitor和bitxor可以用来进行‘与’，‘或’，‘抑或’数位操作