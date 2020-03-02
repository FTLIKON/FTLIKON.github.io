---
layout: post
title:  "Codeforces Round #625 (Div. 2, based on Technocup 2020 Final Round) 题解"
date:   2020-3-2 13:58:16
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}

比赛中途网络和编译器两开花，花了起码20分钟再调试。。最后靠手速超快的sc学长给的C题思路勉强过了3题 因为浪费太多时间，最后提交时间太晚最后掉了5分....我太难了呜呜呜






---

## A - Contest for Robots

* 题意:  
已知有两个机器人需要解决n个问题，1代表可以解决，0代表不能解决，下面给出两个机器人能解决问题的列表，可以给这些问题设定不同的价值，求出最小的可以满足一号能赢过2号机器人的价值的最大值。

* 思路:  
1. 因为两个机器人都需要解决这些问题，故可以不考虑两个机器人都能解决/都不能解决某些问题的情况。
2. 统计出两个机器人不同的情况，可知若要使 (1号机器人价值之和 = 2号机器人价值之和)，因为求的是最小值，也就是只能是平均值，故设定的价值为 (情况2 / 情况1)，因为要使 (1号 > 2号),故该结果+1.
3. 若两个机器人解决题目的情况都相同，则输出-1.

```c++

int s1[105];
int s2[105];
int main()
{
    IOS;
    int n;
    cin >> n;
    int a = 0, b = 0;
    for (int i = 0; i < n; i++)
        cin >> s1[i];
    for (int i = 0; i < n; i++)
        cin >> s2[i];
    for (int i = 0; i < n; i++)
        if (s1[i] != s2[i])
        {
            if (s1[i])
                a++;
            if (s2[i])
                b++;
        }
    if (a == 0)
        cout << "-1" << endl;
    else
        cout << b / a + 1 << endl;
    return 0;
}

```

---

## B - Journey Planning

* 题意:  
一个人旅游，给定n个城市的价值，可以从任意城市出发，要求这个人规划路线中经过城市的价值之差=这些城市的下标之差，求出这个人规划路线的最大价值和。

* 思路:  
正难则反，因为需要求出所有城市的价值之差=这些城市的下标之差的城市路线的最大值，那么可以通过求出所有下标之差相等的城市路线，存入一个map中来计算最大价值的路线。

```c++

ll s[200005];
map<ll, ll> m;
int main()
{
    IOS;
    int n;
    cin >> n;
    for (int i = 0; i < n; i++)
        cin >> s[i];
    ll ans = 0;
    for (int i = 0; i < n; i++)
    {
        m[s[i] - (i + 1)] += s[i];
        ans = max(ans, m[s[i] - (i + 1)]);
    }
    cout << ans <<endl;
    return 0;
}

```

---

## C - Remove Adjacent

* 题意:  
给定一个仅有小写字母的字符串s，当s中有字符满足(s[i] - s[i+1] = 1)或者(s[i] - s[i-1] = 1)时可以删除s[i]，这些操作可以循环进行，问删除后这个字符串最短长度。

* 思路:  
该题是sc学长的思路。因为当时编译器和cf网络双爆炸搞得心态有点裂开，感谢sc学长抬了一手...  
1. 因为这个题数据范围小，直接考虑纯暴力。
2. 因为要使这个字符串最短，那么我们每次需要删除的字符必须为字典序偏大的。
3. 删除字典序较大的，这样才能使剩余的较小字符结合起来，然后循环进行2-3-2操作。
3. 那么死循环遍历这个字符串，删除符合题意的字典序最大的字符即可。

```c++

int main()
{
    IOS;
    int n;
    cin >> n;
    string s;
    cin >> s;
    while (1)
    {
        char maxc = 'a';
        int now = -1;
        for (int i = 0; i < s.size(); i++)
        {
            if ((i - 1 >= 0 && s[i] - s[i - 1] == 1) || (i + 1 < s.size() && s[i] - s[i + 1] == 1))
            {
                if (s[i] > maxc)
                {
                    now = i;
                    maxc = s[i];
                }
            }
        }
        if (now == -1)
            break;
        s.erase(now, 1);
    }
    cout << n - s.size();
    return 0;
}

```

---