---
layout: post
title:  "Codeforces Round #622 (Div. 2) 题解"
date:   2020-2-24 9:15:12
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}


来自2-23的一场时间非常友好的CF（下午五点）。




# 赛后总结

难度依然是div2的标配，只过了三题勉强上了一丢丢的分（差点掉下蓝名）。不过还没来得及学线段树优化导致不会C2只过了暴力C1。不过估计DE很难，过题居然还没100，我菜鸡不配做。总体上来说还可以接受的一场。

---

## A - Fast Food Restaurant

* 题意:
一个厨师有三种材料做菜，每种材料有a，b，c个，每次做菜至少选一种材料，至多选三种材料，且每种材料只能选一个，求最多的不同方案数。

* 思路:
数据范围较小直接考虑暴力。先sort一下，再枚举所有情况直接贪心即可。

```c++

void sol()
{
int a[3];
        cin >> a[0] >> a[1] >> a[2];
        int ans = 0;
        sort(a,a+3);
        for (int i = 0; i < 3; i++)
        {
            if (a[i])
            {
                ans++;
                a[i]--;
            }
        }
        if (a[1] && a[2])
        {
            ans++;
            a[1]--;
            a[2]--;
        }
        if (a[0] && a[2])
        {
            ans++;
            a[0]--;
            a[2]--;
        }
        if (a[0] && a[1])
        {
            ans++;
            a[0]--;
            a[1]--;
        }
        if (a[0] && a[1] && a[2])
            ans++;
        cout << ans << endl;
}
 
int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
        sol();
    return 0;
}

```

---

## B - Different Rules

* 题意:
有n个人参加比赛，比赛分为两轮，每一轮均不会出现并列名次的情况，每个人最后得分为两轮排名之和，分数越小排名越高（会出现并列的情况）。现已知某人第一、二轮排名，问这个人排名最高最低分别为多少。

* 思路:
1. 先求最低情况，当有x+y个人时，此人排名显然为min(x+y-1,n)。

2. 如果x+y<=n，那么对于其他人，若第一场排名为t，必然能让其在第二场排名为(x+y+1)-t甚至更高，从而使得其最终排名变低，此时A的排名为第一名。

3. 如果x+y>n，那么对于其他人，若第一场排名为t，则让其第二场排名也为t，这样就能使得本身排名靠前的人继续靠前，这样我们就能忽略掉这个人，剩下的人就变成了一个规模小一点的子问题(分治法的思想)，而且剩下的人相对排名也会变小。重复这个过程直到x+y<=n,可知此时A的排名一定为（x+y+1）-n。

```c++

void sol()
{
    ll n, x, y;
    cin >> n >> x >> y;
 
    ll t = (x + y + 1) - n;
    ll minn = min(max(t, (ll)1), n);
    ll maxn = min(x + y - 1, n);
    cout << minn << " " << maxn<<endl;
}
 
int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
        sol();
    return 0;
}

```

---

## C - Skyscrapers (easy version)

* 题意:
给定一个大小为n的序列，可以对序列中的任意一个元素进行减小操作，使该序列满足从任一位置向两侧递减。问怎么处理才能使处理后的该序列总和最大？


* 思路:
1000*1000暴力贪心，以当前数为最大值的情况来维护答案序列，枚举所有的数为最大值的答案情况，每次对总和的最值进行判断即可。

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
