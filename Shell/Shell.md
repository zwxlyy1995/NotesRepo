# Shell Notes

***

[TOC]

***

## 说明
本文非原创

***

## 1. 基础

### 1.1 Shell环境

Shell 编程跟 JavaScript、php 编程一样，只要有一个能编写代码的文本编辑器和一个能解释执行的脚本解释器就可以了。

Linux 的 Shell 种类众多，常见的有：
  + Bourne Shell（/usr/bin/sh或/bin/sh）
  + Bourne Again Shell（/bin/bash）
  + C Shell（/usr/bin/csh）
  + K Shell（/usr/bin/ksh）
  + Shell for Root（/sbin/sh）

### 1.2 第一个Shell脚本

打开文本编辑器(可以使用 vi/vim 命令来创建文件)，新建一个文件 `test.sh`，扩展名为 sh（sh代表shell），扩展名并不影响脚本执行。
```
#!/bin/bash
```
`#!` 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell。

echo 命令用于向窗口输出文本。 

### 1.3 运行Shell

* 作为可执行程序
  ```
  chmod +x ./test.sh  #使脚本具有执行权限
  ./test.sh  #执行脚本
  ```
  注意，一定要写成 `./test.sh`，而不是 `test.sh`，运行其它二进制的程序也一样，直接写 `test.sh`，linux 系统会去 PATH 里寻找有没有叫 `test.sh` 的，而只有 `/bin`, `/sbin`, `/usr/bin`，`/usr/sbin` 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 `test.sh` 是会找不到命令的，要用 `./test.sh` 告诉系统说，就在当前目录找。

* 作为解释器参数
  这种运行方式是，直接运行解释器，其参数就是 shell 脚本的文件名，如：
  ```
  /bin/sh test.sh
  /bin/php test.php
  ``` 

***

## 2. Shell变量

### 2.1 定义变量

定义变量时，变量名不加美元符号（$，PHP语言中变量需要），如：
```
name="tom"
```
  **注意**，变量名和等号之间不能有空格，同时，变量名的命名须遵循如下规则：
  + 命名只能使用英文字母，数字和下划线，首个字符不能以数字开头。
  + 中间不能有空格，可以使用下划线（_）。
  + 不能使用标点符号。
  + 不能使用bash里的关键字（可用help命令查看保留关键字）。
  
除了显式地直接赋值，还可以用语句给变量赋值，如：
```
for file in `ls /etc`
或
for file in $(ls /etc)
```
以上语句将 `/etc` 下目录的文件名循环出来。

### 2.2 使用变量

使用一个定义过的变量，只要在变量名前面加美元符号即可，如：
```
your_name="tom"
echo $your_name
echo ${your_name}
```
变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界，比如下面这种情况：
```
for skill in Ada Coffe Action Java; do
    echo "I am good at ${skill}Script"
done
```
如果不给skill变量加花括号，写成echo "I am good at $skillScript"，解释器就会把\$skillScript当成一个变量（其值为空）

已定义的变量，可以被重新定义，如：
```
your_name="tom"
echo $your_name
your_name="alibaba"
echo $your_name
```
这样写是合法的，但注意，第二次赋值的时候不能写\$your_name="alibaba"，使用变量的时候才加美元符（$）。

### 2.3 只读变量

使用 readonly 命令可以将变量定义为只读变量，只读变量的值不能被改变。

下面的例子尝试更改只读变量，结果报错：
```
#!/bin/bash
myUrl="https://www.google.com"
readonly myUrl
myUrl="https://www.runoob.com"
```
运行脚本，结果如下：
```
/bin/sh: NAME: This variable is read only.
```

### 2.4 删除变量

使用 unset 命令可以删除变量。语法：
```
unset variable_name
```
变量被删除后不能再次使用。unset 命令不能删除只读变量。

实例
```
#!/bin/sh
myUrl="https://www.runoob.com"
unset myUrl
echo $myUrl
```
以上实例执行将没有任何输出。

### 2.5 变量类型

运行shell时，会同时存在三种变量：

  1. 局部变量 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
  2. 环境变量 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
  3. shell变量 shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

### 2.6 字符串

字符串可以用单引号，也可以用双引号，也可以不用引号。
* 单引号
    ```
    str='this is a string'
    ```
    单引号字符串的限制：
    + 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
    + 单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。
* 双引号
    ```
    your_name='runoob'
    str="Hello, I know you are \"$your_name\"! \n"
    echo -e $str
    ```
    输出结果为：
    ```
    Hello, I know you are "runoob"! 
    ```
    双引号的优点：
    + 双引号里可以有变量
    + 双引号里可以出现转义字符

#### 2.6.1 拼接字符串

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

#### 2.6.2 获取字符串长度

string="abcd"
echo ${#string} #输出 4

#### 2.6.3 提取字符串

以下实例从字符串第 2 个字符开始截取 4 个字符：
```
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```
注意：第一个字符的索引值为 0。

#### 2.6.4 查找字符串

查找字符 i 或 o 的位置(哪个字母先出现就计算哪个)：
```
string="runoob is a great site"
echo `expr index "$string" io`  # 输出 4
```
注意： 以上脚本中 ` 是反引号，而不是单引号 '，不要看错了哦。

### 2.7 Shell数组

#### 2.7.1 定义数组

在 Shell 中，用括号来表示数组，数组元素用"空格"符号分割开。定义数组的一般形式为：
```
数组名=(值1 值2 ... 值n)
```
例如：
```
array_name=(value0 value1 value2 value3)
```
或者
```
array_name=(
value0
value1
value2
value3
)
```
还可以单独定义数组的各个分量：
```
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen
```
可以不使用连续的下标，而且下标的范围没有限制。

#### 2.7.2 读取数组

读取数组元素值的一般格式是：
```
${数组名[下标]}
```
例如：
```
valuen=${array_name[n]}
```
使用 @ 符号可以获取数组中的所有元素，例如：
```
echo ${array_name[@]}
```

#### 2.7.3 获取数组长度

获取数组长度的方法与获取字符串长度的方法相同，例如：
```
# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}
# 取得数组单个元素的长度
lengthn=${#array_name[n]}
```

### 2.8 Shell注释

* 以 # 开头的行就是注释，会被解释器忽略。
* 多行注释
```
:<<EOF
注释内容...
注释内容...
注释内容...
EOF
```

EOF也可以被其他符号取代

***

## 3. 传递参数

执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。n 代表一个数字， 为执行的文件名（包含文件路径），1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推……

|参数处理 |	说明|
|-|-|
|$# |	传递到脚本的参数个数|
|$* |	以一个单字符串显示所有向脚本传递的参数。如"\$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。|
|$$ |	脚本运行的当前进程ID号|
|$! |	后台运行的最后一个进程的ID号|
|$@ |	与\$*相同，但是使用时加引号，并在引号中返回每个参数。如"\$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。
|$- |	显示Shell使用的当前选项，与set命令功能相同。|
|$? |	显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。|

\$* 与 \$@ 区别：
  + 相同点：都是引用所有参数。
  + 不同点：只有在双引号中体现出来。假设在脚本运行时写了三个参数 1、2、3，，则 " * " 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）。

***

## 4. Shell运算符

### 4.1 Shell基本运算符

Shell 和其他编程语言一样，支持多种运算符，包括：
+ 算数运算符
+ 关系运算符
+ 布尔运算符
+ 字符串运算符
+ 文件测试运算符
 原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 `awk` 和 `expr`。`expr` 最常用，是一款表达式计算工具，使用它能完成表达式的求值操作。

例如，两个数相加(注意使用的是反引号 ` 而不是单引号 ')：
```
#!/bin/bash

val=`expr 2 + 2`
echo "两数之和为 : $val"
```
两点注意：
1. 表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2，这与我们熟悉的大多数编程语言不一样。
2. 完整的表达式要被 \` \` 包含，注意这个字符不是常用的单引号，在 Esc 键下边。

### 4.2 算术运算符

假定变量 a 为 10，变量 b 为 20：

|运算符 |	说明 |	举例|
|-|-|-|
|+ | 加法 |	\`expr \$a + \$b\` 结果为 30。|
|- | 减法 |	\`expr \$a - \$b\` 结果为 -10。|
|* | 乘法 |	\`expr \$a \* \$b\` 结果为  200。|
|/ | 除法 |	\`expr \$b / \$a\` 结果为 2。|
|% | 取余 |	\`expr \$b % \$a\` 结果为 0。|
|= | 赋值 |	a=$b 将把变量 b 的值赋给 a。|
|== | 相等。用于比较两个数字，相同则返回 true。 |	[ \$a == \$b ] 返回 false。|
|!= | 不相等。用于比较两个数字，不相同则返回 true。 |	[ \$a != \$b ] 返回 true。|

**注意**：条件表达式要放在方括号之间，并且要有空格，例如: [\$a==\$b] 是错误的，必须写成 [ \$a == \$b ]。

### 4.3 关系运算符

关系运算符只支持数字，不支持字符串，除非字符串的值是数字。

下表列出了常用的关系运算符，假定变量 a 为 10，变量 b 为 20：

|运算符 |	说明 |	举例|
|-|-|-|
|-eq |	检测两个数是否相等，相等返回 true。 |	[ \$a -eq \$b ] 返回 false。|
|-ne |	检测两个数是否不相等，不相等返回 true。 |	[ \$a -ne \$b ] 返回 true。|
|-gt |	检测左边的数是否大于右边的，如果是，则返回 true。 |	[ \$a -gt \$b ] 返回 false。|
|-lt |	检测左边的数是否小于右边的，如果是，则返回 true。 |	[ \$a -lt \$b ] 返回 true。|
|-ge |	检测左边的数是否大于等于右边的，如果是，则返回 true。 |	[ \$a -ge \$b ] 返回 false。|
|-le |	检测左边的数是否小于等于右边的，如果是，则返回 true。 |	[ \$a -le \$b ] 返回 true。|

### 4.4 布尔运算符

下表列出了常用的布尔运算符，假定变量 a 为 10，变量 b 为 20：

|运算符 | 说明 | 举例|
|-|-|-|
|! | 非运算，表达式为 true 则返回 false，否则返回 true。 | [ ! false ] 返回 true。|
|-o | 或运算，有一个表达式为 true 则返回 true。 |  [ \$a -lt 20 -o \$b -gt 100 ] 返回 true。|
|-a | 与运算，两个表达式都为 true 才返回 true。 | [ \$a -lt 20 -a \$b -gt 100 ] 返回 false。|

### 4.5 逻辑运算符

假定变量 a 为 10，变量 b 为 20:

|运算符|说明|举例|
|-|-|-|
|&&|逻辑的 AND|[ \$a -lt 100 \&\& \$b -gt 100 ] 返回 false|
|\|\||逻辑的 OR|[ \$a -lt 100 \|\| \$b -gt 100 ] 返回 true|

### 4.6 字符串运算符

假定变量 a 为 "abc"，变量 b 为 "efg"：

|运算符|说明|举例|
|-|-|-|
|=|检测两个字符串是否相等，相等返回 true。|[ \$a = \$b ] 返回 false。|
|!= |检测两个字符串是否相等，不相等返回 true。|[ \$a != \$b ] 返回 true。|
|-z|检测字符串长度是否为0，为0返回 true。|[ -z \$a ] 返回 false。|
|-n |检测字符串长度是否不为 0，不为 0 返回 true。|[ -n "$a" ] 返回 true。|
|$|检测字符串是否为空，不为空返回 true。|[ \$a ] 返回 true。|

### 4.7 文件测试运算符

文件测试运算符用于检测 Unix 文件的各种属性。
属性检测描述如下：

|操作符|说明|举例|
|-|-|-|
|-b file |	检测文件是否是块设备文件，如果是，则返回 true。 |	[ -b $file ] 返回 false。|
|-c file |	检测文件是否是字符设备文件，如果是，则返回 true。 |	[ -c $file ] 返回 false。|
|-d file |	检测文件是否是目录，如果是，则返回 true。 |	[ -d $file ] 返回 false。|
|-f file |	检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 |	[ -f $file ] 返回 true。|
|-g file |	检测文件是否设置了 SGID 位，如果是，则返回 true。 |	[ -g $file ] 返回 false。|
|-k file |	检测文件是否设置了粘着位(Sticky Bit)，如果是，则返回 true。 |	[ -k $file ] 返回 false。|
|-p file |	检测文件是否是有名管道，如果是，则返回 true。| 	[ -p $file ] 返回 false。|
|-u file |	检测文件是否设置了 SUID 位，如果是，则返回 true。| 	[ -u $file ] 返回 false。|
|-r file |	检测文件是否可读，如果是，则返回 true。 |	[ -r $file ] 返回 true。|
|-w file |	检测文件是否可写，如果是，则返回 true。 |	[ -w $file ] 返回 true。|
|-x file |	检测文件是否可执行，如果是，则返回 true。| 	[ -x $file ] 返回 true。|
|-s file |	检测文件是否为空（文件大小是否大于0），不为空返回 true。 |	[ -s $file ] 返回 true。|
|e file |	检测文件（包括目录）是否存在，如果是，则返回 true。 |	[ -e $file ] 返回 true。|

其他检查符：
+ -S: 判断某文件是否 socket。
+ -L: 检测文件是否存在并且是一个符号链接。 

***

## 5. Shell echo命令

1. 显示普通字符串:
```
echo "It is a test"
```
这里的双引号完全可以省略，以下命令与上面实例效果一致：
```
echo It is a test
```
2. 显示转义字符
```
echo "\"It is a test\""
```
结果将是:
```
"It is a test"
```
同样，双引号也可以省略
3. 显示变量

read 命令从标准输入中读取一行,并把输入行的每个字段的值指定给 shell 变量
```
#!/bin/sh
read name 
echo "$name It is a test"
```
以上代码保存为 test.sh，name 接收标准输入的变量，结果将是:
```
[root@www ~]# sh test.sh
OK                     #标准输入
OK It is a test        #输出
```
4. 显示换行
```
echo -e "OK! \n" # -e 开启转义
echo "It is a test"
```
输出结果：
```
OK!

It is a test
```
5. 显示不换行
```
#!/bin/sh
echo -e "OK! \c" # -e 开启转义 \c 不换行
echo "It is a test"
```
输出结果：
```
OK! It is a test
```
6. 显示结果定向至文件
```
echo "It is a test" > myfile
```
7. 原样输出字符串，不进行转义或取变量(用单引号)
```
echo '$name\"'
```
输出结果：
```
$name\"
```
8. 显示命令执行结果
```
echo `date`
```
注意： 这里使用的是反引号 `, 而不是单引号 '。

结果将显示当前日期
```
Thu Jul 24 10:08:46 CST 2014
```

***

## 6. Shell printf命令

`printf` 命令模仿 C 程序库（library）里的 `printf()` 程序。

`printf` 由 POSIX 标准所定义，因此使用 `printf` 的脚本比使用 `echo` 移植性好。

`printf` 使用引用文本或空格分隔的参数，外面可以在 `printf` 中使用格式化字符串，还可以制定字符串的宽度、左右对齐方式等。默认 `printf` 不会像 `echo` 自动添加换行符，我们可以手动添加 `\n`。

`printf` 命令的语法：
```
printf  format-string  [arguments...]
```
参数说明：
  + format-string: 为格式控制字符串
  + arguments: 为参数列表。

**%s %c %d %f都是格式替代符**
%-10s 指一个宽度为10个字符（-表示左对齐，没有则表示右对齐），任何字符都会被显示在10个字符宽的字符内，如果不足则自动以空格填充，超过也会将内容全部显示出来。
%-4.2f 指格式化为小数，其中.2指保留2位小数。

* printf的转义序列
  
|序列|说明|
|-|-|
|\a	|警告字符，通常为ASCII的BEL字符|
|\b	|后退|
|\c	|抑制（不显示）输出结果中任何结尾的换行字符（只在%b格式指示符控制下的参数字符串中有效），而且，任何留在参数里的字符、任何接下来的参数以及任何留在格式字符串中的字符，都被忽略|
|\f	|换页（formfeed）|
|\n |换行|
|\r	|回车（Carriage return）|
|\t	|水平制表符|
|\v	|垂直制表符|
|\\\\	|一个字面上的反斜杠字符|
|\ddd 	|表示1到3位数八进制值的字符。仅在格式字符串中有效|
|\0ddd	|表示1到3位的八进制值字符|

***

## 7. Shell test命令

Shell中的 test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。

### 7.1 数值测试

|参数 |	说明|
|-|-|
|-eq |	等于则为真|
|-ne |	不等于则为真|
|-gt |	大于则为真|
|-ge |	大于等于则为真|
|-lt |	小于则为真|
|-le |	小于等于则为真|

实例
```
num1=100
num2=100
if test $[num1] -eq $[num2]
then
    echo '两个数相等！'
else
    echo '两个数不相等！'
fi
```
输出结果：
```
两个数相等！
```
代码中的 [] 执行基本的算数运算，如：
实例
```
#!/bin/bash

a=5
b=6

result=$[a+b] # 注意等号两边不能有空格
echo "result 为： $result"
```
结果为:
```
result 为： 11
```

### 7.2 字符串测试

|参数 |	说明|
|-|-|
|= |	等于则为真|
|!= |	不相等则为真|
|-z 字符串 |	字符串的长度为零则为真|
|-n 字符串 |	字符串的长度不为零则为真|

### 7.3 文件测试

|参数 |	说明|
|-|-|
|-e 文件名 |	如果文件存在则为真|
|-r 文件名 |	如果文件存在且可读则为真|
|-w 文件名 |	如果文件存在且可写则为真|
|-x 文件名 |	如果文件存在且可执行则为真|
|-s 文件名 |	如果文件存在且至少有一个字符则为真|
|-d 文件名 |	如果文件存在且为目录则为真|
|-f 文件名 |	如果文件存在且为普通文件则为真|
|-c 文件名 |	如果文件存在且为字符型特殊文件则为真|
|-b 文件名 |	如果文件存在且为块特殊文件则为真|

Shell 还提供了与( `-a` )、或( `-o `)、非( `!` )三个逻辑操作符用于将测试条件连接起来，其优先级为： `!` 最高， `-a`次之， `-o` 最低。例如：
实例
```
cd /bin
if test -e ./notFile -o -e ./bash
then
    echo '至少有一个文件存在!'
else
    echo '两个文件都不存在'
fi
```
输出结果：
```
至少有一个文件存在!
```

***

## 8. Shell 流程控制

### 8.1 if else

* if
  
if 语句语法格式：
```
if condition
then
    command1 
    command2
    ...
    commandN 
fi
```
写成一行（适用于终端命令提示符）：
```
if [ $(ps -ef | grep -c "ssh") -gt 1 ]; then echo "true"; fi
```
末尾的fi就是if倒过来拼写，后面还会遇到类似的。 

* if else

if else 语法格式：
```
if condition
then
    command1 
    command2
    ...
    commandN
else
    command
fi
```

* if else-if else

if else-if else 语法格式：
```
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

### 8.2 for循环

for循环一般格式为：
```
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```
写成一行：
```
for var in item1 item2 ... itemN; do command1; command2… done;
```
当变量值在列表里，for循环即执行一次所有命令，使用变量名获取列表中的当前取值。命令可为任何有效的shell命令和语句。in列表可以包含替换、字符串和文件名。 

n列表是可选的，如果不用它，for循环使用命令行的位置参数。

例如，顺序输出当前列表中的数字：
```
for loop in 1 2 3 4 5
do
    echo "The value is: $loop"
done
```

### 8.3 While语句

命令通常为测试条件。其格式为：
```
while condition
do
    command
done
```

以下是一个基本的while循环，测试条件是：如果int小于等于5，那么条件返回真。int从0开始，每次循环处理时，int加1。运行上述脚本，返回数字1到5，然后终止。
```
#!/bin/bash
int=1
while(( $int<=5 ))
do
    echo $int
    let "int++"
done
```

while循环可用于读取键盘信息。下面的例子中，输入信息被设置为变量FILM，按<Ctrl-D>结束循环。
```
echo '按下 <CTRL-D> 退出'
echo -n '输入你最喜欢的网站名: '
while read FILM
do
    echo "是的！$FILM 是一个好网站"
done
```

### 8.4 无限循环

无限循环语法格式：
```
while :
do
    command
done
```
或者
```
while true
do
    command
done
```
或者
```
for (( ; ; ))
```

### 8.5 until循环

until 语法格式:
```
until condition
do
    command
done
```

### 8.6 case
```
case 值 in
模式1)
    command1
    command2
    command3
    ;;
模式2）
    command1
    command2
    command3
    ;;
*)
    command1
    command2
    command3
    ;;
esac
```
`case`工作方式如上所示。取值后面必须为单词`in`，每一模式必须以右括号结束。取值可以为变量或常数。匹配发现取值符合某一模式后，其间所有命令开始执行直至 `;;`。

取值将检测匹配的每一个模式。一旦模式匹配，则执行完匹配模式相应命令后不再继续其他模式。如果无一匹配模式，使用星号 `*` 捕获该值，再执行后面的命令。 

例子：
```
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
```

### 8.7 跳出循环

* break命令允许跳出所有循环（终止执行后面的所有循环）。 

* continue命令与break命令类似，只有一点差别，它不会跳出所有循环，仅仅跳出当前循环。 

***

## 9. Shell函数

shell中函数的定义格式如下：
```
[ function ] funname [()]

{

    action;

    [return int;]

}
```

说明：
1. 可以带function fun() 定义，也可以直接fun() 定义,不带任何参数。
2. 参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。 return后跟数值n(0-255 

### 9.1 函数参数

在Shell中，调用函数时可以向其传递参数。在函数体内部，通过 $n 的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数...

带参数的函数示例：
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
**注意**，$10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数。

另外，还有几个特殊字符用来处理参数：
|参数处理 |	说明|
|-|-|
|$# |	传递到脚本或函数的参数个数|
|$* |	以一个单字符串显示所有向脚本传递的参数|
|$$ |	脚本运行的当前进程ID号|
|$! |	后台运行的最后一个进程的ID号|
|$@ |	与$*相同，但是使用时加引号，并在引号中返回每个参数。|
|$- |	显示Shell使用的当前选项，与set命令功能相同。|
|$? |	显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。|

***

## 10. Shell输入、输出重定向

大多数 UNIX 系统命令从你的终端接受输入并将所产生的输出发送回​​到您的终端。一个命令通常从一个叫标准输入的地方读取输入，默认情况下，这恰好是你的终端。同样，一个命令通常将其输出写入到标准输出，默认情况下，这也是你的终端。

重定向命令列表如下：

|命令 |	说明|
|-|-|
|command > file |	将输出重定向到 file。|
|command < file |	将输入重定向到 file。|
|command >> file |	将输出以追加的方式重定向到 file。|
|n > file |	将文件描述符为 n 的文件重定向到 file。|
|n >> file |	将文件描述符为 n 的文件以追加的方式重定向到 file。|
|n >& m |	将输出文件 m 和 n 合并。|
|n <& m |	将输入文件 m 和 n 合并。|
|<< tag |	将开始标记 tag 和结束标记 tag 之间的内容作为输入。|

*需要注意的是文件描述符 0 通常是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。*

### 10.1 输出重定向

重定向一般通过在命令间插入特定的符号来实现。特别的，这些符号的语法如下所示:
```
command1 > file1
```
上面这个命令执行command1然后将输出的内容存入file1。

注意任何file1内的已经存在的内容将被新内容替代。如果要将新内容添加在文件末尾，请使用>>操作符。 

### 10.2 输入重定向

和输出重定向一样，Unix 命令也可以从文件获取输入，语法为：
```
command1 < file1
```
这样，本来需要从键盘获取输入的命令会转移到文件读取内容。

注意：输出重定向是大于号(>)，输入重定向是小于号(<)。 

### 10.3 重定向深入了解

一般情况下，每个 Unix/Linux 命令运行时都会打开三个文件：
+  标准输入文件(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
+  标准输出文件(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
+  标准错误文件(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。

 默认情况下，`command > file` 将 **stdout** 重定向到 file，`command < file`将**stdin**重定向到 file。

如果希望 stderr 重定向到 file，可以这样写：
```
$ command 2 > file
```
如果希望 stderr 追加到 file 文件末尾，可以这样写：
```
$ command 2 >> file
```
2 表示标准错误文件(stderr)。

如果希望将 stdout 和 stderr 合并后重定向到 file，可以这样写：
```
$ command > file 2>&1
```
或者
```
$ command >> file 2>&1
```
如果希望对 stdin 和 stdout 都重定向，可以这样写：
```
$ command < file1 >file2
```
command 命令将 stdin 重定向到 file1，将 stdout 重定向到 file2。 

### 10.4 Here Document

Here Document 是 Shell 中的一种特殊的重定向方式，用来将输入重定向到一个交互式 Shell 脚本或程序。

它的基本的形式如下：
```
command << delimiter
    document
delimiter
```
它的作用是将两个 `delimiter` 之间的内容(`document`) 作为输入传递给 `command。`
注意：
+ 结尾的delimiter 一定要顶格写，前面不能有任何字符，后面也不能有任何字符，包括空格和 tab 缩进。
+ 开始的delimiter前后的空格会被忽略掉。

实例

在命令行中通过 `wc -l` 命令计算 Here Document 的行数：
```
$ wc -l << EOF
    欢迎来到
    菜鸟教程
    www.runoob.com
EOF
3          # 输出结果为 3 行
$
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

### 10.5 /dev/null 文件

如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 `/dev/null`：
```
$ command > /dev/null
```
`/dev/null` 是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。但是 `/dev/null` 文件非常有用，将命令的输出重定向到它，会起到"禁止输出"的效果。

如果希望屏蔽 `stdout` 和 `stderr，可以这样写：`
```
$ command > /dev/null 2>&1
```
*注意：0 是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。
    这里的 2 和 > 之间不可以有空格，2> 是一体的时候才表示错误输出。*

***

## 11. Shell文件包含

和其他语言一样，Shell 也可以包含外部脚本。这样可以很方便的封装一些公用的代码作为一个独立的文件。

Shell 文件包含的语法格式如下：
```
. filename   # 注意点号(.)和文件名中间有一空格
或
source filename
```

实例

创建两个 shell 脚本文件。

`test1.sh` 代码如下：
```
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

url="http://www.runoob.com"
```
`test2.sh` 代码如下：
```
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

#使用 . 号来引用`test1.sh` 文件
. ./test1.sh

# 或者使用以下包含文件代码
# source ./test1.sh

echo "菜鸟教程官网地址：$url"
```
接下来，我们为 `test2.sh` 添加可执行权限并执行：
```
$ chmod +x test2.sh 
$ ./test2.sh 
菜鸟教程官网地址：http://www.runoob.com
```
注：被包含的文件 `test1.sh` 不需要可执行权限。

***

参考来源：<https://www.runoob.com/linux/linux-shell.html>

<p align="right"><b>Author:</b>zwxupc@163.com<p>