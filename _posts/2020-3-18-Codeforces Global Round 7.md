---
layout: post
title:  "Codeforces Global Round 7 题解"
date:   2020-3-18 11:33:14
categories: CodeForces 题解
tags: CF 思维题 div.2
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
给定长度为n的序列，需要把这个序列分成k个区间，求这些区间内最大值的总和最大是多少，以及满足总和最大的方案数。

* 思路:  
1. 因为需要把这个序列分为k个区间，区间最大值的总和也就是前K大的数的总和。
2. 难点在于求方案数，观察可以发现两个用于求和的数之间的数可以随意分割，那么方案数也就是把这些区间的长度乘起来即为答案。

```c++

vector<ll> v;
ll mod = 998244353;
int main()
{
    IOS;
    ll n, k;
    cin >> n >> k;
    ll sum = 0, ans = 1;
    for (int i = n; i >= 1; --i)
    {
        ll x;
        cin >> x;
        if (i >= (n - k + 1))
            sum += i;
        if (x >= (n - k + 1))
            v.push_back(i);
    }
    sort(begin(v), end(v));
    for (int i = 1; i < v.size(); ++i)
    {
        ans = ((ans % mod) * ((v[i] - v[i - 1]) % mod)) % mod;
    }
    cout << sum << " " << ans;
    return 0;
}

```

---

## D1 - Prefix-Suffix Palindrome (Easy version)

* 题意:  
给定字符串，求出由这个字符串的前后缀构成的最长的回文串

* 思路:  
1. 题目所要求的字符串可以划分为两部分：前后能够匹配的最大字符串 与 中间本身就是回文串的最长子串。
2. 那么先将这个字符串的前后字符依次匹配，先求出能构匹配的前后缀，再将中间的字符串依次取，求出是回文串的最长子串。

```c++

bool IsHuiWen(string s, int l, int r)
{
    while (l <= r && s[l] == s[r])
        l++, r--;
    return l > r;
}

int main()
{
    int t;
    cin >> t;
    while (t--)
    {
        string s;
        cin >> s;
        int l = 0, r = s.size() - 1;
        while (l < r && s[l] == s[r])
            ++l, --r;
        int r2, l2;
        for (r2 = r; r2 >= l; r2--)
            if (IsHuiWen(s, l, r2))
                break;
        for (l2 = l; l2 <= r; l2++)
            if (IsHuiWen(s, l2, r))
                break;
        cout << s.substr(0, l);//输出匹配的前缀
        if (r2 - l > r - l2)//输出中间比较长的那段回文串
            cout << s.substr(l, r2 - l + 1);
        else
            cout << s.substr(l2, r - l2 + 1);
        cout << s.substr(r + 1) << endl;//输出匹配的后缀
    }
    return 0;
}

```

---

## ## D2 - Prefix-Suffix Palindrome (Hard version)

* 题意:  
同上

* 思路:  
*第一次在cf上用了自己整理的模板！！还过了！！激动.jpg*
还是跟上题同样的思路，只不过在将中间的字符串依次取，求出是回文串的最长子串这一部时使用了manacher算法取最长回文串。


```c++

//s1为输入的字符串 s为处理的字符串
//P的值为当前字符为中心的回文串半径+1
//Manacher函数返回值为最长回文串(可删去仅使用Len数组)

string Manacher(string s1)
{
    string s = "#"; //开头存放两个与题目无相关的字符进行初始化
    string v;
    for (int i = 0; i < s1.size(); i++)
        s += s1[i], s += "#";
    vector<int> p(s.size(), 0);                   // p数组进行存储回文串的长度
    int id = 0, mx = 0, maxpoint = 0, maxlen = 0; //mx是回文串到达的最右端 id是mx对应的回文串中点 maxpoint是最长回文串的中点 maxlen是最长回文串的长度
    for (int i = 0; i < s.size(); i++)
    {
        p[i] = mx > i + p[i] ? min(mx - i, p[2 * id - i]) : 1; //这一句是关键点
        while (s[i + p[i]] == s[i - p[i]])
            ++p[i]; // 匹配串的过程
        if (i + p[i] > mx)
            id = i, mx = i + p[i]; //更新右端和中心点
        if (p[i] > maxlen)
            maxlen = p[i], maxpoint = i; //更新最大长度和对应中点
    }
    for (int i = 0; i < s.size(); i++)
        if (p[i] == i + 1)
            maxlen = i;
    return s1.substr(0, maxlen);
    // 根据题目需要可 return maxlen / maxpoint
}

int main()
{
    int T;
    cin >> T;
    while (T--)
    {
        string s1, s2, s3;
        cin >> s1;
        reverse(s1.begin(), s1.end());
        s2 = s1;
        reverse(s1.begin(), s1.end());
        int flag = 1;
        for (int i = 0; i < s1.size(); i++)
        {
            if (s1[i] != s2[i])
            {
                flag = 0;
                s3 = s1.substr(0, i);
                s1.erase(0, i);
                s1.erase(s1.size() - i, i);
                s2.erase(0, i);
                s2.erase(s2.size() - i, i);
                break;
            }
        }
        if (flag)
            cout << s1 << endl;
        else
        {
            string st1 = Manacher(s1);
            string st2 = Manacher(s2);
            cout << s3;
            if (st1.size() > st2.size())
                cout << st1;
            else
                cout << st2;
            reverse(s3.begin(), s3.end());
            cout << s3 << '\n';
        }
    }
}

```

---