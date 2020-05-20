## String

#### String是什么

**Class String**  
[java.lang.Object](file:///D:/office/zeal-portable-0.6.1-windows-x64/docsets/Java_SE8.docset/Contents/Resources/Documents/java/lang/Object.html)  
		java.lang.String
All Implemented Interfaces:  
Serializable, CharSequence, Comparable<String>

public `final` class **String**  
extends Object  
implements Serializable, Comparable<String>, CharSequence  
The String class represents character strings. All string literals in Java programs, such as "abc", are implemented as instances of this class.  
Strings are constant; their values cannot be changed after they are created. String buffers support mutable strings. Because String objects are immutable they can be shared. For example:

```java
 String str = "abc";
```

is equivalent to:

```java
 char data[] = {'a', 'b', 'c'};
 String str = new String(data);
```
*以上，来自 Java官方文档*  
可以看出，**String** 来自 **java.lang** 包下，实现了Serializable, CharSequence, Comparable三个接口。  
*Unless otherwise noted, passing a null argument to a constructor or method in this class will cause a NullPointerException to be thrown.*  若不初始化，则会报空指针异常。

#### String是不可改变的

由上述可知，String 底层代码是被**final**修饰的，所以只有只读权限。那当我们修改String引用的时候，实际上是"new"了一个新的String对象，引用指向这个新的对象。

#### “+” 与 StringBuilder

String s = "a" + "b"，`"a"`和`"b"`两个都是编译期常量，编译期可知，即变成 String s = "ab";  
实际就是自动装箱操作，拼接到了一起。对于能够进行优化的(String s = "a" + anotherParam 等)用 StringBuilder 的 append() 方法替代，最后调用 toString() 方法 ，底层就是一个 new String()。

## String、StringBuilder、StringBuffer

#### 三者区别

首先，三者均为对象。但String为不可变的，其他两个均需要`new`关键字创建对象且可变，例如调用`append()`方法，所以，在不创建新对象的情况下，使用StringBuilder和StringBuffer。

#### 线程安全问题

StringBuffer——线程安全，效率低。

StringBuilder——线程不安全，效率高。

#### 陷阱

```java
append(a+":"+b)
```

实际是:

```java
append(a+new String(":")+b)
```

这样又创建了一个新对象进行处理。

## 常见的String方法

###### charAt  

`public char charAt(int index)`  
Returns the char value at the specified index. An index ranges from **0 to length() - 1**. 

**Specified by**:   
charAt in interface CharSequence *//来自CharSequence接口*  

**Returns**:  
the char value at the specified index of this string. The first char value is at index 0. *//返回一个char值，第一个索引值在0处*。  
**Throws**:  
*IndexOutOfBoundsException* - if the index argument is negative or not less than the length of this string. *//如果索引为负数或者超过String长度，越界异常*

###### compareTo    

`public int compareTo(String anotherString)`  
Compares two strings lexicographically. The comparison is based on the Unicode value of each character in the strings. The character sequence represented by this String object is compared lexicographically to the character sequence represented by the argument string. **The result is a negative integer** if this String object lexicographically precedes the argument string. **The result is a positive integer** if this String object lexicographically follows the argument string. The result is zero **if the strings are equal**; compareTo **returns 0** exactly when the equals(Object) method would return true.*//这段主要意思就是，比较两个字符串，如果前字符串大于括号内的字符串，则返回 正整数，如果小于则返回 负整数，相等返回 0*  
This is the definition of lexicographic ordering. If two strings are different, then either they have different characters at some index that is a valid index for both strings, or their lengths are different, or both. If they have different characters at one or more index positions, let k be the smallest such index; then the string whose character at position k has the smaller value, as determined by using the < operator, lexicographically precedes the other string. In this case, compareTo returns the difference of the two character values at position k in the two string -- that is, the value:

 `this.charAt(k)-anotherString.charAt(k)` *//以上说明，若不同字符串，比较是一个字符一个字符比较，若不同，他们索引位置为 k，然后计算 k 位置上他们的字符差值*

If there is no index position at which they differ, then the shorter string lexicographically precedes the longer string. In this case, compareTo returns the difference of the lengths of the strings -- that is, the value:
 `this.length()-anotherString.length()` *//长度差值*

**Specified by**:
compareTo in interface Comparable<String> *//实现了Comparable接口*
**Parameters**:
anotherString - the String to be compared.
**Returns**:
the value 0 if the argument string is equal to this string; a value less than 0 if this string is lexicographically less than the string argument; and a value greater than 0 if this string is lexicographically greater than the string argument. *//前字符串大于括号内的字符串，则返回 正整数，如果小于则返回 负整数，相等返回 0*

###### concat

`public String concat(String str)`  
Concatenates the specified string to the end of this string.*//将指定的字符串连接到此字符串的末尾*

*Examples:*

 		*"cares".concat("s")* returns **"caress"**  
 		*"to".concat("get").concat("her")* returns **"together"**

**Parameters:**  
str - the String that is concatenated to the end of this String.  
**Returns:**  
a string that represents the concatenation of this object's characters followed by the string argument's characters.*//返回一个新字符串，是该字符串对象与字符串参数的拼接*

###### substring

`public String substring(int beginIndex)`  
`public String substring(int beginIndex,int endIndex)`

Returns a string that is a substring of this string. The substring begins at the specified beginIndex and extends to the character at index endIndex - 1. Thus the length of the substring is endIndex-beginIndex.*//返回一个子字符串，子字符串从指定的beginIndex开始，并扩展到索引 endIndex - 1处的字符。因此子字符串的长度是endIndex-beginIndex*

*Examples:*

​		 *"hamburger".substring(4, 8)* returns **"urge"**  
​		 *"smiles".substring(1, 5)* returns **"mile"**  
*//注意，substring的索引是从`1`开始，但**不包括**`1`。*

**Parameters:**  
beginIndex - the beginning index, inclusive.  
endIndex - the ending index, exclusive.  
**Returns:**  
the specified substring.*//返回子字符串*  
**Throws:**  
IndexOutOfBoundsException - if the beginIndex is negative, or endIndex is larger than the length of this String object, or beginIndex is larger than endIndex.*//如果索引为负数或者超过String长度，越界异常*

###### valueOf

`public static String valueOf(int i)`  
Returns the string representation of the int argument.*//返回int参数的字符串表示形式*  

**Parameters:**  
i - an int.   
**Returns:**  
a string representation of the int argument.*//字符串形式*  
**See Also:**  
Integer.toString(int, int) *//用法*

例如，还有`length()`,`toString()`,`replace()`等，详情可看 JavaSE 8 官网文档。