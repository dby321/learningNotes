# Excel笔记

F1帮助

## 认识Excel

> fx =1+2 为3

> Excel是用来进行数据存储，数据处理，数据分析，数据呈现的工具。

## 开始-》查找和替换

*：多个字符

？：单个字符

~*：转义字符*

ctrl+F:查找快捷键

## 开始-》定位工具

定义名称

定位条件选空值+ =上方向键 ctrl+enter 填充重复值（常用）

定位条件选对象，选中所有图片

## 数据-》排序和筛选



打印表头 打印设置-》工作表-》顶端标题行

## 数据-》分类汇总和数据有效性

## 插入-》数据透视表

可以生成多个工作表

## 公式-》公式与函数

文本适合左右相连，数值适合加减乘除

相对引用 D4

绝对引用$D$4 也可以选中然后用F4切换

混合引用 $D4 $是锁

rank有两个参数

ctrl+enter批量填充

## IF函数

+用于数字加，&用于文字连接

ISERROR()

AND() 和 OR()

## Countif函数

=countif(A2:A3,A2&“*”)针对大于十五位的数字		

=countifs(E:E,J5,D:D,I5)

## SUMIF和SUMIFS函数

=SUMIF(E:E,H8,F:F) 在E列找H8，然后把F列符合H8的加起来

=SUMIF(F:F,“>=500”) 在F列找>=500的，然后把F列符合条件的加起来。第三条件是F:F，可省略。

=SUMIFS(F:F,D:D,I5,E:E,J5) F:F是求和列

## Vlookup函数

=vlookup(G6,B5:H10,4,0) 0代表精确匹配，B5:H10范围要包含汪梅和原始分

=vlookup(A2&“*”,数据源！A:B,2.0)

年龄匹配，找小于等于最大的，男找女

找数值档次时会用模糊匹配

F12&“”转文本

F12+0转数值，F12*1转数值

isna()找不到错误

=HLOOKUP(E14,$1:$3,3,0) 行

## match和index函数

match(CHOPS,数据源！A:A,0)

index(数据源！B:B,15)

## column和row函数

返回列号和行号

## 常用日期与时间计算

日期是数字，*24再 *60等于分钟数

=date(year(B5),month(B5)+C5,day(B5))

求本月最后一天，=date(year(B13),month(B13)+1,1)-1

求本月有多少天，=day(date(B21),month(B21)+1,0)

求间隔的年数，=datedif(B5,C5,“y”)

=weeknum(B3,2)

=weekday(B8,2)

整容函数，=text(B3,“aaaa”)得到星期几

=text(B10,“0000-00-00”)*1

## 条件格式

## 文本处理函数

left(A3,3)

right(e3,4)

mid(E3,3,3)

find(@,F2)

LEN 返回文本字符串中的字符个数。

LENB 返回文本字符串中用于代表字符的字节数。 

right(A2,lenb(A2)-len(A2))

## 数学函数

Round(E2,2)四舍五入

Roundup(E2,2)向上进位

Rounddown(E2,2)向下进位

int(E2)直接取整

mod(23,7)求余数