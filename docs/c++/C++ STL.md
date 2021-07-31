

#### 第一章 string类

##### 1.1 string成员函数

 下表列出了 string 类的所有成员函数及它们的功能。 

![image-20200110203428378](C:\Users\asus\AppData\Roaming\Typora\typora-user-images\image-20200110203428378.png)

#####   string构造函数和析构函数

构造函数有四个参数，其中三个具有默认值。要初始化一个 string 类，可以使用 C 风格字符串或 string 类型对象，也可以使用 C 风格字符串的部分或 string 类型对象的部分或序列。

常见的 string 类构造函数有以下几种形式：

```c++
string strs //生成空字符串
string s(str) //生成字符串str的复制品
string s(str, stridx) //将字符串str中始于stridx的部分作为构造函数的初值
string s(str, strbegin, strlen) //将字符串str中始于strbegin、长度为strlen的部分作为字符串初值
string s(cstr) //以C_string类型cstr作为字符串s的初值
string s(cstr,char_len)  //以C_string类型cstr的前char_len个字符串作为字符串s的初值
strings(num, c) //生成一个字符串，包含num个c字符
strings(strs, beg, end)  //以区间[beg, end]内的字符作为字符串s的初值
```

析构函数的形式如下：
如果字符串只包含一个字符，使用构造函数对其初始化时，使用以下两种形式比较合理：

```c++
std::string s('x');  //错误
std::string s(1, 'x');  //正确
```

或

```c++
std::string s("x");  //正确
```


通常，程序员在整个程序中应坚持使用 string 类对象，直到必须将内容转化为 char* 时才将其转换为 C_string。

```c++
#include <iostream>
#include <string>
using namespace std;

int main ()
{
    string str ("12345678");
    char ch[ ] = "abcdefgh";
    string a; //定义一个空字符串
    string str_1 (str); //构造函数，全部复制
    string str_2 (str, 2, 5); //构造函数，从字符串str的第2个元素开始，复制5个元素，赋值给str_2
    string str_3 (ch, 5); //将字符串ch的前5个元素赋值给str_3
    string str_4 (5,'X'); //将 5 个 'X' 组成的字符串 "XXXXX" 赋值给 str_4
    string str_5 (str.begin(), str.end()); //复制字符串 str 的所有元素，并赋值给 str_5
    cout << str << endl;
    cout << a << endl ;
    cout << str_1 << endl;
    cout << str_2 << endl;
    cout << str_3 << endl;
    cout << str_4 << endl;
    cout << str_5 << endl;
    return 0;
}
```

##### C++获取字符串长度

String 类型对象包括三种求解字符串长度的函数：size()和length()、maxsize()和 capacity()

- size() 和 length()：这两个函数会返回 string 类型对象中的字符个数，且它们的执行效果相同。
- max_size()：max_size() 函数返回 string 类型对象最多包含的字符数。一旦程序使用长度超过 max_size() 的 string 操作，编译器会拋出 length_error 异常。
- capacity()：该函数返回在重新分配内存之前，string 类型对象所能包含的最大字符数。


string 类型对象还包括一个 reserve() 函数。调用该函数可以为 string 类型对象重新分配内存。重新分配的大小由其参数决定。reserve() 的默认参数为 0。



##### C++ string获取字符串元素：[]和at()

字符串中元素的访问是允许的，一般可使用两种方法访问字符串中的单一字符：下标操作符[] 成员函数at()两者均返回指定的下标位置的字符。第 1 个字符索引（下标）为 0，最后的字符索引为 length()-1。

需要注意的是，这两种访问方法是有区别的：

- 下标操作符 [] 在使用时不检查索引的有效性，如果下标超出字符的长度范围，会示导致未定义行为。对于常量字符串，使用下标操作符时，字符串的最后字符（即 '\0'）是有效的。对应 string 类型对象（常量型）最后一个字符的下标是有效的，调用返回字符 '\0'。
- 函数 at() 在使用时会检查下标是否有效。如果给定的下标超出字符的长度范围，系统会抛出 out_of_range 异常。



##### C++ string字符串比较方法详解

 Basic_string 类模板既提供了 >、<、==、>=、<=、!= 等比较运算符，还提供了 compare() 函数，其中 compare() 函数支持多参数处理，支持用索引值和长度定位子串进行比较。该函数返回一个整数来表示比较结果。如果相比较的两个子串相同，compare() 函数返回 0，否则返回非零值。 

###### compare()函数

类 basic_string 的成员函数 compare() 的原型如下：

```c++
 int compare (const basic_string& s) const;
int compare (const Ch* p) const;
int compare (size_type pos, size_type n, const basic_string& s) const;
int compare (size_type pos, size_type n, const basic_string& s,size_type pos2, size_type n2) const;
int compare (size_type pos, size_type n, const Ch* p, size_type = npos) const; 
```

如果在使用 compare() 函数时，参数中出现了位置和大小，比较时只能用指定的子串。例如：

```c++
s.compare {pos,n, s2);
```

若参与比较的两个串值相同，则函数返回 0；若字符串 S 按字典顺序要先于 S2，则返回负值；反之，则返回正值。下面举例说明如何使用 string 类的 compare() 函数。

##### string字符串修改和替换方法

 可以通过使用多个函数修改字符串的值。例如 assign()，operator=，erase()，交换（swap），插入（insert）等。另外，还可通过 append() 函数添加字符。 

1. ```c++
   string str1 ("123456");
   string str;
   str.assign (str1); //直接赋值
   str.assign (str1, 3, 3); //赋值给子串
   str.assign (str1,2,str1.npos);//赋值给从位置 2 至末尾的子串
   str.assign (5,'X'); //重复 5 个'X'字符
   ```

erase() 函数的原型为：
erase() 函数的使用方法为：

 iterator erase (iterator first, iterator last);
iterator erase (iterator it);
basic_string& erase (size_type p0 = 0, size_type n = npos); 

str.erase (str* begin(), str.end());
或 str.erase (3);