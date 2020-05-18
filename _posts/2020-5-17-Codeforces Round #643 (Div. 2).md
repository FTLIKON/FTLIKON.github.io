---
layout: post
title:  "Codeforces Round #643 (Div. 2) 题解"
date:   2020-5-17 17:14:26
categories: CodeForces 题解
tags: CF div.2 数学 贪心 好题
---

* content
{:toc}

我TM肝爆



---

## A - Sequence with Digits

* 题意:  
a(n+1)=an+an(每一位的最大)*an(每一位的最小)，已知a1,k，求ak。

* 思路:  
当an的位数拆出来0的时候，后面an的值会陷入循环，即不用再进行计算，那么判断当前位数是否拆出来0即可

```c++

int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        ll n, k;
        cin >> n >> k;
        k--;
        ll bef,num = 0;
        while (k--)
        {
            ll now = n;
            ll minn = 999, maxn = -1;
            while (now)
            {
                minn = min(now % 10, minn);
                maxn = max(now % 10, maxn);
                now /= 10;
            }
            if(minn*maxn==0)
            break;
            n+=minn*maxn;
        }
        cout<<n<<endl;
    }
    return 0;
}

```

---

## B - Young Explorers

* 题意:  
给定一些值，要将这些值分组，要求每个组内的最大值必须小于该组大小，求最多分组数量。

* 思路:  
先将这些值排序，每次贪心的取花费最少数量元素，使这些元素就可以构成题目要求的组。  

solve 1:  

每到一个数先++cnt，如果这时cnt==当前的a[i]说明能凑成一组了，直接ans++，同时cnt=0置零。  

贪心策略就是用尽可能少的数凑出来一组,因此要从小到大遍历。

```c++

int n,a[200005];
int main()
{
    int t;
    cin>>t;
    while(t--)
    {
        cin>>n;
        int i;
        for(i=1;i<=n;i++)scanf("%d",&a[i]);
        sort(a+1,a+n+1);
        int ans=0;
        int cnt=0;
        a[n+1]=0x3f3f3f3f;
        for(i=1;i<=n+1;i++)
        {
            cnt++;
            if(a[i]==cnt)
            {
                ans++;
                cnt=0;
            }
        }
        cout<<ans<<endl;
    }
    
}

```

solve 2:  
直接计算：利用map，当前元素最多可以分的组数即为map[i]/i，如果有多余的元素就传给下一次计算。

```c++

int a[200005];
map<int, int> mp;
 
int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        mp.clear();
        int n;
        cin >> n;
        for (int i = 0; i < n; i++)
        {
            cin >> a[i];
            mp[a[i]]++;
        }
        int ans = 0;
        for (int i = 1; i <= n; i++)
        {
            ans += (mp[i] / i);
            mp[i + 1] += mp[i] % i;
        }
        cout << ans << endl;
    }
    return 0;
}

```

---

## C - Count Triangles

* 题意:  


* 思路:  


```c++



```

---

## D - Game With Array

* 题意:  


* 思路:  


```c++



```

---