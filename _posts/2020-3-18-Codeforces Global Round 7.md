---
layout: post
title:  "Codeforces Global Round 7 题解"
date:   2020-3-18 11:33:14
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}

我又回来更新了！最近cf打的比较少了 练习新算法ing~~





---

## A - Bad Ugly Numbers

* 题意:  
给定一个n，求一个n位数s，满足：s的每一位都不为0，且s不能被组成它的每个数字整除。

* 思路:  
1. 如果n=1，则必被整除，无答案。
2. 如果n>1, 则27777...与23333...都是满足答案的解，均不能除尽。

```c++

int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        int n;
        cin >> n;
        if (n == 1)
            cout << -1 << endl;
        else
        {
            cout << "2";
            for (int i = 2; i <= n; i++)
                cout << "7";
            cout<<endl;
        }
    }
    return 0;
}

```

---

## B - 	Maximums

* 题意:  
给定一个数组a，由非负整数构成，给定数组x，规定xi=max(a1,a2,…,ai−1)，给定数组bi=ai−xi。现已知数组b，要求倒推出a。

* 思路:  
因为bi=ai−xi，那么ai=bi+xi，显然xi就是b1~bi-1取个max，加起来就行。
```c++

ll a[200005];

int main()
{
    IOS;
    int n;
    cin >> n;
    ll x = 0;
    for (int i = 0; i < n; i++)
    {
        cin >> a[i];
        a[i] += x;
        x = max(x, a[i]);
        cout << a[i] << ' ';
    }
}

```

---

## C - Permutation Partitions

* 题意:  


* 思路:  

```c++



```

---

## D1 - Prefix-Suffix Palindrome (Easy version)

* 题意:  


* 思路:  

```c++



```

---

## D2 - Prefix-Suffix Palindrome (Hard version)

* 题意:  


* 思路:  

```c++



```

---