---
layout: post
title:  "Codeforces Round #624 (Div. 3) 题解"
date:   2020-2-26 15:28:25
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}

久违的div3。因为蓝名以上打div3不计分，所以乘机练了个小号美滋滋~~~





---

# 题解正文

---

> A - Add Odd or Subtract Even

* 题意:
输入x，y两个数，每次操作可以使x加任意奇数或减任意偶数，问至少需要操作几次使x y相等。

* 思路:
1. 如果x = y，那么答案就是操作0次。
2. 我们知道奇数+奇数=偶数，奇数+偶数=奇数。那么分情况考虑这个问题。
3. 当y > x时，如果差值为偶数，那么就需要进行a加一次奇数-1，共两次操作；如果差值为奇数，那么直接加一次奇数即可。
4. 当y < x的时候，如果差值为奇数，那么就需要减一个偶数再+1共两次操作；如果差值为偶数，那么直接减一个偶数即可。

```c++

int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        int x, y, c;
        cin >> x >> y;
        if (x == y)
        {
            cout << "0" << endl;
            continue;
        }
        c = y - x;
        if (y > x)
        {
            if (c % 2 == 0)
                cout << "2" << endl;
            else
                cout << "1" << endl;
            continue;
        }
        c *= -1;
        if (c % 2 == 0)
            cout << "1" << endl;
        else
            cout << "2" << endl;
    }
    return 0;
}

```

---

> B - WeirdSort

* 题意:


* 思路:


```c++



```

---

> C - Perform the Combo

* 题意:


* 思路:


```c++



```

---

> D - Three Integers

* 题意:


* 思路:


```c++



```

---

