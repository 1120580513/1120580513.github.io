# CSharp

---

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
* `逆变` **<in T>** *作为输入，只能被使用*
* `协变` **<out T>** *作为输出，只能被返回*

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

## Emit

## 序列化
### 文件序列化反序列化
### XML、JSON、Image Helper

## 加密解密
### RSA、DES，MD5加密类封装

## 异步和多线程
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