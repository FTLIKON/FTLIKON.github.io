---
layout: post
title:  "Kuangbin专题十六 - KMP & 扩展KMP & Manacher模板总结"
date:   2020-1-26 19:20:54
categories: 心得 模板
tags: MyBlog 模板 心得
---

* content
{:toc}

从2019/12-28 - 2020/1/26 学习了kuangbin专题中题量最大的“KMP & 扩展KMP & Manacher”专题。
在此记录下自己痛苦的蜕变和学习获得的关于各类字符串处理模板和心得。





# 学习心得

---

经过这次半个月的自主学习，这确实是和在学校完全不同的感受。
在学校的话经常是天天忙于其他事情，反而想敲代码的欲望非常的强烈，然而回家后因为没有任何强制性的学习要求，自己敲代码的动机反而是变成了“闲的没事干”。这段时间也在和实验室的一些学长做CF的比赛，从最开始的只能A题到现在的能开C题。自己也逐渐从去年蓝桥选拔赛莫名奇妙挂掉的心理阴影走出来了，也算是通过这个契机改变了自己对比赛的看法。有所成长，再接再厉。

![](https://github.com/FTLIKON/FTLIKON.github.io/blob/master/_posts/pic/t16.jpg)

---
# 模板总结
---

> 获取nexts数组

* 获取nexts数组 下标从1开始到l
* l为字符串长度 s为字符串名字

```c++

void getnexts()
{
    int i = 0, j = -1;
    nexts[i] = j;
    while (i < l)
    {
        if (j == -1 || s[i] == s[j])
            nexts[++i] = ++j;
        else
            j = nexts[j];
    }
}

```
---

> KMP匹配核心

* kmp匹配 temp为需要匹配的字符串 s为母串
* 返回值为是否能匹配

```c++

bool kmp()
{
    int i = 0, j = 0;
    while (i < s.size())
    {
        if (j == -1 || temp[j] == s[i])
            ++i, ++j;
        else
            j = nexts[j];
        if (j == temp.size())
            return true;
    }
    return false;
}

```
---

> 最小(最大)表示法

* 返回值为从哪个字符开始的字符串字典序最小(最大)


```c++

int getmin()
{
    int n = strlen(s);
    int i = 0, j = 1, k = 0, t; //表示从i开始k长度和从j开始k长度的字符串相同
    while (i < n && j < n && k < n)
    {
        t = s[(i + k) % n] - s[(j + k) % n]; //t用来计算相对应位置上那个字典序较大
        if (!t)
            k++; //字符相等的情况
        else
        {
            if (t > 0)
                i += k + 1; //i位置大,最大表示法: j += k+1
            else
                j += k + 1; //j位置大,最大表示法: i += k+1
            if (i == j)
                j++;
            k = 0;
        }
    }
    return i > j ? j : i;
}

```

---
> Manacher算法（马拉车算法）

* s1为输入的字符串 s为处理的字符串
* P数组的值为当前字符为中心的回文串半径+1
* Manacher函数返回值为最长回文串(可删去仅使用P数组)

```c++
string Manacher(string s1)
{
    string s = "$#"; //开头存放两个与题目无相关的字符进行初始化
    for (int i = 0; i < s1.size(); i++)
        s += s1[i], s += "#";
    vector<int> p(s.size(), 0);                   // p数组进行存储回文串的长度
    int id = 0, mx = 0, maxpoint = 0, maxlen = 0; 
    //mx是回文串到达的最右端 id是mx对应的回文串中点 maxpoint是最长回文串的中点 maxlen是最长回文串的长度
    for (int i = 1; i < s.size(); i++)
    {
        p[i] = mx > i + p[i] ? min(mx - i, p[2 * id - i]) : 1; //这一句是关键点
        while (s[i + p[i]] == s[i - p[i]])
            ++p[i]; // 匹配串的过程
        if (i + p[i] > mx)
            id = i, mx = i + p[i]; //更新右端和中心点
        if (p[i] > maxlen)
            maxlen = p[i], maxpoint = i; //更新最大长度和对应中点
    }
    return s1.substr((maxpoint - maxlen) / 2, maxlen - 1); //这里范围的是s1中的最长回文串
    // 根据题目需要可 return maxlen / maxpoint
}
```

---
> 最大前后缀匹配

* 在一个由两个字符串连接的字符串中求最大前后缀匹配

```c++

while (nexts[l] > l1 || nexts[l] > l2 && nexts[l] > 0)
    l = nexts[l];

```

---
> 查询字符串

* 关于查询字符串是否存在操作 时间复杂度类KMP

```c++

if (s[k].find(p) == string::npos)

```
