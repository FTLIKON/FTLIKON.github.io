---
layout: post
title:  "Codeforces Round #640 (Div. 4) 题解"
date:   2020-5-10 9:48:14
categories: CodeForces 题解
tags: CF 思维题 div.4
---

* content
{:toc}

cf第一场div.4!! 见证历史了
（话说我div4都不能AK实属菜的真实嗷）




---

## A - Sum of Round Numbers

* 题意:  
给一个数字，要求把这个数字按位拆开，例如789=700+80+9

* 思路:  
暴力就完事了嗷，当时写了一波pow发现调不出来，突然回忆起pow有精度问题。。遂直接手写幂运算了  


```c++

int main() 
{
    IOS;
    int T;
    cin>>T;
    while(T--)
    {
        int n;
        cin>>n;
        int t=0;
        int ans=0;
        int a[15];
        while(n)
        {
            int now=n%10;
            if(now!=0)
            {
                int tt=1;
                for(int i=0;i<t;i++)
                tt*=10;
                a[ans++]=now*tt;
            }
            n/=10;
            t++;
        }
        cout<<ans<<endl;
        for(int i=0;i<ans;i++)
        cout<<a[i]<<" ";
        cout<<endl;
    }
    return 0;
}

```

---

## B - Same Parity Summands

* 题意:  
要求构造出k个正整数，这k个数总和为n，且这k个数 都为奇数 或者 都为偶数。

* 思路:  
事实证明两个人开一个题=WA++。。。  
这个题给清了这k个数都为正整数，故k为奇数的情况就是1，1，1，1...尾数；k为偶数的情况就是2,2,2,...尾数，判断尾数是否为奇/偶数即可，不满足输出-1.

```c++

int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        int n, k;
        cin >> n >> k;
        if ((n - (k - 1)) % 2 == 1 && n >= k&&(n - (k - 1))>0)
        {
            cout<<"YES"<<endl;
            for (int i = 0; i < k - 1; i++)
                cout << 1 << " ";
            cout << (n - (k - 1)) << endl;
        }
 
        else if ((n - (k - 1)*2) % 2 == 0 && n >= k&&(n - (k - 1)*2)>0)
        {
            cout<<"YES"<<endl;
            for (int i = 0; i < k - 1; i++)
                cout << 2 << " ";
            cout << (n - (k - 1)*2) << endl;
        }
        else
            cout << "NO" << endl;
    }
    return 0;
}

```

---

## C - K-th Not Divisible by n

* 题意:  
有一自然数序列（1，2，3，4....），求删除能被n整除的数后的第k个数。

* 思路:  
要删除能被n整除的数，即为每个原大小为n的区间删除数后长度变为n-1。那么只要求出来小于k的有多少个这样的区间（k/(n-1)）即为小于k的数删除了多少个，最后加上原长度k即可。

```c++

int main() 
{
    IOS;
    int T;
    cin>>T;
    while(T--)
    {
        int n,k,ans;
        cin>>n>>k;
        ans=k+k/(n-1);
        if(k%(n-1)==0)
        ans--;
        cout<<ans<<'\n';
    }
    return 0;
}

```

---

## D - Alice, Bob and Candies

* 题意:  
A从左边取物，B从右边取物。要求当前人取物总和尽可能小，但至少大于另一个人上一次取物总和。最后不够时，最后一个人全部取完，游戏结束。求取物次数以及A,B的取物总和。

* 思路:  
暴力模拟即可。。但是我最开始用vector+迭代器调了半个小时没出来。。最后5分钟才调出来，1A还可以接受

```c++

int a[1005], v[1005];
int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        vector<int> a;
        int n;
        cin >> n;
        for (int i = 0; i < n; i++)
        {
            int ttt;
            cin >> ttt;
            a.push_back(ttt);
        }
        int ans = 1;
        int nowa = 0, nowb = 0, aa = 0, bb = 0;
        while (a.size())
        {
            int l = a.size(), num = 0;
            if (ans & 1)
            {
                for (int i = 0; i < l; i++)
                {
                    num += a[0];
                    a.erase(a.begin(), a.begin() + 1);
                    if (num > nowb || a.size() == 0)
                    {
                        aa += num;
                        break;
                    }
                }
                nowa = num;
            }
            else
            {
                for (int i = l - 1; i >= 0; i--)
                {
                    num += a[a.size()-1];
                    a.pop_back();
                    if (num > nowa || a.size() == 0)
                    {
                        bb += num;
                        break;
                    }
                }
                nowb = num;
            }
            ans++;
        }
        cout << ans-1 << " " << aa << " " << bb << endl;
    }
    return 0;
}


```

---

## E - Special Elements

* 题意:  
如果数组里的某个数等于该数组某一个区间内的所有数字之和，那么该数字即为特殊点。求整个数组的特殊点数量。

* 思路:  
1. 为了降低复杂度，查询区间和需要使用前缀和。
2. 思路一：用map存入所有区间的区间和（需要剪枝，大于n的点不存），最后遍历数组查询该值是否存在于map中，统计数量。
3. 思路二：存入所有元素（map和数组都可），最后查询每个区间是否有对于存在的元素。

* solve1：

```c++

const int N = 8e3 + 5;
int sum[N], a[N];
int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        unordered_map<int, int> up;
        int n;
        cin >> n;
        for (int i = 1; i <= n; i++)
        {
            cin >> a[i];
            sum[i] = sum[i - 1] + a[i];
        }
        for (int i = 1; i <= n; i++)
        {
            for (int j = i + 1; j <= n; j++)
            {
                int cnt = sum[j] - sum[i - 1];
                if (cnt > n)//必要的剪枝！！不然会MLE
                    continue;
                up[cnt] = 1;
            }
        }
        int ans = 0;
        for (int i = 1; i <= n; i++)
        {
            if (up[a[i]] == 1)
                ans++;
        }
        cout << ans << endl;
    }
    return 0;
}

```

* solve2：

```c++

const int N = 8e3 + 5;
int sum[N], a[N], vis[N];
int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        memset(vis, 0, sizeof(vis));
        int n;
        cin >> n;
        for (int i = 1; i <= n; i++)
        {
            cin >> a[i];
            sum[i] = sum[i - 1] + a[i];
            vis[a[i]]++;
        }
        int ans = 0;
        for (int i = 1; i <= n; i++)
        {
            for (int j = i + 1; j <= n; j++)
            {
                int cnt = sum[j] - sum[i - 1];
                if (cnt > n)
                    continue;
                if (vis[cnt] > 0)
                {
                    ans+=vis[cnt];
                    vis[cnt]=0;
                }
            }
        }
        cout << ans << endl;
    }
    return 0;
}

```


---

## F - Binary String Reconstruction

* 题意:  
要求构造一个二进制串的，使得所有长度为2的子串满足：1的数量分别为0,1,2的个数为n0,n1,n2

* 思路:  
先00后11最后01。注意00,11,01交界处会多出两个n1序列。以及特判n1为0的情况，由于题目保证一定有解，所以不用担心n1=0时n0,n2同时不为0的情况。

```c++


int main()
{
    int t;
    cin >> t;
    while (t--)
    {
        int a, b, c;
        cin >> a >> b >> c;
        if (b == 0)
        {
            if (a)
                for (int i = 0; i < a + 1; i++)
                    cout << 0;
            if (c)
                for (int i = 0; i < c + 1; i++)
                    cout << 1;
            cout << endl;
            continue;
        }
        for (int i = 0; i < a + 1; i++)
            cout << 0;
        for (int i = 0; i < c + 1; i++)
            cout << 1;
        for (int i = 0; i < b - 1; i++)
            cout << i % 2;
        cout << endl;
    }
    return 0;
}

```

---

## G - Special Permutation

* 题意:  
构造n大小，元素为(1~n)的序列,满足任意相邻数字之差的绝对值在区间[2,4]中。

* 思路:  
*zn的思路，一眼就出tql！！*  
以3，1，4，2为基底，奇数补左边，偶数补右边即可。

```c++

int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        vector<int> v;
        int n;
        cin >> n;
        if (n >= 4)
        {
            v.push_back(3);
            v.push_back(1);
            v.push_back(4);
            v.push_back(2);
            for (int i = 5; i <= n; i++)
            {
                if (i & 1)
                    v.insert(v.begin(), i);
                else
                    v.push_back(i);
            }
            for (int i = 0; i < n; i++)
                cout << v[i] << " ";
            cout << endl;
        }
        else cout << "-1" << endl;
    }
    return 0;
}

```

---
