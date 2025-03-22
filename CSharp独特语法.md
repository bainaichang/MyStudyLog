# 委托与事件

委托就是函数指针
委托不能放在方法里面

而事件是一种特殊的, 在类里面的委托,更加安全
可以在类的方法里调用事件实现回调函数的功能
事件具有委托没有的一些性质,例如 事件后面只能跟+=, 无法使用=清空函数回调列表(委托可以+=添加多个方法按顺序执行)

```csharp
public delegate void A_Func(int i);
public static void Main () {
    AFunc aFunc = i => Console.WriteLine("the value is " + i);
    aFunc(Int32.MaxValue);
}
```

一个简单明了的案例

```csharp
public class Program {
    public static void Main () {
        HTTPTest client = new HTTPTest();
        client.Begin += data => {
            Console.WriteLine("接收到: "+data);
        };
        client.Begin += data => {
            Console.WriteLine("将数据存入数据库中...");
        };
        client.Start();
    }
}

public class HTTPTest {
    // 这是委托
    public delegate void Success (string data);
    // 该事件需要接收为成功接收网络数据后进行的处理操作
    public event Success Begin;

    public void Start () {
        // 建立网络请求
        Console.WriteLine("开始建立网络连接,等待响应....");
        Thread.Sleep(1000);
        // 假设这是响应的数据
        string data = "{\"name\":\"Man\",\"age\":\"16\"}";
        Begin(data);
    }
}
```

在C#中,官方已经定义好了部分类型的委托

```csharp
// 可以有0~16个参数, 但没返回值
Action<T> a;
// 最后一个是返回值类型
Func<RESULTtype>;
Func<T,RESULT>;
// 条件委托, 定义参数是否符合你给定的条件
Predicate<T>;
// 等...
```

## 异步函数案例

```csharp
using System;
using System.Diagnostics;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args) // C# 7.1+ 支持异步Main
    {
        Console.WriteLine("【同步调用】开始");
        var syncWatch = Stopwatch.StartNew();
        SyncMethod();
        Console.WriteLine($"同步方法耗时: {syncWatch.ElapsedMilliseconds}ms\n");

        Console.WriteLine("【异步调用】开始");
        var asyncWatch = Stopwatch.StartNew();
        await AsyncMethod(); // 等待异步方法完成
        Console.WriteLine($"异步方法耗时: {asyncWatch.ElapsedMilliseconds}ms");
    }

    // 同步方法：顺序执行三个耗时操作
    static void SyncMethod()
    {
        DoWork("任务1", 1000); // 耗时1秒
        DoWork("任务2", 1000); // 再耗时1秒
        DoWork("任务3", 1000); // 再耗时1秒
    }

    // 异步方法：并行执行三个耗时操作
    static async Task AsyncMethod()
    {
        Task t1 = DoWorkAsync("异步任务1", 1000);
        Task t2 = DoWorkAsync("异步任务2", 1000);
        Task t3 = DoWorkAsync("异步任务3", 1000);

        await Task.WhenAll(t1, t2, t3); // 等待所有任务完成
    }

    // 同步耗时操作
    static void DoWork(string name, int delay)
    {
        Console.WriteLine($"{name} 开始 [线程ID: {Environment.CurrentManagedThreadId}]");
        Task.Delay(delay).Wait(); // 同步等待
        Console.WriteLine($"{name} 完成");
    }

    // 异步耗时操作
    static async Task DoWorkAsync(string name, int delay)
    {
        Console.WriteLine($"{name} 开始 [线程ID: {Environment.CurrentManagedThreadId}]");
        await Task.Delay(delay); // 异步等待
        Console.WriteLine($"{name} 完成 [线程ID: {Environment.CurrentManagedThreadId}]");
    }
}
// 类似于golang的go
    public static Task Main () {
        Task.Run(do1);
        Task.Run(do2);
        Console.Read();
        return Task.CompletedTask;
    }

    public static async void do1() {
        Console.WriteLine("a..");
        Console.WriteLine("a..");
        Console.WriteLine("a..");
        Thread.Sleep(1000);
        Console.WriteLine("a..");
        Console.WriteLine("a..");
        Console.WriteLine("a..");
        Console.WriteLine("a..");
        Console.WriteLine("a..");
        
    }

    public static void do2() {
        Console.WriteLine("b..");
        Console.WriteLine("b..");
        Thread.Sleep(1000);
        Console.WriteLine("b..");
        Console.WriteLine("b..");
        Console.WriteLine("b..");
        Console.WriteLine("b..");
        Console.WriteLine("b..");
        Console.WriteLine("b..");
    }
}
```

## 字符串

`@`代表原字符串不进行转义,`$`代表里面可以用{varName}进行拼接字符串

# Switch

C#的switch语句能进行跳转

```csharp
void ShowCard(int cardNumber)
{
    switch (cardNumber)
    {
        case 13:
           Console.WriteLine("国王");
           break;
        case 12:
           Console.WriteLine("皇后");
           break;
        case 11:
           Console.WriteLine("杰克");
           break;
        case -1:  // 小丑角是 -1
           goto case 12;  // 在这个游戏中丑角算作皇后
        default:  // 执行任何其他 cardNumber
           Console.WriteLine(cardNumber);
           break;
    }
}
// 进行类型判断
TellMeTheType(12);
TellMeTheType("hello");
TellMeTheType(true);
void TellMeTheType(object x)  // object 允许任何类型
{
    switch (x)
    {
        case int i:
           Console.WriteLine("It's an int!");
           Console.WriteLine($"The square of {i} is {i * i}");
           break;
        case string s:
           Console.WriteLine("It's a string");
           Console.WriteLine($"The length of {s} is {s.Length}");
           break;
        default:
           Console.WriteLine("I don't know what x is");
           break;
    }
}
// 使用when关键字进行范围判断
switch (x)
{
    case float f when f > 1000:
    case double d when d > 1000:
    case decimal m when m > 1000:
       Console.WriteLine("We can refer to x here but not f or d or m");
       break;
}
// switch还能对字符串进行赋值
int cardNumber = int.Parse(Console.ReadLine());
string cardName = cardNumber switch
{
    13 => "King",
    12 => "Queen",
    11 => "Jack",
    _ => "Pip card"  // 相当于 'default'
};
// 还有元组模式
int cardNumber = 12;
string suite = "spades";
string cardName = (cardNumber, suite) switch
{
    (13, "spades") => "King of spades",
    (13, "clubs") => "King of clubs",
    ...
};
```

# C#元组类型

以下案例来自微软官方

```csharp
(double, int) t1 = (4.5, 3);
Console.WriteLine($"Tuple with elements {t1.Item1} and {t1.Item2}.");
// Output:
// Tuple with elements 4.5 and 3.

(double Sum, int Count) t2 = (4.5, 3);
Console.WriteLine($"Sum of {t2.Count} elements is {t2.Sum}.");
// Output:
// Sum of 3 elements is 4.5.
(double, int) t = (4.5, 3);
Console.WriteLine(t.ToString());
Console.WriteLine($"Hash code of {t} is {t.GetHashCode()}.");
// Output:
// (4.5, 3)
// Hash code of (4.5, 3) is 718460086.


// 下面为元组和函数结合的案例
int[] xs = new int[] { 4, 7, 9 };
var limits = FindMinMax(xs);
Console.WriteLine($"Limits of [{string.Join(" ", xs)}] are {limits.min} and {limits.max}");
// Output:
// Limits of [4 7 9] are 4 and 9

int[] ys = new int[] { -9, 0, 67, 100 };
var (minimum, maximum) = FindMinMax(ys);
Console.WriteLine($"Limits of [{string.Join(" ", ys)}] are {minimum} and {maximum}");
// Output:
// Limits of [-9 0 67 100] are -9 and 100

(int min, int max) FindMinMax(int[] input)
{
    if (input is null || input.Length == 0)
    {
        throw new ArgumentException("Cannot find minimum and maximum of a null or empty array.");
    }

    // Initialize min to MaxValue so every value in the input
    // is less than this initial value.
    var min = int.MaxValue;
    // Initialize max to MinValue so every value in the input
    // is greater than this initial value.
    var max = int.MinValue;
    foreach (var i in input)
    {
        if (i < min)
        {
            min = i;
        }
        if (i > max)
        {
            max = i;
        }
    }
    return (min, max);
}

// 元组的字段名
var t = (Sum: 4.5, Count: 3);
Console.WriteLine($"Sum of {t.Count} elements is {t.Sum}.");

(double Sum, int Count) d = (4.5, 3);
Console.WriteLine($"Sum of {d.Count} elements is {d.Sum}.");
//
var sum = 4.5;
var count = 3;
var t = (sum, count);
Console.WriteLine($"Sum of {t.count} elements is {t.sum}.");
//
var a = 1;
var t = (a, b: 2, 3);
Console.WriteLine($"The 1st element is {t.Item1} (same as {t.a}).");
Console.WriteLine($"The 2nd element is {t.Item2} (same as {t.b}).");
Console.WriteLine($"The 3rd element is {t.Item3}.");
// Output:
// The 1st element is 1 (same as 1).
// The 2nd element is 2 (same as 2).
// The 3rd element is 3.
```

#  C# 的 **LINQ 查询表达式**语法

```csharp
List<int> list = new List<int>() { 1, 2, 3, 4, 5, 6 };
var result = list.Where(i => i % 2 == 0);
string s = string.Join(",",result);
Console.WriteLine(s);
```

# C#的 in out ref

```csharp
ref 修饰符，指定参数由引用传递，可以由调用方法读取或写入。
out 修饰符，指定参数由引用传递，必须由调用方法写入。
in 修饰符，指定参数由引用传递，可以由调用方法读取，但不可以写入。
```

## C#神奇的类,跟py一样可以直接通过点运算符增加字段

```csharp
dynamic obj = new ExpandoObject();
obj.name = "小明";
obj.age = 20;
Console.WriteLine(obj.name);
Console.WriteLine(obj.age);
```

