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

