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

- **栈**：用于局部变量（`localValue`和`squaredValue`），方法调用结束时自动释放。
- **堆**：用于引用类型对象（`person1`和`person2`），生命周期较长，由垃圾回收器管理。
- **全局数据区**：用于存储静态字段（`staticCounter`），贯穿整个程序生命周期。