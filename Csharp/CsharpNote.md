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

#### 拆箱（Unboxing）

- **定义**：将一个引用类型转换回值类型的过程。
- **机制**：将装箱后的引用类型的内容取出，还原为值类型。
- **用途**：当需要从一个`object`或接口类型中提取值类型的数据时，需要进行拆箱操作。



```
int unboxedNumber = (int)boxedNumber; // 拆箱：将object类型转换回int类型
```

拆箱操作需要显式的类型转换。拆箱不是简单地删除引用，而是将数据还原为值类型，因此可能会产生性能开销。