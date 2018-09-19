# CSharp

* [C#历史](https://docs.microsoft.com/zh-cn/dotnet/csharp/whats-new/csharp-version-history)

---

## 语法

```csharp
#define DEFINE//定义预处理器指令

using System;
using x = System;//别名
using System.Collections;
using System.Collections.Generic;
//C# 6.0 静态导入
using static System.Console;
using System.Threading.Tasks;
using System.Diagnostics;

#region  预处理器指令
#if DEFINE
//如果是DEBUG会执行的代码
#endif
#warning "遇到此指使会在警告窗口显示";
#endregion

namespace ConsoleApp1
{
    class Programe
    {
        //入口
        static int Main(string[] args)
        {
            return 0;
        }
    }

    /// <summary>
    /// 枚举
    /// </summary>
    [Flags]//特性、注解
    public enum Enum1
    {
        a = 0x00000001,
        b = 0x00000010
    }

    /// <summary>
    /// 结构体
    /// </summary>
    public struct Struct1
    {
        public int MyProperty { get; set; }
        public void Function() { }
    }
    //协变：T 仅用于返回值，IOutInterface<父类> = IOutInterface<子类>
    public interface IOutInterface<out T> { }
    //抗变：T 仅用于参数值，父类 = 子类
    public interface IInInterface<in T> { }

    /// <summary>
    /// 类
    /// </summary>
    // partial：分部类型 C# 2.0
    public partial class Class1
    {
        void KeyWord()
        {
            object o = null;
            Type t = typeof(string);//得到 Type
            var nbr = default(int);//默认值
            var name = nameof(nbr);//得到变量名 C# 6.0
            checked { }//检查算术溢出
            unchecked { }//不检查算术溢出
            //unsafe { }//不安全代码(需要:项目->属性->生成>允许不安全代码)

            //grammar

            var mopera = 3 > 4 ? 0 : 1;//三元运算符
            var nullgrammer = null ?? string.Empty;//空运算符
            var nullx = nullgrammer?.ToString();//Null 传播器 C# 6.0
            var format = $"ac{nullgrammer}";//拼接
            var aa = @"//可换行
asdfsdf ""//转义："" => 引号 
";
        }
        static Class1() { }
        public Class1() : this("") { }
        public Class1(string a) { }

        private readonly string _readonly_field;//只读
        private string _field;
        private int? _nullable;//可空类型
        private Nullable<int> _nullable2;//可空类型

        public string Property
        {
            private get { return _field; }
            set { _field = value; }
        }
        public string AutoImplProperty { get; set; }//自动实现的属性
        public string InitProperty { get; set; } = "";//属性初始值

        public delegate void DelegateHandler(string a);
        void _handler(string x) { }
        public event DelegateHandler Events;
        void InvokeEvent()
        {
            Events += _handler;
            Events -= _handler;
            Events += s => { };//lambda
            Events += delegate (string a) { };//匿名方法 C# 2.o
            Events.Invoke(string.Empty);//执行事件
        }

        public static explicit operator string(Class1 x)//显示类型转换
        {
            return x.ToString();
        }
        public static implicit operator int(Class1 x)//隐式类型转换
        {
            return int.Parse(x.ToString());
        }
        public static Class1 operator +(Class1 c)//运算答重载
        {
            return c;
        }

        public int this[int i]//索引器
        {
            get { return 0; }
            set { var nbr = value; }
        }

        public IEnumerator GetEnumerator()//迭代器
        {
            for (int i = 0; i < 10; i++)
            {
                yield return i;
            }
        }
        public void OutParam(out int a, ref int b)
        {
            a = 3;//out 必须要赋值
            b = 2;//ref 引用传递
        }
        //async & await C# 5.0
        async void TestAsync(
            //调用方信息 C# 5.0
            [System.Runtime.CompilerServices.CallerMemberName]string memberName = "调用方的方法或属性名称",
            //源文件中调用方法的行号
            [System.Runtime.CompilerServices.CallerLineNumber]int lineNumber = 0,
            [System.Runtime.CompilerServices.CallerFilePath]string filePath = "调用方的源文件的路径")
        {
            try
            {
                await Task.FromResult(0);
            }
            catch (Exception) when (DateTime.Now == DateTime.Now)
            {

                throw;
            }
        }

        ~Class1() { }
    }
    public partial class Class1<T> where T : class, new()//泛型
    {
        static void StaticFunction<F>(out F a)
        {
            a = default(F);
        }
    }
    public static class ExtendFunction
    {
        static ExtendFunction()
        {
            global::System.Int32 nbr = "3".ToInt();//global::从全局开始查找
        }
        //扩展方法（只能在静态类中）
        public static int ToInt(this string str)
        {
            return int.Parse(str);
        }
    }
}
```

##  泛型
### 理解泛型原理
* **在编译器编译成MSIL的时候不会为T指定特定的类型**
* **对于值类型，JIT会检查是否有参数的代码，如果没有就生成，并将T替换成传入的参数**
* **对于引用类型，JIT把T换成Object**
> 实际上，泛型对性能的提升大多在于值类型(JIT会为每种值类型生成封闭代码)，引用类型几乎可以忽略不计。主要用途在于编译时的安全、提高编码效率
### 泛型约束
* **T : class** *T为引用类型*
* **T : struct** *T为值类型*
* **T : new()** *必须有无参的构造函数*
* **T : interface|class** *继承接口或类*

## 集合
 ### 接口
 * **IEnumerable、IEnumerator** *将 foreach 用于集合*
 * **ICollection：IEnumerable** *提供 Count、CopyTo、Add、Remove、Clear等方法*
 * **IList：ICollection,IEnumerable** *提供索引器，提供 Insert、RemoveAt 等方法*
 * **ISet：ICollection,IEnumerable** *提供索引器，提供 Insert、RemoveAt等方法*
 * **IDictionary：ICollection,IEnumerable** *Key：Value*
 * **ILookup** *Key：Value[]*
 * **IProducerConsumerCollection** *线程安全集合*
---
 * **IComparer、IEqualityComparer** *比较器(可实现集合排序、可比较字典的K和对象*
---
 * **IReadOnlyCollection、IReadOnlyDictionary、IReadOnlyList** *只读集合*
---
 * **IImmutableList、IImmutableDictionary、IImmutableQueue、IImmutableSet、IImmutableStack** *不可变集合*
 ### 常用集合
 * `List` `LinkList` `Dictionary` `Queue` `Stack` `HashSet` `BitArray(BitVector32)` `BlockingCollection`
 * `SortedList` `SortedDictionary` `SortedSet`
 * `ReadOnly[只读集合]` `Immutable[不可变集合]` `Concurrent[并发集合]` `ObservableCollection(元素发生变化时触发事件)`

| 集合 | Add | Insert | Remove | Item | Sort | Find |
|-----|-----|--------|--------|-------|-----|-------|
| List | O(1)【重置大小:O(n)】 | O(n) | O(n) | O(1) | O(n log n)【最坏是O(n^2)】 |  |
| Stack | Push():O(1)【重置大小:O(n)】 | n/a | Pop():O(1) | n/a | n/a | n/a |
| Queue | Qnqueue():O(1)【重置大小:O(n)】 | n/a | Dequeue:O(1) | n/a | n/a | n/a |
| HashSet | O(1)【重置大小:O(n)】 | Add():O(1)或O(n) | O(1) | n/a | n/a | n/a |
| SortSet | O(1)【重置大小:O(n)】 | Add():O(1)或O(n) | O(1) | n/a | n/a | n/a |
| LinkList | AddLast():O(1) | AddAfter():O(1) | O(1) | n/a | n/a | O(n) |
| Dictionary | O(1)【重置大小:O(n)】 | n/a | O(1) | O(1) | n/a | n/a |
| SortedDictionary | O(log n) | n/a | O(log n) | O(log n) | n/a | n/a |
| SortedList | 无序:O(n),至尾部:O(log n),重置大小:O(n) | n/a | O(n) | RW:O(log n),containKey:O(log n),!containKey:O(n) | n/a | n/a |

 > 使用列表作为自动增长的集合，队列以先进先出的方式访问元素，堆以先进后出的方式访问元素。链表可以快速插入和删除元素，但搜索较慢，通过键和值可以使用字典他的插入和搜索较快，集用于唯一项，可以是有序的也可以是无序的

## 表达式解析，解析表达式
 * **表达式解析** *继承ExpressionVisitor(模仿ExpressionStringBuilder)*
 * **解析表达式** *TODO*

## 反射
```csharp
var t = typeof(Test);

var obj = Activator.CreateInstance(t, new object[] { "xxx" }) as Test;

t.GetField("a", BindingFlags.Instance | BindingFlags.NonPublic).SetValue(obj, "yyyyy");

t.GetMethod("c", BindingFlags.Instance | BindingFlags.NonPublic).Invoke(obj, new object[] { "非静态私有方法" });
t.GetMethod("d", BindingFlags.Instance | BindingFlags.Public).Invoke(obj, new object[] { "非静态公有方法" });
t.GetMethod("e", BindingFlags.Instance | BindingFlags.Public | BindingFlags.Static).Invoke(null, new object[] { "静态公有方法" });

t.GetMethod("f", BindingFlags.Instance | BindingFlags.NonPublic | BindingFlags.Static).MakeGenericMethod(typeof(string)).Invoke(obj, new object[] { "非静态泛型对象私有方法" });
t.GetMethod("g", BindingFlags.Instance | BindingFlags.Public | BindingFlags.Static).MakeGenericMethod(typeof(string)).Invoke(null, new object[] { "静态泛型对象公有方法" });
t = Assembly.GetExecutingAssembly().GetType("test.Test`1").MakeGenericType(typeof(string));

t.GetMethod("a", BindingFlags.Instance | BindingFlags.Public | BindingFlags.Static).Invoke(null, new object[] { "泛型类方法" });
```

## 动态语言扩展
* `DynamicObject` `ExpandoObject`

## 加密服务提供程序(CSP)
```csharp
MD5 md5 = new MD5CryptoServiceProvider();
var buffer = Encoding.Default.GetBytes(value);
var md5Str = BitConverter.ToString(buffer).Replace("-","");
```
* `HashAlgorithm`
* `SHA1CryptoServiceProvider` `SHA256CryptoServiceProvider` `SHA384CryptoServiceProvider` `SHA512CryptoServiceProvider` `MD5CryptoServiceProvider` **Hash**
* `DESCryptoServiceProvider()` `AesCryptoServiceProvider` `TripleDESCryptoServiceProvider(3Des)` `RC2CryptoServiceProvider` **对称**
* `RSACryptoServiceProvider` `DSACryptoServiceProvider` **非对称**

---

```csharp
byte[] bytes = new byte[16];
RNGCryptoServiceProvider r = new RNGCryptoServiceProvider();
for (int i = 0; i < 10; i++)
{
    r.GetBytes(bytes);
    int number = (int) ((decimal) bytes[0] / 256 * 100) + 1;//1-100
    Console.WriteLine(number);
}
r.Dispose();
```
* `RNGCryptoServiceProvider` **加密随机数生成器**

## 异步编程
* **`线程同步`** *当一个线程执行递增或递减操作时其他线程需要依次等待*
* **`原子操作`** *操作只占用一个量子的时间，一次就可以完成*
* **`上下文切换`** *指操作系统的线程调度器，保存等待的线程的状态，并切换到另一个线程，依次恢复等待的线程状态*
* **`用户模式`** *让线程只是简单的等待(while)，虽然浪费CPU时间，但节省了上下文切换的CPU消耗*
* **`内核模式`** *让线程进入阻塞状态(Sleep)，当处于阻塞状态时，只会占用尽可能少的CPU时间，但是该词条我非常引入一次上下文切换*
* **`混合模式`** *先尝试用户模式，如果线程等待了足够长的时间，会切换到阻塞模式以节省CPU资源*

---

### 线程、任务
* `Thread` `ThreadPool` `Timer`
* `await/async` `Task` `Parallel` `CancellationTokenSource`

### 同步
* `(EventWaitHandler,Semaphore)：WaitHandle`
* `(AutoResetEvent,ManualResetEvent)：EventWaitHandler`

---

* `Monitor(lock)` `SpinLock(自旋锁)` `SpinWait(自旋等待)` `Interlocked(原子操作)` `Mutex(互斥量)`
* `SemaphoreSlim` 限制访问同一个资源的线程数量
* `AutoResetEvent、ManualResetEventSlim` 在线程间传递信号
* `CountDowmEvent` 等待一定数量的操作完成
* `Barrier` 障碍(使多个任务能够采用并行方式依据某种算法在多个阶段中协同工作)
* `ReaderWriterLockSlim` 允许多个线程同时读取及独占写
> lock语法糖内部使用Monitor.Entry()和Monitor.Exit()
> SpinLock会先使用用户模式，在多个迭代后使用内核模式
> 具名的Mutex是操作系统对象，一定要关闭，最好使用using

## T4
* [manager.ttinclude](file/manager.ttinclude)
### 说明
```csharp
<#@ template debug="false" hostspecific="false" language="C#"#> 
//debug：是否可调试，hostspecific：是否提供Host属性，language：语言
<#@ assembly name="System.Core"#>
//添加dll
<#@ import namespace="System.Linq"#>
//类似于 using System.Linq
<#@ output extension=".txt"#>
// 输出文件的后缀名
<#@ include file="manager.ttinclude"#>
//等效于将manager.ttinclude的文本放到此位置
<#@ parameter type="System.String" name="ParameterName"#>
//type：参数类型，ParameterName：参数名称
//语法说明：
<#   标准控制块    #> //可以包含语句。
<#= 表达式控制块 #> //可以包含表达式。
<#+ 类特征控制块 #> //可以包含方法、字段和属性，就像一个类的内部。
```
### Demo
```csharp
<#@ include file="manager.ttinclude" #>
<#    var manager = Manager.Create(Host,GenerationEnvironment); #>
<#    manager.StartHeader(); #>
using System;
<#    manager.EndBlock(); #>
<# manager.StartNewFile("Employee.cs"); #> 
public class Employee {  } 
<# manager.EndBlock(); #> 
<# manager.StartNewFile("User.cs"); #> 
public class User {  } 
<# manager.EndBlock(); #> 
<# manager.StartFooter(); #> 
// It's the end 
<# manager.EndBlock(); #> 
<# manager.Process(true); #>
``
```

## 反向工程

* `NET.Reflector` `ildasm40` `dnSpy`