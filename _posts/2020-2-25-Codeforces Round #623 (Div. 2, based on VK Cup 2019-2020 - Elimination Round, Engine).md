---
layout: post
title:  "Codeforces Round #623 (Div. 2, based on VK Cup 2019-2020 - Elimination Round, Engine) 题解"
date:   2020-2-25 14:21:37
categories: CodeForces 题解
tags: CF div.2 模拟
---

* content
{:toc}

依旧是常规CFround。这场ABC题相对之前的比较简单 但是D题难得出奇- -.....






---

## A - Dead Pixel

* 题意:
给定a*b大小的屏幕，在这个屏幕上的（x,y）坐标处有一个坏点，问怎么裁切屏幕使新屏幕没有坏点。

* 思路:
以这个坏点为边界，求上下左右能取的最大矩形面积，求这四个矩形的最大值即可。

```c++

int main()
{
    IOS;
    int T;
    cin>>T;
    while(T--)
    {
        ll a,b,x,y;
        cin>>a>>b>>x>>y;
        ll s1,s2;
        s1=max((a-x-1)*b,x*b);
        s2=max((b-y-1)*a,a*y);
        cout<<max(s1,s2)<<endl;
    }
    return 0;
}

```

---

## B - Homecoming

* 题意:
输入一段路程的公交车和电车的排布，一个人需要从第一个站坐公交出行到最后一个站，他有p元钱，坐一次公交车和电车的花费分别是a，b元，问他至少需要走多少步开始坐车才能到达终点。

* 思路:
1. 从后往前遍历，从钱数中依次扣除需要花费的公交车/电车费用，当钱数小于0时输出答案即可。
2. 注意答案至少为1 因为走到第一个站需要1步。

```c++

char s[100005];
int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        int a, b, p, flag = 0;
        cin >> a >> b >> p;
        cin >> s + 1;
        int len = strlen(s + 1);
        for (int i = len - 1; i >= 1; i--)
        {
            int j = i;
            while (s[j] == s[i])
                j--;
            if (s[i] == 'A')
                p -= a;
            else
                p -= b;
            if (p < 0)
            {
                flag = 1;
                cout << i + 1 << endl;
                break;
            }
            i = j;
            i++;
        }
        if (flag == 0)
            cout << 1 << endl;
    }
    return 0;
}

```

---

## C - Restoring Permutation

* 题意:
给定一个大小为n的序列b，求大小为2n的满足bi=min(ai−1,ai)的a序列，要求所有ai必须满足在1~2n之间，若ai不够的话就输出-1。


* 思路:
1. 因为数据范围较小，故该题直接暴力模拟。
2. 枚举1-2n中所有没有数使用过的数，对每一个空判断大小后依次填入，如果1~2n中符合条件的数都填完了就只能输出-1.
3. 话说这个题比B题简单一点吧。。。题意更好理解。

```c++

int a[1005], b[1005], vis[100005];

int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        int flag = 0;
        memset(vis, 0, sizeof(vis));
        int n;
        cin >> n;
        for (int i = 0; i < n; i++)
        {
            cin >> a[i];
            vis[a[i]] = 1;
        }
        for (int i = 0; i < 2 * n; i++)
        {
            if (i % 2 == 0 || i == 0)
            {
                b[i] = a[i / 2];
                continue;
            }
            for (int j = 1; j <= 2 * n + 1; j++)
            {
                if (vis[j] == 0 && j > b[i - 1])
                {
                    b[i] = j;
                    if (j == 2 * n + 1)
                        flag = 1;
                    vis[j] = 1;
                    break;
                }
            }
        }
        if (flag)
            cout << "-1" << endl;
        else
        {
            for (int i = 0; i < 2 * n; i++)
                cout << b[i] << " ";
            cout << endl;
        }
    }
    return 0;
}

```
