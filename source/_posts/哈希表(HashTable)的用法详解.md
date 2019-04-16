---
title: C#中哈希表(HashTable)的用法详解

thumbnail: http://a4.mzstatic.com/us/r30/Purple3/v4/e5/a5/b8/e5a5b843-5cc9-135f-cbba-9a905597d27e/pr_source.175x175-75.png?downloadKey=1418984844_a21422c0567186cb62e73ebdabf81753
date: 2016/5/27 13:48:25  # 文章发表时间
tags:
- C#
categories: 编程 # 分类
layout: post
lede: ""
featured: true
---

## 1.  哈希表(HashTable)简述

在.NET Framework中，Hashtable是System.Collections命名空间提供的一个容器，用于处理和表现类似keyvalue的键值对，其中key通常可用来快速查找，同时key是区分大小写；value用于存储对应于key的值。Hashtable中keyvalue键值对均为object类型，所以Hashtable可以支持任何类型的keyvalue键值对.

<!-- more -->

## 2.什么情况下使用哈希表

1. 某些数据会被高频率查询
2. 数据量大
3. 查询字段包含字符串类型
4. 数据类型不唯一

## 3.哈希表的使用方法

哈希表需要使用的namespace

```c#
using System.Collections;
using System.Collections.Generic;
```

哈希表的基本操作：

```c#
//添加一个keyvalue键值对：
HashtableObject.Add(key,value);

//移除某个keyvalue键值对：
HashtableObject.Remove(key);

//移除所有元素：           
HashtableObject.Clear(); 

// 判断是否包含特定键key：
HashtableObject.Contains(key);
```

控制台程序例子：

```c#
using System;
using System.Collections; //file使用Hashtable时，必须引入这个命名空间
class Program
{
  public static void Main()
  {
     Hashtable ht = new Hashtable(); //创建一个Hashtable实例
     ht.Add("北京", "帝都"); //添加keyvalue键值对
     ht.Add("上海", "魔都");
     ht.Add("广州", "省会");
     ht.Add("深圳", "特区");

     string capital = (string)ht["北京"];
     Console.WriteLine(ht.Contains("上海")); //判断哈希表是否包含特定键,其返回值为true或false
     ht.Remove("深圳"); //移除一个keyvalue键值对
     ht.Clear(); //移除所有元素
  }
}     
```

哈希表中使用多种数据类型的例子：

```c#
using System;
using System.Collections;

class Program
{
    static Hashtable GetHashtable()
    {
    　　Hashtable hashtable = new Hashtable();
　　　　
    　　hashtable.Add("名字", "小丽");
    　　hashtable.Add("年龄", 22);
    　　return hashtable;
    }

    static void Main()
    {
    　　Hashtable hashtable = GetHashtable();

    　　string name = (string)hashtable["名字"];
    　　Console.WriteLine(name);

    　　int age = (int)hashtable["年龄"];
    　　Console.WriteLine(age);
    }
}
```

当获取哈希表中数据时，如果类型声明的不对，会出现InvalidCastException错误。使用as-statements可以避免该错误。

```c#
using System;
using System.Collections;
using System.IO;

class Program
{
    static void Main()
    {
    Hashtable hashtable = new Hashtable();
    hashtable.Add(100, "西安");

    // 能转换成功
    string value = hashtable[100] as string;
    if (value != null)
    {
        Console.WriteLine(value);
    }

    // 转换失败，获取的值为null，但不会抛出错误。
    StreamReader reader = hashtable[100] as StreamReader;

    if (reader == null)
    {
         Console.WriteLine("西安不是StreamReader型");
    }

    // 也可以直接获取object值，再做判断
    object value2 = hashtable[100];
    if (value2 is string)
    {
        Console.Write("这个是字符串型: ");
        Console.WriteLine(value2);
    }
    }
}
```

## 4.遍历哈希表

遍历哈希表需要用到DictionaryEntry Object，代码如下：

```c#
for(DictionaryEntry de in ht) //ht为一个Hashtable实例
{
   Console.WriteLine(de.Key);  //de.Key对应于keyvalue键值对key
   Console.WriteLine(de.Value);  //de.Key对应于keyvalue键值对value
}
```

遍历键

```c#
foreach (int key in hashtable.Keys)
{
    Console.WriteLine(key);
}
```

遍历值

```c#
foreach (string value in hashtable.Values)
{
    Console.WriteLine(value);
}
```

## 5.对哈希表进行排序

 对哈希表按key值重新排列的做法：

```c#
ArrayList akeys=new ArrayList(ht.Keys); 
akeys.Sort(); //按字母顺序进行排序
foreach(string key in akeys)
{
   Console.WriteLine(key + ": " + ht[key]);  //排序后输出
}
```

## 6.哈希表的效率

System.Collections下的哈希表（Hashtable）和System.Collections.Generic下的字典（Dictionary）都可用作lookup table，下面比较一下二者的执行效率。

```c#
Stopwatch sw = new Stopwatch();
Hashtable hashtable = new Hashtable();
Dictionary<string, int> dictionary = new Dictionary<string, int>();
int countNum = 1000000;

sw.Start();
for (int i = 0; i < countNum; i++)
{
    hashtable.Add(i.ToString(), i);
}
sw.Stop();
Console.WriteLine(sw.ElapsedMilliseconds);  //输出: 744

sw.Restart();
for (int i = 0; i < countNum; i++)
{
    dictionary.Add(i.ToString(), i);
}
sw.Stop();
Console.WriteLine(sw.ElapsedMilliseconds);  //输出: 489

sw.Restart();
for (int i = 0; i < countNum; i++)
{
    hashtable.ContainsKey(i.ToString());
}
sw.Stop();
Console.WriteLine(sw.ElapsedMilliseconds);  //输出: 245

sw.Restart();
for (int i = 0; i < countNum; i++)
{
    dictionary.ContainsKey(i.ToString());
}
sw.Stop();
Console.WriteLine(sw.ElapsedMilliseconds);  //输出: 192
```

由此可见，添加数据时Hashtable快。频繁调用数据时Dictionary快。

结论：Dictionary<K,V>是泛型的，当K或V是值类型时，其速度远远超过Hashtable。

