# CSharp

---

* [CSharp](#csharp)
  * [基础语法](#%E5%9F%BA%E7%A1%80%E8%AF%AD%E6%B3%95)
  * [泛型](#%E6%B3%9B%E5%9E%8B)
    * [理解泛型原理](#%E7%90%86%E8%A7%A3%E6%B3%9B%E5%9E%8B%E5%8E%9F%E7%90%86)
    * [泛型约束](#%E6%B3%9B%E5%9E%8B%E7%BA%A6%E6%9D%9F)
    * [协变逆变](#%E5%8D%8F%E5%8F%98%E9%80%86%E5%8F%98)
  * [集合](#%E9%9B%86%E5%90%88)
    * [接口](#%E6%8E%A5%E5%8F%A3)
    * [常用集合](#%E5%B8%B8%E7%94%A8%E9%9B%86%E5%90%88)
  * [表达式解析，解析表达式](#%E8%A1%A8%E8%BE%BE%E5%BC%8F%E8%A7%A3%E6%9E%90%E8%A7%A3%E6%9E%90%E8%A1%A8%E8%BE%BE%E5%BC%8F)
  * [反射](#%E5%8F%8D%E5%B0%84)
  * [动态语言扩展](#%E5%8A%A8%E6%80%81%E8%AF%AD%E8%A8%80%E6%89%A9%E5%B1%95)
  * [加密服务提供程序(CSP)](#%E5%8A%A0%E5%AF%86%E6%9C%8D%E5%8A%A1%E6%8F%90%E4%BE%9B%E7%A8%8B%E5%BA%8Fcsp)
  * [异步编程](#%E5%BC%82%E6%AD%A5%E7%BC%96%E7%A8%8B)
    * [线程、任务](#%E7%BA%BF%E7%A8%8B%E4%BB%BB%E5%8A%A1)
    * [同步](#%E5%90%8C%E6%AD%A5)
    * [线程安全、异常处理、线程取消](#%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8%E5%BC%82%E5%B8%B8%E5%A4%84%E7%90%86%E7%BA%BF%E7%A8%8B%E5%8F%96%E6%B6%88)
    * [Thread、ThreadPool、异步、Task、await/async、Parallel](#threadthreadpool%E5%BC%82%E6%AD%A5taskawaitasyncparallel)
  * [面向对象编程 O0P (封装、继承、多态，接口抽象类选择)](#%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%BC%96%E7%A8%8B-o0p-%E5%B0%81%E8%A3%85%E7%BB%A7%E6%89%BF%E5%A4%9A%E6%80%81%E6%8E%A5%E5%8F%A3%E6%8A%BD%E8%B1%A1%E7%B1%BB%E9%80%89%E6%8B%A9)
  * [面向切面编程AOP (OOP思想补充，C\#多种实现AOP，定制个性化AOP扩展)](#%E9%9D%A2%E5%90%91%E5%88%87%E9%9D%A2%E7%BC%96%E7%A8%8Baop-oop%E6%80%9D%E6%83%B3%E8%A1%A5%E5%85%85c%E5%A4%9A%E7%A7%8D%E5%AE%9E%E7%8E%B0aop%E5%AE%9A%E5%88%B6%E4%B8%AA%E6%80%A7%E5%8C%96aop%E6%89%A9%E5%B1%95)
  * [CLR核心机制](#clr%E6%A0%B8%E5%BF%83%E6%9C%BA%E5%88%B6)
    * [异常和状态管理](#%E5%BC%82%E5%B8%B8%E5%92%8C%E7%8A%B6%E6%80%81%E7%AE%A1%E7%90%86)
    * [托管堆和垃圾回收](#%E6%89%98%E7%AE%A1%E5%A0%86%E5%92%8C%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6)
    * [CLR寄宿和性能提升](#clr%E5%AF%84%E5%AE%BF%E5%92%8C%E6%80%A7%E8%83%BD%E6%8F%90%E5%8D%87)
  * [设计模式六大原则(单\-职责、里氏替换、依赖倒置、接口隔离、迪米特、开闭〉](#%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E5%85%AD%E5%A4%A7%E5%8E%9F%E5%88%99%E5%8D%95-%E8%81%8C%E8%B4%A3%E9%87%8C%E6%B0%8F%E6%9B%BF%E6%8D%A2%E4%BE%9D%E8%B5%96%E5%80%92%E7%BD%AE%E6%8E%A5%E5%8F%A3%E9%9A%94%E7%A6%BB%E8%BF%AA%E7%B1%B3%E7%89%B9%E5%BC%80%E9%97%AD)
  * [设计模式专训(单例、三大工厂、 装饰器、迭代器、观察者、代理等)](#%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F%E4%B8%93%E8%AE%AD%E5%8D%95%E4%BE%8B%E4%B8%89%E5%A4%A7%E5%B7%A5%E5%8E%82-%E8%A3%85%E9%A5%B0%E5%99%A8%E8%BF%AD%E4%BB%A3%E5%99%A8%E8%A7%82%E5%AF%9F%E8%80%85%E4%BB%A3%E7%90%86%E7%AD%89)
  * [数据库设计优化(数据库设计、分库分表表分区、读写分离高可用、索引优化、执行计划分析)](#%E6%95%B0%E6%8D%AE%E5%BA%93%E8%AE%BE%E8%AE%A1%E4%BC%98%E5%8C%96%E6%95%B0%E6%8D%AE%E5%BA%93%E8%AE%BE%E8%AE%A1%E5%88%86%E5%BA%93%E5%88%86%E8%A1%A8%E8%A1%A8%E5%88%86%E5%8C%BA%E8%AF%BB%E5%86%99%E5%88%86%E7%A6%BB%E9%AB%98%E5%8F%AF%E7%94%A8%E7%B4%A2%E5%BC%95%E4%BC%98%E5%8C%96%E6%89%A7%E8%A1%8C%E8%AE%A1%E5%88%92%E5%88%86%E6%9E%90)
  * [DDD领域驱动设计(学习领域驱动设计，POP\-OOP\-DDD，用EF完成领域模型设计)](#ddd%E9%A2%86%E5%9F%9F%E9%A9%B1%E5%8A%A8%E8%AE%BE%E8%AE%A1%E5%AD%A6%E4%B9%A0%E9%A2%86%E5%9F%9F%E9%A9%B1%E5%8A%A8%E8%AE%BE%E8%AE%A1pop-oop-ddd%E7%94%A8ef%E5%AE%8C%E6%88%90%E9%A2%86%E5%9F%9F%E6%A8%A1%E5%9E%8B%E8%AE%BE%E8%AE%A1)
  * [缓存原理和应用，解析各环节Cache、封装缓存基类](#%E7%BC%93%E5%AD%98%E5%8E%9F%E7%90%86%E5%92%8C%E5%BA%94%E7%94%A8%E8%A7%A3%E6%9E%90%E5%90%84%E7%8E%AF%E8%8A%82cache%E5%B0%81%E8%A3%85%E7%BC%93%E5%AD%98%E5%9F%BA%E7%B1%BB)

## 基础语法
```csharp
using System;
using x = System;
using static System.String;

public class Class1
{
#if DEBUG
    	//如果是DEBUG会执行的代码
#endif
#region
    	//折叠功能
#endregion
#warning "遇到此指使会在警告窗口显示"
#error "遇到此指使会在错误窗口显示"
    public Class1() //构造函数
    {
    	object o = null;
        Type t = typeof(string);
        var nbr = default(int);
        var name = nameof(nbr);
        checked { }//检查算术溢出
        unchecked { }//不检查算术溢出
        unsafe { }//不安全代码(需要:项目->属性->生成>允许不安全代码)
        Lazy<object> lazy = new Lazy<object>();//懒加载
        Nullable<int> nbrnullable = new int?(0);//可空类型
        var str = null ?? "sdfsdf";
        str = o?.ToString();
        Action<string> action = delegate(string str){};//匿名函数
    }
    static Class1() { }//静态构造函数
    //字段 属性 方法
    public delegate void ClickHandler();
    public event ClickHandler Click;
    public void AddClickHandler()
    {
        Click += () => { };//绑定事件
        Click -= () => { };//解除绑定
        Click.Invoke();//触发事件
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
    public IEnumerable<int> GetRange(int length)//迭代器
    {
        for (int i = 0; i < length; i++)
        {
            yield return i;
        }
    }
    ~Class1() { }
}
public static class Class2
{
    static Class2()
    {
        global::System.Int32 nbr = "3".ToInt();//global::从全局开始查找
    }
    public static int ToInt(this string str)//扩展方法只能在静态类中
    {
        return int.Parse(str);
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
### 协变逆变
* `逆变` **in** *作为输入，只能被使用*
* `协变` **out** *作为输出，只能被返回*

## 集合
 ### 接口
 * **IEnumerable、IEnumerator** *将foreach用于集合*
 * **ICollection：IEnumerable** *提供Count、CopyTo、Add、Remove、Clear等方法*
 * **IList：ICollection,IEnumerable** *提供索引器，提供Insert、RemoveAt等方法*
 * **ISet：ICollection,IEnumerable** *提供索引器，提供Insert、RemoveAt等方法*
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
* `Semaphore、SemaphoreSlim` 限制访问同一个资源的线程数量
* `ManualResetEvent、AutoResetEvent、ManualResetEventSlim` 在线程间传递信号
* `CountDowmEvent` 等待一定数量的操作完成
* `Barrier` 障碍(使多个任务能够采用并行方式依据某种算法在多个阶段中协同工作)
* `ReaderWriterLockSlim` 允许多个线程同时读取及独占写
> lock语法糖内部使用Monitor.Entry()和Monitor.Exit()
> SpinLock会先使用用户模式，在多个迭代后使用内核模式
> 具名的Mutex是操作系统对象，一定要关闭，最好使用using
> SemaphoreSlim使用混合模式，Semaphore使用内核模式
> ManualResetEvent

### 线程安全、异常处理、线程取消
### Thread、ThreadPool、异步、Task、await/async、Parallel

## 面向对象编程 O0P (封装、继承、多态，接口抽象类选择)

## 面向切面编程AOP (OOP思想补充，C#多种实现AOP，定制个性化AOP扩展)

## CLR核心机制
### 异常和状态管理
### 托管堆和垃圾回收
### CLR寄宿和性能提升

## 设计模式六大原则(单-职责、里氏替换、依赖倒置、接口隔离、迪米特、开闭〉

## 设计模式专训(单例、三大工厂、 装饰器、迭代器、观察者、代理等)

## 数据库设计优化(数据库设计、分库分表表分区、读写分离高可用、索引优化、执行计划分析)

## DDD领域驱动设计(学习领域驱动设计，POP-OOP-DDD，用EF完成领域模型设计)

## 缓存原理和应用，解析各环节Cache、封装缓存基类