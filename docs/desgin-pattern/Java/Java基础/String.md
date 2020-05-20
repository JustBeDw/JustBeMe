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
*Unless otherwise noted, passing a null argument to a constructor or method in this class will cause a NullPointerException to be thrown.*若不初始化，则会报空指针异常。

#### String是不可改变的

#### “+” 与 StringBuilder

#### 常见的String方法

###### charAt

`public char charAt(int index)`
Returns the char value at the specified index. An index ranges from **0 to length() - 1**. 

**Specified by**:
charAt in interface CharSequence *//来自CharSequence接口*
**Returns**:
the char value at the specified index of this string. The first char value is at index 0. *//返回一个char值，第一个索引值在0处*
**Throws**:
*IndexOutOfBoundsException* - if the index argument is negative or not less than the length of this string. *//如果索引为负数或者超过String长度，越界异常*



###### compareTo

`public int compareTo(String anotherString)`
Compares two strings lexicographically. The comparison is based on the Unicode value of each character in the strings. The character sequence represented by this String object is compared lexicographically to the character sequence represented by the argument string. The result is a negative integer if this String object lexicographically precedes the argument string. The result is a positive integer if this String object lexicographically follows the argument string. The result is zero if the strings are equal; compareTo returns 0 exactly when the equals(Object) method would return true.
This is the definition of lexicographic ordering. If two strings are different, then either they have different characters at some index that is a valid index for both strings, or their lengths are different, or both. If they have different characters at one or more index positions, let k be the smallest such index; then the string whose character at position k has the smaller value, as determined by using the < operator, lexicographically precedes the other string. In this case, compareTo returns the difference of the two character values at position k in the two string -- that is, the value:

 this.charAt(k)-anotherString.charAt(k)

If there is no index position at which they differ, then the shorter string lexicographically precedes the longer string. In this case, compareTo returns the difference of the lengths of the strings -- that is, the value:
 this.length()-anotherString.length()

**Specified by**:
compareTo in interface Comparable<String>
**Parameters**:
anotherString - the String to be compared.
**Returns**:
the value 0 if the argument string is equal to this string; a value less than 0 if this string is lexicographically less than the string argument; and a value greater than 0 if this string is lexicographically greater than the string argument.