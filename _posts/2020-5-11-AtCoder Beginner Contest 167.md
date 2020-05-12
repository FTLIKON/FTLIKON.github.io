---
layout: post
title:  "AtCoder Beginner Contest 167题解"
date:   2020-5-10 9:48:14
categories: AtCoder 题解
tags: AtCoder 思维题 搜索 好题
---

* content
{:toc}

话说之前打了很多次AtCoder了。。不过好像这是第一次写题解（我承认我是懒狗呜呜呜）




---

## A - Registration	

* 题意:  
给两个仅由小写字母构成的字符串AB，问B是否是在A上添加一个小写字母构成的。

* 思路:  
直接判断即可，保险起见多写了些特判。


```c++

int main()
{
    IOS;
    string s1, s2;
    cin >> s1 >> s2;
    int i;
    for (i = 0; i < s1.size(); i++)
    {
        if (s1[i] != s2[i])
            break;
    }
    if (i == s1.size() && s2.size() - 1 == s1.size() && s2[i] >= 'a' && s2[i] <= 'z')
        cout << "Yes";
    else
        cout << "No";
    return 0;
}

```

---

## B - Easy Linear Programming

* 题意:  
给定放在一堆的、权值为1 0 -1的三种牌，现在从牌堆中取k张牌，求能获得的最大权值总和。

* 思路:  
暴力模拟即可。


```c++

int main() 
{
    IOS;
    ll a[3],k;
    cin>>a[0]>>a[1]>>a[2]>>k;
    int ans=0;
    for(int i=0;i<3;i++)
    {
        if(a[i]<=k)
        {
            ans+=a[i]*(1-i);
            k-=a[i];
        }else
        {
            ans+=k*(1-i);
            k=0;
        }
    }
    cout<<ans;
    return 0;
}

```

---

## C - Skill Up

* 题意:  
给定n本书，每种书都有m种知识，给定书的价值和读书所能获得的知识，问至少需要花费多少才能使获得的m种知识，每一种都大于x

* 思路:  
*dfs实在是不熟练，于是花了快半个小时写了一波bfs。。但是看这个题榜单过的超级快，估计有更简单的做法吧*  
题目给定n，m都小于12，故直接使用bfs枚举所有可能的书本组合，判断最小花费即可。


```c++

struct node
{
    ll c;
    ll a[15];
} s[15];
ll vis[15], ans[15];
ll n, m, x;
struct node2
{
    ll num;
    ll ans[15];
    ll vis[15];
    ll cnt;
};
bool check(node2 top)
{
    for (ll i = 0; i < m; i++)
    {
        if (top.ans[i] < x)
            return false;
    }
    return true;
}
ll minn = 999999999;
void bfs()
{
    queue<node2> q;
    node2 ss;
    ss.num = 0, ss.cnt = 0;
    for (ll i = 0; i < n; i++)
    {
        ss.ans[i] = 0;
        ss.vis[i] = 0;
    }

    q.push(ss);
    node2 top;

    while (!q.empty())
    {
        top = q.front();
        if (check(top))
        {
            minn = min(minn, top.num);
        }
        q.pop();
        for (ll i = top.cnt; i < n; i++)
        {
            if (!top.vis[i])
            {
                node2 temp;
                for (ll j = 0; j < n; j++)
                    temp.vis[j] = top.vis[j];
                temp.vis[i] = 1;
                temp.num = top.num + s[i].c;
                for (ll j = 0; j < m; j++)
                    temp.ans[j] = top.ans[j] + s[i].a[j];
                temp.cnt = i;
                q.push(temp);
            }
        }
    }
}

int main()
{
    IOS;
    cin >> n >> m >> x;
    for (ll i = 0; i < n; i++)
    {
        cin >> s[i].c;
        for (ll j = 0; j < m; j++)
            cin >> s[i].a[j];
    }
    bfs();
    if (minn != 999999999)
        cout << minn;
    else
        cout << -1;
    return 0;
}

```

---

## D - Teleporter

* 题意:  
给定n个城市，和k次操作，一个人从第一个城市开始，每次可以从当前城市传送到当前城市的值所指向的城市，问k次操作后这个人在哪

* 思路:  
*再一次被OI大佬清晰且简短的代码折服。。*  
观察题目发现，因为城市之间可以互相传送，故这个人可能传送中途中陷入到一个环内，那么只需要判断坏的大小和初始位置，将k%(环的大小)即为位置


```c++

const ll N = 2e5 + 5;
int n, a[N], v[N], cnt, f[N];
ll k;

int main()
{
    cin >> n >> k;
    for (int i = 1; i <= n; i++)
        cin >> a[i];
    int x = 1;
    do
    {
        v[x] = 1;
        if (cnt == k)
        {
            cout << x;
            return 0;
        }
        f[cnt++] = x;
        x = a[x];
    } while (!v[x]);
    k -= cnt;
    cnt = 0;
    do
    {
        v[x] = 2;
        f[cnt++] = x;
        x = a[x];
    } while (v[x]!=2);
    cout << f[k % cnt];
    return 0;
}

```

---

## F - Bracket Sequencing

* 题意:  
给n个由 “(” , “)” 构成的字符串，问能否把这些字符串串联起来，构成括号相互匹配的序列。

* 思路:  
*绝世好题！！我最开始还以为要用什么复杂的算法 一看题解 ORZ*  
1. 先将输入的字符串中的已经形成匹配的括号删掉，之后剩余的无非就是三种："(((..."、"...)))"、"...)))(((..."，那么就可以将左右括号映射为单独的数字，从而降低复杂度。
2. 统计这些字符串中左右括号的数量，再将这些字符串按照左右括号的数量进行分类
3. 分类后进行排序，目的是使排序后能够尽可能构成题目要求的序列
4. 排序后即为已经符合题目要求的字符串，判断这个字符串是否符合题意用栈模拟即可（我这里是直接实现了一个类似栈的做法）

```c++

const int N = 1e6 + 5;

string s;

struct str
{
    int l, r;
} a[N], b[N];

bool cmp2(str a,str b)// 使这个序列中不能匹配的‘(’尽可能在前面
{
    return a.l>b.l;
}

bool cmp1(str a,str b)// 使这个序列中不能匹配的‘)’尽可能在前面
{
    return a.r<b.r;
}

int main()
{
    IOS;
    int n;
    cin >> n;
    int at = 0, bt = 0;
    for (int i = 0; i < n; i++)
    {
        cin >> s;
        int ln = 0, rn = 0;
        for (int j = 0; s[j]; j++)
        {
            if (s[j] == '(')
                ln++;
            else if (s[j] == ')'&&ln>0)
                ln--;
            else
                rn++;
        }
        if (ln >= rn)
            a[at++] = (str){ln, rn};
        else
            b[bt++] = (str){ln, rn};
    }
    sort(a,a+at,cmp1);
    sort(b,b+bt,cmp2);
    int suml=0;
    for(int i=0;i<at;i++)//判断a+b这个长字符串是否题意，类似栈。
    {
        if(suml<a[i].r)
        {
            cout<<"No";
            return 0;
        }
        suml-=a[i].r;
        suml+=a[i].l;
    }
    for(int i=0;i<bt;i++)
    {
        if(suml<b[i].r)
        {
            cout<<"No";
            return 0;
        }
        suml-=b[i].r;
        suml+=b[i].l;
    }
    if(suml==0)
    cout<<"Yes";
    else
    cout<<"No";
    return 0;
}

```

---
