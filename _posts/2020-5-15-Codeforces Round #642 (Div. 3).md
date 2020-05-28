---
layout: post
title:  "Codeforces Round #642 (Div. 3) 题解"
date:   2020-5-15 10:48:14
categories: CodeForces 题解
tags: CF div.3 数学 贪心 模拟 dp
---

* content
{:toc}

填坑填坑。。又懒了两天...



---

## A - Most Unstable Array

* 题意:  
给定两个整数 n 和 k，构造一个只包含非负整数的序列 a，使得相邻的两个数差的绝对值的和最大。

* 思路:  
日常A题看样例猜结论。当n=1时，那么结果是0； 当n=2时，结果是m。当n>2时，结果是2*m。


```c++

int main() 
{
    IOS;
    int T;
    cin>>T;
    while(T--)
    {
        ll n,m;
        cin>>n>>m;
        if(n==1)
        cout<<0<<endl;
        else if(n==2)
        cout<<m<<endl;
        else
        cout<<m*2<<endl;
    }
    return 0;
}

```

---

## B - Two Arrays And Swaps

* 题意:  
给定两个序列 a 和 b，你最多可以进行 k 次操作，每次交换a,b同一位置的元素，要求进行操作完 a 序列的和最大。

* 思路:  
排序贪心即可，依次交换k个a的最小和b的最大。


```c++

const int N=35;
int a[N],b[N];
 
int main() 
{
    IOS;
    int T;
    cin>>T;
    while(T--)
    {
        int n,k;
        cin>>n>>k;
        for(int i=0;i<n;i++)
        cin>>a[i];
        for(int i=0;i<n;i++)
        cin>>b[i];
        sort(a,a+n);
        sort(b,b+n);
        int ans=0;
        for(int i=0;i<n;i++)
        {
            if(a[i]<b[n-i-1]&&k>0)
            {
                swap(a[i],b[n-i-1]);
                k--;
            }
        }
        for(int i=0;i<n;i++)
        ans+=a[i];
        cout<<ans<<endl;
    }
    return 0;
}

```

---

## C - Board Moves

* 题意:  
有n*n个格子，每个格子上有一个元素，每一次操作可以将一个格子移动到它八个方向的八个格子，问最少需要多少次才能够将所有格子上的元素移到同一个格子，n一定是奇数

* 思路:  
*zn给的公式。。强就完事了*  
首先很明显可以移动到最中间的格子。然后这道题的答案可以全部先预处理出来然后一次作答，因为n=x的答案可以从n=x-2的答案中推得。


```c++

ll a[500010];
int main()
{
    IOS;
    int T;
    cin >> T;
    a[1] = 0;
    for (ll i = 3; i < 500002; i++)
    {
        if (i & 1 == 0)
            continue;
        a[i] = a[i - 2] + (i * i - (i - 2) * (i - 2)) * (i / 2);
    }
    while (T--)
    {
        ll n;
        cin >> n;
        cout << a[n] << endl;
    }
    return 0;
}

```

---

## D - Constructing the Array

* 题意:  
要求构造长度为n的序列，每次操作依次1~n，每次操作填入的位置为左边或者右边的中间位置。
* 思路:  
纯模拟，利用优先队列，长度长的先处理即可。


```c++

const ll N = 5e5 + 5;
 
int a[N];
 
struct node
{
    ll x, y;
    node(ll a, ll b) { x = a, y = b; }
    friend bool operator<(node a, node b)
    {
        if (a.y - a.x != b.y - b.x)
        {
            return a.y - a.x < b.y - b.x;
        }
        return a.x > b.x;
    }
};
 
void solve()
{
    ll n;
    cin >> n;
    priority_queue<node> q;
    node tt(1, n);
    q.push(tt);
    ll cnt = 1;
    while (cnt <= n)
    {
        node now = q.top();
        q.pop();
        ll x = now.x, y = now.y;
        a[(x + y) / 2] = cnt++;
        ll mid = (x + y) / 2;
        if (!(y - x + 1) & 1)
        {
            q.push(node(mid + 1, y));
            q.push(node(x, mid - 1));
        }
        else
        {
            q.push(node(x, mid - 1));
            q.push(node(mid + 1, y));
        }
    }
    for (ll i = 1; i <= n; i++)
        if (i == 1)
            cout << a[i];
        else
            cout << " " << a[i];
    cout << endl;
}
 
int main()
{
    IOS;
    ll T;
    cin >> T;
    while (T--)
    {
 
        solve();
    }
    return 0;
}

```

---

## E - K-periodic Garland

* 题意:  
给一串长度为n的串，里面只有0和1，每次操作可以将某一位的0变成1或者1变成0 ，操作的最后需要串里面两个1之间有k-1个0，问最少需要操作多少次

* 思路:  
1. 对于某一位都只有两种选择，填0或者填1，设  

dp[i][0]:第i位填0，前面都合法，最小需要的操作数  

dp[i][1]:第i位填1，前面都合法，最小需要的操作数

2. 因为本题对于 0001000这样的串也算是合法的，所以对于每一位都可以考虑当前位为1，然后前面全是0

```c++

const int maxn = 1e6 + 7;
int dp[maxn][2];
int sum[maxn];
char s[maxn];
int main()
{
    int T;
    cin >> T;
    int cnt = 0;
    while (T--)
    {
        int n, k;
        cin >> n >> k;
        cin >> (s + 1);
        for (int i = 1; i <= n; i++)
        {
            if (s[i] == '1')
                sum[i] = sum[i - 1] + 1;
            else
                sum[i] = sum[i - 1];
        }

        for (int i = 1; i <= n; i++)
        {
            int las = max(0, i - k);
            //当前位是0，因为000010000是合法的，可以直接继承上一位的最小操作数
            dp[i][0] = min(dp[i - 1][1], dp[i - 1][0]) + (s[i] == '1');
            //当前位是1，可以考虑: 一.将前面K个都变为0，并继承第i-k的操作数
            //                   二.将前面都变为0的操作数（即为第i个时的前缀和）
            dp[i][1] = min(dp[las][1] + sum[i - 1] - sum[las], sum[i - 1]) + (s[i] == '0');
        }
        cout << min(dp[n][0], dp[n][1]) << endl;
    }
}

```

---