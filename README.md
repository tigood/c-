# C#常用数据结构和算法



## 字符串

### 静态字符串 String

这是最常用的字符串操作类，可以完成大部分的字符串操作

#### 比较字符串

比较字符串是指按照字典排序规则判断两个字符的相对大小。按照字典规则，在一本英文字典中出现在前面的单词小于出现在后面的英文单词。在`String`类中常用的比较字符串的方法有`Compare`、`CompareTo`、`CompareOrdinal`、`Equals`。

##### Compare方法

该方法有一下几种重载的方法：

* **两个字符串参数**

  该形式的Compare方法接受两个参数，分别为两个字符串，会从两个字符串的第一个字符开始比较，如果两个字符串的第一个字符不相同，则根据两个字符在字典中位置的相对前后返回$1$或者$-1$（第一个字符串中的字符在字典中的位置靠前则返回$-1$，否则返回$1$），如果两个字符串的第一个字符相等，则比较两个字符串的下一个字符，依次类推，如果到最后一个字符依然相等的话返回$0$​。**一般在字典中出现在前面的字符被认为小于出现在后面的字符**

  **这种情况下是区分字符的大小写的哦！**

  ```c#
  public static int Compare(string strA, string strB);
  ```

  使用例子：

  ```c#
  string s1 = "Hello", s1 = "World";
  Console.WriteLine(System.String.Compare(s1, s2));  // -1
  Console.WriteLine(System.String.Compare(s1, s1));  // 0
  Console.WriteLine(System.String.Compare(s2, s1));  // 1
  ```

* **两个字符串参数和一个布尔值参数**

  比较方式与上一个形式的重载方式相同，不过多了一个**布尔值参数**，可以通过这个布尔值参数来指定**是否忽略大小写**。

  ```c#
  public static int Compare(string strA, string strB, bool ignoreCase);
  ```

  使用例子：

  ```c#
  // 在这里因为这个大写字母在Unicode编码中比小写字母靠前，前者靠前返回1
  string s1 = "Hello", s2 = "hello";
  Console.WriteLine(System.String.Compare(s1, s2)); // 1   如果不加该参数是区分大小写的 默认为false区分大小写
  Console.WriteLine(System.String.Compare(s1, s2, true)); // 0 加上该参数表示开启不区分大小写，等同于上面
  Console.WriteLine(System.String.Compare(s1, s2, false)); // 1 加上该参数表示默认区分大小写，所以两个字符串被视为相等
  ```

* **重载3**

  比较指定部分长度的两个字符串，并指定比较选项

  ```csharp
  public static int Compare(string strA, int indexA, string strB, int indexB, int length);
  ```

* **重载4**

  ```csharp
  public static int Compare(string strA, int indexA, string strB, int indexB, int length, StringComparison comparisonType);
  ```

* **重载5**

  ```csharp
  public static int Compare(string strA, int indexA, string strB, int indexB, int length, bool ignoreCase);
  ```

##### CompareTo 方法

CompareTo方法将当前字符串与另一个对象做比较，其作用与Compare类似，返回值也相同。



>CompareTo 方法和 Compare 相比区别如下：
>
>* CompareTo 不是静态方法，可以通过一个String对象调用
>* CompareTo 没有重载形式，**只能按照大小写敏感方式比较两个字符串**

CompareTo 方法使用如下：

```csharp
// 定义两个String对象，并对其赋值
System.String strA = "Hello";
System.String strB = "World";
Console.WriteLine(strA.CompareTo(strB));   // -1    
```

##### Equals 方法

Equals 方法用于方便地**判断两个字符串是否相同**，它有一下两种重载方式：

* ```csharp
  public bool Equals(string);
  ```

* ```csharp
  public static bool Equals(string, string);
  ```

显而易见，这两种方式**一种**是静态方法，**另一种**是非静态方法。静态方法接受两个参数，表示两个用来比较的字符串。非静态方法被一个字符串对象调用，接受一个参数，是当前调用方法的字符串对象与接受的字符串参数的比较。

使用方法如下：

```csharp
// Equals
string strA = "Hello", strB = "World";
Console.WriteLine(System.String.Equals(strA, strB));   //false   静态方法
Console.WriteLine(strA.Equals(strB));  // false  非静态方法
```

##### 比较运算符

String 支持通过`==`和`!=`两个比较运算符，分别**用来判定两个字符是否相等和不等**，并**区分大小写**。

实例：

```csharp
string strA = "Hello", strB = "World";
Console.WriteLine(strA == strB);
Console.WriteLine(strA != strB);
```

#### 定位字符和子串

定位子串是指在一个字符串中寻找包含的子串或者某个字符

##### StartsWith / EndsWith 方法

* StartsWith 方法可以判定一个字符串对象是否以另一个字符串开头，如果是就返回True，否则返回False。其定义如下：

  ```csharp
  public bool StartsWith(string value); // 参数value代表待判定的子字符串
  ```

  用法：

  ```csharp
  Console.WriteLine(strA.StartsWith("He"));  // True
  Console.WriteLine(strA.StartsWith("Ha"));  // False
  ```

* EndsWith 方法可以判定一个字符串对象是否以另一个字符串为**结尾**，如果是就返回True，否则返回False。定义如下：

  ```csharp
  public bool EndsWith(string value)  // 参数value代表待判定的子字符串
  ```

  用法：

  ```csharp
  Console.WriteLine(strA.EndsWith("lo"));  // True
  Console.WriteLine(strA.EndsWith("le"));  // False
  ```

##### IndexOf / LastIndexOf 方法

IndexOf 方法用于搜索一个字符串，某个特定的字符或子串第一次出现的位置，该方法**区分大小写**，并从字符串的首字符开始以$0$计数。如果字符串中不包含这个字符或者子串，则返回$-1$。其共有下面6中重载形式。

>定位字符：
>
>```csharp
>int IndexOf(char value);  // value代表特定的字符
>int IndexOf(char value, int startIndex);
>int IndexOf(char value, int startIndex, int count);
>```
>
>定位子串：
>
>```csharp
>int IndexOf(string value);
>int IndexOf(string value, int startIndex);
>int IndexOf(string value, int startIndex, int count);
>```

对于以上重载形式中的参数的解释如下：

* **value**：待定位的字符或者子串。
* **startIndex**：在总串中开始搜索的起始位置。
* **count**：在总串中从起始位置开始搜索的字符数。

使用实例：

```csharp
System.String strA = "Hello";
Console.WriteLine(strA.IndexOf("l"));  // 2
Console.WriteLine(strA.LastIndexOf("l"));  // 3
Console.WriteLine(strA.IndexOf("l", 4));  // -1
Console.WriteLine(strA.IndexOf("l", 0, 2)); // -1
```

与 IndexOf 方法类似，LastIndexOf 方法用于在一个字符串中搜索某个特定字符或者子串最后一次出现的位置，其方法定义和返回值都与 IndexOf 相同，这里不再赘述，参照 IndexOf 方法。

##### IndexOfAny / LastIndexAny 方法

IndexOfAny 方法的功能和 IndexOf 方法类似，区别在于可以在一个字符串中搜索出现在**一个字符数组中的任意字符第一次出现的位置**。同样该方法**严格区分大小写**，并从字符串的首字符以$0$开始计数。如果字符串中不包含这个字符或者子串，则返回$-1$。

IndexOfAny 方法有以下三种重载方式，LastIndexOfAny的重载方法与之类似：

>```csharp
>int IndexOfAny(string char[] anyOf);
>int IndexOfAny(string char[] anyOf, int startIndex);
>int IndexOfAny(string char[] anyOf, int startIndex, int count);
>```

对于以上的重载方法中的参数列表解释如下：

* **anyOf**：待定位的字符数组，方法将返回这个数组中的任意一个字符第一次出现的位置。
* **startIndex**：在总串中开始搜索的起始位置，不设置的话从0开始（首位字符）。
* **count**：在总串的起始位置开始搜索的字符数。

使用用例：

```csharp
string strA = "Hello";
char[] char_1 = {'H', "l", "0"};
Console.WriteLine(strA.IndexOfAny(char_1));  // 上述重载形式1
Console.WriteLine(strA.IndexOfAny(char_1, 2));  // 上述重载形式2
Console.WriteLine(strA.IndexOfAny(char_1, 2, 3));  // 上述重载形式3
```

#### 格式化字符串

##### Format 方法

Format 方法用于创建格式化的字符串以及连接多个字符串对象。Format 方法也有多个重载形式，最常用的如下：

```csharp
// 最常用的重载形式
public static string Format(string format, params object[] args);
```

其中，参数 format 用于指定返回字符串的格式，而 args 为一系列变量的参数。示例如下：

```csharp
// Format
string resultStr = "";
string strA = "Hello", strB = "World";
int a = 1;
resultStr = String.Format("{0}, {1} : {2}", strA, strB, a);
Console.WriteLine(resultStr);  // Hello, World : 2
```

这里有一点需要注意后面这一系列的变量参数并不要求必须是 String 类型。

如果你想输入一定格式的时间，那么通过Format方法就可以快速的实现：

```csharp
string newStr = String.Format("CurrentTime = {0:yyyy-MM-dd}", System.DateTime.Now);
Console.WriteLine(newStr); // 2024-09-19
```

#### 连接字符串

##### Concat 方法

Concat 方法用于连接两个和多个字符串。Concat 方法也有多个重载形式，最常用的为：

```csharp
public static string Concat(params string[] values);
```

>额外加餐：
>
>这里该方法接受的参数为：params string[] values。我们来解释一个为什么是这个。
>
>首先，先了解这段话的意思，有了这个形式，该方法可以接受任意数量的字符串参数。
>
>params 关键字表示该参数是可变数量的，可以传递零个或者多个数组
>
>string[] values 表示参数的类型是一个字符串数组，即传递一个字符串数组作为参数
>
>通过了 params 的修饰，我们可以像该方法传递任意数量的元素，而如果不加这个修饰，我们则需要显示的创建一个数组，再来传递该字符串数组，这样就会显得麻烦了。

使用示例：

```csharp
// Concat
string resultStr = "";
resultStr = String.Concat(strA, " ", strB);
Console.WriteLine(resultStr);  // Hello World
```

##### Join 方法

Join 方法利用一个字符数组和一个分隔符构造新的字符串，常用于把多个字符串连接在一起，并用一个特殊的符号分隔开。

Join 方法的常用形式如下：

```csharp
public static string Join(string separator, string[] values);
```

其中，separator为指定的连接符，values 为字符串列表，将列表中的多个字符串连接到一起。

使用示例

```csharp
string resultStr = "", strA = "Hello", strB = "World";
String[] str_list = {strA, strB};
resultStr = String.Join("++", str_list);
Console.WriteLine(resultStr); // Hello++World
```

##### 连接运算符 + 

String 支持使用 + 运算符，这样可以方便的将多个字符串连接在一起。

```csharp
string resultStr;
resultStr = strA + strB;
Console.WriteLine(resultStr);  // HelloWorld
```

#### 分隔字符串

##### Split 方法

前面可以通过 Join 方法来通过**分隔符**将一个字符串数组中的字符串拼接成一个完整的字符串，当然我们也能通过一个**分隔符**来将一个字符串分隔为一系列小的字符串，Split 有多个重载形式，最常用的形式如下：

```csharp
public string[] Split(params char[] separator);
```

>其中 separator 包含分隔符

```csharp
//Split
newStr = "Hello^^World";
char[] separator = {"^"};
String[] splitStrings = new String[100];
splitStrings = newStr.Split(separator);
int i = 0;
while (i < splitStrings.Length)
{
    Console.WriteLine("item{0} : {1}", i, splitStrings[i]);
    i++;
}
//输出结果如下
//item0 : Hello
//item1 : 
//item2 : World
```

>当然这个例子中只传递了一个字符'^'，你也可以传递更多的字符参数，这样就会根据这些字符来分隔。

#### 插入和填充字符串

##### Insert 方法

该方法用于在一个字符串的指定位置插入另一个字符串，从而构造一个新的字符串。Insert 方法也含有多个重载形式，最常用的如下：

```csharp
public string Insert(int startIndex, string value);
```

>其中，参数 startIndex 用于指定所要插入的位置，从0开始索引；value 指定所要插入的字符串。

示例如下：

```csharp
//Insert
string newStr;
newStr = strA.Insert(strB);
Console.WriteLine(newStr);  // HWorldello
```

#### 删除和剪切字符串

在 String 类中包含着删除和剪切字符串的方法，接下来分别介绍一下。

##### Remove 方法

Remove 方法从一个字符串的指定位置开始，删除指定数量的字符，最常用的形式如下：

```csharp
public string Remove(int startIndex, int count);
public string Remove(int startIndex);
```

>* startIndex 用于指定开始删除的位置，从$0$开始索引
>* count 指定删除的字符数量。

使用实例：

```csharp
string strA = "Hello";
string resultStr = strA.Remove(3, 1);
Console.WriteLine(resultStr);  // Hell
string resultStr1 = strA.Remove(3);
Console.WriteLine(resultStr1); // Hel
```

##### Trim、TrimStart、TrimEnd 方法

若想把一个字符串首尾处的一些特殊字符剪切掉，例如**去掉一个字符串的首位的空格等**，可以使用 String 方法的 Trim() 方法。

其常用的两种形式如下：

```c#
public string Trim();
public string Trim(params char[] trimChars);
```

>其中，参数字符数组 trimChars 包含了要去掉的字符。如果**省略**，则**删除空格符号**。

实例如下：

```csharp
//Trim
string newStr;
char[] trimChars = {'@', '#', '$', ' '};
String strC = "@Hello# $";
newStr = strC.Trim(trimChars);
Console.WriteLine(newStr); // Hello
```

其余两个方法 TrimStart 方法和 TrimEnd 方法分别剪切掉一个字符串首部处和结尾处的字符。

#### 复制字符串

String 类包含了复制字符串 Copy 方法和 CopyTo 方法。可以完成对一个字符串及其一部分的复制操作。

##### Copy 方法

若想将一个字符串复制到另一个字符数组中，可以使用 String 类的静态方法 Copy 实现，其形式如下：

```csharp
public static string Copy(string str);  // 返回的是一个字符串
```

>其中，参数 str 为需要复制的源字符串，方法返回目标字符串。

```csharp
// 将strA字符串复制到 newStr 中
string newStr;
newStr = String.Copy(strA);
Console.WriteLine(newStr);
```

##### CopyTo 方法

CopyTo 方法 可以实现与 Copy 方法相同的功能，但是功能更加丰富，可以复制字符串的一部分到另一个字符数组中。另外，CopyTo 不是静态方法，其形式如下：

```csharp
public void CopyTo(int sourceIndex, char[] destination, int destinationIndex, int count);
```

>* sourceIndex：为需要复制的字符起始位置
>* destination：为目标字符数组
>* destinationIndex：为指定目标字符数组的开始存放位置
>* count：指定要复制的字符个数

```csharp
// 将strA字符串"Hello"中的"ell"复制到newCharArr中，并在newCharArr中从第0个元素开始存放
//CopyTo
char[] newCharArr = new char[100];
string strA = "Hello";
strA.CopyTo(2, newCharArr, 0, 3);
Console.WriteLine(newCharArr);
```

#### 替换字符串

##### Replace 方法

如果想要替换一个字符串中的某些特定字符或者某个子串，可以使用 Replace 方法实现，其形式如下：

```csharp
public string Replace(char oldChar, char newChar);
public string Replace(string oldValue, string newValue);
```

> 其中，oldChar和oldValue为待替换的字符和子串，而newChar和oldValue为替换后的新字符和新子串。

示例如下：

```csharp
// Replace
string strA = "Hello";
string newStr;
newStr = strA.Replace("ll", "r");
Console.WriteLine(newStr);   // hero
```

#### 更改大小写

##### ToUpper  和  ToLower

String 类提供了方便的转换字符串中所有的字符的大小写的方法，ToUpper 方法和 ToLower 方法，这两个方法没有传入参数，使用也十分简单。示例如下：

```csharp
string strA = "Hello";
string newStr;
newStr = strA.ToUpper();
Console.WriteLine(newStr);  // HELLO
newStr = strA.ToLower();
Console.WriteLine(newStr);  // hello
```

#### 提取子串

利用 String 类提取子串方法 Substring 方法 可以从一个字符串中得到一个子串，其使用形式如下：

```csharp
public string Substring(int startIndex);
public string Substring(int startIndex, int length); 
```

> 返回一个从startIndex开始到结束的子字符串，或返回一个从startIndex开始长度为length的子字符串

使用示例如下：

```csharp
string strOriginal = "I loves China!";
string strSub = strOriginal.Substring(2, 12);
Console.WriteLine("strOriginal: {0}", strOriginal);
Console.WriteLine("strSub: {0}", strSub);
string strTemp;
for (int i = 0; i < strOriginal.Length; i += 2)
{
    strTemp = strOriginal.Substring(i, 1);
    Console.Write(strTemp);
}
//输出结果如下：
//strOriginal: I loves China!
//strSub: loves China!
//IlvsCia
```

#### String 小结

上述介绍了 String 类提供的多种方法，比较、定位、格式化、连接、分隔、插入、删除、复制、替换、更改大小写、提取这11种方法。之所以称 String 为静态字符串，是因为一旦定义了一个 String 对象，这个 String 对象就是无法修改的。使用上述的方法操作该String对象的时候，都需要在内存中新生成一个 新的 String 对象来接受这个操作完的String对象。如果我们需要对一个字符串进行频繁的操作，那么它的内存消耗将是巨大的。所以，下面我们引出动态字符串 StringBuilder。

### 动态字符串 StringBuilder

与 String 类相比，使用 System.Text.StringBuilder 类可以实现动态字符串。动态字符串在进行修改等操作时，不会产生新的对象，不会占用新的内存，而是直接在 StringBuilder 的基础上进行修改。

#### 声明 StringBuilder 动态字符串

StringBuilder 类在命名空间 System.Text 中，在使用时可以在文件头中通过 using 语句引入该命名空间：

```csharp
using System.Text;
```

声明一个 StringBuilder 对象，并且对其进行初始化

```csharp
using System.Text;
StringBuilder myStringBuilder = new StringBuilder("Hello");
```

如果没有在开头引入 System.Text 命名空间，也可以这样使用：

```csharp
System.Text.StringBuilder myStringBuilder = new StringBuilder("Hello");
```

#### 设置 StringBuilder 容量

StringBuilder 为动态字符串，可以对其设置号的字符数量进行扩充等操作，不过，我们也可以来为它设置一个最大长度，这个最大长度被称之为这个动态字符串的容量（Capacity）。

为动态数组设置容量的意义在于，如果该动态数组的实际字符长度没有达到动态字符串的容量之前，StringBuilder 不会重新分配空间，当实际字符长度达到了容量时，就会为该 StringBuilder 动态数组会在原动态字符串的基础之上自动分配一个新的空间，容量翻倍。如果不进行设置容量，那么 StringBuilder **默认初始化分配16个字符长度**。

有两种方法可以为 StringBuilder 设置容量：

1. 使用构造函数

   StringBuilder 构造函数可以接受容量参数，实例如下：

   ```csharp
   StringBuilder myStringBuilder = new StringBuilder("Hello", 10);
   // 声明并且初始化了一个动态字符串，并且通过构造函数传入Capacity参数将动态字符串的容量设置为了10
   ```

2. 使用 Capacity 属性

   Capacity 属性指定 StringBuilder 对象的容量，示例如下：

   ```csharp
   StringBuilder myStringBuilder = new StringBuilder("Hello");
   myStringBuilder.Capacity = 100;
   // 上述声明并初始化了一个动态字符串，并且通过修改Capacity属性来设置的该动态字符串的容量大小。
   ```


#### 追加操作

追加一个 StringBuilder 是指将新的字符串添加到当前 StringBuilder 字符串的结尾处，可以使用 Append 和 AppendFormat 方法实现这个功能

##### Append 方法

可以使用它实现简单的追加功能，常用形式如下：

```csharp
public StringBuilder Append(object value);   // 其中这个value可以是字符串，也可以是其他的数据类型。
```

使用实例如下：

```csharp
//Append
StringBuilder myStringBuilder = new StringBuilder("Hello");
myStringBuilder.Append(" World");
Console.WriteLine(myStringBuilder);   // Hello World
```

##### AppendFormat 方法

通过这个方法可以在当前动态字符串后面追加一个格式化后的字符串，其常用的形式如下：

```csharp
public StringBuilder AppendFormat(string format, params object[] args);
```

>其中，args 数组指定所要追加的多个变量。format 参数包含格式规范的字符串，其中包含一系列用大括号括起来的格式字符，例如{0:u}。这里，0代表着对应参数数组 args 中的第 0 个变量，而用”“u”定义其格式。

示例如下：

```csharp
//AppendFormat
StringBuilder sb1 = new StringBuilder("Today is ");
sb1.AppendFormat("{0:yyyy-MM-dd}", System.DateTime.Now);
Console.WriteLine(sb1);  // Today is 2024-4-17
```

#### 插入操作

##### Insert 方法

常用形式如下：

```csharp
public StringBuilder Insert(int index, object value);
```

>其中，index 指定所要插入的位置，从 0 开始索引，例如index= 1，此时会在原字符串中的第2个字符**之前**进行插入操作。value 指的是要插入进字符串的数据内容，该 value 并不是只能取为字符串类型，也可以使用其他的数据类型。

实例如下：

```csharp
//Insert
StringBuilder sb2 = new StringBuilder("Hello");
sb2.Insert(2, "eee");
Console.WriteLine(sb2);  // Heeeello
```

#### 删除操作

##### Remove 方法

该删除方法可以从当前的 StringBuilder 字符串的指定位置删除一定数量的字符。常用的形式如下：

```csharp
public StringBuilder Remove(int startIndex, int Length);
```

> * startIndex 指的是所要删除的起始位置
> * length 指定所要删除的字符的数量



示例如下：

```csharp
//Remove
StringBuilder sb3 = new StringBuilder("Heeello");
sb3.Remove(2, 2);
Console.WriteLine();  // Hello
```

#### 替换操作

##### Replace 方法

与 String 类的 Replace 方法非常类似，其常用的形式如下：

```csharp
public StringBuilder Replace(char oldChar, char newChar);
public StringBuilder Replace(string oldValue, string newValue);
```

示例如下：

```csharp
//Replace
StringBuilder sb5 = new StringBuilder("Hello");
sb5.Replace("ll", "r");
Console.WriteLine(sb5);   // Hero
```



## 数组

### System.Array 类

该类是 C# 中各种数组的基类， 其中常用的属性和方法的简单说明如下：

**属性**：

* IsFixedSize  指示 Array 是否具有固定的大小。
* Length  获得一个32位的整数，表示 Array 的所有维数中元素的总数。
* Rank  获得 Array 的秩（维数）

**方法**：

* BinarySearch  使用二分搜索算法在一维的有序数组中搜索值。
* Clone  创建 Array 的浅表副本
* Copy / CopyTo  将一个 Array 的一部分复制到另一个 Array 中。
* GetLength  获取一个 32 位的整数，表示 Array 的指定维中的元素。
* GetLowerBound / GetUpperBound  获取 Array 的指定维度的上 / 下限。
* GetValue / SetValue  获取/设置 Array 中的指定元素值
* IndexOf / LastIndexOf  返回一维 Array 或部分 Array 中某个值的第一个 / 最后一个匹配项的索引。
* Sort  对一维 Array 对象的元素进行排序。

### 一维数组

#### 一维数组的定义

数组在使用之前应该先进行定义。定义一维数组的格式如下：

```csharp
数据类型[] 数组名;
```

如下：

```csharp
char[] charArr;   // 定义了一个字符型的一维数组
int[] intArr;   // 定义了一个整形的一维数组
string[] strArr;   // 定义了一个字符串类型的一位数组
```

定义之后，要对数组进行初始化才能使用，数组的初始化方法有两种，分别为**动态初始化**和**静态初始化**。

#### 动态初始化

动态初始化需要借助 new 运算符为数组元素分配内存空间，并为数据元素赋初值。动态初始化数组的格式如下：

```csharp
数组名 = new 数据类型[数组长度];
```

当然，我们也可以同时进行数组的定义和数组的动态初始化，格式如下:

```csharp
数据类型[] 数组名 = new 数据类型[数组长度];
```

例如：

```csharp
int[] intArr = new int[5];
```

通过如上的定义和初始化，该数组中的所有元素的值都被默认赋值为了 0 ，我们也可以自己为该数组赋初值：

```csharp
int[] intArr = new int[5]{2, 3, 5, 3, 1};
```

#### 静态初始化

如果数组中包含的元素不多，而且初始的元素可以穷举，可以采用静态初始化的方法。**在静态初始化数组时必须与数组定义结合在一起**，否则程序会报错。格式如下：

```csharp
数据类型[] 数组名字 = {元素1 [, 元素2...]};
```

在用这种方法对数组进行初始化时，无须说明数组元素的个数，只需按照顺序列出数组中的全部元素即可，系统会自动计算并分配数组所需要的内存空间。

示例如下：

```csharp
int[] intArr = {3, 6, 1, 1, 2};
string[] strArr = {"English", "Math", "Chinese", "Computer"};
```

#### 关于一维数组初始化的几点说明

1. 在动态初始化数组时可以把定义与初始化分开在不同的语句中：

   示例如下：

   ```csharp
   int[] intArr;  // 定义数组
   intArr = new int[5];  // 动态初始化，初始化元素的值均为 0 
   ```

   或者

   ```csharp
   int[] intArr;  // 定义数组
   intArr = new int[5]{2, 3, 5, 1, 2};  // 自定义了初始赋值
   ```

   > 因为这里的大括号中已经列出了数组中的全部元素，所以这里[]里面的5可以省略掉，像这样：
   >
   > ```csharp
   > intArr = new int[]{2, 3, 5, 1, 2};
   > ```

2. **静态初始化数组必须与数组结合在一条语句中**，否则程序会出错。

   ```csharp
   int[] intArr;   // 定义数组
   intArr = {3, 4, 5, 1, 2};   // 错误，定义与静态初始化在两条语句中
   ```

3. 在数组的初始化语句中，如果大括号中已经明确的列出了数组中的元素，即确定了元素个数，则表示数组元素个数的数值（即方括号中的数值）必须是常量，并且该数值必须与数组元素的个数一致。

   ```csharp
   int j = 3;  // 定义一个整型变量j，并为j赋初值3
   int[] intArrayX = new int[3]{2, 4, 7};  // 正确
   int[] intArrayY = new int[j]{1, 3, 1};  // 错误，这里的j不是一个常量
   int[] intArrayZ = new int[3]{1, 2, 1, 3, 5};  // 错误，真实元素数量与方括号中的数值不一致
   ```



#### 访问一维数组的元素

```csharp
intArr[0]、strArr[3]、intArr[j]、strArr[2 * j - 1]
```

如果出现下标越界，则会抛出一个 System.IndexOutOfRangeException 异常



#### 查找元素

这个查找元素有两种解释，第一种就是给定一个元素的值value，找出该元素在数组中的下标。第二种就是判断该元素是否在该数组中。

##### BinarySearch 方法

BinarySearch 方法又称为二分查找搜索算法（虽然书上是二进制搜索算法，不过我不喜欢那样的名字），该方法只有在有序数组中才会发挥出它真实的威力

```csharp
public static int BinarySearch(Array array, object value);
```

> 该方法是一个静态的类方法，所以通过Array类来调用，传递两个参数，第一个为要搜索的数组，第二个是要搜索的元素。

示例如下：

```csharp
int[] intArr = {1, 2, 3, 1, 5, 12};
//对数组进行排序
Array.Sort(intArr);
//搜索
int target = 5;  // 目标元素的值
int result = Array.BinarySearch(myArr, target);   // 4
Console.WriteLine("{0}的下标是{1}", target, result);
```

##### Contains 方法

该方法可以确定某个特定的值是否包含在数组中，返回一个bool值。Array类的这个方法实际上是对 IList 接口中方法的实现，其常用的形式如下：

```csharp
bool IList.Contains(object value);
```

> value 代表所要验证的元素值

实例如下：

```csharp
string[] strArr = {"大宝", "张三", "李四", "赵六"};
//判断是否存在赵六
string target = "赵六";
bool result = ((System.Collections.IList)strArr).Contains("赵六");
Console.WriteLine(result);  // true
```



#### 把数组作为参数传递给方法

可以将数组作为一个参数传递给方法。该方法就可以来使用这个数组参数

```csharp
int[] intArr = {1, 2, 4, 3, 2};
int sum = 0;
public void PrintArr(int[] _intArr)
{
    foreach(int num in _intArr)
    {
        sum += num;
    }
    Console.WriteLine("该整数数组中元素之和为：{0}", sum);
}
```



### 二维数组

多维数数组可以被看做数组的数组，就是在多维数组中，数组元素是一个数组，其中多维数组中最常用的就是二维数组。

#### 二维数组的定义

二维数组的定义与一维数组的相似，一般语法格式如下：

```c#
数据类型[,]数组名;
```

```csharp
int[,] intArr;
char[,] charArr;
```

与一维数组不同的地方就是在数据类型后面跟着的小括号中加个一个" **,**" 其他高维的数组的定义也是类似，二维添加了一个逗号，三维数组的定义需要添加两个逗号，以此类推，即 N 维的数组需要在定义时的数组方括号中添加 N - 1 个逗号。

与一维数组一样，二维数组定义完成后也不能直接使用，需要对其进行初始化，分配好内存之后才能使用。

#### 二维数组的初始化

同样是有两种初始化方法：**动态初始化**和**静态初始化**

动态初始化二维数组的格式如下：

```csharp
// 假设该数组已经定义好了
数组名 = new 数据类型[数组长度1, 数组长度2];
```

> 其中，数组长度1 和 数组长度2 可以是整型的常量或者是变量，它们分别表示数组的第一维的长度和第二维的长度。

当然也可以将定义与动态初始化合并在一条语句中：

```csharp
int[3, 2] intArr = new int[3, 2];
```

上面这行代码对数组进行了初始化，其中因为没有自行赋值，所有元素默认被赋值为了 0 .

```csharp
int[3, 2] intArr = new int[3, 2]{{1, 2}, {3, 4}, {3, 2}};
```

上面进行了定义和初始化，并且为该数组赋予了初值

二维数组的静态初始化格式如下所示：

```csharp
int[,] intArr = {{1, 2}, {3, 4}, {3, 1}};
```

当然，注意**静态初始化数组时必须与数组定义结合在一条语句中**，否则程序会报错。

#### 访问二维数组的元素

访问二维数组的元素也是十分的简单，数组的索引是从0开始的

```csharp
//定义一个二维数组，并对它进行静态初始化
int[,] intArr = {{2, 3, 4}, {2, 1, 5}, {3, 5, 3}};
Console.WriteLine(intArr[0, 2]);  // 4   这样访问到的是数组中第一行第三列的元素
Console.WriteLine(intArr[2, 1]);  // 5
```



## 枚举

枚举类型是用户自定义的数据类型，是一种符号代表值的类型。就是让程序中的某些变量具有了一组确定的值，通过“枚举”可以一一列出来。有助于用户更好的阅读和理解

### 枚举类型的定义

枚举类是由用户自己定义的由一组常量集合组成的特殊的数据类型。

常用的定义格式如下：

```csharp
enum 枚举名
{枚举成员表}[;]
```

>说明：
>
>1. 在说明枚举类型时必须带上 enum 关键字
>2. 枚举类型的名字必须是 C# 中的合法名字
>3. 枚举类型中定义的所有枚举值都默认为整形
>4. 花括号中间的部分是枚举成员表，枚举成员通常使用用户易于理解的标识符字符串表示，它们之间使用逗号分隔开。在花括号之后可以选择带或者不带分号

定义的实例如下：

```csharp
enum WeekEnd
{
    Sun, Mon, Tue, Wed, Thu, Fri, Sat
};
```

> 上面定义了一个名为 WeekEnd 的枚举类型，其中包含着周一到周日这七个枚举成员。定义完成之后，用户就可以在程序中像使用常量一样来使用这些符号。**注意**枚举成员不能重复。

### 枚举成员的赋值

在定义枚举类型的时候，它可以包含0个或者多个的枚举成员，不过任意两个枚举成员不能重名。在上面例子中定义的枚举类型，它包含七个枚举成员，它们的名字各不相同，虽然没有为它们赋值，不过它们已经被系统默认的赋予值了，Sun, Mon, Tue, Wed, Thu, Fri, Sat 被默认的分别赋值为了  0, 1, 2, 3, 4, 5, 6 。**对于每个枚举成员对应的常量值，系统默认的将第一个枚举类型赋值为 0 ， 它后面的每一个枚举成员的值自动加 1 递增**

在真实的场景中，我们可能对不同位置的枚举成员赋值，下面逐一来陈列

#### 为第 1 个枚举成员赋值

在定义的时候，为第一个枚举成员赋值：

```csharp
enum color
{
    yellow = -1,
    brown, 
    blue,
    red, 
    black
}
Console.WriteLine((int)color.yellow);   // -1
Console.WriteLine((int)color.brown);   // 0
Console.WriteLine((int)color.blue);   // 1
Console.WriteLine((int)color.red);   // 2
Console.WriteLine((int)color.black);   //3 
```

通过这个输出结果可以发现，在我们对第一个枚举成员进行赋值之后，后面的枚举成员则会根据我们为第一个枚举成员赋予的值，自动加1递增。

#### 为其中间的某一个枚举成员赋值

```csharp
enum color
{
    yellow,
    brown,
    blue = 2,
    red,
    black
}
Console.WriteLine((int)color.yellow);   // 0
Console.WriteLine((int)color.brown);   // 1
Console.WriteLine((int)color.blue);   // 2
Console.WriteLine((int)color.red);   // 3
Console.WriteLine((int)color.black);   //4 
```

通过上述的输出可以发现，由于第一个枚举成员我们是没有赋予它初始值的，所以系统依旧默认的将它的值赋值为了 0 ，它后面的值也随着它依次的递增 1 ，不过在当他执行到了我们自行赋值的哪一个枚举成员的时候发生了变化，该枚举成员的值变为了我们赋给它的值，它后面的成员的值也随着它的值而发生了改变，它后面的值会根据它的值来递增加 1 .

#### 为多个枚举成员赋值

当然，我们也可以为多个枚举成员赋值，根据前面的两个例子，这种情况的结果我们也能轻易的推测出来。

```csharp
enum color
{
    yellow = 1,
    brown,
    blue = 4,
    red,
    black
}
Console.WriteLine((int)color.yellow);   // 1
Console.WriteLine((int)color.brown);   // 2
Console.WriteLine((int)color.blue);   // 4
Console.WriteLine((int)color.red);   // 5
Console.WriteLine((int)color.black);   //6 
```

系统会从上往下的为每一个枚举成员赋值，如果在该枚举成员之前没有过赋值操作，则系统对它进行默认的赋值，如果该枚举成员被赋值了，那么该成员的值成为被赋予值的同时，它后面的元素的递增初始值也会发生改变，它会将该成员的值作为初始值来递增.

#### 为多个枚举成员赋予相同的值

```csharp
enum color
{
    yellow = 1,
    brown,
    blue = 4,
    red = blue,
    black
}
static void Main()
{
    Console.WriteLine((int)color.yellow);   // 1
    Console.WriteLine((int)color.brown);   // 2
    Console.WriteLine((int)color.blue);   // 4
    Console.WriteLine((int)color.red);   // 4
    Console.WriteLine((int)color.black);   //6 
}
```

C# 允许不同的枚举成员被赋予相同的值，这样调用这两个不同的枚举成员时，它们代表着相同的常量。

### 枚举成员的访问

在 C# 中可以通过枚举类型变量和枚举名两种方式访问枚举成员

#### 通过枚举类型变量访问枚举成员

在通过枚举类型变量访问枚举成员前要先声明一个枚举类型的变量，声明枚举类型的变量的格式一般如下：

```csharp
枚举类型名 变量名;
```

示例如下：

```csharp
enum WeekDay
{
    Sun, Mon, Wed, Thu, Fri, Sat
};
WeekDay Myweekday;    // 声明一个枚举类型变量 Myweekday
```

在声明枚举类型变量之后就可以用该变量来访问定义的枚举成员了。

语句如下：

```csharp
Myweekday = WeekDay.Sun;
```

完整示例：

```csharp
namespace EnumDemo
{
    class EnumDemo
    {
        enum color
        {
            yellow = -1, 
            brown,
            blue, 
            red = black,
            black = 4
        }
        // 声明枚举类型变量
        static color color1, color2, color3, color4, color5;
        static void Main()
        {
            color1 = color.yellow;
            color2 = color.brown;
            color3 = color.blue;
            color4 = color.red;
            color5 = color.black;
            Console.WriteLine("yellow = {0}", (int)color1);   // 只有进行了数组转换，才会输出常量值
            Console.WriteLine("brown = {1}", (int)color2);
            Console.WriteLine("blue = {2}", (int)color3);
            Console.WriteLine("red = {3}", (int)color4);
            Console.WriteLine("black = {4}", (int)color5);
        }
    }
}
```

#### 通过枚举名访问枚举成员

通过枚举名访问枚举成员的方法比较简单，一般形式如下：

```cs
枚举名.枚举成员
```

示例如下：

```csharp
namespace EnumDemo
{
    class EnumDemo
    {
        enum color
        {
            yellow = -1, 
            brown,
            blue, 
            red = black,
            black = 4
        }
        // 声明枚举类型变量
        static void Main()
        {
            color1 = color.yellow;
            color2 = color.brown;
            color3 = color.blue;
            color4 = color.red;
            color5 = color.black;
            Console.WriteLine("yellow = {0}", (int)color.yellow);   // 只有进行了数组转换，才会输出常量值
            Console.WriteLine("brown = {1}", (int)color.brown);
            Console.WriteLine("blue = {2}", (int)color.blue);
            Console.WriteLine("red = {3}", (int)color.red);
            Console.WriteLine("black = {4}", (int)color.black);
        }
    }
}
```

























