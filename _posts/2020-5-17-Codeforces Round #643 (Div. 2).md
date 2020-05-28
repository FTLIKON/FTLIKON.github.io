---
layout: post
title:  "Codeforces Round #643 (Div. 2) 题解"
date:   2020-5-17 17:14:26
categories: CodeForces 题解
tags: CF div.2 数学 贪心 好题 构造
---

* content
{:toc}

我TM肝爆11122222




---

## A - Sequence with Digits

* 题意:  

a(n+1)=an+an(每一位的最大)*an(每一位的最小)，已知a1, k，求ak。

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

给定A，B，C，D，要求算出三边z,y,z满足 A <= x <= B <= y <= C <= z <= D 的三角形的个数

* 思路:  

*真是好题啊。。奈何自己数学差，，哎*

枚举x+y的值，此时x的数量=y的数量，那么只需要通过x+y来计算x，z的范围即可，详细见代码。

```c++

int main() 
{
    IOS;
    ll A,B,C,D;
    cin>>A>>B>>C>>D;
    ll ans=0;
    for(ll i=max(A+B,C+1);i<=B+C;i++)
    // 根据x+y>z可知x+y的最大范围为[max(A+B,C+1),B+C]
    // 关于C+1: 例如2 3 6 10,此时x=2,y=3,z最小要取6,不合法
    {
        ll l=max(A,i-C), r=min(B,i-B); // x的最大范围[l,r]
        if(l<=r)
        ans+=(r-l+1)*(min(i-1,D)-C+1); 
        // 因为x+y>z ,故z可取的最大值为min(i-1,D)。
    }
    cout<<ans;
    return 0;
}

```

---

## D - Game With Array

* 题意:  

给定 n，s，要求构造长度为n的序列以及k，要求序列总和sum=k或者sum=s-k。

* 思路:  

因为序列的值必须>=1，也就是说需要满足k>=s-(n-1), 也就是k的极限情况。那么只需要构造1 1 1 ... s-(n-1)即可。

```c++

int main()
{

    IOS;
    int n, s;
    cin >> n >> s;
    int t = s - (n - 1);
    if (t <= n)
        cout << "NO" << endl;
    else
    {
        cout << "YES" << endl;
        for (int i = 1; i <= n - 1; i++)
            cout << 1 << " ";
        cout << t << endl;
        cout << n << endl;
    }
    return 0;

}

```

---
