

[TOC]

[韩顺平-一周学会Linux+Shell](https://www.bilibili.com/video/BV1Sv411r7vd/)

# Shell教程

Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。

Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。（ 在计算机科学中，Shell俗称壳（用来区别于核），是指“为使用者提供操作界面”的[软件](https://baike.baidu.com/item/软件/12053)（command interpreter，命令解析器）。它类似于[DOS](https://baike.baidu.com/item/DOS/32025)下的[COMMAND.COM](https://baike.baidu.com/item/COMMAND.COM/8063418)和后来的[cmd.exe](https://baike.baidu.com/item/cmd.exe/8192925)。它接收用户命令，然后调用相应的[应用程序](https://baike.baidu.com/item/应用程序/5985445)。 ）

Ken Thompson 的 sh 是第一种 Unix Shell，Windows Explorer 是一个典型的图形界面 Shell。

## 第一个shell脚本

```
#!/bin/bash
echo "Hello World !"
```

**1、作为可执行程序**

将上面的代码保存为 test.sh，并 cd 到相应目录：

```
chmod +x ./test.sh  #使脚本具有执行权限
./test.sh  #执行脚本
```

注意，一定要写成 **./test.sh**，而不是 **test.sh**，运行其它二进制的程序也一样，直接写 test.sh，linux 系统会去 PATH 里寻找有没有叫 test.sh 的，而只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 test.sh 是会找不到命令的，要用 ./test.sh 告诉系统说，就在当前目录找。

**2、作为解释器参数**

这种运行方式是，直接运行解释器，其参数就是 shell 脚本的文件名，如：

```
/bin/sh test.sh
/bin/php test.php
```

这种方式运行的脚本，不需要在第一行指定解释器信息，写了也没用。

## Shell变量

> 注意，变量名和等号之间不能有空格，这可能和你熟悉的所有编程语言都不一样

使用变量

```
your_name="qinjx"
echo $your_name
echo ${your_name}
```

只读变量:只读变量不能unset

```
myUrl="http://www.google.com"
readonly myUrl
```

删除变量

```
unset variable_name
```

### 命令替换：``与$()

```
A=`date` # 将指令执行的结果赋给A
A=$(date) # 将指令执行的结果赋给A
```





## Shell 字符串

### 单引号

> 不好用，除非定义变量

### 双引号 

```
your_name='runoob'
str="Hello, I know you are \"$your_name\"! \n"
echo -e $str
```

输出结果为：

```
Hello, I know you are "runoob"! 
```

###  拼接字符串 

> 最好不要用单引号拼接字符串

```
your_name="runoob"
# 使用双引号拼接
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting  $greeting_1
# 使用单引号拼接
greeting_2='hello, '$your_name' !'
greeting_3='hello, ${your_name} !'
echo $greeting_2  $greeting_3
```

输出结果为：

```
hello, runoob ! hello, runoob !
hello, runoob ! hello, ${your_name} !
```

### 获取字符串长度

> ${#string}

```
string="abcd"
echo ${#string} #输出 4
```

###  提取子字符串 

以下实例从字符串第 **2** 个字符开始截取 **4** 个字符：

```
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```

###  查找子字符串 

查找字符 **i** 或 **o** 的位置(哪个字母先出现就计算哪个)：

```
string="runoob is a great site"
echo `expr index "$string" io`  # 输出 4
```

## Shell 数组

### 读取数组 

```
array_name=(
value0
value1
value2
value3
)
```

```
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen
```

```
valuen=${array_name[n]}
```

###  获取数组的长度 

 获取数组长度的方法与获取字符串长度的方法相同，例如： 

```
# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}


# 取得数组单个元素的长度
lengthn=${#array_name[n]}
```

## Shell 传递参数

```
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```

```shell
# 增加执行权限
$ chmod +x test.sh
# 给.sh传参数1,2,3
$ ./test.sh 1 2 3
Shell 传递参数实例！
执行的文件名：./test.sh
第一个参数为：1
第二个参数为：2
第三个参数为：3
```

| 参数处理 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| $#       | 传递到脚本的参数个数                                         |
| $*       | 以一个单字符串显示所有向脚本传递的参数。  如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。 |
| $$       | 脚本运行的当前进程ID号                                       |
| $!       | 后台运行的最后一个进程的ID号                                 |
| $@       | 与$*相同，但是使用时加引号，并在引号中返回每个参数。   如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。 |
| $-       | 显示Shell使用的当前选项，与[set命令](https://www.runoob.com/linux/linux-comm-set.html)功能相同。 |
| $?       | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。 |

```
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

echo "Shell 传递参数实例！";
echo "第一个参数为：$1";

echo "参数个数为：$#";
echo "传递的参数作为一个字符串显示：$*";
```

执行脚本，输出结果如下所示：

```
$ chmod +x test.sh 
$ ./test.sh 1 2 3
Shell 传递参数实例！
第一个参数为：1
参数个数为：3
传递的参数作为一个字符串显示：1 2 3
```

## Shell 运算符

### 算数运算符

下表列出了常用的算术运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明                                          | 举例                          |
| :----- | :-------------------------------------------- | :---------------------------- |
| +      | 加法                                          | `expr $a + $b` 结果为 30。    |
| -      | 减法                                          | `expr $a - $b` 结果为 -10。   |
| *      | 乘法                                          | `expr $a \* $b` 结果为  200。 |
| /      | 除法                                          | `expr $b / $a` 结果为 2。     |
| %      | 取余                                          | `expr $b % $a` 结果为 0。     |
| =      | 赋值                                          | a=$b 把变量 b 的值赋给 a。    |
| ==     | 相等。用于比较两个数字，相同则返回 true。     | [ $a == $b ] 返回 false。     |
| !=     | 不相等。用于比较两个数字，不相同则返回 true。 | [ $a != $b ] 返回 true。      |

```
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

a=10
b=20

val=`expr $a + $b`
echo "a + b : $val"

val=`expr $a - $b`
echo "a - b : $val"

val=`expr $a \* $b`
echo "a * b : $val"

val=`expr $b / $a`
echo "b / a : $val"

val=`expr $b % $a`
echo "b % a : $val"

if [ $a == $b ]
then
   echo "a 等于 b"
fi
if [ $a != $b ]
then
   echo "a 不等于 b"
fi
```

### 关系运算符

关系运算符只支持数字，不支持字符串，除非字符串的值是数字。

下表列出了常用的关系运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明                                                  | 举例                       |
| :----- | :---------------------------------------------------- | :------------------------- |
| -eq    | 检测两个数是否相等，相等返回 true。                   | [ $a -eq $b ] 返回 false。 |
| -ne    | 检测两个数是否不相等，不相等返回 true。               | [ $a -ne $b ] 返回 true。  |
| -gt    | 检测左边的数是否大于右边的，如果是，则返回 true。     | [ $a -gt $b ] 返回 false。 |
| -lt    | 检测左边的数是否小于右边的，如果是，则返回 true。     | [ $a -lt $b ] 返回 true。  |
| -ge    | 检测左边的数是否大于等于右边的，如果是，则返回 true。 | [ $a -ge $b ] 返回 false。 |
| -le    | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | [ $a -le $b ] 返回 true。  |

### 布尔运算符

下表列出了常用的布尔运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明                                                | 举例                                     |
| :----- | :-------------------------------------------------- | :--------------------------------------- |
| !      | 非运算，表达式为 true 则返回 false，否则返回 true。 | [ ! false ] 返回 true。                  |
| -o     | 或运算，有一个表达式为 true 则返回 true。           | [ $a -lt 20 -o $b -gt 100 ] 返回 true。  |
| -a     | 与运算，两个表达式都为 true 才返回 true。           | [ $a -lt 20 -a $b -gt 100 ] 返回 false。 |

### 逻辑运算符

以下介绍 Shell 的逻辑运算符，假定变量 a 为 10，变量 b 为 20:

| 运算符 | 说明       | 举例                                       |
| :----- | :--------- | :----------------------------------------- |
| &&     | 逻辑的 AND | [[ $a -lt 100 && $b -gt 100 ]] 返回 false  |
| \|\|   | 逻辑的 OR  | [[ $a -lt 100 \|\| $b -gt 100 ]] 返回 true |

### 字符串运算符

下表列出了常用的字符串运算符，假定变量 a 为 "abc"，变量 b 为 "efg"：

| 运算符 | 说明                                         | 举例                     |
| :----- | :------------------------------------------- | :----------------------- |
| =      | 检测两个字符串是否相等，相等返回 true。      | [ $a = $b ] 返回 false。 |
| !=     | 检测两个字符串是否不相等，不相等返回 true。  | [ $a != $b ] 返回 true。 |
| -z     | 检测字符串长度是否为0，为0返回 true。        | [ -z $a ] 返回 false。   |
| -n     | 检测字符串长度是否不为 0，不为 0 返回 true。 | [ -n "$a" ] 返回 true。  |
| $      | 检测字符串是否不为空，不为空返回 true。      | [ $a ] 返回 true。       |

### 文件测试运算符

文件测试运算符用于检测 Unix 文件的各种属性。

属性检测描述如下：

| 操作符  | 说明                                                         | 举例                      |
| :------ | :----------------------------------------------------------- | :------------------------ |
| -b file | 检测文件是否是块设备文件，如果是，则返回 true。              | [ -b $file ] 返回 false。 |
| -c file | 检测文件是否是字符设备文件，如果是，则返回 true。            | [ -c $file ] 返回 false。 |
| -d file | 检测文件是否是目录，如果是，则返回 true。                    | [ -d $file ] 返回 false。 |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 | [ -f $file ] 返回 true。  |
| -g file | 检测文件是否设置了 SGID 位，如果是，则返回 true。            | [ -g $file ] 返回 false。 |
| -k file | 检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。  | [ -k $file ] 返回 false。 |
| -p file | 检测文件是否是有名管道，如果是，则返回 true。                | [ -p $file ] 返回 false。 |
| -u file | 检测文件是否设置了 SUID 位，如果是，则返回 true。            | [ -u $file ] 返回 false。 |
| -r file | 检测文件是否可读，如果是，则返回 true。                      | [ -r $file ] 返回 true。  |
| -w file | 检测文件是否可写，如果是，则返回 true。                      | [ -w $file ] 返回 true。  |
| -x file | 检测文件是否可执行，如果是，则返回 true。                    | [ -x $file ] 返回 true。  |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true。     | [ -s $file ] 返回 true。  |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true。          | [ -e $file ] 返回 true。  |

```
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

file="/var/www/runoob/test.sh"
if [ -r $file ]
then
   echo "文件可读"
else
   echo "文件不可读"
fi
if [ -w $file ]
then
   echo "文件可写"
else
   echo "文件不可写"
fi
if [ -x $file ]
then
   echo "文件可执行"
else
   echo "文件不可执行"
fi
if [ -f $file ]
then
   echo "文件为普通文件"
else
   echo "文件为特殊文件"
fi
if [ -d $file ]
then
   echo "文件是个目录"
else
   echo "文件不是个目录"
fi
if [ -s $file ]
then
   echo "文件不为空"
else
   echo "文件为空"
fi
if [ -e $file ]
then
   echo "文件存在"
else
   echo "文件不存在"
fi
```

执行脚本，输出结果如下所示：

```
文件可读
文件可写
文件可执行
文件为普通文件
文件不是个目录
文件不为空
文件存在
```

### 使用 $(( )) 进行算术运算

**$(( ))** 语法也是进行算术运算的一种方式。

```
*#!/bin/bash*

*# 初始化变量*
num=5

*# 自增*
num=$((num + 1))

*# 自减*
num=$**((**num - 1**))

echo $num
```

### 使用 expr 命令

expr 命令可以用于算术运算，但在现代脚本中不如 let 和 $(( )) 常用。

```
*#!/bin/bash*

*# 初始化变量*
num=5

*# 自增*
num=$(expr $num + 1)

*# 自减*
num=$(expr $num - 1)

echo $num
```

### 使用 (( )) 进行算术运算

与 $(( )) 类似，(( )) 语法也可以用于算术运算。

```
*#!/bin/bash*

*# 初始化变量*
num=5

*# 自增*
((num++))

*# 自减*
((num--))

echo $num
```

### 使用 $[ ] 进行算术运算

类似上面

## Shell echo命令

### 5.显示不换行 

```
#!/bin/sh
echo -e "OK! \c" # -e 开启转义 \c 不换行
echo "It is a test"
```

输出结果：

```
OK! It is a test
```

###  6.显示结果定向至文件

```
echo "It is a test" > myfile
```

###  8.显示命令执行结果

```
echo `date`
```

**注意：** 这里使用的是反引号 `, 而不是单引号 '。

 结果将显示当前日期

```
Thu Jul 24 10:08:46 CST 2014
```

## Shell printf 命令

接下来,我来用一个脚本来体现printf的强大功能：

```
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com
 
printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg  
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234 
printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543 
printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876 
```

执行脚本，输出结果如下所示：

```
姓名     性别   体重kg
郭靖     男      66.12
杨过     男      48.65
郭芙     女      47.99
```

### printf的转义序列

![1569491312018](.\images\1569491312018.png)

## Shell test 命令

### 数值测试

![1569491365000](.\images\1569491365000.png)

```bash
num1=100
num2=100
if test $[num1] -eq $[num2]
then
    echo '两个数相等！'
else
    echo '两个数不相等！'
fi
```

### 字符串测试

![1569491391857](.\images\1569491391857.png)

```bash
num1="ru1noob"
num2="runoob"
if test $num1 = $num2
then
    echo '两个字符串相等!'
else
    echo '两个字符串不相等!'
fi
```



### 文件测试

![1569491416632](.\images\1569491416632.png)

```bash
cd /bin
if test -e ./bash
then
    echo '文件已存在!'
else
    echo '文件不存在!'
fi
```



## Shell 流程控制 

### if else-if else 

```bash
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```

### for 循环 

```bash
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

### while 语句

```bash
while condition
do
    command
done
```

### until 循环

> ntil 循环执行一系列命令直至条件为 true 时停止

```bash
until condition
do
    command
done
```

### case 

```bash
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2）
    command1
    command2
    ...
    commandN
    ;;
esac
```

## Shell read命令

**1、简单读取**

```
#!/bin/bash

#这里默认会换行  
echo "输入网站名: "  
#读取从键盘的输入  
read website  
echo "你输入的网站名是 $website"  
exit 0  #退出
```

测试结果为：

```
输入网站名: 
www.runoob.com
你输入的网站名是 www.runoob.com
```

**2、-p 参数，允许在 read 命令行中直接指定一个提示。**

```
#!/bin/bash

read -p "输入网站名:" website
echo "你输入的网站名是 $website" 
exit 0
```

测试结果为：

```
输入网站名:www.runoob.com
你输入的网站名是 www.runoob.com
```

**3、-t 参数指定 read 命令等待输入的秒数，当计时满时，read命令返回一个非零退出状态。**

```
#!/bin/bash

if read -t 5 -p "输入网站名:" website
then
    echo "你输入的网站名是 $website"
else
    echo "\n抱歉，你输入超时了。"
fi
exit 0
```

执行程序不输入，等待 5 秒后：

```
输入网站名:
抱歉，你输入超时了
```

4、除了输入时间计时，还可以使用 **-n** 参数设置 **read** 命令计数输入的字符。当输入的字符数目达到预定数目时，自动退出，并将输入的数据赋值给变量。

```
#!/bin/bash

read -n1 -p "Do you want to continue [Y/N]?" answer
case $answer in
Y | y)
      echo "fine ,continue";;
N | n)
      echo "ok,good bye";;
*)
     echo "error choice";;

esac
exit 0
```

该例子使用了-n 选项，后接数值 1，指示 read 命令只要接受到一个字符就退出。只要按下一个字符进行回答，read 命令立即接受输入并将其传给变量，无需按回车键。

只接收 2 个输入就退出：

```
#!/bin/bash

read -n2 -p "请随便输入两个字符: " any
echo "\n您输入的两个字符是:$any"
exit 0
```

执行程序输入两个字符：

```
请随便输入两个字符: 12
您输入的两个字符是:12
```

5、**-s** 选项能够使 **read** 命令中输入的数据不显示在命令终端上（实际上，数据是显示的，只是 **read** 命令将文本颜色设置成与背景相同的颜色）。输入密码常用这个选项。

```
#!/bin/bash

read  -s  -p "请输入您的密码:" pass
echo "\n您输入的密码是 $pass"
exit 0
```

执行程序输入密码后是不显示的：

```
请输入您的密码:
您输入的密码是 runoob
```

**6.读取文件**

每次调用 read 命令都会读取文件中的 "一行" 文本。当文件没有可读的行时，read 命令将以非零状态退出。

通过什么样的方法将文件中的数据传给 read 呢？使用 cat 命令并通过管道将结果直接传送给包含 read 命令的 while 命令。

测试文件 test.txt 内容如下：

```
123
456
runoob
```

测试代码：

```
#!/bin/bash
  
count=1    # 赋值语句，不加空格
cat test.txt | while read line      # cat 命令的输出作为read命令的输入,read读到>的值放在line中
do
   echo "Line $count:$line"
   count=$[ $count + 1 ]          # 注意中括号中的空格。
done
echo "finish"
exit 0
```

执行结果为：

```
Line 1:123
Line 2:456
Line 3:runoob
finish
```

使用 **-e** 参数，以下实例输入字符 **a** 后按下 **Tab** 键就会输出相关的文件名(该目录存在的)：

```
$ read -e -p "输入文件名:" str 
输入文件名:a
a.out    a.py     a.pyc    abc.txt  
输入文件名:a
```

## Shell date命令

Linux **date** 命令可以用来显示或设定系统的日期与时间。

### 语法

```
date [OPTION]... [+FORMAT]
date [-u] [-d datestr] [-s datestr] [--utc] [--universal] [--date=datestr] [--set=datestr] [--help] [--version] [+FORMAT] [MMDDhhmm[[CC]YY][.ss]]
```

#### 可选参数

- **-d, --date=STRING**：通过字符串显示时间格式，字符串不能是'now'。
- **-f, --file=DATEFILE**：类似于--date; 一次从DATEFILE处理一行。
- **-I[FMT], --iso-8601[=FMT]**：按照 ISO 8601 格式输出时间，FMT 可以为'date'(默认)，'hours'，'minutes'，'seconds'，'ns'。 可用于设置日期和时间的精度，例如：2006-08-14T02:34:56-0600。
- **-R, --rfc-2822** ： 按照 RFC 5322 格式输出时间和日期，例如: Mon, 14 Aug 2006 02:34:56 -0600。
- **--rfc-3339=FMT**：按照 RFC 3339 格式输出，FMT 可以为'date', 'seconds','ns'中的一个，可用于设置日期和时间的精度， 例如：2006-08-14 02:34:56-06:00。
- **-r, --reference=FILE**：显示文件的上次修改时间。
- **-s, --set=STRING**：根据字符串设置系统时间。
- **-u, --utc, --universal**：显示或设置协调世界时(UTC)。
- **--help**：显示帮助信息。
- **--version**：输出版本信息。

#### FORMAT 参数

在显示方面，使用者可以设定欲显示的格式 ，格式设定为一个加号后接数个标记，其中可用的标记列表如下：

```
%%   输出字符 %
%a   星期几的缩写 (Sun..Sat)
%A   星期的完整名称(Sunday..Saturday)。 
%b   缩写的月份名称（例如，Jan）
%B   完整的月份名称（例如，January）
%c   本地日期和时间（例如，Thu Mar  3 23:05:25 2005）
%C   世纪，和%Y类似，但是省略后两位（例如，20）
%d   日 (01..31)
%D   日期，等价于%m/%d/%y
%e   一月中的一天，格式使用空格填充，等价于%_d
%F   完整的日期；等价于 %Y-%m-%d
%g   ISO 标准计数周的年份的最后两位数字
%G   ISO 标准计数周的年份，通常只对%V有用
%h   等价于 %b
%H   小时 (00..23)
%I   小时 (01..12)
%j   一年中的第几天 (001..366)
%k   小时，使用空格填充 ( 0..23); 等价于 %_H
%l   小时, 使用空格填充 ( 1..12); 等价于 %_I
%m   月份 (01..12)
%M   分钟 (00..59)
%n   新的一行，换行符
%N   纳秒 (000000000..999999999)
%p   用于表示当地的AM或PM，如果未知则为空白
%P   类似 %p, 但是是小写的
%r   本地的 12 小时制时间(例如 11:11:04 PM)
%R   24 小时制 的小时与分钟; 等价于 %H:%M
%s   自 1970-01-01 00:00:00 UTC 到现在的秒数
%S   秒 (00..60)
%t   插入水平制表符 tab
%T   时间; 等价于 %H:%M:%S
%u   一周中的一天 (1..7); 1 表示星期一
%U   一年中的第几周，周日作为一周的起始 (00..53)
%V   ISO 标准计数周，该方法将周一作为一周的起始 (01..53)
%w   一周中的一天（0..6），0代表星期天
%W   一年中的第几周，周一作为一周的起始（00..53）
%x   本地的日期格式（例如，12/31/99）
%X   本地的日期格式（例如，23:13:48）
%y   年份后两位数字 (00..99)
%Y   年
%z   +hhmm 格式的数值化时区格式（例如，-0400）
%:z  +hh:mm 格式的数值化时区格式（例如，-04:00）
%::z  +hh:mm:ss格式的数值化时区格式（例如，-04:00:00）
%:::z  数值化时区格式，相比上一个格式增加':'以显示必要的精度（例如，-04，+05:30）
%Z  时区缩写 （如 EDT）
```

若是不以加号作为开头，则表示要设定时间，而时间格式为 **MMDDhhmm[[CC]YY][.ss]**，其中 **MM** 为月份，**DD** 为日，**hh** 为小时，**mm** 为分钟，**CC** 为年份前两位数字，**YY** 为年份后两位数字，**ss** 为秒数。

使用权限：所有使用者。

当您不希望出现无意义的 0 时(比如说 1999/03/07)，则可以在标记中插入 - 符号，比如说 **date '+%-H:%-M:%-S'** 会把时分秒中无意义的 0 给去掉，像是原本的 08:09:04 会变为 8:9:4。另外，只有取得权限者(比如说 root)才能设定系统时间。

当您以 root 身分更改了系统时间之后，请记得以 **clock -w** 来将系统时间写入 **CMOS** 中，这样下次重新开机时系统时间才会持续保持最新的正确值。

### 实例

显示当前时间

```
# date
Tue May 24 09:29:43 CST 2022
# date '+%c' 
Tue 24 May 2022 09:30:03 AM CST
# date '+%D' //显示完整的时间
05/24/22
# date '+%x' //显示数字日期
05/24/2022
# date '+%T' //显示日期，年份用四位数表示
14:09:31
# date '+%X' //显示24小时的格式
09:31:31 AM
```

格式化输出：

```
# date +"%Y-%m-%d"
2009-12-07
```

输出昨天日期：

```
# date -d "1 day ago" +"%Y-%m-%d"
2012-11-19
```

输出 2 秒后的时间：

```
# date -d "2 second" +"%Y-%m-%d %H:%M.%S"
2012-11-20 14:21.31
```

传说中的 1234567890 秒：

```
# date -d "1970-01-01 1234567890 seconds" +"%Y-%m-%d %H:%M:%S"
2009-02-13 23:02:30
```

或者:

```
# date -d@1234567890 +"%F %T"
2009-02-13 23:02:30
```

时间格式转换：

```
# date -d "2009-12-12" +"%Y/%m/%d %H:%M.%S"
2009/12/12 00:00.00
```

apache 格式转换：

```
# date -d "Dec 5, 2009 12:00:37 AM" +"%Y-%m-%d %H:%M.%S"
2009-12-05 00:00.37
```

格式转换后时间游走：

```
# date -d "Dec 5, 2009 12:00:37 AM 2 year ago" +"%Y-%m-%d %H:%M.%S"
2007-12-05 00:00.37
```

按自己的格式输出

```
# date '+usr_time: $1:%M %P -hey'
usr_time: $1:16 下午 -hey
```

显示时间后跳行，再显示目前日期

```
date '+%T%n%D'
```

显示月份与日数

```
date '+%B %d'
```

显示日期与设定时间(12:34:56)

```
date --date '12:34:56'
```

时间加减操作：

```
date +%Y%m%d                   # 显示年月日
date -d "+1 day" +%Y%m%d       # 显示后一天的日期
date -d "-1 day" +%Y%m%d       # 显示前一天的日期
date -d "-1 month" +%Y%m%d     # 显示上一月的日期
date -d "+1 month" +%Y%m%d     # 显示下一月的日期
date -d "-1 year" +%Y%m%d      # 显示前一年的日期
date -d "+1 year" +%Y%m%d      # 显示下一年的日期
```

设定时间：

```
date -s                         # 设置当前时间，只有root权限才能设置，其他只能查看
date -s 20120523                # 设置成20120523，这样会把具体时间设置成00:00:00
date -s 01:01:01                # 设置具体时间，不会对日期做更改
date -s "01:01:01 2012-05-23"   # 这样可以设置全部时间
date -s "01:01:01 20120523"     # 这样可以设置全部时间
date -s "2012-05-23 01:01:01"   # 这样可以设置全部时间
date -s "20120523 01:01:01"     # 这样可以设置全部时间
```

## Shell 函数

> 在Shell中，调用函数时可以向其传递参数

###  带参数的函数示例 

>  注意，$10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数。 

```
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```

 输出结果： 

```
第一个参数为 1 !
第二个参数为 2 !
第十个参数为 10 !
第十个参数为 34 !
第十一个参数为 73 !
参数总数有 11 个!
作为一个字符串输出所有参数 1 2 3 4 5 6 7 8 9 34 73 !
```

### 常见系统函数

1. basename 基本语法

功能：返回路径中最后一个'/'后的部分内容，常用于获取文件名。

basename NAME \[SUFFIX\]

如果指定 SUFFIX 参数，且它和字符串中所有字符都不相同，但和字符串的后缀相同，则除去指定后缀。

可用的参数：

\-z  ： 使用NUL而不是换行符来分隔输出

\-s,--suffix=SUFFIX  ： 效果和指定SUFFIX参数相同

\-a  ： 支持多个参数并将每个参数视为一个NAME，如：

```
[root@study ~]# basename -a any/str1 any/str2
str1
str2
```

2. dirname 基本语法

功能：返回完整路径最后，常用于返回文件路径。

实例1：返回 /root/fileName.txt 的 fileName.txt  ；返回 /root/fileName.txt 的 fileName。

```
#返回fileName.txt
basename /root/fileName.txt

#返回fileName
basename /root/fileName.txt .txt
```

实例2：返回 /root/fileName.txt的路径（不包含文件名），即/root

```
dirname /root/fileName.txt
```

## Shell 脚本实战分析-数据库定时备份

```
#！/bin/bash
#完成数据库的定时备份
#备份的路径
BACKUP=/mnt/backup/db
#当前的时间作为文件名
DATETIME=$(date +%Y_%m_%d_%H%M%S)
#可以输出变量调试
#echo ${DATETIME}
echo "==========开始备份==========="
echo "备份的路径是 $BACKUP/$DATETIME.tar.gz"

#主机
HOST=localhost
#用户名
DB_USER=root
#密码
DB_PWD=root
#备份数据库名
DATABASE=yuanchangliang
#创建备份的路径
#如果备份的路径文件夹存在就使用，否则创建
[ ! -d "$BACKUP/$DATETIME"  ]  && mkdir -p "$BACKUP/$DATETIME" 
#执行mysql的备份数据库的指令
mysqldump -u${DB_USER} -p${DB_PWD} --host=$HOST  $DATABASE | gzip  > $BACKUP/$DATETIME/$DATETIME.sql.gz
#打包备份文件
cd $BACKUP
tar -zcvf  $DATETIME.tar.gz  $DATETIME
#删除临时目录
rm -rf  $BACKUP/$DATETIME

#删除10天前的备份文件(-exec rm -rf {} \是固定写法，删除查询出来的数据)
find $BACKUP -mtime +10 -name  "*.tar.gz" -exec rm -rf {} \;
echo "==========备份完成==========="
```

![在这里插入图片描述](/Users/dongbinyu/learningNotes/images/Linux+Shell笔记/cfc0b9dd99c8da1d25f95c47fffd190c-20250211211317462.png)


通过以上几步，就可以实现在Linux中**定时**备份mysql数据库中的数据

如果想要立刻执行数据备份，可以直接执行以下命令：

```bash
[root@localhost mnt]# ./mysql_backup.sh 
```

## Shell 输入/输出重定向

| 命令            | 说明                                               |
| :-------------- | :------------------------------------------------- |
| command > file  | 将输出重定向到 file。                              |
| command < file  | 将输入重定向到 file。                              |
| command >> file | 将输出以追加的方式重定向到 file。                  |
| n > file        | 将文件描述符为 n 的文件重定向到 file。             |
| n >> file       | 将文件描述符为 n 的文件以追加的方式重定向到 file。 |
| n >& m          | 将输出文件 m 和 n 合并。                           |
| n <& m          | 将输入文件 m 和 n 合并。                           |
| << tag          | 将开始标记 tag 和结束标记 tag 之间的内容作为输入。 |

> 需要注意的是文件描述符 0 通常是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。

------

### 输出重定向

重定向一般通过在命令间插入特定的符号来实现。特别的，这些符号的语法如下所示:

```
command1 > file1
```

上面这个命令执行command1然后将输出的内容存入file1。

注意任何file1内的已经存在的内容将被新内容替代。如果要将新内容添加在文件末尾，请使用>>操作符。

#### 实例

执行下面的 who 命令，它将命令的完整的输出重定向在用户文件中(users):

```
$ who > users
```

执行后，并没有在终端输出信息，这是因为输出已被从默认的标准输出设备（终端）重定向到指定的文件。

你可以使用 cat 命令查看文件内容：

```
$ cat users
_mbsetupuser console  Oct 31 17:35 
tianqixin    console  Oct 31 17:35 
tianqixin    ttys000  Dec  1 11:33 
```

输出重定向会覆盖文件内容，请看下面的例子：

```
$ echo "菜鸟教程：www.runoob.com" > users
$ cat users
菜鸟教程：www.runoob.com
$
```

如果不希望文件内容被覆盖，可以使用 >> 追加到文件末尾，例如：

```
$ echo "菜鸟教程：www.runoob.com" >> users
$ cat users
菜鸟教程：www.runoob.com
菜鸟教程：www.runoob.com
$
```

------

### 输入重定向

和输出重定向一样，Unix 命令也可以从文件获取输入，语法为：

```
command1 < file1
```

这样，本来需要从键盘获取输入的命令会转移到文件读取内容。

注意：输出重定向是大于号(>)，输入重定向是小于号(<)。

#### 实例

接着以上实例，我们需要统计 users 文件的行数,执行以下命令：

```
$ wc -l users
       2 users
```

也可以将输入重定向到 users 文件：

```
$  wc -l < users
       2 
```

注意：上面两个例子的结果不同：第一个例子，会输出文件名；第二个不会，因为它仅仅知道从标准输入读取内容。

```
command1 < infile > outfile
```

同时替换输入和输出，执行command1，从文件infile读取内容，然后将输出写入到outfile中。

### 重定向深入讲解

一般情况下，每个 Unix/Linux 命令运行时都会打开三个文件：

- 标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
- 标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
- 标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。

默认情况下，command > file 将 stdout 重定向到 file，command < file 将stdin 重定向到 file。

如果希望 stderr 重定向到 file，可以这样写：

```
$ command 2>file
```

如果希望 stderr 追加到 file 文件末尾，可以这样写：

```
$ command 2>>file
```

**2** 表示标准错误文件(stderr)。

如果希望将 stdout 和 stderr 合并后重定向到 file，可以这样写：

```
$ command > file 2>&1

或者

$ command >> file 2>&1
```

如果希望对 stdin 和 stdout 都重定向，可以这样写：

```
$ command < file1 >file2
```

command 命令将 stdin 重定向到 file1，将 stdout 重定向到 file2。

------

### Here Document

Here Document 是 Shell 中的一种特殊的重定向方式，用来将输入重定向到一个交互式 Shell 脚本或程序。

它的基本的形式如下：

```
command << delimiter
    document
delimiter
```

它的作用是将两个 delimiter 之间的内容(document) 作为输入传递给 command。

> 注意：
>
> - 结尾的delimiter 一定要顶格写，前面不能有任何字符，后面也不能有任何字符，包括空格和 tab 缩进。
> - 开始的delimiter前后的空格会被忽略掉。

#### 实例

在命令行中通过 **wc -l** 命令计算 Here Document 的行数：

```
(base) dongbinyu@dongbinyudeMacBook-Pro ~ % wc -l <<EOF
欢迎来到
菜鸟教程
heredoc> 欢迎来到
heredoc> 菜鸟教程
heredoc> www.runoob.com
heredoc> EOF
       5
```

我们也可以将 Here Document 用在脚本中，例如：

```
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

cat << EOF
欢迎来到
菜鸟教程
www.runoob.com
EOF
```

执行以上脚本，输出结果：

```
欢迎来到
菜鸟教程
www.runoob.com
```

------

### /dev/null 文件

如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null：

```
$ command > /dev/null
```

/dev/null 是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。但是 /dev/null 文件非常有用，**将命令的输出重定向到它，会起到"禁止输出"的效果。**

**如果希望屏蔽 stdout 和 stderr，可以这样写：**

```
$ command > /dev/null 2>&1
```

> **注意：**0 是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。
>
> 这里的 **2** 和 **>** 之间不可以有空格，**2>** 是一体的时候才表示错误输出。

## Shell 文件包含

```bash
. filename   # 注意点号(.)和文件名中间有一空格

或

source filename
```



# Linux笔记

[菜鸟教程-Linux命令](https://www.runoob.com/linux/linux-command-manual.html)

## 1.Linux基础

### 1.1.前言

### 1.2.CentOS安装和说明

#### 1.2.1.虚拟机网络连接的三种方式

![1601381592051](.\images\1601381592051.png)



#### 1.2.2.CentOS终端使用和联网

[CSDN-CentOS7联网](https://blog.csdn.net/lsc476782158/article/details/51721909)

> 为了能够在centOS和windows共享文件夹，
>
> 为了能够在centOS中粘贴windows下的复制.
>
> 需要安装**VMtools**

[CSDN-VMtools重新安装 灰色问题](https://blog.csdn.net/l979951191/article/details/50596986/)

### 1.3.Linux目录结构 

[菜鸟教程-Linux  系统目录结构](https://www.runoob.com/linux/linux-system-contents.html)

以下是对这些目录的解释：

- **/bin**：
   bin 是 Binaries (二进制文件) 的缩写, 这个目录存放着最经常使用的命令。

![1641783246503](images/1641783246503.png)

- **/boot：**
  这里存放的是启动 Linux 时使用的一些核心文件，包括一些连接文件以及镜像文件。

![1641783231762](images/1641783231762.png)

- **/dev ：**
  dev 是 Device(设备) 的缩写, 该目录下存放的是 Linux 的外部设备，在 Linux 中访问设备的方式和访问文件的方式是相同的。

  ![1641783276034](images/1641783276034.png)
  
- **/etc：**
  etc 是 Etcetera(等等) 的缩写,这个目录用来存放所有的系统管理所需要的配置文件和子目录。

  ![1641783314872](images/1641783314872.png)
  
- **/home**：
  用户的主目录，在 Linux 中，每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。

- **/lib**：
  lib 是 Library(库) 的缩写这个目录里存放着系统最基本的动态连接共享库，其作用类似于 Windows 里的 DLL 文件。几乎所有的应用程序都需要用到这些共享库。

![1641783350606](images/1641783350606.png)

- **/lost+found**：
  这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。

- **/media**：
   linux 系统会自动识别一些设备，例如U盘、光驱等等，当识别后，Linux 会把识别的设备挂载到这个目录下。

- **/mnt**：
  系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在 /mnt/ 上，然后进入该目录就可以查看光驱里的内容了。

- **/opt**：
  opt 是 optional(可选) 的缩写，这是给主机额外安装软件所摆放的目录。比如你安装一个ORACLE数据库则就可以放到这个目录下。默认是空的。

- **/proc**：
  proc 是 Processes(进程) 的缩写，/proc 是一种伪文件系统（也即虚拟文件系统），存储的是当前内核运行状态的一系列特殊文件，这个目录是一个虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息。
  这个目录的内容不在硬盘上而是在内存里，我们也可以直接修改里面的某些文件，比如可以通过下面的命令来屏蔽主机的ping命令，使别人无法ping你的机器：

  ```
  echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_all
  ```

- **/root**：
  该目录为系统管理员，也称作超级权限者的用户主目录。

- **/sbin**：
   s 就是 Super User 的意思，是 Superuser Binaries (超级用户的二进制文件) 的缩写，这里存放的是系统管理员使用的系统管理程序。

- **/selinux**：
   这个目录是 Redhat/CentOS 所特有的目录，Selinux 是一个安全机制，类似于 windows 的防火墙，但是这套机制比较复杂，这个目录就是存放selinux相关的文件的。

- **/srv**：
   该目录存放一些服务启动之后需要提取的数据。

- **/sys**：

  这是 Linux2.6 内核的一个很大的变化。该目录下安装了 2.6 内核中新出现的一个文件系统 sysfs 。

  sysfs 文件系统集成了下面3种文件系统的信息：针对进程信息的 proc 文件系统、针对设备的 devfs 文件系统以及针对伪终端的 devpts 文件系统。

  该文件系统是内核设备树的一个直观反映。

  当一个内核对象被创建的时候，对应的文件和目录也在内核对象子系统中被创建。

- **/tmp**：
   tmp 是 temporary(临时) 的缩写这个目录是用来存放一些临时文件的。

- **/usr**：
   usr 是 unix shared resources(共享资源) 的缩写，这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于 windows 下的 program files 目录。

- **/usr/bin：**
  系统用户使用的应用程序。

- **/usr/sbin：**
  超级用户使用的比较高级的管理程序和系统守护程序。

- **/usr/src：**
  内核源代码默认的放置目录。

- **/var**：
   var 是 variable(变量) 的缩写，这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。

- **/run**：
  是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除。如果你的系统上有 /var/run 目录，应该让它指向 run。

在 Linux 系统中，有几个目录是比较重要的，平时需要注意不要误删除或者随意更改内部文件。

**/etc**： 上边也提到了，这个是系统中的配置文件，如果你更改了该目录下的某个文件可能会导致系统不能启动。

**/bin, /sbin, /usr/bin, /usr/sbin**: 这是系统预设的执行文件的放置目录，比如 ls 就是在 /bin/ls 目录下的。

值得提出的是，/bin, /usr/bin 是给系统用户使用的指令（除root外的通用户），而/sbin, /usr/sbin 则是给 root 使用的指令。 

**/var**： 这是一个非常重要的目录，系统上跑了很多程序，那么每个程序都会有相应的日志产生，而这些日志就被记录到这个目录下，具体在 /var/log 目录下，另外 mail 的预设放置也是在这里。

## 2.Linux实操

### 2.1.远程登录Xshell5

### 2.2.远程上传下载Xftp5

### 2.3.Vi和Vim编辑器

#### 2.3.1.常见的三种模式

#### 2.3.2.常见快捷键

[菜鸟教程-vi/vim 按键说明](https://www.runoob.com/linux/linux-vim.html)

### 2.4.开机，重启，用户登录注销

- shutdown
  - `shutdown -h now `立即关机
  - `shutdown -h 1 `一分钟后关机
  - `shutdown -r now`立即重启
- `halt` 停止使用，等效于关机
- `reboot`重启 
- `sync`把内存的数据保存到磁盘【关机和重启前为了安全需要使用这个指令，防止数据丢失】

- `logout`注销用户

### 2.5.用户管理

#### 2.5.1.添加用户

[Could not chdir to home directory /home/tom: No such file or directory](https://blog.csdn.net/qq_42479695/article/details/120545317)

[Linux下用户的创建与删除](https://blog.csdn.net/yychuyu/article/details/84190390)

`useradd 用户名` 创建用户

1. 当创建用户成功后，会自动创建和用户同名的家目录
2. 也可以通过`useradd -d 指定目录 新的用户名`给新创建的用户指定家目录
3. `useradd -g 组名 用户名`新增用户到指定用户组

`passwd 用户名` 给用户指定或修改密码

#### 2.5.2.删除用户

> 一般删除用户，不会删除该用户的家目录

- 删除用户，保存该用户的家目录 `userdel 用户名`
- 删除用户，以及用户的的家目录 `userdel -r 用户名`

> 报错：userdel: user xm is currently used by process 62520
>
> 解决：第一次使用ctrl+d退出root用户，回到user1用户；第二次使用ctrl+d退出user1用户，此时会返回到root用户（再按ctrl+d退出登陆连接），此时使用userdel user1正常删除。

#### 2.5.3.查询用户信息

`id 用户名`

```
uid=1000(waterplants) gid=1000(waterplants) 组=1000(waterplants),10(wheel)
```

- uid 组名
- gid 所在组ID号
- 组 组名

#### 2.5.4.切换用户

`su 用户名` 切换用户

`exit` 回退到原先的用户

> 从权限高的用户切换到权限低的用户不需要输入密码

`whoami`查看当前用户

#### 2.5.5.用户组

`groupadd 组名` 创建用户组

`groupdel 组名`删除用户组

`useradd -g 用户组 用户名` 创建用户时指定用户组

`usermod -g 用户组 用户名`修改用户对应的用户组

#### 2.5.6.用户和组相关的文件

`/etc/passwd`用户的配置文件，记录用户的各种信息。

每行的含义： `用户名:口令:用户表示号:组标识号:注释性描述:主目录:登录shell`

`/etc/shadow`口令的配置文件。

每行的含义： `登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志`

`/etc/group`用户组的配置文件。

每行的含义： `组名:口令:组标识号:组内用户列表`

### 2.6.实用指令

#### 2.6.1.指定运行级别

![1601446284814](.\images\1601446284814.png)

`init [0123456]`切换运行级别

> 通过`init 1`进入单用户模式，可以不输入密码登录root。
>
> 可以用来修改root密码

![1601446782294](.\images\1601446782294.png)

#### 2.6.2.帮助相关指令

`man 指令名` 如`man ls` 显示指令的帮助文档

`help 指令名` 如`help cd` 显示指令的帮助文档

#### 2.6.3.文件目录类

`pwd` 显示当前目录的绝对路径

`ls` 显示当前目录下所有内容（包括文件和目录）

- `ls -l`或`ll` 以列表的方式显示信息
- `ls -a` 显示当前目录所有内容（包括文件和目录），包括隐藏的
- `ls -la`或`ls -al` 等效于同时使用上面两句

`cd 路径` 切换目录

- `cd ../`或`cd ..`回到上级目录
- `cd ~`回到家目录

`mkdir 目录名` 创建目录

- `mkdir -p 多级目录名` 创建多级目录

`rmdir 目录名` 只能删除空目录

`touch 文件名称` 创建空文件

`cp 来源地文件 目的地目录`复制来源地文件到目的地目录

- `cp -r 来源地目录 目的地目录` 递归复制整个文件夹
- `\cp -r 来源地目录 目的地目录` 递归复制整个文件夹,并强制覆盖

`rm 文件名`删除文件

- `rm -f 文件名` 强制删除文件，不提示
- `rm -rf 目录名` 删除目录

`mv` 移动文件与目录或重命名

- `mv 旧文件名 新文件名` 文件重命名
- `mv 文件名 目录名` 文件移动

`cat 文件名` 查看文件内容

- `cat -n 文件名` 查看文件内容，并显示行号
- `cat 文件名 | more`查看文件内容，分页显示【按空格切换下一页 按回车切换下一行】

`more 文件名`分页查看文件内容【按空格切换下一页 按回车切换下一行 ctrl+b切换上一页】

`less 文件名` 分页查看文件内容【会根据显示需要加载内容，对于显示大型文件有较高效率】

`>`输出重定向，即覆盖写

- `ls -a > 文件`列表的内容覆盖写入到文件中

`>>`输出追加，即追加写

`echo `输出

- `echo $path`输出环境变量
- `echo helloworld`输出helloworld

`head 文件名`查看文件头10行内容

- `head -n 5 文件名` 查看文件头5行内容

`tail 文件名`查看文件尾10行内容

- `tail -n 5 文件名` 查看文件尾5行内容
- `tail -f 文件名`实时追踪文档所有更新

`ln -s 原文件或目录 软连接名` 创建软连接，即快捷方式【当使用`pwd`查看目录时，仍然看到的是软连接所在目录】

- `rm- rf 软连接名` 删除软连接【软连接名后不要带`/`】

`history`查看已经执行过的指令

- `history 10` 显示最近执行过的10条指令
- `!178`执行编号为178的指令

#### 2.6.4.时间日期类

`date`显示时间

- `date +%Y`显示年
- `date “+%Y-%m-%d %H:%M:%S”`显示年月日时分秒【2020-09-30 15:41:39】
- `date -s 字符串时间` 设置时间

`cal` 显示当前月日历

- `cal 2020`显示2020整年日历

#### 2.6.5.搜索查找类

`find 搜索范围 选项`查找

- `find /home -name hello.txt `【在`/home`下按文件名查找`hello.txt`文件】
- `find /opt -user root`【在`/opt`下按文件拥有者查找文件】
- `find / -size +[-,=]20M`【在`/`下查找大于20M的文件】
- `find / -name *.txt`【在`/home`下按文件名查找`txt`后缀的文件】

`locate`快速定位文件

- `updatedb`【使用`locate`前先要初始化locate数据库】
- `locate hello.txt `定位`hello.txt`的位置

`grep`文件内查找【通常和`|`管道符结合使用】

- `cat hello.txt |grep -n yes`查看`hello.txt`然后文件内查找所有`yes`并显示它的行号
- `cat hello.txt |grep -ni yes`查看`hello.txt`然后文件内不区分大小写的查找所有`yes`并显示它的行号

```shell
[root@hadoop1 home]# cat hello.txt |grep -n yes
4:yes
[root@hadoop1 home]# vim hello.txt 
[root@hadoop1 home]# cat hello.txt |grep -ni yes
4:yes
7:YES111
8:YES
```

#### 2.6.6.压缩解压类

- 压缩：`gzip 文件名` 将对应的文件压缩为`文件名.gz`文件

- 解压：`gunzip 文件名.gz` 解压文件

**注意：`gzip`和`gunzip`不会保留原文件**



- 压缩:`zip -r mypackage.zip /home`压缩整个`/home`目录下的文件到`mypackage.zip`

- 解压：`unzip -d /opt/tmp/ mypackage.zip`解压`mypackage.zip`到`/opt/tmp`



- 压缩：`tar -zvcf a.tar.gz hello.txt`打包并压缩文件到`a.tar.gz`
- 解压：`tar -zxvf a.tar.gz `解压`a.tar.gz`到当前目录
- 解压到指定目录：`tar -zxvf a.tar.gz -C /opt/tmp `解压`a.tar.gz`到`/opt/tmp`【指定解压的目录必须存在】

### 2.7.组管理和权限管理

[菜鸟教程-Linux 文件基本属性](https://www.runoob.com/linux/linux-file-attr-permission.html)

#### 2.7.1.组管理

- `chown [–R] 所有者 文件名`更改文件所有者（owner），也可以同时更改文件所属组。
  `chown [-R] 所有者:属组名 文件名`更改文件所有者（owner），也可以同时更改文件所属组。
- `chgrp [-R] 属组名 文件名`更改文件属组
- `usermod`修改用户所在组
  - `usermod -g 组名 用户名`改变用户所在组
  - `usermod -d 目录名 用户名`改变该用户登录的初始目录
-  `chmod [-R] xyz 文件或目录`

#### 2.7.2.权限详情

![img](images/Linux+Shell笔记/file-llls22.jpg)



Linux文件属性有两种设置方法，一种是数字，一种是符号。

Linux 文件的基本权限就有九个，分别是 **owner/group/others(拥有者/组/其他)** 三种身份各有自己的 **read/write/execute** 权限。

先复习一下刚刚上面提到的数据：文件的权限字符为： **-rwxrwxrwx** ， 这九个权限是三个三个一组的！其中，我们可以使用数字来代表各个权限，各权限的分数对照表如下：

- r:4
- w:2
- x:1

每种身份(owner/group/others)各自的三个权限(r/w/x)分数是需要累加的，例如当权限为： **-rwxrwx---** 分数则是：

- owner = rwx = 4+2+1 = 7
- group = rwx = 4+2+1 = 7
- others= --- = 0+0+0 = 0

**第0位代表文件的类型**，它可以是：

- 当为[ d ]则是目录；
- 当为[ - ]则是普通文件；
- 若是[ l ]则表示为软连接；
- 若是[ b ]则表示为装置文件里面的可供储存的接口设备(可随机存取装置)；在dev目录下能看到
- 若是[ c ]则表示为装置文件里面的串行端口设备，例如键盘、鼠标(一次性读取装置)；在dev目录下能看到



### 2.8.任务调度

![img](images/Linux+Shell笔记/EfG2i-BUMAAp_C1.jpeg)

#### 2.8.1.定时任务详解

**cron定时任务**

- `crontab -e`进入定时任务编辑
- `*/1 * * * * ls -l /etc >> /tmp/to.txt`每隔一分钟执行`ls -l /etc`并将结果追加输出到`/tmp/to.txt`

**cron相关指令**

- `crontab -r` 终止任务调度
- `crontab -l` 列出当前有哪些任务调度
- `service crond restart` 重启任务调度
- `crontab -e`时删除某行，来去除某个定时任务

**at定时任务**

使用 `at` 命令，你可以在特定时间自动完成你所设定的任务，也可以实现自动化，非常方便快捷！

[博客园-at命令详解](https://www.cnblogs.com/yychuyu/p/15483186.html)

### 2.9.磁盘分区和挂载

[CSDN-Linux磁盘分区详解（新建分区，现有分区扩容，分区减容）](https://blog.csdn.net/zhanhjxxx/article/details/123232402)

#### 2.9.1.分区的方式

> GTP分区代表了未来

- MBR分区
  - 最多支持四个主分区
  - 系统只能安装在主分区
  - 拓展分区要站一个主分区
  - MBR最大只支持2TB，但拥有最好的兼容性
- GTP分区
  - 支持无限多个主分区（但操作系统可能限制，比如windows下最多128个分区）
  - 最大支持18EB的大容量（1EB=1024PB,1PB=1024TB）
  - windows7 64位 以后支持GTP

#### 2.9.2.Linux分区

![1601696674513](.\images\1601696674513.png)

![1601696824058](.\images\1601696824058.png)



- `lsblk -f`老师不离开 查看当前系统分区情况

  ![1601696945005](.\images\1601696945005.png)

  ![1601697135108](.\images\1601697135108.png)

#### 2.9.3.给Linux增加一个硬盘

[CSDN-Linux磁盘扩容三种方式](https://blog.csdn.net/Hlroliu/article/details/109764269)

Linux在使用过程中由于数据量不断增大，导致磁盘空间不足，需要增加磁盘空间，主要有以下三种方式
1、直接给 / 分区（或者某一分区）扩容，直接在原有磁盘上增大空间
2、给虚拟机新增一块磁盘，为这块磁盘新建一个分区
3、给虚拟机新增一块磁盘，并把磁盘空间扩容到原有分区

#### 2.9.4.磁盘查询指令

[菜鸟教程-du命令](https://www.runoob.com/linux/linux-comm-du.html)

[CSDN-磁盘情况查询与磁盘实用指令](https://blog.csdn.net/wish_you_luck/article/details/125502718)

- `df -l` 查询系统整体磁盘使用情况
- `du -h /目录` 查询指定目录的磁盘占用情况
- `tree`树形展示目录结构

### 2.10.网络配置

目前网络配置采用的是NAT模式

[CSDN-CentOS7虚拟机上网](https://blog.csdn.net/a785975139/article/details/53023590?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-2)

[博客园-Linux修改IP、主机名、DNS ](https://www.cnblogs.com/ataoxz/p/18606532)

[CSDN-linux添加或修改hosts的方法](https://blog.csdn.net/sqlquan/article/details/99974553)

`netstat -anp` 查看系统网络情况

![1601798250140](.\images\1601798250140.png)

**主机名解析过程的分析**：

1. 当用户在浏览器中输入百度的域名（www.baidu.com），浏览器会首先检查本地hosts文件中是否存在对应的映射关系。
2. 如果在hosts文件中找到了对应的映射关系，浏览器会将主机名解析为对应的IP地址，并直接向该IP地址发送请求。
3. 如果在hosts文件中未找到对应的映射关系，则浏览器会向本地DNS缓存发送查询请求。
4. 如果在本地DNS缓存中找到了对应的IP地址，浏览器会将主机名解析为对应的IP地址，并直接向该IP地址发送请求。
5. 如果在本地DNS缓存中未找到对应的IP地址，则本地DNS缓存会向本地DNS服务器发送查询请求。
6. 本地DNS服务器会检查自己的缓存，如果找到了对应的IP地址，它会将结果返回给本地DNS缓存。
7. 如果本地DNS服务器未找到对应的IP地址，它会根据域名的层次结构，从根域名服务器开始逐级查询。
8. 本地DNS服务器会向根域名服务器发送查询请求，根域名服务器会返回顶级域名服务器的IP地址。
9. 本地DNS服务器会向顶级域名服务器发送查询请求，顶级域名服务器会返回二级域名服务器的IP地址。
10. 本地DNS服务器会继续向二级域名服务器发送查询请求，直到找到与主机名对应的IP地址。
11. 一旦本地DNS服务器找到了对应的IP地址，它会将结果返回给本地DNS缓存。
12. 本地DNS缓存将结果返回给浏览器，浏览器将主机名解析为IP地址，并向该IP地址发送请求。
13. 百度服务器接收到请求后，会返回相应的网页内容给浏览器，浏览器将网页内容显示给用户。

### 2.11.进程管理

#### 2.11.1进程的基本介绍

#### 2.11.2.查看进程

`ps -aux`

`ps -ef`

`pstree -p`以树状展示进程ID

`pstree -u`以树状展示进程的用户ID

![1601711359546](.\images\1601711359546.png)

#### 2.11.3.终止进程

`kill [选项] 进程号`通过进程号杀死进程

`killall 进程名称` 通过进程名杀死进程，也支持通配符，这在系统因负载过大而变慢时很有用

1. 踢掉某个非法登录的用户![1601712121241](.\images\1601712121241.png)
2. 终止远程登录服务sshd，在适当时候再重启sshd服务![1601712176210](.\images\1601712176210.png)
3. 终止多个gedit编辑器【killall】![1601712302072](.\images\1601712302072.png)
4. 强制杀掉一个终端【甚至可以自杀！！！】![1601712419874](.\images\1601712419874.png)

#### 2.11.4.服务管理

[博客园-CentOS7服务管理（一）](https://www.cnblogs.com/tkqasn/p/9379242.html)

> 服务本质就是进程，但是是运行在后台的，通常会监听某个端口，等待其他进程的请求，比如（mysql,sshd,防火墙等），因此我们又称为**守护进程**

CentOS7之前：`service 服务名 [start|stop|restart|reload|status]` 

CentOS7之后:`systemctl [start|stop|restart|reload|status] 服务名`

> 注意：这种方式只是**临时生效**，重启系统后，回归以前对服务的配置。

------

> 在windows启动Telnet功能后，
>
> 使用`telnet 10.0.0.102 22 `可以连接上虚拟机上的CentOS7系统的sshd服务

------

查看服务名几种方式：

- `setup`图形化界面查看服务
- 到`/etc/init.d`下查看
- `systemctl list-unit-files`列出所有服务
- `systemctl list-dependencies`查看所有服务 

服务的运行级别：见 **2.6.1.指定运行级别**

`chkconfig`给某个服务的某个运行级别设置自启动/不自启动

- `chkconfig --list|grep xxx`查看某个服务
- `chkconfig 服务名 --list`查看某个服务
- `chkconfig --level 5 服务名 on/off`设置某个服务在运行级别5情况下自启动/不自启动

`top`显示正在执行的进程

[CSDN-linux top命令详解（看这一篇就够了）](https://blog.csdn.net/weixin_45465395/article/details/115728520)

![在这里插入图片描述](images/Linux+Shell笔记/1219762540438ff06ca61dcfffc435b2.png)



### 2.12.RPM和YUM

#### 2.12.1.RPM

> RPM 是RedHat Package Manager(红帽软件包管理工具)，它生成`.rpm`拓展名的文件，类似于windows的`setup.exe`。

`rpm -qa|grep xx`查询已安装的RPM软件列表

![1601798605828](.\images\1601798605828.png)

`rpm -e RPM软件包名称`卸载某个软件

`rpm -e -nodeps RPM软件包名称 `强制卸载某个软件，不管有没有依赖

`rpm -ivh RPM软件包名称`

#### 2.12.2.YUM

> YUM是一个Shell前端软件包管理器。基于RPM包管理，能够从指定服务器自动下载RPM包并安装。而且自动处理依赖性关系，并以此安装所有依赖的软件包。

`yum list|grep xxx`查看软件列表

`yum install xxx`下载安装某软件，默认安装最新版本

![1601799521307](.\images\1601799521307.png)

> Ubuntu使用apt来安装软件。
>
> APT是Advance Packaging Tool（高级包装工具）的缩写，APT可以自动下载，配置，安装二进制或者源代码格式的软件包。

### 2.13.防火墙

[CSDN-Linux 防火墙配置（iptables和firewalld）](https://blog.csdn.net/m0_49864110/article/details/129150960)

### 2.14.Linux 日志

[博客园-【Linux日志】系统日志及分析](https://www.cnblogs.com/yingsong/p/6022181.html)

[CSDN-一文带你看懂Linux日志分析（非常详细）零基础入门到精通，收藏这一篇就够了](https://blog.csdn.net/leah126/article/details/140314144)

[博客园-Rsyslog 详解](https://www.cnblogs.com/shu-sheng/p/13275474.html)

[C语言中文网-Linux日志轮替（日志转储）及logrotate配置文件分析](https://c.biancheng.net/view/1106.html)

 

日志文件是Linux系统中极为重要的一部分，它们记录了系统和应用程序的各种活动信息。通过日志文件，系统管理员可以监控系统的运行状态、发现潜在的问题，并进行故障排除。

在这篇文章中，我将详细介绍一些常用的Linux日志分析命令和工具，帮助您更好地管理和分析系统日志。

####  2.14.1.日志类型

下面是常见的日志类型，但并不是所有的Linux发行版都包含这些类型：

 

| 类型          | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| auth          | 用户认证时产生的日志，如login命令、su命令。                  |
| authpriv      | 与 auth 类似，但是只能被特定用户查看。                       |
| console       | 针对系统控制台的消息。                                       |
| cron          | 系统定期执行计划任务时产生的日志。                           |
| daemon        | 某些守护进程产生的日志。                                     |
| ftp           | FTP服务。                                                    |
| kern          | 系统内核消息。                                               |
| local0.local7 | 由自定义程序使用。                                           |
| lpr           | 与打印机活动有关。                                           |
| mail          | 邮件日志。                                                   |
| mark          | 产生时间戳。系统每隔一段时间向日志文件中输出当前时间，每行的格式类似于 May 26 11:17:09 rs2 -- MARK --，可以由此推断系统发生故障的大概时间。 |
| news          | 网络新闻传输协议(nntp)产生的消息。                           |
| ntp           | 网络时间协议(ntp)产生的消息。                                |
| user          | 用户进程。                                                   |
| uucp          | UUCP子系统。                                                 |

#### 2.14.2.日志优先级

常见的日志优先级请见下标：

 

| 优先级  | 说明                                                       |
| ------- | ---------------------------------------------------------- |
| emerg   | 紧急情况，系统不可用（例如系统崩溃），一般会通知所有用户。 |
| alert   | 需要立即修复，例如系统数据库损坏。                         |
| crit    | 危险情况，例如硬盘错误，可能会阻碍程序的部分功能。         |
| err     | 一般错误消息。                                             |
| warning | 警告。                                                     |
| notice  | 不是错误，但是可能需要处理。                               |
| info    | 通用性消息，一般用来提供有用信息。                         |
| debug   | 调试程序产生的信息。                                       |
| none    | 没有优先级，不记录任何日志消息。                           |

#### 2.14.3.常见的日志文件

在介绍具体的日志分析命令之前，首先了解一下Linux系统中常见的日志文件。它们通常位于`/var/log`目录下，包括：

*   `/var/log/syslog`：记录系统的通用日志信息，适用于大多数Linux发行版。
    
*   `/var/log/messages`：记录系统的通用日志信息，通常在Red Hat系的发行版中使用。
    
*   `/var/log/auth.log`：记录身份验证相关的日志信息，如登录尝试、sudo命令使用等。
    
*   `/var/log/kern.log`：记录内核相关的日志信息。
    
*   `/var/log/dmesg`：记录系统启动时的内核消息。
    
*   `/var/log/boot.log`：记录系统启动过程中的消息。
    
*   `/var/log/cron.log`：记录定时任务相关的日志信息。
    
*   `/var/log/mail.log`：记录邮件系统相关的日志信息。
    
*   `/var/log/apache2/access.log`和`/var/log/apache2/error.log`：记录Apache服务器的访问日志和错误日志。
    
*   `/var/log/nginx/access.log`和`/var/log/nginx/error.log`：记录Nginx服务器的访问日志和错误日志。
    

#### 2.14.4.查看日志文件的命令

1\. cat

cat命令用于查看文件的内容，适用于查看较短的日志文件。

示例：

```
cat /var/log/syslog

```

2\. less

less命令用于分页查看文件内容，可以方便地向上或向下滚动，适用于查看较长的日志文件。

示例：

```
less /var/log/syslog

```

3\. tail

tail命令用于查看文件的末尾部分，默认显示最后10行。常用选项包括-n指定行数和-f用于实时跟踪日志文件的更新。

示例：

```
tail -n 20 /var/log/syslog``tail -f /var/log/syslog

```

4\. head

head命令用于查看文件的开头部分，默认显示前10行。

示例：

```
head -n 20 /var/log/syslog

```

5\. grep

grep命令用于在文件中搜索匹配特定模式的行，适用于从日志文件中提取特定信息。

示例：

```
grep "error" /var/log/syslog``grep -i "fail" /var/log/auth.log

```

选项-i表示忽略大小写。

6\. awk

awk是一种强大的文本处理工具，适用于从日志文件中提取和处理结构化信息。

示例：

```
awk '{print $1, $2, $3}' /var/log/syslog

```

上述命令提取并显示日志文件的前三列。

7.sed

sed是一种流编辑器，适用于对日志文件进行搜索、替换和过滤。

示例：

sed -n ‘/error/p’ /var/log/syslog

上述命令显示包含“error”的行。

#### 2.14.5 分析日志文件的工具

1. logrotate

logrotate命令用于管理和轮转日志文件，防止日志文件过大。配置文件通常位于/etc/logrotate.conf和/etc/logrotate.d/目录下。

示例配置：

```
/var/log/syslog {`    `daily`    `rotate 7`    `compress`    `missingok`    `notifempty``}

```

上述配置表示对/var/log/syslog进行每日轮转，保留7个历史日志文件，并进行压缩。

2. journalctl

journalctl命令用于查看和分析systemd的日志信息。

示例：

```
journalctl -xe``journalctl -u nginx.service

```

选项-xe用于显示最近的日志并详细解释错误，-u用于查看特定服务的日志。

3. logwatch

logwatch是一款日志分析工具，可以生成易读的日志报告。

安装并使用示例：

```
sudo apt-get install logwatch``sudo logwatch --detail High --mailto admin@example.com --service all --range today

```

上述命令生成详细的日志报告，并通过邮件发送给管理员

4. goaccess

goaccess是一款实时的Web日志分析工具，适用于分析Apache和Nginx日志。

安装并使用示例：

sudo apt-get install goaccess\`\`goaccess /var/log/nginx/access.log -o report.html --log-format=COMBINED

上述命令生成一份HTML格式的访问日志报告。

#### 2.14.6.Rsyslog 详解

 日常工作中，常遇到些问题，会查看Linux的系统日志，日志多种多样，boot.log, messages, auth.log, syslog等等，但每次出现问题总是凭借直觉和经验去一个一个翻，是下下策。搭建ELK，或者Graylog等日志分析系统也是极好的，但是体积太大了，需要考虑和维护的东西也就更多。故而通过一些更轻量级的配置，加上自己的一些理解分析，想实现一套日志分析系统。而针对系统模块的日志，首当其冲的是要搞定rsyslog。

 以下主要是针对rsyslog配置的讲解。

##### 日志整理

对日志进行分析，首先第一步要规整日志。

- `/etc/rsyslog.conf` 是rsyslog服务的总配置文件
- `/etc/rsyslog.d` 该目录是单独配置的rsyslog配置文件

```bash
vim /etc/rsyslog.conf

# Include all config files in /etc/rsyslog.d/
#
$IncludeConfig /etc/rsyslog.d/*.conf

# 个人建议，将所有的rule都配置在该目录下，在/etc/rsyslog.conf中不写rule
```

##### 结合使用的 rule 示例（懒人福利，CV大法，即粘即用）

```bash
# 记录mail日志等级为error及以上日志
mail.err							/var/log/mail_err.log

# 记录mail所有等级为warn级别的日志（仅记录warn级别）
mail.=warn							/var/log/mail_err.log

# 记录kern所有日志
kern.*								/var/log/kern.log

# 将mail的所有信息，除了info以外，其他的都写入/var/adm/mail
mail.*;mail.!=info   /var/adm/mail

# 将日志等级为crit或更高的内核消息定向到远程主机finlandia
# 如果主机崩溃，磁盘出现不可修复的错误，可能无法读取存储的消息。如果有日志在远程主机上，可以尝试找出崩溃的原因。
kern.crit    						 @finlandia

# 记录所有类型的warning等级及以上日志
*.warning							/var/log/syslog_warn.log

# 记录mail的warning日志和kern的error日志,其他所有的info日志
*.info;mail.warning;kern.error		/var/log/messages

# 记录kernel的info到warning日志
kern.info;kern.!err   /var/adm/kernel-info

# 将mail和news的info级别日志写入/var/adminfo
mail,news.=info    /var/adm/info

# 将所有系统中所有类型的info日志和notice日志存入/var/log/massages,mail的所有日志除外。
*.=info;*.=notice;\
mail.none /var/log/messages

# 紧急消息（emerg级别）将使用wall显示给当前所有登录的用户
*.=emerg   		*

# 该规则将所有alert以及更高级别的消息定向到操作员的终端，即登录的用户“root”和“joey”的终端。
*.alert      root,joey
```

##### 注意事项

单独把这个拎出来写。

上面的大概就是所有能用到的规则了，而这些规则有时候还是有些问题的

For example?

Exapmple A

```bash
mail.crit,*.err					/var/log/syslog_err.log

# 这样的情况，最终的结果还是会把mail的err级别日志输出到syslog_err.log
```

Exapmple B

```bash
mail.!warn						/var/log/mail.log

# 看起来是将mail的warn以下级别的日志输出到/var/log/mail.log，其实不然，你会发现你什么也得不到。
# 官方的解释是，感叹号(就是形似这个的符号 ==> !) 就是个过滤器,你得先有东西，才能去过去，比如：
mail.*;mail.!warn						/var/log/mail.log
```

Exapmple C

如果在规则结束后立即使用反斜杠，而中间没有空格，那么使用反斜杠将行一分为二是无效的。

以上都是官方建议

以下是个人建议

 既然是规整日志，不管是出于什么原因，那一定是为了用起来更方便，看起来更简洁。别整太多花里胡哨的，实用就行。想明白自己要啥效果，捡自己用得着的看就行。莫要本末倒置，化简为繁。

- 修改系统日志的输出格式

##### rsyslog Properties

**模板元素属性**

| 属性                    | 释义\|                                                       |
| ----------------------- | ------------------------------------------------------------ |
| **msg**                 | 日志的信息内容，message。                                    |
| **rawmsg**              | 不转义的日志内容。转义是默认开启的(EscapecontrolCharactersOnReceive),所以它有可能与socket中接收到的内容不同。 |
| **rawmsg-after-pri**    | 几乎与rawmsg相同，但是删除了syslog PRI。                     |
| **hostname**            | 打印该日志的主机名。                                         |
| **source**              | hostname属性的别名。                                         |
| **fromhost**            | 接收的信息来自于哪个节点。这里是DNS解析的名字。              |
| **fromhost-ip**         | 接收的信息来自于哪个节点，这里是IP，本地的是127.0.0.1。      |
| **syslogtag**           | 信息标签。大致形如 programed[14321] 。                       |
| **programname**         | tag的一部分，就是上面的programed那个位置。                   |
| **pri**                 | 消息的PRI部分-未解码(单值)                                   |
| **pri-text**            | 文本形式的消息的PRI部分，并在括号中添加数值PRI(例如“local0.err<133>”) |
| **iut**                 | InfoUnitType 一款监视器软件，在与监视器后端通信的时候使用    |
| **syslogfacility**      | 设备信息，数字形式表示                                       |
| **syslogfacility-text** | 设备信息，文本形式表示                                       |
| **syslogseverity**      | 日志严重性等级，数字形式表示                                 |
| **syslogseverity-text** | 日志严重性等级，文本形式表示                                 |
| **syslogpriority**      | 同 syslogseverity                                            |
| **syslogpriority-text** | 同 syslogseverity-text                                       |
| **timegenerated**       | 高精度时间戳                                                 |
| **timereported**        | 日志中的时间戳。精度取决于日志中提供的内容(在大多数情况下，为秒级) |
| **timestamp**           | 同 timereported                                              |
| **protocol-version**    | IETF draft draft-ietf-syslog-protocol 中的 PROTOCOL-VERSION 字段的内容 |
| **structured-data**     | IETF draft draft-ietf-syslog-protocol 中的 STRUCTURED-DATA 字段的内容 |
| **app-name**            | IETF draft draft-ietf-syslog-protocol 中的 APP-NAME 字段的内容 |
| **procid**              | IETF draft draft-ietf-syslog-protocol 中的 PROCID 字段的内容 |
| **msgid**               | IETF draft draft-ietf-syslog-protocol 中的 MSGID 字段的内容  |
| **inputname**           | 生成日志的输入模块的名称(如“imuxsock”、“imudp”)              |
| **jsonmesg**            | 整个日志对象作为json表示。可能出现数据重复，譬如syslogtag包含着programname，但两者都会分别表示。所以这个属性有一些额外开销，建议只有在实际需要的时候再用。 |

##### Time-Related System Properties

**与时间相关的系统属性**（以 `2020-07-08 16:57:36` 为例）

| 属性        | 释义                                            |
| ----------- | ----------------------------------------------- |
| **$now**    | 当前日期时间戳，格式为YYYY-MM-DD (2020-07-08)   |
| **$year**   | 当前年份， 四位数 (2020)                        |
| **$month**  | 当前月份， 两位数 (07)                          |
| **$day**    | 当前月份的日期，两位数 (08)                     |
| **$wday**   | 当前天数周几 ：0=Sunday,...6=Saturday           |
| **$hour**   | 当前小时（24小时机制），两位数(16)              |
| **$hhour**  | 半小时机值，就是0-29分钟显示0，30-59分钟显示1。 |
| **$qhour**  | 一刻钟机值，通过0-3显示，每15分钟一截。         |
| **$minute** | 当前分钟数，两位数(57)                          |

通过模板修改日志

```bash
vim /etc/rsyslog.conf

# 创建一个名为cky_format的模板，其中 TIMESTAMP:8:15 表示timestamp属性值切片第八位到第十五位。
$template cky_format, "%$NOW% %TIMESTAMP:8:15% %hostname% %syslogseverity-text% %syslogtag% %msg%\n"
$ActionFileDefaultTemplate cky_format

#重启rsyslog
systemctl restart rsyslog
```

日志格式效果样例

```bash
# NOW | timestamp:8:15| hostname| syslogseverity-text | syslogtag | msg
2020-07-09 09:59:54 mycomputer    info    systemd:  Started System Logging Service.
#    时间戳         | 	 主机名   | 日志等级 | 服务进程 |   日志内容
```

#### 2.14.7.Linux日志轮替（日志转储）及logrotate配置文件分析

日志是重要的系统文件，记录和保存了系统中所有的重要事件。但是日志文件也需要进行定期的维护，因为日志文件是不断增长的，如果完全不进行日志维护，而任由其随意递增，那么用不了多久，我们的硬盘就会被写满。  

**日志维护的最主要的工作就是把旧的日志文件删除，从而腾出空间保存新的日志文件。**这项工作如果靠管理员手工来完成，那其实是非常烦琐的，而且也容易忘记。那么 Linux 系统是否可以自动完成日志的轮替工作呢？  

logrotate 就是用来进行日志轮替（也叫日志转储）的，也就是把旧的日志文件移动并改名，同时创建一个新的空日志文件用来记录新日志，当旧日志文件超出保存的范围时就删除。  

##### 日志文件的命名规则

日志轮替最主要的作用就是把旧的日志文件移动并改名，同时建立新的空日志文件，当旧日志文件超出保存的范围时就删除。那么，旧的日志文件改名之后，如何命名呢？主要依靠 /etc/logrotate.conf 配置文件中的“dateext”参数。  

如果配置文件中有“dateext”参数，那么日志会用日期来作为日志文件的后缀，如“secure-20130605”。这样日志文件名不会重叠，也就不需要对日志文件进行改名，只需要保存指定的日志个数，删除多余的日志文件即可。  

如果配置文件中没有“dateext”参数，那么日志文件就需要进行改名了。当第一次进行日志轮替时，当前的“secure”日志会自动改名为“secure.1”，然后新建“secure”日志，用来保存新的日志；当第二次进行日志轮替时，“secure.1”会自动改名为“secure.2”，当前的“secure”日志会自动改名为“secure.1”，然后也会新建“secure”日志，用来保存新的日志；以此类推。  

##### logrotate配置文件

我们来查看一下 logrotate 的配置文件 /etc/logrotate.conf 的默认内容。  

\[root@localhost ~\]# vi /etc/logrotate.conf  
```
#see "man logrotate" for details  
#rotate log files weekly  
weekly  
#每周对日志文件进行一次轮替  
#keep 4 weeks worth of backlogs rotate 4  
#保存4个日志文件,也就是说,如果进行了5次日志轮替，就会删除第一个备份曰志  
#create new (empty) log files after rotating old ones create  
#在日志轮替时,自动创建新的日志文件  
#use date as a suffix of the rotated file dateext  
#使用日期作为日志轮替文件的后缀  
#uncomment this if you want your log files compressed #compress  
#日志文件是否压缩。如果取消注释,则日志会在转储的同时进行压缩  
#以上日志配置为默认配置,如果需要轮替的日志没有设定独立的参数,那么都会遵循以上参数  
#如果轮替曰志配置了独立参数,那么独立参数的优先级更高  
#RPM packages drop log rotation information into this directory include /etc/logrotate.d  
#包含/etc/logrotate.d/目录中所有的子配置文件。也就是说,会把这个目录中所有的子配置文件读取进来，进行日志轮替  
#no packages own wtmp and btmp -- we'11 rotate them here  
#以下两个轮替曰志有自己的独立参数，如果和默认的参数冲突，则独立参数生效  
/var/log/wtmp {  
#以下参数仅对此目录有效  
monthly  
#每月对日志文件进行一次轮替  
create 0664 root utmp  
#建立的新日志文件,权限是0664,所有者是root,所属组是utmp组  
minsize 1M  
#日志文件最小轮替大小是1MB。也就是日志一定要超过1MB才会轮替，否则就算时间达到一个月，也不进行曰志轮替  
rotate 1  
#仅保留一个曰志备份。也就是只保留wtmp和wtmp.1曰志)  
/var/log/btmp {  
#以下参数只对/var/log/btmp生效  
missingok  
#如果日志不存在,则忽略该日志的警告信患  
monthly  
create 0600 root utmp  
rotate 1  
}  
# system-specific logs may be also be configured here.
```



在这个配置文件中，主要分为三部分：

*   第一部分是默认设置，如果需要转储的日志文件没有特殊配置，则遵循默认设置的参数；
*   第二部分是读取 /etc/logrotate.d/ 目录中的日志轮替的子配置文件，也就是说，在 /etc/logrotate.d/ 目录中的所有符合语法规则的子配置文件也会进行日志轮替；
*   第三部分是对 wtmp 和 btmp 日志文件的轮替进行设定，如果此设定和默认参数冲突，则当前设定生效（如 wtmp 的当前参数设定的轮替时间是每月，而默认参数的轮替时间是每周，则对 wtmp 这个日志文件来说，轮替时间是每月，当前的设定参数生效）。


logrotate 配置文件的主要参数如表 1 所示。  


<table><caption>表 1 logrotate配置文件的主要参数</caption><tbody><tr><th>参&nbsp;致</th><th>参数说明</th></tr><tr><td>daily</td><td>日志的轮替周期是毎天</td></tr><tr><td>weekly</td><td>日志的轮替周期是每周</td></tr><tr><td>monthly</td><td>日志的轮控周期是每月</td></tr><tr><td>rotate数宇</td><td>保留的日志文件的个数。0指没有备份</td></tr><tr><td>compress</td><td>当进行日志轮替时，对旧的日志进行压缩</td></tr><tr><td>create mode owner group</td><td>建立新日志，同时指定新日志的权限与所有者和所属组.如create 0600 root utmp</td></tr><tr><td>mail address</td><td>当进行日志轮替时.输出内存通过邮件发送到指定的邮件地址</td></tr><tr><td>missingok</td><td>如果日志不存在，则忽略该日志的警告信息</td></tr><tr><td>nolifempty</td><td>如果曰志为空文件，則不进行日志轮替</td></tr><tr><td>minsize&nbsp;大小</td><td>日志轮替的最小值。也就是日志一定要达到这个最小值才会进行轮持，否则就算时间达到也不进行轮替</td></tr><tr><td>size大小</td><td>日志只有大于指定大小才进行日志轮替，而不是按照时间轮替，如size&nbsp;100k</td></tr><tr><td>dateext</td><td>使用日期作为日志轮替文件的后缀，如secure-20130605</td></tr><tr><td>sharedscripts</td><td>在此关键宇之后的脚本只执行一次</td></tr><tr><td>prerotate/cndscript</td><td>在曰志轮替之前执行脚本命令。endscript标识prerotate脚本结束</td></tr><tr><td>postrolaie/endscripl</td><td>在日志轮替之后执行脚本命令。endscripi标识postrotate脚本结束</td></tr></tbody></table>


这些参数中较为难理解的应该是 prerotate/endscript 和 postrotate/endscript，我们利用“man logrotate”中的例子来解释一下这两个参数。例如：  

```
"/var/log/httpd/access.log" /var/log/httpd/error.log {  
#日志轮替的是/var/log/httpd/中RPM包默认安装的apache正确访问日志和错误日志  
    rotate 5  
    #轮替5次  
    mail www@my.org  
    #把信息发送到指定邮箱  
    size 100k  
    #日志大于100KB时才进行日志轮替,不再按照时间轮替  
    sharedscripts  
    #以下脚本只执行一次  
    postrotate  
    #在日志轮替结束之后,执行以下脚本  
    /usr/bin/killall -HUP httpd  
    #重启apache 服务  
endscript  
#脚本结束  
}
```



prerotate 和 postrotate 主要用于在日志轮替的同时执行指定的脚本，一般用于日志轮替之后重启服务。这里强调一下，如果你的日志是写入 rsyslog 服务的配置文件的，那么把新日志加入 logrotate 后，一定要重启 rsyslog 服务，否则你会发现，虽然新日志建立了，但数据还是写入了旧的日志当中。那是因为虽然 logrotate 知道日志轮替了，但是 rsyslog 服务并不知道。  

同理，如果采用源码包安装了 apache、Nginx 等服务，则需要重启 apache 或 Nginx 服务，同时还要重启 rsyslog 服务，否则日志也不能正常轮替。  

不过，这里有一个典型应用就是给予特定的日志加入 chattr 的 a 属性。如果系统文件加入了 a 属性，那么这个文件就只能增加数据，而不能删除和修改已有的数据，root 用户也不例外。  

因此，我们会给重要的日志文件加入 a 属性，这样就可以保护日志文件不被恶意修改。不过，一旦加入了 a 属性，那么在进行日志轮替时，这个日志文件是不能被改名的，当然也就不能进行日志轮替了。我们可以利用 prerotate 和 postrotate 参数来修改日志文件的 chattr 的 a 属性。  

##### 把自己的日志加入日志轮替

如果有些日志默认没有加入日志轮替（比如源码包安装的服务的日志，或者自己添加的日志），那么这些日志默认是不会进行日志轮替的，这样当然不符合我们对日志的管理要求。如果需要把这些日志也加入日志轮替，那该如何操作呢？  

这里有两种方法：

*   第一种方法是直接在 /etc/logrotate.conf 配置文件中写入该日志的轮替策略，从而把日志加入轮替；
*   第二种方法是在 /etc/logrotate.d/ 目录中新建立该日志的轮替文件，在该轮替文件中写入正确的轮替策略，因为该目录中的文件都会被包含到主配置文件中，所以也可以把日志加入轮替。


我们推荐第二种方法，因为系统中需要轮替的日志非常多，如果全部直接写入 /etc/logrotate.conf 配置文件，那么这个文件的可管理性就会非常差，不利于此文件的维护。  

说起来很复杂，我们举个例子。还记得我们自己生成的 /var/log/alert.log 日志吗？这个日志不是系统默认日志，而是我们通过 /etc/rsyslog.conf 配置文件自己生成的日志，所以默认这个日志是不会进行轮替的。如果我们需要把这个日志加入日志轮替策略，那该怎么实现呢？我们采用第二种方法，也就是在 /etc/logrotate.d/ 目录中建立此日志的轮替文件。  

具体步骤如下：  

```
[root@localhost ~]# chattr +a /var/log/alert.log #先给日志文件赋予chattr的a属性，保证日志的安全  
[root@localhost ~]# vi /etc/logrotate.d/alter  
#创建alter轮替文件,把/var/log/alert.log加入轮替  
/var/log/alert.log {  
    weekly  
    #每周轮替一次  
    rotate 6  
    #保留6个轮替曰志  
    sharedscripts  
    #以下命令只执行一次  
    prerotate  
    #在日志轮替之前执行  
        /usr/bin/chattr -a /var/log/alert.log  
        #在日志轮替之前取消a属性,以便让日志可以轮替  
    endscript  
    #脚本结朿  
    sharedscripts  
    postrotate  
    #在日志轮替之后执行  
        /usr/bin/chattr +a /var/log/alert.log  
        #在日志轮替之后,重新加入a属性  
    endscript  
    sharedscripts  
    postrotate  
    /bin/kill -HUP $(/bin/cat /var/run/syslogd.pid 2>/dev/null) fi>/dev/null  
    endscript  
    #重启rsyslog服务，保证日志轮替正常进行  
}
```



这样我们自己生成的日志 /var/log/alert.log 也就可以进行日志轮替了，当然这些配置信息也是可以直接写入 /etc/logrotate.conf 这个配置文件的。

### 2.15.定制自己的linux系统

[CSDN-Linux操作系统——定制自己的 Linux 系统](https://blog.csdn.net/m0_61163395/article/details/126044857)

### 2.16.Linux内核阅读

[腾讯云开发者社区-好多网友都不知道怎么阅读Linux内核源码，这篇让你快速理解](https://cloud.tencent.com/developer/article/2137779)

### 2.17.Linux内核升级

[CSDN-centos内核升级](https://blog.csdn.net/MssGuo/article/details/127184206)

[CSDN-ubuntu内核升级](https://blog.csdn.net/Long_xu/article/details/126710992)

### 2.18.Linux备份与恢复

[博客园-linux数据备份、复原 | dump restore指令](https://www.cnblogs.com/CrispyCandy/p/17586360.html)

### 2.19.可视化管理Linux

[CSDN-Linux 可视化管理（webmin 和 bt(宝塔)运维工具的详细安装教程：webmin 安装配置和使用，bt 宝塔 的安装配置和使用）](https://blog.csdn.net/weixin_57558097/article/details/127730188)

#### Webmin

[Webmin官网](https://webmin.com/)

Webmin是一个基于Web的管理工具，可以使用它在服务器上执行几乎所有系统管理任务，包括创建用户帐户和数据库，以及配置和管理磁盘配额，PHP，MySQL和其他开源应用。它的功能也可以使用在线提供的众多第三方模块中的任何一个进行扩展。

[![img](/Users/dongbinyu/learningNotes/images/Linux+Shell笔记/8a5367cb099078110fc19ee68bf5acef.jpg)](https://s4.51cto.com/oss/201907/17/8a5367cb099078110fc19ee68bf5acef.jpg)

#### Cockpit

[Cockpit官网](https://cockpit-project.org/)

Cockpit是一个开源的，易于使用的基于Web的服务器管理器，由红帽开发，可以在不受任何干扰的情况下有效监控和管理多台服务器。

[![img](/Users/dongbinyu/learningNotes/images/Linux+Shell笔记/85f2eccbfa55cf67b1200bac12e79dbc.jpg)](https://s5.51cto.com/oss/201907/17/85f2eccbfa55cf67b1200bac12e79dbc.jpg)

#### 宝塔工具

[宝塔官网](https://www.bt.cn/new/index.html)

bt 宝塔 Linux 面板是提升运维效率的服务器管理软件，支持一键 [LAMP](https://so.csdn.net/so/search?q=LAMP&spm=1001.2101.3001.7020)/LNMP/集群/监控/网站/FTP/数据库/JAVA 等多项服务器管理功能。

### 2.20.Linux面试题

P142后面，哥们学不下去了，太长了

https://www.bilibili.com/video/BV1Sv411r7vd?vd_source=f58f2e2144be4e99a8cf800afeecbbcb&spm_id_from=333.788.videopod.episodes&p=142
