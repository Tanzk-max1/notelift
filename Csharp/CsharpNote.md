类似与random  这个就是对象传递，然后new一个对象，这个对象建立在栈上的
# 10月
## 10-30
```C#
// See https://aka.ms/new-console-template for more information

namespace HelloWorldApp
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello, World!");
            Console.WriteLine("two peresong integers are {0} and {1}", 3, 6);
            Console.WriteLine("shuchu {0} and {1}", 4564564, 46546);
            Random random = new Random();//这种写法就是经典的new对象
            int targetNumber = random.Next(1,51);
            int attempts = 0;
            int guess  = 0;
            Console.WriteLine("猜猜：");
            Console.WriteLine("hihihih:");
            while (guess != targetNumber) 
            {
                Console.WriteLine("请输入你的猜测：");
                while(!int.TryParse(Console.ReadLine(),out guess)|| guess < 1 || guess > 100 )//用于验证用户输入是否为有效的整数，并在1到100的范围内
                {
                    Console.WriteLine("无效输入！请输入一个1到100之间的数字。");
                    Console.WriteLine("请重新输入你的猜测： ");
                }
                attempts++;
                if (guess < targetNumber)
                {
                    Console.WriteLine("太小了，再试一次！");
                }
                else if (guess > targetNumber)
                {
                    Console.WriteLine("太大了，再试一次！");
                }
                else
                {
                    Console.WriteLine($"恭喜你！你猜对了，数字就是 {targetNumber}！");
                    Console.WriteLine($"你总共猜了 {attempts} 次。");
                }
            }
            //game over
            Console.WriteLine("感谢你的参与！按任意键退出游戏。");
            Console.ReadKey();//用于等待用户按任意键退出，方便查看结果
        }
    }
}
```
 #### 理解C# 的堆栈和静态变量的内存分配
```c#
 using System;

namespace MemoryAllocationExample
{
    class Program
    {
        // 静态字段，分配在全局数据区
        static int staticCounter = 0;

        static void Main(string[] args)
        {
            // 局部变量（值类型），分配在栈中
            int localValue = 10;

            // 调用方法，增加堆栈帧
            Console.WriteLine("Main方法开始执行...");
            DisplayMemoryUsage(localValue);

            // 引用类型对象，分配在堆中
            Person person1 = new Person("Alice");
            Person person2 = new Person("Bob");

            Console.WriteLine("两个Person对象已经创建。");

            // 静态字段增值
            staticCounter++;
            Console.WriteLine($"静态字段 staticCounter 的值：{staticCounter}");

            Console.WriteLine("按任意键退出...");
            Console.ReadKey();
        }

        // 方法：增加新的栈帧
        static void DisplayMemoryUsage(int value)
        {
            // 局部变量（值类型），分配在栈中
            int squaredValue = value * value;
            Console.WriteLine($"DisplayMemoryUsage 方法中 squaredValue 的值：{squaredValue}");
        }
    }

    // 引用类型类，创建的对象在堆中
    class Person
    {
        public string Name { get; private set; }

        public Person(string name)
        {
            // name 是一个字符串，它是引用类型，字符串内容分配在堆中
            this.Name = name;
            Console.WriteLine($"Person 对象创建，Name = {name}");
        }
    }
}

//代码解析：内存分配
//全局数据区：

//static int staticCounter 是静态字段，属于类级别的变量。这个变量存储在全局数据区，从程序开始到结束都存在，不会随方法调用或退出而销毁。
//在Main方法中，每次访问这个变量时，都在同一个全局存储位置读取和写入。
//栈内存：

//localValue 是Main方法的局部变量（值类型int），它存储在栈内存中。
//squaredValue 是DisplayMemoryUsage方法的局部变量，同样存储在栈中。当调用DisplayMemoryUsage时，系统在栈上为该方法创建一个新的栈帧，存储方法中的局部变量。方法调用结束后，这个栈帧自动销毁，squaredValue变量也随之释放。
//堆内存：

//Person类是引用类型，当创建person1和person2两个对象时，它们存储在堆内存中。
//Name字段是字符串（string是引用类型），因此字符串内容也存储在堆中，指向字符串的引用则保存在对象中。
//person1和person2的引用在栈中，而它们的实际数据存储在堆中，这样引用可以跨方法传递。
//程序的执行流程
//Step 1: 程序启动，加载staticCounter到全局数据区。
//Step 2: Main方法开始执行，创建localValue变量并分配到栈内存中。
//Step 3: 调用DisplayMemoryUsage方法，squaredValue在该方法的栈帧中创建，调用结束后，squaredValue被自动释放。
//Step 4: 创建person1和person2对象，分配到堆内存中，每个对象的引用存储在栈中。
//Step 5: 增加staticCounter的值并打印。由于staticCounter存储在全局数据区，修改后的值会保留，直到程序结束。
//Step 6: 程序结束，堆上对象的引用丢失，垃圾回收器稍后会回收内存，全局数据区也被释放。

```
### 内存分配总结

- **栈**：用于局部变量（`localValue`和`squaredValue`），方法调用结束时 ***自动*** 释放。
- **堆**：用于引用类型对象（`person1`和`person2`），生命周期较长，由垃圾回收器管理。
- **全局数据区**：用于存储静态字段（`staticCounter`），贯穿整个程序生命周期。


## 10-31

#### C# 数据类型
#### 1. 申请方式不同
##### 内存分配总结

### 内存分配总结

stack 由系统自动分配，heap 需要程序员自己申请
C中用函数malloc 分配空间，用Free释放空间，C++用new分配和delete释放

#### 系统申请后的响应不同

栈:只要栈的剩余空间大于所申请的空间，系统将为程序提供内存，否则将报异常提示溢出。

堆:首先应该知道操作系统有一个记录内存地址的链表，当系统收到程序的申请时，遍历该链表，寻找第一个空间大于所申请的空间的堆结点，然后将该结点从空闲结点链表中删除，并将该结点的空间分配给程序。另外，对于大多数系统，会在这块内存空间中的首地址处记录本次分配的大小，这样代码中的 delete 或 free 语句就能够正确的释放本内存空间。另外，由于找到的堆结点的大小不一定正好等于申请的大小，系统会将多余的那部分重新放入空闲链表中。

#### 申请的效率不同
堆比较慢，速度比较慢，会产生碎片，但是容易管理，
栈系统分配，速度快，但是无法控制，要注意栈溢出

###### 值类型(Value Types)主要包括哪些?-----**实际数据存储栈中，操作系统负责管理分配释放**
整型数值类型
浮点型数值类型
布尔类型(bool)
字符类型(char)
结构类型(struct type)
枚举类型(enum type)
###### 引用类型(Reference Types)主要包括哪些?------- **实际数据存储在堆中，GC负责管理分配释放**
对象类型(object)
字符串类型(string)
类(class)
接口(interface)
委托类型(delegate)
动态类型(dynamic)
数组类型(array)


#### C# 拆箱和装箱
装箱：  值类型 -- 》 引用类型
拆箱： 引用  --> 值类型

##### 装箱（Boxing）

- **定义**：将一个值类型转换为一个引用类型的过程。
- **机制**：当把一个值类型分配给`object`或接口类型时，C#会在堆内存上分配一块内存来存储这个值，并返回一个指向该内存的引用。
- **用途**：装箱用于值类型和引用类型之间的交互。例如，当我们想将`int`或`double`值放入一个集合（如`ArrayList`）时，因为集合存储的是引用类型，需要将值类型“装箱”。

```

int number = 123; object boxedNumber = number; // 装箱：将int类型转换为object类型
```

在上面的代码中，`number`是一个值类型`int`，而`boxedNumber`是一个引用类型`object`。通过装箱，`number`的值被复制到堆内存中并作为引用类型存储。

其实就是，被赋值的对象，进行转换成引用类型



#### 拆箱（Unboxing）

- **定义**：将一个引用类型转换回值类型的过程。
- **机制**：将装箱后的引用类型的内容取出，还原为值类型。
- **用途**：当需要从一个`object`或接口类型中提取值类型的数据时，需要进行拆箱操作。



```
int number = 123; object boxedNumber = number; // 装箱：将int类型转换为object类型
int unboxedNumber = (int)boxedNumber; // 拆箱：将object类型转换回int类型
```

拆箱操作需要显式的类型转换。拆箱不是简单地删除引用，而是将数据还原为值类型，因此可能会产生性能开销。

其实就是object模式下，变成非object 模式而已






## 11-4

其实主要是三个部分组成格式化：索引号，对齐说明符，格式字段
具体参考{index ， alignment:format} index 是必须的，指定列表中某一项，alignment可选，控制字符串输出的宽度以及对齐方式（左对齐或右对齐），format用来指定项的格式--》ps： ":" 两个作用存在差异，一个是对齐，使用的是逗号，如果是格式，使用的是冒号
![[Pasted image 20241104234123.png]]
```c#
int number = 123;
Console.WriteLine("{0,10}", number);  // 输出：       123
//这个是右对齐

string name = "Alice";
Console.WriteLine("{0,-10}!", name);  // 输出：Alice     !
//这个是左对齐
//`{0,-10}` 表示输出参数索引 `0` 的值，宽度为 `10`，并左对齐。
//如果 `name` 是 `"Alice"`，则它会在宽度为 `10` 的空间内，左对齐显示，因此后面会有 `5` 个空格。
Console.WriteLine("{0,-10} {1,10}", "Name", "Age");
Console.WriteLine("{0,-10} {1,10}", "Alice", 30);
Console.WriteLine("{0,-10} {1,10}", "Bob", 25);
Console.WriteLine("{0,-10}{2,-5}{1,10}", "Name","78945", "Age4");//这个输出顺序是按照index的顺序的，name，AGE4 , 78945 这个顺序的
//`{0,-10}` 表示第一个参数（`Name`, `Alice`, `Bob`）左对齐并占 `10` 个字符的宽度。
// `{1,10}` 表示第二个参数（`Age`, `30`, `25`）右对齐并占 `10` 个字符的宽度。
// 这样可以确保输出对齐整齐，适合用在表格化的数据展示中。
```
如果表示的字符数 比对齐说明符中指定的字符数少， 其余字符会使用 空格填充
如果~~~~ 比~~~~ 多，回忽视对齐符，然后用所需字符进行表示
###### 方法1
格式化数字字符串
其实就是使用 “ $ ” 这个符号，然后可以直接在字符串中插入变量
```c#
int age = 30;
string name = "alice";
string greeting = $"Hello, {name}. You are {age} years old."; Console.WriteLine(greeting); // 输出: Hello, Alice. You are 30 years old.
```
插值字符串使用 `$` 前缀，变量用 `{}` 包裹。 

 ###### **方法2**
string result = string.Format("模板字符串 {index}", 值1, 值2, ...);
```c#
//string.Format();//格式化输入
 string name = "Alice";
int age = 25;

string result = string.Format("名字是 {0}, 年龄是 {1}", name, age);
Console.WriteLine(result);
//输出：名字是 Alice, 年龄是 25

```
###### 3. `Console.WriteLine()` 格式化

`Console.WriteLine()` 也支持使用 `{}` 进行格式化。

```csharp
`int age = 30; string name = "Alice"; Console.WriteLine("Hello, {0}. You are {1} years old.", name, age); // 输出: Hello, Alice. You are 30 years old.`
```

###### 4. `string.Concat()` 方法

可以通过 `string.Concat()` 方法将多个字符串拼接在一起。



```csharp
`string part1 = "Hello, "; string part2 = "world!"; string result = string.Concat(part1, part2); Console.WriteLine(result); // 输出: Hello, world!`
```

###### 5. 使用 `+` 拼接字符串

可以使用 `+` 运算符拼接字符串，简单但不推荐用于大量拼接，因为效率较低。

```csharp
`string name = "Alice"; int age = 30; string greeting = "Hello, " + name + ". You are " + age + " years old."; Console.WriteLine(greeting); // 输出: Hello, Alice. You are 30 years old.`
```

###### 6. `StringBuilder` 类

对于大量字符串拼接，使用 `StringBuilder` 更有效率。

```csharp
`using System.Text;  StringBuilder sb = new StringBuilder(); sb.Append("Hello, "); sb.Append("Alice."); sb.Append(" You are "); sb.Append(30); sb.Append(" years old."); Console.WriteLine(sb.ToString()); // 输出: Hello, Alice. You are 30 years old.`
```

###### 7. 格式化数值和日期

C# 支持对数值和日期进行格式化。

```csharp
`double price = 123.456; Console.WriteLine(string.Format("Price: {0:C}", price)); // 输出: Price: $123.46 （C表示货币格式）  DateTime today = DateTime.Now; Console.WriteLine(string.Format("Today is {0:yyyy-MM-dd}", today)); // 输出: Today is 2024-11-04`
```

- `C` 表示货币格式，`F` 表示固定点数格式，`yyyy-MM-dd` 是日期格式。

###### 8. `ToString()` 方法中的格式化

数值类型的 `ToString()` 方法可以直接接受格式字符串：


```csharp
`double price = 123.456; string formattedPrice = price.ToString("C"); // 输出类似：$123.46  DateTime today = DateTime.Now; string formattedDate = today.ToString("yyyy-MM-dd"); // 输出: 2024-11-04`
```

###### 9. 自定义格式化器

你也可以使用自定义格式化器，例如格式化浮点数的精度或整数的位数：

```csharp
`double number = 123.456789; Console.WriteLine(string.Format("{0:F2}", number)); // 输出: 123.46 (保留两位小数)  int num = 42; Console.WriteLine(string.Format("{0:D5}", num)); // 输出: 00042 (将数字格式化为 5 位宽度)`

```
这些方式可以帮助你根据需要格式化字符串，以便更好地控制输出。根据具体情况，选择合适的字符串格式化方式。

## 11-5

###  标准数字格式说明符
#### 1. "C" 或 "c" - 货币格式

- 格式化为货币格式（包括货币符号）。
- 例如：`1234.5678.ToString("C")` 输出结果可能是 `$1,234.57`，具体货币符号取决于系统的区域设置。

#### 2. "D" 或 "d" - 十进制整数格式

- 用于整数的十进制表示，不带小数点。仅用于整型数据。
- 可选的精度指定最少显示的数字个数（不足时补零）。
- 例如：`123.ToString("D5")` 输出结果是 `"00123"`。

#### 3. "E" 或 "e" - 科学计数法格式

- 将数字格式化为科学计数法表示法（即指数形式），显示为 `E+` 或 `E-` 表示法。
- 例如：`1234.5678.ToString("E2")` 输出结果是 `"1.23E+03"`。

#### 4. "F" 或 "f" - 固定小数点格式

- 格式化为固定小数点格式。
- 可选的精度指定小数位数。
- 例如：`1234.5678.ToString("F2")` 输出结果是 `"1234.57"`。

#### 5. "G" 或 "g" - 常规格式

- 格式化为常规格式，即根据数值的大小，自动选择最合适的格式（固定点或科学计数法）。
- 例如：`1234.5678.ToString("G")` 输出结果可能是 `"1234.5678"`。

#### 6. "N" 或 "n" - 数字格式（带千位分隔符）

- 格式化为带千位分隔符的数字。
- 可选的精度指定小数位数。
- 例如：`1234.5678.ToString("N2")` 输出结果是 `"1,234.57"`。

#### 7. "P" 或 "p" - 百分比格式

- 格式化为百分比（乘以 100，并附带 `%` 符号）。
- 可选的精度指定小数位数。
- 例如：`0.1234.ToString("P1")` 输出结果是 `"12.3%"`。

#### 8. "R" 或 "r" - 圆整格式

- 尽可能精确地格式化数值，通常用于表示 `float` 或 `double` 类型。
- 例如：`1234.5678.ToString("R")` 会尝试精确地输出原始数值。

#### 9. "X" 或 "x" - 十六进制格式

- 将整数格式化为十六进制表示法。`X` 表示大写字母，`x` 表示小写字母。
- 例如：`255.ToString("X")` 输出结果是 `"FF"`。
###  *类型，存储和变量*
#### 类型说明
```c#
namespace XXXX // 创建新的命名空间
{
	asifgjiasf//声明类型
	asifgjiasf5//声明类型
	class C
	{
	static void main()//声明类型
	{
	
	}
	}
}
```

#### 1. 类型其实就是一种模板
```c#
namespace HelloWorldApp
{
    class Test
    {
        static void Main(string[] args)
        {
            Car myCar = new Car();
            myCar.Make = "Toyota";
            myCar.Model = "Camry";
            myCar.Drive();

        }

    }
}
//在这个示例中，`Car` 是一个类型（模板），它定义了每个 `Car` 对象都包含一个 `Make` 和 `Model` 属性，以及一个 `Drive` 方法。这些都是任何 `Car` 实例所具有的特性和行为。

public class Car
{
    public string Make{get; set;}
    public string Model{get; set;}
    public void Drive()
    {
        Console.WriteLine("The car is driving.");
    }
}


```

#### 2. 泛型：模板的更直接应用

C# 中的泛型（Generics）是类型作为模板这一概念的更直接的体现。泛型类型使得程序员能够编写可以适应任意数据类型的代码，从而提高了代码的**重用性**和**类型安全性**。

##### 泛型类示例

泛型类型的一个例子是 `List<T>`，其中 `T` 是一种占位符，代表某种类型：

```
`List<int> numbers = new List<int>(); numbers.Add(1); numbers.Add(2);`
```

在这个例子中，`List<int>` 是基于 `List<T>` 泛型模板的实例，其中 `T` 被指定为 `int` 类型。`List<T>` 是一个类型模板，允许用户在创建 `List` 对象时指定它将存储的元素类型。

这种模板化的方式为开发者提供了很大的灵活性。例如，你可以同样使用 `List<string>` 来存储字符串，而不需要为不同类型的集合编写多个实现。

#### 3. 面向对象的抽象

C# 中的类型也支持面向对象编程的核心概念，例如**封装、继承和多态**，这些概念使得类型模板具有更强的扩展性和重用性。

- **封装**：类型封装了数据和操作数据的方法，使得对象的内部细节对外部隐藏。
- **继承**：类型可以继承自其他类型，重用和扩展父类型的行为。
- **多态**：不同类型的对象可以通过统一的接口进行操作，支持多态行为。

这些特性都帮助程序员可以把类型作为一个模板，定义更抽象和通用的代码结构。

#### 4. 常见类型模板

除了类之外，C# 还有一些其他类型的模板，包括：

- **接口（Interface）**：定义行为的模板。接口规定了实现类必须提供哪些方法，而具体的实现细节由实现类来完成。一个类在 C# 中**只能继承一个基类**，但是可以实现**多个接口**
- **委托（Delegate）**：一种函数的模板，用于定义可以引用的方法的签名。

#### 数据成员和函数成员
字段和方法是最重要的类成员类型。  
字段是数据成员，方法是函数成员。
**数据成员**，保存了与这个类的对象或者整个类相关的数据
**函数成员**，执行代码。函数成员定义类型的行为


C#提供16种预定义类型，包括13种简单类型和3种非简单类型
- 简单类型
    - 11种数值类型
        - 不同长度的有符号和无符号整数类型
        - 浮点数的float和double
        - 高精度小数类型decimal（常用于货币计算）（表达分数的）
    - 一种Unicode字符类型 char
    - 一种bool类型，布尔值只能为true或false
- 非简单类型
    - string Unicode字符数组
    - object 所有其他类型的基类
    - dynamic 使用动态语言编写程序集时使用

![](https://images2015.cnblogs.com/blog/759721/201610/759721-20161028153632484-21356889.jpg)
![[Pasted image 20241106230531.jpg]]

![[Pasted image 20241106230537.jpg]]

#### 用户定义类型
用户可以自定义6种类型
- class 类类型
- struct 结构类型
- array 数组类型
- enum 枚举类型
- delegate 委托类型
- interface 接口类型


类型通过类型声明创建，类型声明包含以下信息

- 要创建类型的种类
- 新类型名称
- 类型中每个成员的声明(array和delegate除外，它们不含命名成员)

![](https://images2015.cnblogs.com/blog/759721/201610/759721-20161028153731906-1615918688.jpg)
预定义类型只需要实例化，用户定义类型需要两步骤：声明和实例化

### 堆和栈

运行中程序使用两个内存区域来存储数据：栈和堆

##### 栈

栈是一个LIFO(后进先出)的内存数组
栈存储以下几种类型数据

- 某些类型变量的值
- 程序当前的执行环境
- 传递给方法的参数

栈的特征

- 数据只能从栈的顶端插入或删除
- 把数据放到栈顶称为入栈(push)
- 从栈顶删除数据称为出栈(pop)

![](https://images2015.cnblogs.com/blog/759721/201610/759721-20161028153757187-1162667669.jpg)
##### 堆

在堆里可以分配大块内存来存储某类型的数据对象。  
与栈不同，堆里的内存能以任意顺序存入或移除。  
![](https://images2015.cnblogs.com/blog/759721/201610/759721-20161028153827765-297865527.jpg)  
CLR的GC(Garbage Collector,垃圾收集器)自动删除堆上不再访问的数据。

![](https://images2015.cnblogs.com/blog/759721/201610/759721-20161028153843390-1982237851.jpg)



```C#
public class Program
{
    public class Person
    {
        public string Name;

    }
    static void Main(string[] args)
    {
        // 栈中的例子
        int age = 30;// "age" 是一个局部变量，存储在调用栈中

        // 堆中的例子
        Person person = new Person(); // "person" 是引用类型的变量，其本身存储在栈中，但引用的对象存在于堆中
        person.Name = "Alice";

        // 调用栈中的方法调用
        PrintDetails(age, person);
    }
    static void PrintDetails(int age, Person person)
    {
        // "age" 是值类型，存储在栈中
        // "person" 是引用类型变量，存储在栈中，但它指向的 "Person" 对象存储在堆中
        Console.WriteLine($"Age: {age}, Name: {person.Name}");
    }
}
```

>- **调用栈中的变量**：
    
    - 在 `Main` 方法中，`int age = 30;` 定义了一个 `age` 变量。
    - **`age`** 是一个值类型（`int`），因此它的值 `30` 直接存储在调用栈中。
    - **方法调用**也发生在调用栈中，当调用 `PrintDetails(age, person)` 时，栈会存储该方法调用的帧，包括方法的局部变量和参数。
>- **堆中的变量**：
    
    - `Person person = new Person();` 创建了一个 `Person` 对象。
    - **`person`** 是一个引用类型的变量，存储在调用栈中，它是一个引用，指向了堆中分配的内存，该内存存储了 `Person` 对象的数据。
    - **堆** 用于存储 `person` 对象的实际数据（例如 `Name` 属性），而变量 `person` 只是指向该对象的引用。
>- **方法调用**：
    
    - 当 `PrintDetails` 方法被调用时，`age` 和 `person` 作为参数被复制。
    - **`age`** 是一个值类型，因此它的副本在调用栈中创建。
    - **`person`** 是一个引用类型，因此栈上存储的是对堆中实际 `Person` 对象的引用。这意味着在方法中对 `person` 对象属性的更改会影响原始对象。


### GC判断堆里变量![[Pasted image 20241116000854.jpg]]是否再被使用
#### 1. 引用计数和可达性分析

在 .NET 中，垃圾回收器主要采用**可达性分析**的方式来判断对象是否可以被回收。它不是基于简单的引用计数（尽管引用计数也是一种常用的垃圾回收策略），而是通过**可达性（Reachability）**的概念来判断对象是否仍在被引用。

#### 2. 可达性（Reachability）

垃圾回收器通过**根对象（GC Roots）**来进行可达性分析。如果从 GC Roots 出发，无法找到对某个对象的引用，则认为该对象是不可达的，也就是说它不再被使用，可以进行回收。以下是 GC 进行可达性分析的一般步骤：

- **GC Roots** 是一组特殊的引用，通常包括：
    
    - **静态变量**：存活于应用程序的整个生命周期。
    - **局部变量和方法参数**：当前处于活动状态的线程栈帧中的变量。
    - **全局对象和类的静态成员**。
    - **CPU 寄存器**：某些情况下，存储在寄存器中的引用也会被视为 GC Roots。
- **可达性分析**：
    
    - GC 从所有的 GC Roots 开始，遍历引用链，找到堆中仍然可达的对象。
    - 如果对象从 GC Roots 出发不能通过任何引用路径到达，则认为该对象是不可达的。
    - 不可达的对象被认为是垃圾，可以由垃圾回收器进行回收。

#### 3. 标记-清除算法

在可达性分析中，.NET 的垃圾回收器主要采用了类似于**标记-清除（Mark-and-Sweep）**的算法，其工作原理可以简单描述为以下几步：

1. **标记阶段**：
    
    - GC 遍历 GC Roots，标记所有从根对象可以到达的对象为 "存活"。
    - 如果某个对象未被标记为存活，则表示它不再被引用（不可达）。
2. **清除阶段**：
    
    - GC 清除未标记的对象，释放它们占用的内存空间。

#### 4. 分代垃圾回收

**.NET 的垃圾回收器采用了分代（Generational Garbage Collection）** 的技术，将堆中的对象分为**三代**：

- **第 0 代**（Generation 0）：刚刚分配的对象（短期生存对象）。
- **第 1 代**（Generation 1）：经历过一次垃圾回收但没有被回收的对象（中期生存对象）。
- **第 2 代**（Generation 2）：经历多次垃圾回收的对象（长期生存对象）。

垃圾回收器假设大多数对象是短命的，因此首先会优先回收**第 0 代**，以提高效率。当一个对象经历多次垃圾回收而仍然存活时，它会逐渐被提升到更高一代（第 1 代或第 2 代）。对第 2 代的回收被称为“完全回收”，因为它会回收整个堆内存区域。

**分代垃圾回收的优势**：

- **效率更高**：短期生存对象通常占用较少的内存，因此对第 0 代的回收非常快。
- **降低开销**：垃圾回收器不必每次都遍历整个堆，而是根据对象代数只遍历一部分。

#### 5. 示例 - 对象被回收的情况

以下是一个示例代码来演示什么时候对象会被垃圾回收：



```C#
public class Program
{
    public class Demo
    {
        public void Display()
        {
            Console.WriteLine("Demo instance is alive.");
        }
    }

    static void Main(string[] args)
    {
        // 创建一个对象实例
        Demo demo = new Demo();
        demo.Display(); // 输出 "Demo instance is alive."

        // 超出作用域后，demo 引用不再存在
        demo = null;

        // 显式地调用垃圾回收（不推荐在实际代码中使用）
        GC.Collect();
        GC.WaitForPendingFinalizers();

        Console.WriteLine("Garbage collection has been triggered.");
    }
}

```

#### 解释：

- **`Demo demo = new Demo();`**：创建了一个 `Demo` 类的实例，并将引用赋值给变量 `demo`。
- **`demo = null;`**：将 `demo` 置为 `null`，此时没有任何变量指向堆中的 `Demo` 对象。这个对象变得**不可达**，因此可以被垃圾回收。
- **`GC.Collect();`**：显式地触发垃圾回收，尽管一般情况下，垃圾回收器会自动运行，无需显式调用。

#### 6. 垃圾回收的自动化

在正常情况下，开发者无需手动调用垃圾回收，垃圾回收器会在需要时自动运行，例如当堆空间不够用或者内存压力增大时。GC 是自动化的，但可以通过一些策略来优化它的行为：

- **减少对短期对象的引用**：尽量缩小变量的作用范围，使对象尽早脱离引用。
- **及时取消不再需要的引用**：例如，将引用置为 `null`。
- **避免频繁使用大对象**：大对象会直接分配到第 2 代，因此频繁创建和销毁大对象可能会导致频繁的完全垃圾回收，影响性能。

#### 7. 总结

- 垃圾回收器通过**可达性分析**来判断哪些对象不再被使用。
- GC 使用 **GC Roots** 作为起点，遍历引用链，确定哪些对象是存活的，哪些是可以回收的。
- **分代垃圾回收** 使得回收过程更加高效，优先清理短命对象，从而减少回收的性能开销。
- **自动垃圾回收**：在 C# 中，开发者不需要手动管理内存，GC 会自动在适当的时间回收未被使用的对象，确保程序正常运行并避免内存泄漏。


## 11-16

#### 值类型和引用类型
* 值类型只需要一段单独的内存
* 引用类型需要两段内存
	* 第一段存储实际数据，它总是位于堆中
    - 第二段是一个引用，指向数据在堆中的存放位置（个人理解就是堆和栈的混用结合![[Pasted image 20241116001221.jpg]]

### C# 的类型分类
![[Pasted image 20241116001247.jpg]]

### 变量

---

变量允许程序存取数据

- 变量是一个名称，表示程序执行时存储在内存中的数据
- C#提供4种变量

![](https://images2015.cnblogs.com/blog/759721/201610/759721-20161028154026218-851551754.jpg)

### 静态类型和dynamic关键字

---

每个变量都有变量类型，这样编译器就可以确定运行时需要的内存总量以及哪些部分应该存在栈上，哪些存在堆上。  
变量类型在编译时就确定且不能在运行时修改，这叫静态类型。  
dynamic代表一个特定的、实际的C#类型，它知道如何在运行时解析自身。

### 可空类型

---

某些情况下，特别是使用数据库时，你希望表示变量目前未保存有效的值（数据库中的null）。  
对于引用类型，你可以直接把变量设置为null，但值类型不行。  
可空类型允许创建可以标记为有效或无效的值类型

```C#
int? i =10;
double? d1 =3.14;
bool? flag =null;
char? letter ='a';
int?[] arr =newint?[10];
```

## 11-18
今天第四章啦
### 类的基本概念

**类是一种活动的数据结构**  
在面向对象分析和设计产生前，程序员仅把程序当做指令的序列。那时的焦点在指令的组合和优化上。  
随着面向对象的出现，焦点转移到组织程序的数据和功能上。  
程序的数据和功能被组织为逻辑上相关的数据项和函数的封装集合，并被称为类。
程序的数据和功能被组织为逻辑上相关的数据项和函数的封装集合，并被称为类。

类是一个能存储数据并执行代码的数据结构。包含数据成员和函数成员。

- 数据成员：存储与类或类的实例相关的数据。数据成员通常模拟现实世界事物的特性
- 函数成员：执行代码。通常模拟现实世界事物的功能和操作
类是逻辑相关的数据和函数的封装，通常代表真实世界中或概念上的事物。

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161107140352139-540877489.jpg)


#### 声明类

---

类的声明定义新类的特征和成员。它并不创建类的实例，但创建了用于创建实例的模板。

- 类的名称
- 类的成员
- 类的特征

```
关键字  类名
  ↓      ↓
class MyExcellentClass
{
    成员声明
}

```
##### 类成员

字段和方法是最重要的类成员类型。  
字段是数据成员，方法是函数成员。

##### 字段

字段隶属于类的变量

- 它可以是任何类型，无论预定义类型还是用户定义类型
- 和所有变量一样，字段用于保存数据
    - 可以被写入
    - 可以被读取

```
class MyClass
{
    类型   字段名称
     ↓     ↓
    int MyField
}
```


# 12月
## 12-10 

##### 方法
方法是具有名称的可执行代码块
方法声明包括一下组成部分、

- 返回类型
- 名称
- 参数列表
- 方法体

## 12-11

为数据分配内存

声明类型的变量所分配的内存都是用来保存引用的
而不是类对象实际数据的

要为实际数据分配内存，需要使用new运算符。
- new运算符为任意指定类型的实例分配并初始化内存。它依据类型的不同从栈或堆里分配
- 使用new运算符组成一个对象创建表达式
    - 关键字 new
    - 要分配内存的实例的类型名称
    - 成对的圆括号，可能包括参数或无参数
- 如果内存分配给一个引用类型，则对象创建表达式返回一个引用，指向在堆中被分配并初始化的对象实例



```
Dealer theDealer;    //声明引用变量
theDealer = new Dealer();//为类对象分配内存并赋给变量
```

![[Pasted image 20241211224507.png]]

## 12-15

之前学到了在声明了之后，可以通过设置对象，然后引用的对象是theDealer；
然后为他分配内存这个做法

后面就是实例成员
类声明相当于模板

### 实例成员
类声明相当于模板，通过这个模板想创建多少个类的实例都可以。

- 实例成员：类的每个实例都是不同的实体，它们有自己的一组数据成员，不同于同一类的其他实例。因为这些数据成员都和类的实例相关，所以被称为实例成员
- 静态成员：静态成员是与类相关的成员，静态成员被加载到静态存储区，且只被创建一次，类的所有实例共享静态成员


1. **实例变量（字段）**：这些是类的非静态字段，每个对象实例都有自己的独立副本。

    ```csharp
    public class MyClass {
        public int instanceField;
    }
    ```
    
2. **实例方法**：这些方法可以在对象实例上调用，并且可以访问类的实例变量。

    ```csharp
    public class MyClass {
        public void InstanceMethod() {
            // 可以访问实例变量
        }
    }
    ```
    
3. **实例属性**：这些属性也是每个对象实例独有的。
    ```csharp
    public class MyClass {
        private int _instanceProperty;
        public int InstanceProperty {
            get { return _instanceProperty; }
            set { _instanceProperty = value; }
        }
    }
    ```
    
4. **实例事件**：这些事件是每个对象实例独有的，可以在对象实例上订阅和触发。

    ```csharp
    public class MyClass {
        public event EventHandler InstanceEvent;
        protected virtual void OnInstanceEvent() {
            InstanceEvent?.Invoke(this, EventArgs.Empty);
        }
    }
    ```
5. **构造函数**：虽然构造函数本身不是成员，但它们用于创建类的实例，并且每个实例在创建时都会调用。

    ```csharp
    public class MyClass {
        public MyClass() {
            // 构造函数代码
        }
    }
    ```
    

与实例成员相对的是**静态成员**，它们属于类本身而不是类的任何特定实例。静态成员包括静态字段、静态方法、静态属性和静态事件。静态成员可以通过类名直接访问，而不需要创建类的实例。
```csharp
public class MyClass {
    public static int staticField;
}
// 访问静态字段
int value = MyClass.staticField;
```


### 实例成员（Instance Members）

实例成员是属于类的实例（对象）的成员，每个对象实例都有自己的副本。

#### 实例字段（Instance Fields）

```csharp
public class Person
{
    public string Name; // 实例字段，每个Person对象都有自己的Name
}
```

#### 实例方法（Instance Methods）

```csharp
public class Person
{
    public void IntroduceYourself() // 实例方法
    {
        Console.WriteLine($"Hello, my name is {Name}.");
    }
}
```

#### 实例属性（Instance Properties）

```csharp
public class Person
{
    public string Name { get; set; } // 实例属性
}
```

#### 实例事件（Instance Events）

```csharp
public class Person
{
    public event EventHandler NameChanged; // 实例事件

    public void ChangeName(string newName)
    {
        Name = newName;
        NameChanged?.Invoke(this, EventArgs.Empty);
    }
}
```

### 静态成员（Static Members）

静态成员是属于类的，而不是类的任何特定实例的成员。它们通过类名直接访问，而不是通过类的实例。

#### 静态字段（Static Fields）


```csharp
public class Person
{
    public static int NumberOfPeople; // 静态字段，所有实例共享同一个NumberOfPeople
}
```

#### 静态方法（Static Methods）


```csharp
public class MathHelper
{
    public static int Add(int a, int b) // 静态方法
    {
        return a + b;
    }
}
```

#### 静态属性（Static Properties）

```csharp
public class Configuration
{
    public static string ApiKey { get; set; } // 静态属性
}
```

#### 静态事件（Static Events）

```csharp
public class Logger
{
    public static event EventHandler LogEvent; // 静态事件

    public static void WriteLog(string message)
    {
        LogEvent?.Invoke(null, new EventArgs { Message = message });
    }
}
```

### 使用示例

#### 使用实例成员

```csharp
Person person1 = new Person();
person1.Name = "Alice";
person1.IntroduceYourself(); // 输出: Hello, my name is Alice.

Person person2 = new Person();
person2.Name = "Bob";
person2.IntroduceYourself(); // 输出: Hello, my name is Bob.
```

#### 使用静态成员

```csharp
Person.NumberOfPeople = 2; // 直接通过类名访问静态字段

int sum = MathHelper.Add(5, 3); // 直接通过类名访问静态方法
Console.WriteLine(sum); // 输出: 8

Configuration.ApiKey = "12345"; // 直接通过类名访问静态属性
Console.WriteLine(Configuration.ApiKey); // 输出: 12345

Logger.WriteLog("Log message"); // 直接通过类名访问静态方法
```

在实际编程中，选择使用实例成员还是静态成员取决于你的设计需求。实例成员适用于需要在不同对象之间保持独立状态的情况，而静态成员适用于所有对象共享相同数据的情况。



### 访问修饰符
类的内部，成员之家你可以随意访问
访问修饰符，指明外部程序如何访问类中的成员。

```
字段
    访问修饰符 类型 标识符;
方法
    访问修饰符 返回类型 方法名()
    {
        ...
    }
```

五种访问修饰符

- 私有的（private）
- 公有的（public）
- 受保护的（protected）
- 内部的（internal）
- 受保护内部的（protected internal）

##### 私有访问和公用访问

- 私有访问是默认的访问级别
- 实例的公有成员可以被程序中的其他对象访问


### 从类的内部访问成员

---

类的成员仅用其他类成员的名称就可以访问它们

```cs
class DaysTemp
｛
    //字段
    private int High = 75;
    private int Low = 45;
    //方法
    private int GetHigh()
    {
        return High;
    }
    private int GetLow()
    {
        return Low;
    }
    public float Average()
    {
        return (GetHigh()+GetLow())/2;//访问私有方法
    }

```
![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161107140731920-764925545.jpg)



### 本地变量

---

与类的字段一样，本地变量也保存数据。字段通常保存和对象状态有关的数据，而本地变量通常用于保存本地的或临时的计算数据。

- 本地变量的存在性和生存期仅限于创建它的块及其内嵌的块
    - 它从声明它的那一点开始存在
    - 它在块完成执行时结束存在
- 可以在方法体内任意位置声明本地变量，但必须在使用前声明

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161114192209029-1673180578.jpg)

##### 类型推断和var关键字

观察下面的代码，你会发现编译器其实能从初始化语句的右边推断出来类型名。

- 第一个变量声明中，编译器能推断出15是int型
- 第二个变量声明中，右边的对象创建表达式返回了一个MyExcellentClass类型对象

所以在这两种情况中，在声明开始的显式的类型名是多余的。

```cs
static void Main()
{
    int myInt = 15;
    MyExcellentClass mec = new MyExcellentClass();
    ...
}

为了避免这种冗余，可以在变量声明开始的显式类型名位置使用var关键字

static void Main()
{
    var myInt = 15;
    var mec = new MyExcellentClass();
    ...
}

```
var不是特定的类型变量符号。它表示任何可以从初始化语句的右边推断出来的类型。  
使用var有一些重要的条件

- 只能用于本地变量，不能用于字段
- 只能在变量声明中包含初始化时使用
- 一旦编译器推断出变量的类型，它就是固定且不能更改的

> 说明：var关键字不像JavaScript的var那样可以引用不同的类型。它是从等号右边推断出的实际类型的速记。var关键字并不改变C#的强类型性质。

var不改变强转类型的
`var` 关键字用于局部变量的隐式类型化，而显式类型转换（cast）用于将一个类型强制转换为另一个类型。

派生类（Derived Class）的概念在面向对象编程（OOP）中指的是一个类（称为子类）继承另一个类（称为基类或父类）的属性和方法。这并不是说派生类被“包含”在基类里面，而是说派生类从基类“继承”了代码。这种继承关系允许派生类获得基类的功能，并可以添加或修改这些功能以满足特定的需求。

以下是派生类的一些关键点：

1. **继承**：派生类继承了基类的公共和受保护的成员，但不包括私有成员。这意味着派生类的对象可以访问基类的公共接口。
    
2. **多态**：派生类可以重写（Override）基类的方法，这允许在派生类中定义与基类不同的行为。
    
3. **扩展**：派生类可以添加新的成员（字段、属性、方法等），这些成员在基类中不存在。
    
4. **特化**：派生类可以是对基类的一个特化，意味着它提供了更具体的实现。
    
5. **层次结构**：类之间可以形成层次结构，一个基类可以有多个子类，每个子类都可以进一步有自己的子类。
    

在你提到的 `Dog` 和 `Animal` 的例子中：

- `Animal` 是基类，它定义了所有动物共有的基本属性和行为。
- `Dog` 是派生自 `Animal` 的派生类，它继承了 `Animal` 的属性和行为，并可以添加特有的属性（如 `Breed`）和行为（如 `Bark` 方法）。

这种关系可以这样理解：

- `Dog` **is a** `Animal`，意味着 `Dog` 是 `Animal` 的一种特殊类型。
- `Dog` **has a** `Animal`，意味着 `Dog` 拥有 `Animal` 的所有属性和方法，并且可能还有更多。

因此，派生类 `Dog` 并不是被包含在基类 `Animal` 里面，而是 `Dog` 从 `Animal` 继承了特性，并可能扩展了新的特性。这种继承关系是面向对象设计中的一个核心概念，它允许代码的复用和层次结构的构建。


此处讲解一下派生类的向上和向下转型
```cs
// 定义基类 Animal
public class Animal
{
    // 基类属性
    public string Name { get; set; }
    public int Age { get; set; }

    // 基类方法
    public void Eat()
    {
        Console.WriteLine("The animal is eating.");
    }

    public void Sleep()
    {
        Console.WriteLine("The animal is sleeping.");
    }
}

// 定义派生类 Dog
public class Dog : Animal
{
    // 派生类特有属性
    public string Breed { get; set; }

    // 覆盖基类方法
    public override void Eat()
    {
        Console.WriteLine("The dog is eating.");
    }

    // 派生类特有方法
    public void Bark()
    {
        Console.WriteLine("The dog says: Woof!");
    }
}

class Program
{
    static void Main()
    {
        // 创建 Dog 类型的对象
        Dog dog = new Dog
        {
            Name = "Buddy",
            Age = 3,
            Breed = "Golden Retriever"
        };

        // 向上转型：将 Dog 对象赋值给 Animal 类型的引用
        Animal animal = dog;

        // 调用基类方法
        animal.Eat(); // 输出: The dog is eating.（因为 Eat 方法被覆盖）

        // 向下转型：将 Animal 类型的引用转换回 Dog 类型
        Dog specificDog = (Dog)animal;

        // 调用派生类特有方法
        specificDog.Bark(); // 输出: The dog says: Woof!
    }
}
```



![[Pasted image 20241219234305.png]]

sizeof是无法表示出var的类型的，不安全的




## 12-16 第六章

### 方法体内部代码的执行

---

方法体可包含以下项目

- 本地变量
- 控制流结构
- 方法调用
- 内嵌的块

```cs
static void Main()
{
    int myInt = 3;        //本地变量
    while(myInt > 0)      //控制流结构
    {
        --myInt;
        PrintMyMessage(); //方法调用
    }
}
```

![[Pasted image 20241219224738.jpg]]

### 从类的外部访问静态成员

静态成员可以使用点运算符从类的外部访问。但因为没有实例，所以必须使用类名。

```cs
类名
 ↓
D.Mem2=5;
   ↑
  成员名
```

##### 静态字段示例

- 一个方法设置两个数据成员的值
- 另一个方法显示两个数据成员的值

```cs
class D
{
    int Mem1;
    static int Mem2;
    public void SetVars(int v1,int v2)
    {
        Mem1=v1;
        Mem2=v2;
    }
    public void Display(string str)
    {
        Console.WriteLine("{0}:Mem1={1},Mem2={2}",str,Mem1,Mem2);
    }
}
class Program
{
    static void Main()
    {
        D d1=new D(),d2=new D();
        d1.SetVars(2,4);
        d1.Display("d1");
        d2.SetVars(15,17);
        d2.Display("d2");
        d1.Display("d1");
    }
}
```



>return 对于void类型也有用，用来返回调用的代码，这样子就可以将函数进行返回值
>局部变量必须先声明才能使用，但是局部函数并不需要


### 静态成员的生存期

- 之前我们已经看到了，只有在实例创建后才产生实例成员，实例销毁后实例成员也就不在存在
- 即使类没有实例，也存在静态成员，并可以访问


在 C# 中，静态成员不需要像实例成员那样声明实例就可以被访问，但是它们仍然需要在类中声明。以下是一些需要声明静态成员的情况：

1. **访问静态成员**：尽管静态成员不需要实例就可以访问，但它们必须在类中声明后才能被访问。例如，如果你想要访问一个类的静态属性或方法，你首先需要在类定义中声明它们。

   ```csharp
   public class UtilityClass
   {
       public static string Greeting = "Hello, World!";
   }

   // 可以在不创建实例的情况下访问静态成员
   Console.WriteLine(UtilityClass.Greeting);
   ```

2. **静态方法中的静态成员访问**：在静态方法中，你不能访问类的实例成员，因为静态方法不与类的任何特定实例相关联。在这种情况下，静态成员需要被声明以便在静态方法中使用。

   ```csharp
   public class Calculator
   {
       public static int Add(int a, int b)
       {
           return a + b;
       }
   }

   // 调用静态方法，不需要创建实例
   int result = Calculator.Add(5, 3);
   ```

3. **静态构造函数**：静态成员有时在静态构造函数中初始化，这需要声明静态成员。

   ```csharp
   public class Configuration
   {
       public static string Settings;

       static Configuration()
       {
           Settings = "Default Settings";
       }
   }
   ```

4. **静态字段初始化**：静态字段可以在声明时初始化，这也是一种声明。

   ```csharp
   public class Constants
   {
       public static readonly int MaxValue = 100;
   }
   ```

5. **参与静态类**：如果一个类被声明为静态的，那么它所有的成员都必须是静态的，并且这个类不能被实例化。

   ```csharp
   public static class Helper
   {
       public static void PrintMessage(string message)
       {
           Console.WriteLine(message);
       }
   }
   ```

6. **继承和多态**：虽然静态成员不支持多态，但在继承中，基类的静态成员可以被子类访问，但它们需要在基类中声明。

   ```csharp
   public class BaseClass
   {
       public static string BaseMessage = "Base Message";
   }

   public class DerivedClass : BaseClass
   {
       // 可以访问BaseClass的静态成员
       public static void DisplayBaseMessage()
       {
           Console.WriteLine(BaseMessage);
       }
   }
   ```

总的来说，静态成员需要在类中声明，以便它们可以被识别和访问。静态成员的声明是类定义的一部分，它们的存在不依赖于类的任何特定实例。

 
![[Pasted image 20241223225322.jpg]]


### 成员常量

---
```

成员常量类似本地常量，只是它被声明在类声明中而不是方法内。

class MyClass
{
    const int IntVal=100;//定义值为100的int类型常量
}
const double PI=3.1416; //错误：不能在类型声明之外

与本地常量类似，初始化成员常量的值在编译时必须是可计算的。

class MyClass
{
    const int IntVal1=100;
    const int IntVal2=2*IntVal1;//正确：因为IntVal的值在前一行已设置
}

与本地常量类似，不能在成员常量声明后给它赋值

class MyClass
{
    const int IntVal//错误：声明时必须初始化
    IntVal=100;     //错误：不允许赋值
}

> 与C和C++不同，在C#中没有全局变量。每个常量都必须声明在类型内。
```

### 常量与静态量

---

成员常量比本地常量更有趣，因为它们表现得像静态值。它们对类的每个实例都是“可见的”，而且即使没有类的实例也可以用。与真正的静态量不同，常量没有自己的存储位置，而是在编译时被编译器替换。常量类似C和C++中的#define。  
正因为常量在内存没有存储位置，所以它也不能作为左值（被赋值）。  
static静态量是有自己的存储位置的。

例：常量示例

```cs
class X
{
    public const double PI=3.1416;
}
class Program
{
    static void Main()
    {
        Console.WriteLine("pi={0}",X.PI);
    }
}
```

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161124131234534-249564764.jpg)

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161124131253862-137897376.jpg)

虽然常量成员表现得像一个静态量，但不能将常量声明为static

static const double PI=3.14;//错误：不能将常量声明为static

##### 使用属性
- 要写入属性，在赋值语句的左边使用属性名 (set 访问器)
- 要读取属性，把属性名用在表达式中(get 访问器)
- 属性根据是写入还是读取，隐式调用访问器。（不能显示调用访问器）
![[Pasted image 20241224215627.png]]


##### 自动实现属性

因为属性经常关联到后备字段，C#提供了自动实现属性(automatically implemented property)，允许只声明属性而不声明后备字段。编译器为你创建隐藏的后备字段，并且字段挂接到get和set访问器上。  
自动属性的要点如下

- 不声明后备字段-编译器根据属性类型分配存储
- 不能提供访问器的方法体-它们必须被简单地声明为分号。get相当于简单的内存读，set相当于简单的写
- 除非通过访问器，否则不能访问后备字段。因为不能用其他方法访问，所以实现只读和只写属性没有意义，因此使用自动属性必须同时提供读写访问器。

```cs
class C1
{
    public int MyValue          //属性：分配内存
    {
        set;get;//因为
    }
}
class Program
{
    static void Main()
    {
        C1 c=new C1();
        Console.WriteLine("MyValue:{0}",c.MyValue);
        c.MyValue=20;
        Console.WriteLine("MyValue:{0}",c.MyValue);
    }
}
```


![[Pasted image 20241227215600.jpg]]

### 实例构造函数

_实例构造函数_是一个特殊的方法，它在创建类的每个新实例时执行。

- 构造函数用于初始化类实例的状态
- 如果希望从类的外部创建类的实例，需要将构造函数声明为public

```
class MyClass
{         和类名相同
             ↓
    public MyClass()
    {     ↑
       没有返回类型
        ...
    }
}
```

- 构造函数的名称与类相同
- 构造函数不能有返回值

例：使用构造函数初始化TimeOfInstantiation字段为当前时间

```
class MyClass
{
    DateTime TimeOfInstantiation;
    ...
    public MyClass()
    {
        TimeOfInstantiation=DateTime.Now;
    }
    ...
}

```
> 在学完静态属性后，我们可以仔细看看初始化TimeOfInstantiation那一行。DateTime类(实际上它是一个结构，但由于还没介绍结构，你可以先把它当成类)是从BCL中引入的，Now是类DateTime的静态属性。Now属性创建一个新的DateTime类实例，将其初始化为系统时钟中的当前日期和时间，并返回新DateTime实例的引用。

##### 带参数的构造函数

- 构造函数可以带参数。参数语法和其他方法完全相同
- 构造函数可以被重载

例：有3个构造函数的Class

```cs
class Class1
{
    int Id;
    string Name;
    public Class1(){Id=28;Name="Nemo";}
    public Class1(int val){Id=val;Name="Nemo";}
    public Class1(String name){Name=name;}
    //函数重载了
    public void SoundOff()
    {
        Console.WriteLine("Name{0},Id{1}",Name,Id);
    }
}
class Program
{
    static void Main()
    {
        CLass1 a=new Class1(),
               b=new Class1(7),
               c=new Class1("Bill");
        a.SoundOff();
        b.SoundOff();
        c.SoundOff();
    }
}
```

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161124131920440-1777400707.jpg)

##### 默认构造函数

如果在类的声明中没有显式的提供实例构造函数，那么编译器会提供一个隐式的默认构造函数，它有以下特征。

- 没有参数
- 方法体为空

只要你声明了构造函数，编译器就不再提供默认构造函数。

例：显式声明了两个构造函数的Class2

```cs
class Class2
{
    public Class2(int Value){...}
    public Class2(string Value){...}
}
class Program
{
    static void Main()
    {
        Class2 a=new Class2();//错误！没有无参数的构造函数
        ...
    }
}
```

- 因为已经声明了构造函数，所以编译器 **不提供无参数的默认构造函数
- 在Main中试图使用无参数的构造函数创建实例，编译器产生一条错误信息


### 静态构造函数

---

实例构造函数初始化类的每个新实例，static构造函数初始化类级别的项。通常，静态构造函数初始化类的静态字段。

- 初始化类级别的项
    - 在引用任何静态成员之前
    - 在创建类的任何实例之前
- 静态构造函数在以下方面与实例构造函数类似
    - 静态构造函数的名称和类名相同
    - 构造函数不能返回值
- 静态构造函数在以下方面和实例构造函数不同
    - 静态构造函数声明中使用static
    - 类只能有一个静态构造函数，而且不能带参数
    - 静态构造函数不能有访问修饰符

```
class Class1
{
    static Class1
    {
        ...
    }
}

```
关于静态构造函数还有其他要点

- 类既可以有静态构造函数也可以有实例构造函数
- 如同静态方法，静态构造函数不能访问类的实例成员，因此也不能是一个this访问器
- 不能从程序中显式调用静态构造函数，系统会自动调用它们，在：
    - 类的任何实例被创建前
    - 类的任何静态成员被引用前


在 C# 中，类可以同时拥有静态构造函数和实例构造函数，它们各自有不同的用途和执行时机：

#### 静态构造函数（Static Constructor）
- **用途**：静态构造函数用于初始化包含静态成员的类。它在类第一次被引用时执行，并且仅执行一次。静态构造函数不能显式调用，它由 CLR（公共语言运行时）自动调用。
- **特点**：
  - 没有访问修饰符（如 public、private 等）。
  - 不能带有任何参数。
  - 用于执行只需要执行一次的初始化，例如静态变量的赋值。（换个说法就是只能被调用一次）
  - 如果类中没有静态构造函数，并且类中定义了静态成员，那么这些静态成员的默认值将在类加载时自动初始化。

#### 实例构造函数（Instance Constructor）
- **用途**：实例构造函数用于初始化类的实例。每次创建类的实例时，都会调用实例构造函数。
- **特点**：
  - 可以有零个或多个，每个构造函数可以有不同的参数列表（即构造函数重载）。
  - 用于执行创建对象时必须进行的操作，例如实例变量的初始化。
  - 可以被显式调用（使用 `this` 关键字）或者被隐式调用（如果没有定义任何构造函数，编译器会提供一个无参数的默认构造函数）。

#### 理解两者的关系和区别
- **执行时机**：静态构造函数在类被首次加载到内存时执行，而实例构造函数在创建类的实例时执行。
- **执行次数**：静态构造函数在整个应用程序的生命周期中只执行一次，而实例构造函数每次创建新实例时都会执行。
- **访问静态成员**：静态构造函数可以访问类的静态成员，但不能访问实例成员，因为静态构造函数不与任何特定实例关联。
- **访问实例成员**：实例构造函数可以访问类的静态成员和实例成员，因为它们与特定实例关联。

##### 示例代码
```csharp
public class MyClass
{
    static MyClass()
    {
        // 静态构造函数，用于初始化静态成员
        Console.WriteLine("Static constructor called.");
    }

    public MyClass()
    {
        // 实例构造函数，用于初始化实例成员
        Console.WriteLine("Instance constructor called.");
    }

    static int staticField;
    int instanceField;
}

class Program
{
    static void Main()
    {
        // 首次引用 MyClass 时，静态构造函数被调用
        Console.WriteLine("MyClass is referenced.");

        // 创建 MyClass 的实例，实例构造函数被调用
        MyClass myInstance = new MyClass();
    }
}
```
在这个例子中，当你首次引用 `MyClass` 类时，静态构造函数会被调用。然后，当你创建 `MyClass` 的实例时，实例构造函数会被调用。这展示了静态构造函数和实例构造函数可以共存于同一个类中，并且各自执行不同的初始化任务。


![[Pasted image 20241229161226.png]]**静态构造函数示例**

```cs
class RandomNumberClass
{
    private static Random RandomKey;
    static RandomNumberClass()
    {
        RandomKey=new Random();
    }
    public int GetRandomNumber()
    {
        return RandomKey.Next();
    }
}
class Program
{
    static void Main()
    {
        var a=new RandomNumberClass();//第一次调用了静态构造
        var b=new RandomNumberClass();//只能使用了实例构造了
        Console.WriteLine("Next Random #:{0}",a.GetRandomNumber());
        Console.WriteLine("Next Random #:{0}",b.GetRandomNumber());
    }
}
```


### 对象初始化语句

---

对象初始化语句扩展了创建语法，允许你在创建新的对象实例时，设置字段和属性的值。

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161124132222643-1633297838.jpg)  

例：

new Point {X=5,Y=6};

- 创建对象的代码必须能够访问初始化的字段和属性。如上例中，X和Y必须是public
- 初始化发生在构造方法执行之后，因为构造方法中设置的值可能会在对象初始化中重置为不同的值

```cs
public class Point
{
    public int X=1;
    public int Y=2;
}
class Program
{
    static void Main()
    {
        var pt1=new Point();
        var pt2=new Point(X=5,Y=6);
        Console.WriteLine("pt1:{0},{1}",pt1.X,pt1.Y);
        Console.WriteLine("pt2:{0},{1}",pt2.X,pt2.Y);
    }
}

```
![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161124132306909-30751795.jpg)


### 析构函数

---

析构函数(destructor)执行在类的实例被销毁前需要的清理或释放非托管资源行为。非托管资源通过Win32 API获得文件句柄，或非托管内存块。使用.NET资源无法得到它们，因此如果坚持使用.NET类，就无需为类编写析构函数。  
因此，我们等到第25章再描述析构函数。

### readonly修饰符

---

字段可用readonly修饰。其作用类似于将字段声明为const，一旦值被设定就不能改变。

- const字段只能在字段声明语句中初始化，而readonly字段可以在下列任意位置设置它的值
    - 字段声明语句，类似const
    - 类的任何构造函数。如果是static字段，初始化必须在静态构造函数中完成
- const字段的值必须在编译时决定，而readonly字段值可以在运行时决定。这种增加的自由性允许你在不同环境或构造函数中设置不同的值
- 和const不同，const的行为总是静态的，而readonly字段有以下两点
    - 它可以是实例字段，也可以是静态字段
    - 它在内存中有存储位置

例：Shape类，两个readonly字段

- 字段PI在它的声明中初始化
- 字段NumberOfSides根据调用的构造函数被设置为3或4

```cs
class Shape
{
    readonly double PI=3.1416;
    readonly int NumberOfSides;
    public Shape(double side1,double side2)
    {
        // 矩形
        NumberOfSides=4;
        ...
    }
    public Shape(double side1,double side2,double side3)
    {
        // 三角形
        NumberOfSides=3;
        ...
    }
}
//`NumberOfSides` 是一个 `readonly` 字段，这意味着它只能在声明时或在构造函数中被赋值，不能在类的其他方法中被访问或修改。
```


### this关键字

---

this关键字在类中使用，表示对当前实例的引用。它只能被用在下列类成员的代码块中。

- 实例构造函数
- 实例方法
- 属性和索引器的实例访问器

静态成员不是实例的一部分，所以不能在静态函数成员中使用this。换句话说，this用于下列目的：

- 用于区分类的成员和本地变量或参数
- 作为调用方法的实参

例：MyClass类，在方法内使用this关键字区分两个Var1

class MyClass
{
    int Var1=10;
    public int ReturnMaxSum(int Var1)
    {          参数  字段
                 ↓    ↓
        return Var1>this.Var1?Var1:this.Var1;
    }
}
class Program
{
    static void Main()
    {
        var mc=new MyClass();
        Console.WriteLine("Max:{0}",mc.ReturnMaxSum(30));
        Console.WriteLine("Max:{0}",mc.ReturnMaxSum(5));
    }
}

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161124132346628-629002704.jpg)

### 索引器

---

假如我们定义一个Employee类，它带有3个string型字段，如果不用索引器，我们用字段名访问它们。

class Employee
{
    public string LastName;
    public string FirstName;
    public string CityOfBirth;
}
class Program
{
    static void Main()
    {
        var emp1=new Employee();
        emp1.LaseName="Doe";
        emp1.FirstName="Jane";
        emp1.CityOfBirth="Dallas";
    }
}

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161124132406409-1332004672.jpg)

如果能使用索引访问它们将会很方便，好像该实例是字段的数组一样。

    static void Main()
    {
        var emp1=new Employee();
        emp1[0]="Doe";
        emp1[1]="Jane";
        emp1[2]="Dallas";
    }

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161124132427346-1278891777.jpg)

##### 什么是索引器

索引器是一组get和set访问器，与属性类似。

##### 索引器和属性

索引器和属性在很多方法类似

- 和属性一样，索引器不用分配内存来存储
- 索引器通常表示多个数据成员

> 可以认为索引器是为类的多个数据成员提供get、set属性。通过索引器，可以在许多可能的数据成员中进行选择。索引器本身可以是任何类型。

关于索引器的注意事项

- 和属性一样，索引器可以只有一个访问器，也可以两个都有
- 索引器总是实例成员，因此不能声明为static
- 和属性一样，实现get、set访问器的代码不必一定关联到某字段或属性。这段代码可以什么都不做，只要get访问器返回某个指定类型值即可

##### 声明索引器

- 索引器没有名称。在名称的位置，关键词是this
- 参数列表在方括号中
- 参数列表中至少声明一个参数

Return Type this [Type param1,...]
{
    get{...}
    set{...}
}

声明索引器类似于声明属性。

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161124132454550-1106582661.jpg)  

##### 索引器的set访问器

当索引器被用于赋值时，set访问器被调用，并接受两项数据

- 一个隐式参数，名为value，value持有要保存的数据
- 一个或多个索引参数，表示数据应该保存在哪里

下图例表明set访问器有如下语义

- 它的返回类型为void
- 它使用的参数列表和索引器声明中的相同
- 它有一个名为value的隐式参数，值参类型和索引类型相同

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161124132530643-1647745673.jpg)

##### 索引器的get访问器

get访问器方法体内的代码必须检查索引参数，确定它表示哪个字段，并返回字段值。  
get访问器有如下语义

- 它的参数列表和索引器声明中的相同
- 它返回与索引器相同类型的值

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161124132558315-840405952.jpg)

##### 关于索引器的补充

和属性一样，不能显示调用get、set访问器。取而代之，当索引器用在表达式中取值时，将自动调用get访问器。索引器被赋值时，自动调用set访问器。  
在“调用”索引器时，要在方括号中提供参数。

   索引   值
    ↓    ↓
emp[0]="Doe";           //调用set访问器
string NewName=emp[0];  //调用get访问器

##### 为Employee示例声明索引器

下面代码为示例中的类Employee声明了一个索引器

- 索引器需要去写string类型的值，所以string必须声明为索引器的类型。它必须声明为public，以便从类外部访问
- 3个字段被强行索引为整数0-2，所以本例中方括号中间名为index的形参必须为int型
- 在set访问器方法体内，代码确定索引指的是哪个字段，并把隐式变量value赋给它。在get访问器方法体内，代码确定索引指的哪个字段，并返回该字段的值

class Employee
{
    public string LastName;
    public string FirstName;
    public string CityOfBirth;
    public string this[int index]
    {
        set
        {
            switch(index)
            {
                case 0:LaseName=value;
                    break;
                case 1:FirstName=value;
                    break;
                case 2:CityOfBirth=value;
                    break;
                default:
                    throw new ArgumentOutOfRangeException("index");
            }
        }
        get
        {
            switch(index)
            {
                case 0:return LaseName;
                case 1:return FirstName;
                case 2:return CityOfBirth;
                default:throw new ArgumentOutOfRangeException("index");
            }
        }
    }
}

##### 另一个索引器示例

例：为类Class1的两个int字段设置索引

class Class1
{
    int Temp0;
    int Temp1;
    public int this[int index]
    {
        get
        {
            return(index==0?Temp0:Temp1;)
        }
        set
        {
            if(index==0){Temp0=value;}
            else{Temp1=value;}
        }
    }
}
class Example
{
    static void Main()
    {
        var a=new Class1();
        Console.WriteLine("Values -- T0:{0},T1:{1}",a[0],a[1]);
        a[0]=15;
        a[1]=20;
        Console.WriteLine("Values--T0:{0},T1:{1}",a[0],a[1]);
    }
}

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161124132702034-1507289681.jpg)

##### 索引器重载

类可以有任意多个参数列表不同的索引器。（返回类型不同，不是重载）

例：下面示例有3个索引器

class Myclass
{
    public string this[int index]
    {
        get{...}
        set{...}
    }
    public string this[int index1,int index2]
    {
        get{...}
        set{...}
    }
    public int this[float index1]
    {
        get{...}
        set{...}
    }
}

### 访问器的访问修饰符

---

本章中，你已看到了两种带get、set访问器的函数成员：属性和索引器。默认情况下，成员的两个访问器的访问级别和成员自身相同。也就是说，如果一个属性有public访问级别，那么它的两个访问器也是public的。

不过，你可以为两个访问器分配不同访问级别。例如，下面代码演示了一个常见且**重要**的例子–set访问器声明为private，get访问器声明为public。(get之所以是public，是因为属性的访问级别就是public)

注意：在这段代码中，尽管可以从类的外部读取该属性，但却只能在类的内部设置它。这是非常重要的封装工具。

class Person
{
    public string Name{get;private set;}
    public Person(string name)
    {
        Name=name;
    }
}
class Program
{
    static public void Main()
    {
        var p=new Person("Capt,Ernest Evans");
        Console.WriteLine("Person's name is {0}",p.Name);
    }
}

访问器的访问修饰符有几个限制。最重要的限制如下。

- 仅当成员（属性或索引器）既有get访问器也有set访问器时，其访问器才能有访问修饰符
- 虽然两个访问器都必须出现，但它们中只能有一个有访问修饰符
- 访问器的访问修饰符必须比成员的访问级别有更严格的限制性，即访问器的访问级别必须比成员的访问级别低，详见下图

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161124132747393-479209873.jpg)

例如，如果一个属性的访问级别是public，在图里较低的4个级别中，它的访问器可以使用任意一个。但如果属性的访问级别是protected，则其访问器唯一能使用的访问修饰符是private。

### 分部类和分部类型

---

类的声明可以分割成几个分部类的声明

- 每个分部类的声明都含有一些类成员的声明
- 类的分部类声明可以在同一文件中也可以在不同文件中

每个局部声明必须标为partial class，而不是class。分部类声明看起来和普通类声明相同。

> 类型修饰符partial不是关键字，所以在其他上下文中，可以把它用作标识符。但直接用在关键字class、struct或interface前时，它表示分部类型。

例：分部类

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161124132805596-801525369.jpg)  

Visual Studio为标准的Windows程序模板使用了这个特性。当你从标准模板创建ASP.NET项目、Windows Forms项目或Windows Persentation Foudation(WPF)项目时，模板为每个Web页面、表单、WPF窗体创建两个类文件。

- 一个文件的分部类包含由VS生成的代码，声明了页面上的组件。你不应该修改这个文件中的分部类，因为如果修改页面组件，VS会重新生成
- 另一个文件包含的分部类可用于实现页面或表单组件的外观和行为
- 除了分部类，还有另外两种分部类型
    - 局部结构([第10章](http://www.cnblogs.com/moonache/p/6197243.html#wiz_toc_12))
    - 局部接口(第15章)

### 分部方法

---

分部方法是声明在分部类中不同部分的方法。  
分部方法的两个部分如下

- 定义分部方法声明
    - 给出签名和返回类型
    - 声明的实现部分只是一个分号
- 实现分部方法声明
    - 给出签名和返回类型
    - 是以正常形式的语句块实现

关于分部方法需要了解的重要内容如下

- 定义声明和实现声明的签名和返回类型必须匹配。签名和返回类型有如下特征
    - 返回类型必须是void
    - 签名不能包括访问修饰符，这使分部方法是隐式私有的
    - 参数列表不能包含out参数
    - 在定义声明和实现声明中都必须包含上下文关键字partial，直接放在关键字void前
- 可以有定义部分而没有实现部分。这种情况下，编译器把方法的声明以及方法内部任何对方法的调用都移除。不能只有实现部分而没有定义部分。

下面是一个名为PrintSum的分部方法的示例

- 因为分部方法是隐式私有的，PrintSum不能从类的外部调用。方法Add是调用PrintSum的公有方法

partial class MyClass
{
         必须是void
             ↓
    partial void PrintSum(int x,int y);//定义分部方法
    public void Add(int x,int y)
    {
        PrintSum(x,y);
    }
}
partial class MyClass
{
    partial void PrintSum(int x,int y)//实现分部方法
    {
        Console.WriteLine("Sum i {0}",x+y);
    }
}
class Program
{
    static void Main()
    {
        var mc=new MyClass();
        mc.Add(5,6);
    }
}

![](https://images2015.cnblogs.com/blog/759721/201611/759721-20161124132835878-1497486010.jpg)

## 第七章 类和继承

### 类继承

---

通过继承我们可以定义一个新类，新类纳入一个已经声明的类并进行扩展。

- 可以使用已存在的类作为新类的基础。已存在类称为基类(base class)，新类称为派生类(derived class)。派生类组成如下：
    - 本身声明中的成员
    - 基类的成员
- 声明派生类，需要在类名后加入基类规格说明
- 派生类扩展它的基类，因为它包含了基类的成员，加上它本身声明中的新增功能
- 派生类不能删除它所继承的任何成员

例：OtherClass类，继承自SomeClass

class OtherClass:SomeCLass
{               ↑   ↑
    ...       冒号  基类
}

![](https://images2015.cnblogs.com/blog/759721/201612/759721-20161201094448771-388603077.jpg)


```cs
class SomeClass
{
    public string Field1 = "base class field;";
    public void Method1 (string value)
    {
        Console.WriteLine("base class -- method1: {0} ", value);

    }
}
class OtherClass : SomeClass
{
    public string Field2 = "derived class firld";
    public void Method2(string value)
    {
        Console.WriteLine("Derived class -- Method2: {0}", value);
    }
}
class Program
{
    static void Main()
    {
        var oc = new OtherClass();
        oc.Method1(oc.Field2);  //以派生字段为参数的基类方法
        oc.Method2(oc.Field1);  //以基类字段为参数的派生方法
        oc.Method2(oc.Field2);  //以派生字段为参数的派生方法
    }
}
```


![[Pasted image 20250207210147.jpg]]

### 所有类都派生自object类

---

没有基类规格说明的类隐式直接派生自类object。

关于类继承的其他重要内容如下

- 一个类声明的基类规格说明中只能有一个单独的类。即单继承
- 虽然类只能直接继承一个基类，但继承的层次没有限制。

基类和派生类是相对的术语。所有类都是派生类，要么派生自object，要么派生自其他类。

![](https://images2015.cnblogs.com/blog/759721/201612/759721-20161201094550756-1463545118.jpg)


- **层次结构统一**：C# 中的所有类都有一个共同的基类，`object` 位于类层次结构的顶端。所有类都是从 `object` 派生而来的，这为 .NET 框架提供了一个统一的类型层次。
    
- **默认继承**：当你在 C# 中定义一个类时，即使你没有明确地继承某个类，它也会隐式地继承自 `object` 类。例如，`class MyClass {}` 实际上等价于 `class MyClass : object {}`。
    
- **通用功能**：`object` 类提供了几个通用的方法，这些方法可以在所有对象上使用。例如：
    
    - `ToString()`：返回对象的字符串表示形式。
        
    - `Equals(object obj)`：确定两个对象是否相等。
        
    - `GetType()`：返回对象的运行时类型的 Type。
        
    - `GetHashCode()`：返回对象的哈希代码。



### 屏蔽基类的成员

---

虽然派生类不能删除它继承的任何成员，但可以用与基类同名的成员来屏蔽(mask)基类成员。这是继承的主要功能之一，非常实用。

- 要屏蔽一个继承的数据成员，需要声明一个新的同类型成员，并使用相同名称
- 通过在派生类中声明新的带有相同签名的函数成员，可以隐藏或屏蔽继承的函数成员。(请记住，签名由名称和参数列表组成，不包括返回类型)
- 要让编译器知道你在故意屏蔽继承的成员，使用new修饰符。否则，程序可以成功编译，但编译器会警告你隐藏了一个继承的成员
- 也可屏蔽静态成员

```cs
class SomeClass
{
    public string Field1;
    ...
}
class OtherClass:SomeClass
{
    new public string Field1;
    ...
}
```

![[Pasted image 20250207212037.png]]
如果没有new就会出现这个提示，能用，但是会识别出来
![[Pasted image 20250207212244.jpg]]

```cs
class SomeClass
{
    public string Field1="SomeClass Field1 ";
    public void Method1(string value)
    {
        Console.WriteLine("SomeClass.Method1: {0}",value);
    }
}
class OtherClass:SomeClass
{
    new public string Field1="OtherClass Field1";//屏蔽基类成员
    new public void Method1(string value)//屏蔽基类成员
    {
        Console.WriteLine("OtherCLass.Method1: {0}",value);
    }
}
class Program
{
    static void Main()
    {
        var oc=new OtherClass();
        oc.Method1(oc.Field1);
    }
}
```

![[Pasted image 20250208220020.jpg]]

![[Pasted image 20250208220022.jpg]]

### 对于基类访问

如果派生类必须完全访问被隐藏的继承成员，可以使用 基类访问（base access）
Console.WriteLie("{0}",base.Field1);

```cs
class SomeClass
{
    public string Field1="Field1 -- In the base class";
}
class OtherClass:SomeClass
{
    new public string Field1="Field1 -- In the derived class";
    public void PrintField1()
    {
        Console.WriteLine(Field1);
        Console.WriteLine(base.Field1);
    }
}
class Program
{
    static void Main()
    {
        var oc=new OtherClass();
        oc.PrintField1();
    }
}
```
![[Pasted image 20250208220213.jpg]]

相当于就是base类直接使用了基类函数上面的参数



### 使用基类的引用

---

如果有一个派生类对象的引用，就可以获取该对象基类部分的引用。  
例：使用基类引用

- 第一行声明并初始化了变量derived，它包含一个MyDerivedClass类型对象的引用
- 第二行声明了一个基类类型MyBaseClass的变量，并把derived中的引用转换为该类型，给出对象的基类部分的引用
    - 基类部分的引用被存储在变量mybc中，在赋值运算符的左边
    - 其他部分的引用不能“看到”派生类对象的其余部分，因为它通过基类类型的引用“看”这个对象

MyDerivedClass derived=new MyDerivedClass();
MyBaseClass mybc=(MyBaseClass)derived;

![](https://images2015.cnblogs.com/blog/759721/201612/759721-20161201094824412-2013804290.jpg)

例：两个类的声明和使用

```cs
class MyBaseClass
{
    public void Print()
    {
        Console.WriteLine("This is the base class.");
    }
}
class MyDerivedClass:MyBaseClass
{
    new public void Print()
    {
        Console.WriteLine("This is the derived class");
    }
}
class Program
{
    static void Main()
    {
        var derived=new MyDerivedClass();
        var mybc=(MyBaseClass)derived;
        derived.Print();
        mybc.Print();
    }
}

```
![](https://images2015.cnblogs.com/blog/759721/201612/759721-20161201094850724-1769582696.jpg)

![](https://images2015.cnblogs.com/blog/759721/201612/759721-20161201095443599-1248226856.jpg)

##### 虚方法和覆写方法

虚方法可以使基类的引用访问“升至”派生类内。  
可以使用基类引用调用派生类的方法，只需满足下面的条件。

- 派生类的方法和基类的方法有相同的签名和返回类型
- 基类的方法使用virtual标注
- 派生类的方法使用override标注

```cs
class MyBaseClass
{
    virtual public void Print()
    ...
}
class MyDerivedClass:MyBaseCLass
{
    override public void Print()
    ...
}
```


下图阐明了这组virtual和override方法。注意和上一种情况(用new隐藏基类成员)相比在行为上的区别

- 当使用基类引用(mybc)调用Print方法时，方法调用被传递到派生类执行，因为：
    - 基类的方法被标记为virtual
    - 在派生类中有匹配的override方法
- 下图阐明了这一点，显示了一个从virtual Print方法后面开始，并指向override Print方法的箭头

![](https://images2015.cnblogs.com/blog/759721/201612/759721-20161201095527115-1639919192.jpg)

下面的代码和上一节相同，但由于使用了virtual和override，产生的结果大不相同。

```cs
class MyBaseClass
{
    virtual public void Print()
    {
        Console.WriteLine("This is the base class.");
    }
}
class MyDerivedClass:MyBaseClass
{
    override public void Print()
    {
        Console.WriteLine("This is the derived class");
    }
}
class Program
{
    static void Main()
    {
        var derived=new MyDerivedClass();
        var mybc=(MyBaseClass)derived;
        derived.Print();
        mybc.Print();
    }
}

```
![](https://images2015.cnblogs.com/blog/759721/201612/759721-20161201095601318-286909778.jpg)

其他关于virtual和override的重要信息

- 覆写和被覆写的方法必须有相同的可访问性。换句话说，当被覆写为private时，覆写方法不能是public等
- 不能覆写static方法和非虚方法
- 方法、属性和索引器，以及另一种成员类型事件(将在后面阐述)，都可以被声明为virtual和override

> [!关于虚方法的多态性] 关于虚方法的多态性
> ### 1. **虚方法与多态**
>当你在基类中将一个方法标记为 `virtual`，并在派生类中使用 `override` 来重写它时，这个方法就成为了虚方法。虚方法的调用将在运行时根据对象的实际类型来确定调用哪个实现。
>### 2. **对象的实际类型**
>在你的代码中，`derived` 是一个 `MyDerivedClass` 类型的实例，尽管你将它强制转换为 `MyBaseClass` 类型（`var mybc = (MyBaseClass)derived;`），但对象的实际类型仍然是 `MyDerivedClass`。
>### 3. **动态绑定**
>当调用 `mybc.Print()` 时，C# 编译器会在运行时检查对象的实际类型，而不是引用类型。因此，即使 `mybc` 是 `MyBaseClass` 类型的引用，它仍然会调用 `MyDerivedClass` 的 `Print` 方法。

如果 `MyDerivedClass` 的 `Print` 方法中调用了基类的 `Print` 方法（使用 `base.Print()`），例如：
```cs
override public void Print()
{
    base.Print(); // 调用 MyBaseClass 的 Print 方法
    Console.WriteLine("this is the deriver class");
}
```

```cs
this is the base
this is the deriver class
this is the base
this is the deriver class
```



##  枚举
枚举是由程序员定义的类型与类或结构一样。

- 与结构一样，枚举是值类型，因此直接存储它们的数据，而不是分开存储成引用和数据
- 枚举只有一种类型的成员：命名的整数值常量


```
关键字 枚举名称
  ↓      ↓
enum TrafficLight
{
    Green,    ←  逗号分隔，没有分号
    Yellow,
    Red
}
```

每个枚举类型都有一个底层整数类型，默认为int。

- 每个枚举成员都被赋予一个底层类型的常量值
- 在默认情况下，编译器把第一个成员赋值为0，并对每个后续成员赋的值比前一个多1

```CS
var t1=TrafficLight.Green;
var t2=TrafficLight.Yellow;
var t3=TrafficLight.Red;
Console.WriteLine("{0},\t{1}",t1,(int)t1);
Console.WriteLine("{0},\t{1}",t2,(int)t2);
Console.WriteLine("{0},\t{1}",t3,(int)t3);
```

##### 设置底层类型和显式值

可以把冒号和类型名放在枚举名之后，这样就可以使用int以外的整数类型。类型可以是任何整数类型。所有成员常量都属于枚举的底层类型。

```
enum TrafficLight:ulong
{
    ...
}
```


##### 使用位标志的示例

```cs
[Flags]
enum CardDeckSettings:uint
{
    SingleDeck    =0x01, //位0
    LargePictures =0x02, //位1
    FancyNumbers  =0x04, //位2
    Animation     =0x08  //位3
}
class MyClass
{
    bool UseSingleDeck               =false,
         UseBigPics                  =false,
         UseFancyNumbers             =false,
         UseAnimation                =false,
         UseAnimationAndFancyNumbers =false;
    public void SetOptions(CardDeckSettings ops)
    {
        UseSingleDeck=ops.HasFlag(CardDeckSettings.SingleDeck);
        UseBigPics=ops.HasFlag(CardDeckSettings.LargePictures);
        UseFancyNumbers=ops.HasFlag(CardDeckSettings.FancyNumbers);
        UseAnimation=ops.HasFlag(CardDeckSettings.Animation);
        CardDeckSettings testFlags=CardDeckSettings.Animation|CardDeckSettings.FancyNumbers;
        UseAnimationAndFancyNumbers=ops.HasFlag(testFlags);
        //HasFlag主要是用来辨认
    }
    public void PrintOptions()
    {
        Console.WriteLine("Option settings:");
        Console.WriteLine("Use Single Deck                   - {0}",UseSingleDeck);
        Console.WriteLine("Use Large Pictures                - {0}",UseBigPics);
        Console.WriteLine("Use Fancy Numbers                 - {0}",UseFancyNumbers);
        Console.WriteLine("Show Animation                    - {0}",UseAnimation);
        Console.WriteLine("Show Animation And FancyNumbers   - {0}",UseAnimationAndFancyNumbers);
    }
}
class Program
{
    static void Main()
    {
        var mc=new MyClass();
        CardDeckSettings ops=CardDeckSettings.SingleDeck
                             |CardDeckSettings.FancyNumbers
                             |CardDeckSettings.Animation;//相当于枚举，然后我选择这三个功能
        mc.SetOption(ops);
        mc.PrintOptions();
    }
}

```
![](https://images2015.cnblogs.com/blog/759721/201612/759721-20161227115156992-1901197672.jpg)