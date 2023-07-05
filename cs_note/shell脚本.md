# shell变量

定义变量时，变量名不加$符号，同时`=`前后不能有空格

```sh
name='Joah'
```

使用定义过的变量，只需要在变量名前加$(花括号可选)

```sh
echo $name
echo ${name}
# 加花括号是为了帮助解释器识别变量的边界
echo "my name is ${name}." 
```

**只读变量**不能修改

```sh
name="Joah"
readonly name
name="Tom"
```

**删除变量**，不能删除只读变量

```sh
unset variable_name
```



## **字符串**

字符串是shell编程中最常用最有用的数据类型（除了数字和字符串，也没啥其它类型好用了），字符串可以用单引号，也可以用双引号，也可以不用引号。

### **单引号**

```sh
str='this is a string'
```

单引号字符串的限制：

- 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
- 单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。

### 双引号

```sh
your_name="Joah"
str="Hello, I know you are\"$your_name\"! \n"
echo -e $str
```

输出结果为：

```
Hello, I know you are "Joah"! 
```

双引号的优点：

- 双引号里可以有变量
- 双引号里可以出现转义字符

**单引号与双引号的最大不同在于双引号仍然可以引用变量的内容，但单引号内仅是 普通字符 ，不会作变量的引用，直接输出字符串。**



### 拼接字符串

```sh
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

```sh
hello, runoob ! hello, runoob !
hello, runoob ! hello, ${your_name} !
```

### 获取字符串长度

```sh
string="abcd"
echo ${#string} # 输出4
```

### 字符串提取

```sh
string="hello, world!"
echo ${string:1:4} #输出 ello
```

### 查找子字符串

查找字符 **i** 或 **o** 的位置(哪个字母先出现就计算哪个)

```sh
string="hello, world!"
echo `expr index "$string" lo` # 输出3
```



## 数组

只支持一维数组，不限制数组大小

### 数组定义

在 Shell 中，用括号来表示数组，数组元素用"空格"符号分割开。定义数组的一般形式为：

```sh
# 数组名=(值1 值2 ... 值n)
array_name=(value0 value1 value2 value3)
```

### 关联数组

可以使用任意的字符串、或者整数作为下标来访问数组元素

使用`declare` 命令来声明

```sh
# declare -A array_name
declare -A site=(["google"]="www.google.com" ["runoob"]="www.runoob.com" ["taobao"]="www.taobao.com")
# 或者
site["google"]="www.google.com"
site["runoob"]="www.runoob.com"
site["taobao"]="www.taobao.com"

# 取数据
echo ${site["runoob"]}
```

### 读取数组

```sh
# ${数组名[下标]}
valuen=${array_name[n]}

# 使用 @ 符号可以获取数组中的所有元素，例如：
echo ${array_name[@]}
```

### 获取数组长度

```sh
# 与获取字符串长度的方法相同
# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}
# 取得数组单个元素的长度
lengthn=${#array_name[n]}
```

### 获取数组中的所有元素

```sh
# 使用 @ 或 * 可以获取数组中的所有元素
${site[*]}
${site[@]}

# 在数组前加一个感叹号 ! 可以获取关联数组的所有键，例如：
${!site[*]}
${!site[@]}
```



## 注释

```sh
# 单行注释

:<<EOF
多行注释内容...
多行注释内容...
多行注释内容...
EOF
```



### 参数传递

```sh
echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```

```sh
$ chmod +x test.sh 
$ ./test.sh 1 2 3
Shell 传递参数实例！
执行的文件名：./test.sh
第一个参数为：1
第二个参数为：2
第三个参数为：3
```



# 基本运算符

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用，它是一款表达式计算工具，使用它能完成表达式的求值操作。

```sh
# 表达式和运算符之间要有空格
val=`expr 2 + 2`
echo "两数之和为 : $val"
```

## 算术运算符

假定变量 a 为 10，变量 b 为 20：

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

但bash shell提供了一种更简单的方法来执行数学运算，可以使用$和方括号（`$[operation]`）

```sh
val1=$[1+5]
val2=$[11-5]
val3=$[11/5]
val4=$[11*5]
val5=$[11%5]
```



## 关系运算符

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

## 布尔运算符

下表列出了常用的布尔运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明                                                | 举例                                     |
| :----- | :-------------------------------------------------- | :--------------------------------------- |
| !      | 非运算，表达式为 true 则返回 false，否则返回 true。 | [ ! false ] 返回 true。                  |
| -o     | 或运算，有一个表达式为 true 则返回 true。           | [ $a -lt 20 -o $b -gt 100 ] 返回 true。  |
| -a     | 与运算，两个表达式都为 true 才返回 true。           | [ $a -lt 20 -a $b -gt 100 ] 返回 false。 |

## 逻辑运算符

以下介绍 Shell 的逻辑运算符，假定变量 a 为 10，变量 b 为 20:

| 运算符 | 说明       | 举例                                       |
| :----- | :--------- | :----------------------------------------- |
| &&     | 逻辑的 AND | [[ $a -lt 100 && $b -gt 100 ]] 返回 false  |
| \|\|   | 逻辑的 OR  | [[ $a -lt 100 \|\| $b -gt 100 ]] 返回 true |

## 字符串运算符

下表列出了常用的字符串运算符，假定变量 a 为 "abc"，变量 b 为 "efg"：

| 运算符 | 说明                                         | 举例                     |
| :----- | :------------------------------------------- | :----------------------- |
| =      | 检测两个字符串是否相等，相等返回 true。      | [ $a = $b ] 返回 false。 |
| !=     | 检测两个字符串是否不相等，不相等返回 true。  | [ $a != $b ] 返回 true。 |
| -z     | 检测字符串长度是否为0，为0返回 true。        | [ -z $a ] 返回 false。   |
| -n     | 检测字符串长度是否不为 0，不为 0 返回 true。 | [ -n "$a" ] 返回 true。  |
| $      | 检测字符串是否不为空，不为空返回 true。      | [ $a ] 返回 true。       |

## 文件测试运算符

文件测试运算符用于检测 Unix 文件的各种属性。

属性检测描述如下：

| 操作符  | 说明                                                         | 举例                      |
| :------ | :----------------------------------------------------------- | :------------------------ |
| -b file | 检测文件是否是块设备文件，如果是，则返回 true。              | [ -b $file ] 返回 false。 |
| -c file | 检测文件是否是字符设备文件，如果是，则返回 true。            | [ -c $file ] 返回 false。 |
| -d file | 检测文件是否是目录，如果是，则返回 true。                    | [ -d $file ] 返回 false。 |
| -f file | 检测文件是否是普通文件（既不是目录，也不是设备文件），如果是，则返回 true。 | [ -f $file ] 返回 true。  |
| -r file | 检测文件是否可读，如果是，则返回 true。                      | [ -r $file ] 返回 true。  |
| -w file | 检测文件是否可写，如果是，则返回 true。                      | [ -w $file ] 返回 true。  |
| -x file | 检测文件是否可执行，如果是，则返回 true。                    | [ -x $file ] 返回 true。  |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回 true。     | [ -s $file ] 返回 true。  |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回 true。          | [ -e $file ] 返回 true。  |

# Shell echo命令

可以使用echo实现更复杂的输出格式控制。

### **1.显示普通字符串:**

```sh
echo "It is a test"
# 或者
echo It is a test
```

### **2.显示转义字符**

```sh
echo "\"It is a test\""
# 结果是："It is a test"
```

### **3.显示变量**

read 命令从标准输入中读取一行,并把输入行的每个字段的值指定给 shell 变量

```sh
#!/bin/sh
read name 
echo "$name It is a test"
```

以上代码保存为 test.sh，name 接收标准输入的变量，结果将是:

```sh
[root@www ~]# sh test.sh
OK                     #标准输入
OK It is a test        #输出
```

### 4.显示换行

```sh
echo -e "OK! \n" # -e 开启转义
echo "It is a test"
```

输出结果：

```sh
OK!

It is a test
```

### 5.显示不换行

```sh
#!/bin/sh
echo -e "OK! \c" # -e 开启转义 \c 不换行
echo "It is a test"
# 输出结果：OK! It is a test
```

### 6.显示结果定向至文件

```sh
echo "It is a test" > myfile
```

### 7.原样输出字符串，不进行转义或取变量(用单引号)

```sh
echo '$name\"'
# 输出结果：$name\"
```

### 8.显示命令执行结果

```sh
echo `date`
# 结果将显示当前日期：Thu Jul 24 10:08:46 CST 2014
```

# Shell 条件判断命令

test可以用于`if-else` 语句中测试不同的条件，如果测试条件成立，那么返回状态码0，执行`if-then` 的语句，反之执行`else`语句。

```sh
num1=100
num2=100
if test $[num1] -eq $[num2]
then
    echo '两个数相等！'
else
    echo '两个数不相等！'
fi
```



但bash shell提供了另一种更简单的条件测试方式，使用方括号定义测试条件。但是第一个方括号之后和第二个方括号之前**必须留有空格**，否则就会报错。

test命令和[]测试条件可以判断3类条件：数值比较、字符串比较和文件比较。

## 数值测试

| 比较      | 说明           |
| :-------- | :------------- |
| n1 -eq n2 | 等于则为真     |
| n1 -ne n2 | 不等于则为真   |
| n1 -gt n2 | 大于则为真     |
| n1 -ge n2 | 大于等于则为真 |
| n1 -lt n2 | 小于则为真     |
| n1 -le n2 | 小于等于则为真 |

```sh
if [ 6 -eq 5 ]

if [ $value -gt 5 ]

if [ $value1 -ne $value2 ]
```



## 字符串测试

| 参数        | 说明                      |
| :---------- | :------------------------ |
| str1 = str2 | 等于则为真                |
| str != str2 | 不相等则为真              |
| -z str      | 字符串的长度为零则为真    |
| -n str      | 字符串的长度不为零则为真  |
| str1 < str2 | 小于（需要使用转义字符\） |
| str1 > str2 |                           |

```sh
num1="ru1noob"
num2="runoob"

if [ $num1 = ru1noob ]

if [ $num1 \> $num2 ]
```



## 文件测试

| 参数      | 说明                                 |
| :-------- | :----------------------------------- |
| -e 文件名 | 如果文件存在则为真                   |
| -r 文件名 | 如果文件存在且可读则为真             |
| -w 文件名 | 如果文件存在且可写则为真             |
| -x 文件名 | 如果文件存在且可执行则为真           |
| -s 文件名 | 如果文件存在且至少有一个字符则为真   |
| -d 文件名 | 如果文件存在且为目录则为真           |
| -f 文件名 | 如果文件存在且为普通文件则为真       |
| -c 文件名 | 如果文件存在且为字符型特殊文件则为真 |
| -b 文件名 | 如果文件存在且为块特殊文件则为真     |



```sh
if test -e ./bash
then
    echo '文件已存在!'
else
    echo '文件不存在!'
fi
```

# shell 流程控制

## if-else

```sh
# if 
if condition
then
    command1 
    command2
fi

# 写成一行
if [ $(ps -ef | grep -c "ssh") -gt 1 ]; then echo "true"; fi

# if else
if condition
then
    command1 
else
    command
fi

# if else-if else 
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



if else 的 **[...]** 判断语句中大于使用 **-gt**，小于使用 **-lt**。

```sh
if [ "$a" -gt "$b" ]; then
    ...
fi
```

如果使用 **((...))** 作为判断语句，大于和小于可以直接使用 **>** 和 **<**。

```sh
if (( a > b )); then
    ...
fi
```



## for

```sh
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done

# 写成一行
for var in item1 item2 ... itemN; do command1; command2… done;
```



## while

```sh
while condition
do
    command
done
```



## case ... esac

```sh
case 值 in
模式1)
    command1
    ;;
模式2)
    command1
    ;;
esac
```

```sh
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

### 跳出循环

`break`、`continue`





## shell 函数

```sh
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

| 参数处理 | 说明                                                         |
| :------- | :----------------------------------------------------------- |
| $#       | 传递到脚本或函数的参数个数                                   |
| $*       | 以一个单字符串显示所有向脚本传递的参数                       |
| $$       | 脚本运行的当前进程ID号                                       |
| $!       | 后台运行的最后一个进程的ID号                                 |
| $@       | 与$*相同，但是使用时加引号，并在引号中返回每个参数。         |
| $-       | 显示Shell使用的当前选项，与set命令功能相同。                 |
| $?       | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。 |
| $0       | 当前执行的脚本或者命令名称                                   |
| $1-$9    | 代表参数的位置. 举例 $1 代表第一个参数.                      |

> 注意:
> **$?** 用于检查上一个命令执行是否正确(在Linux中，命令退出状态为0表示该命令正确执行，任何非0值表示命令出错)
> **$$** 变量最常见的用途是用做暂存文件的名字以保证暂存文件不会重复
> $* 和 $@ 结果输出是一样的，但是在使用for循环，在使用 双引号(“”)引用时 **”$\*”** 会输出成一个元素 而 **”$@”** 会按照每个参数是一个元素方式输出





# 重定向

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



如果希望屏蔽 stdout 和 stderr，可以这样写：

```sh
$ command > /dev/null 2>&1
```

> **注意：**0 是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。
>
> 这里的 **2** 和 **>** 之间不可以有空格，**2>** 是一体的时候才表示错误输出。





逐行读取文件

1. 使用for循环读取文件

```shell
for line in `cat file.txt`
do
	echo $line
done

# 使用IFS指定字符串分隔符，默认情况下 IFS 是使用 空格 \t \n 来作为默认的分割符的
IFS_old=$IFS      #将原IFS值保存，以便用完后恢复
IFS=$’\n’         #更改IFS值为$’\n’
for line in `cat file.txt`
do
    echo $line
done
```

> 注意：由于使用for来读入文件里的行时，会自动把空格和换行符作为一样分隔符，如果行里有空格的时候，输出的结果会很乱，所以只适用于行连续不能有空格或者换行符的文件

2. 使用while循环读取文件

   ```shell
   cnt=0
   cat file.txt |while read line
   do
       echo $line
       cnt=$cnt+1   # 管道符中对变量的修改在循环体外无效
   done
   # 或
   while read line
   do
       echo $line
       cnt=$cnt+1   # 可以对变量cnt正常修改
   done < file.txt
   ```

   > 注意:由于使用while来读入文件里的行时，会整行读入，不会关注行的内容(空格..)，所以比for读文件有更好的适用性，推荐使用while循环读取文件



> **注：**该文来自[菜鸟教程](https://www.runoob.com/)