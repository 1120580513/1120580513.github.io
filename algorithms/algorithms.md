# Algorithms

---

## 排序
### 选择
```csharp
int l = a.Length, pos;
for (int i = 0; i < l - 1; i++)
{
    pos = i;
    for (int j = pos + 1; j < l; j++)
    {
        if (a[j] < a[pos])
            pos = j;
    }
    var tem = a[i];
    a[i] = a[pos];
    a[pos] = tem;
}
```
> 将第i个元素和i~n个元素比较，找出最小的元素和i交换位置
### 插入
```csharp
for (int i = 1; i < array.Length; i++)
{
    for (int j = i; j > 0 && array[j] < array[j - 1]; j--)
    {
        array[j] = array[j] ^ array[j - 1];
        array[j - 1] = array[j] ^ array[j - 1];
        array[j] = array[j] ^ array[j - 1];
    }
}
```
> 将第i+1个元素和0~i+1比较，把最小的插入i的位置
### 希尔
```csharp
var n = arr.Length;
var h = 1;
while (h < n / 3) h = 3 * h + 1;
while (h >= 1)
{
    for (int i = h; i < n; i++)
    {
        for (int j = i; j >= h && arr[j] < arr[j - h]; j -= h)
        {
            arr[j] = arr[j] ^ arr[j - h];
            arr[j - h] = arr[j] ^ arr[j - h];
            arr[j] = arr[j] ^ arr[j - h];
        }
    }
    h /= 3;
}
```
> 使数组中任意间隔为n的元素都是有序的。
> **N很大时可以考虑使用希尔排序**
### 归并
#### 归并操作
```csharp
static int[] aux;
static void merge(int[] a, int lo, int mid, int hi)
{
    int le = lo, ri = mid + 1;
    for (int i = lo; i <= hi; i++)
    {
        aux[i] = a[i];
    }    
    for (int i = lo; i <= hi; i++)
    {
        if (le > mid) a[i] = aux[ri++];
        else if (ri > hi) a[i] = aux[le++];
        else if (aux[ri] < aux[le]) a[i] = aux[ri++];
        else a[i] = aux[le++];
    }
}
```
> 将一个数组分成若干个有序数组，再对他们进行合并。
#### 自上向下归并
```csharp
public static void Sort(int[] a)
{
    var l = a.Length;
    if (l < 2) return;
    aux = new int[l];
    Sort(a, 0, l - 1);
}
private static void Sort(int[] a, int lo, int hi)
{
    if (hi <= lo) return;
    var mid = (lo + hi) / 2;
    Sort(a, lo, mid);
    Sort(a, mid + 1, hi);
    merge(a, lo, mid, hi);
}
```
#### 自下向上归并
```csharp
var n = a.Length;
aux = new int[n];
for (int i = 1; i < n; i *= 2)
{
    for (int j = 0; j < n - i; j += i * 2)
    {
        merge(a, j, j + i - 1, Math.Min(j + i + i - 1, n - 1));
    }
}
```
> 当n%2==0时，自下向上和自上向下的比较次数和访问次数正好相同
> 归并告诉我们，当能用一种方法解决问题时，你都应该试试另一种
### 快速
```csharp
public override void Sort(int[] a)
{
    var n = a.Length;
    Sort(a, 0, n - 1);
}
void Sort(int[] a, int lo, int hi)
{
    if (lo >= hi) return;
    //打乱数组
    var j = Partition(a, lo, hi);
    Sort(a, 0, j - 1);
    Sort(a, j + 1, hi);
}
int Partition(int[] a, int lo, int hi)
{
    int le = lo, ri = hi + 1, tmp;
    var nbr = a[lo];
    while (true)
    {
        while (a[++le] < nbr) if (le == hi) break;
        while (a[--ri] > nbr) if (ri == lo) break;
        if (le >= ri) break;
        tmp = a[le];
        a[le] = a[ri];
        a[ri] = tmp;
    }
    tmp = a[ri];
    a[ri] = a[lo];
    a[lo] = tmp;
    return ri;
}
```
### 堆排序

## 查找
### 二分查找
```csharp
var n = arr.Length;
var lo = 0;
var hi = n - 1;
while (lo <= hi)
{
    var mid = lo + (hi - lo) / 2;
    if (nbr > arr[mid]) lo = mid + 1;
    else if (nbr < arr[mid]) hi = mid - 1;
    else return mid;
}
return -1;
```
> 一般情况下二分查找比顺序查找要快的多
### 符号表

### 平衡查找树
### 散列表

## 图
### 无向图
### 有向图
### 最小生成树
### 最短路径

## 字符串
### 字符串排序
### 单词查找树
### 子字符串查找
### 正则表达式
### 数据压缩

## 动态规划