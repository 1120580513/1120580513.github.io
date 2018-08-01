# Algorithms

---

## 排序
### 选择(冒泡)
```csharp
for (int i = 0; i < array.Length; i++)
{
    for (int j = i + 1; j < array.Length; j++)
    {
        if (array[j] < array[i])
        {
            array[i] = array[i] ^ array[j];
            array[j] = array[i] ^ array[j];
            array[i] = array[i] ^ array[j];
        }
    }
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
### 快速
### 优先队列

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
