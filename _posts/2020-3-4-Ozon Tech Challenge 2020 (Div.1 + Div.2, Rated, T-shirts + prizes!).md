---
layout: post
title:  "Ozon Tech Challenge 2020 (Div.1 + Div.2, Rated, T-shirts + prizes!) 题解"
date:   2020-3-4 14:22:58
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}

又是掉分白给场....神奇的prize round，有生之年我也想拿一次抽奖周边哈哈哈哈（做梦ing）






---

## A - Kuroni and the Gifts

* 题意:  
给定x，y两种物品各有n个，每个x1,x2,...xn和y1，y2,....yn的价值都不同，现在需要将x，y两两组成n对，要求所有成对的x1+y1，x2+y2，....xn+yn的价值都不同。

* 思路:  
因为所有的xi,yi都不重复，那么只需要将这两个数组排个序依次取即可得到不重复的答案。

```c++

int a[105],b[105];
int main() 
{
    IOS;
    int T;
    cin>>T;
    while(T--)
    {
        int n;
        cin>>n;
        for(int i=0;i<n;i++)
        cin>>a[i];
        for(int i=0;i<n;i++)
        cin>>b[i];
        sort(a,a+n);
        sort(b,b+n);
        for(int i=0;i<n;i++)
        cout<<a[i]<<" ";
        cout<<endl;
        for(int i=0;i<n;i++)
        cout<<b[i]<<" ";
        cout<<endl;
    }
    return 0;
}

```

---

## B - Kuroni and Simple Strings

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

## C - Kuroni and Impossible Calculation

* 题意:  
给定一个序列a,要求计算|a1−a2|⋅|a1−a3|⋅ … ⋅|a1−an|⋅|a2−a3|⋅|a2−a4|… ⋅|a2−an|⋅ … ⋅|a(n−1)−an|

* 思路:  
真没想到混合场的c题这么水。。。本来想着一发暴力过不去就睡觉，结果真就让我过了。。我佛了  
1. 如果输入的数有取余后相同的情况，那么也就是两数之差为0,0的任意积也是0，答案则必为0。
2. 两层循环纯暴力，根据题意模拟。


```c++

ll a[200005];
ll s[10005];
void sol()
{
    IOS;
    ll n, mod;
    cin >> n >> mod;
    ll flag = 0;
    for (ll i = 1; i <= n; i++)
        cin >> a[i];
    for (ll i = 1; i <= n; i++)
    {
        ++s[a[i] % mod];
        if (s[a[i] % mod] > 1)
        {
            flag = 1;
            break;
        }
    }
    ll ans = 1;
    if (flag)
    {
        cout << "0" << endl;
        return;
    }
    for (ll i = 1; i <= n; i++)
        for (ll j = i + 1; j <= n; j++)
            ans = ans * (abs(a[i] - a[j])) % mod;
    cout << ans << endl;
}
int main()
{
    sol();
    return 0;
}

```

---