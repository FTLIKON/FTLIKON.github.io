---
layout: post
title:  "CodeCraft-20 (Div. 2) 题解"
date:   2020-3-5 14:57:23
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}

本周的最后一场cf，学长们都上蓝了~~我也成功的结束了刚上蓝名就连续掉分的诅咒（差点蓝名体验劵）   
话说又鸽了几天博客了（我鸽我自己），要加油补题鸭！






---

## A - Grade Allocation

* 题意:  
给定n个数，想要使第一个数的最大值达到m，问第一个数能达到的最大值。

* 思路:  
对除了第一个数以外的求和，如果总和>m就输出m，否则输出总和.

```c++

int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        ll n, m;
        cin >> n >> m;
        n--;
        ll a;
        cin >> a;
        ll num = 0;
        while (n--)
        {
            ll temp;
            cin >> temp;
            num += temp;
        }
        if (a + num >= m)
            cout << m << endl;
        else
            cout << a + num << endl;
    }
    return 0;
}

```

---

## B - String Modification

* 题意:  
给定长为n的字符串，查找一个数k，使得整个原字符串以k为长度的子串进行翻转，求k取多少的时候得到的反转后的字符串字典序最小。

* 思路:  
1. 找规律。观察k为奇数时即为将前(k-1)个字符移到后面，为偶数使就将前(k-1)个字符反转后移到后面。
2. 那么就只需要枚举1-n，求出字典序最小的k与字符串即可。

```c++

int main()
{
    int T;
    cin >> T;
    while (T--)
    {
        int n;
        string s;
        cin >> n >> s;
        string mn = s;
        int lc = 1;
        for (int i = 0; i < n; i++)
        {
            string x = s.substr(i);
            string l = s.substr(0, i);
            if ((n - i) % 2)
                reverse(l.begin(), l.end());
            x += l;
            if (mn > x)
            {
                lc = i + 1;
                mn = x;
            }
        }
        cout << mn << '\n';
        cout << lc << '\n';
    }
    return 0;
}

```

---

## C - Primitive Primes

* 题意:  
f(x) = a0+a1\*x+  +an\*x^(n-1) , g(x) = b0+b1\*x+  +bn\*x^(n-1)，h(x) = f(x)\*g(x)，问h（x）的哪一项系数模p!=0

* 思路:  
1. 如果数a，b%p!=0，那么a\*b%p = a%p \* b%p ！=0
2. 输入时判断一下是第几项%p不为0即可。

```c++

int main()
{
    IOS;
    ll n, m, p, x;
    cin >> n >> m >> p;
    ll ans1 = -1;
    for (ll i = 1; i <= n; i++)
    {
        cin >> x;
        if (ans1 == -1 && x % p)
            ans1 = i - 1;
    }
    ll ans2 = -1;
    for (ll i = 1; i <= m; i++)
    {
        cin >> x;
        if (ans2 == -1 && x % p)
            ans2 = i - 1;
    }
    cout << ans1 + ans2;
    return 0;
}

```

---