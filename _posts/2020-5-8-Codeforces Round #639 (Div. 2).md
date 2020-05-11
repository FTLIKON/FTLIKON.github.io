---
layout: post
title:  "Codeforces Round #639 (Div. 2) 题解"
date:   2020-5-8 18:34:14
categories: CodeForces 题解
tags: CF 思维题 搜索 div.2
---

* content
{:toc}

潜心修(摸)炼(鱼)了一个月，写了一场重现赛
写来写去，还是 cf真香.jpg





---

## A - Puzzle Pieces

* 题意:  
给n\*m个拼图，问这些拼图能不能按照n\*m的图形拼在一起,要注意拼图的形状构成.

* 思路:  
1A. 这个题看得比较快，猜了一波结论秒了。  
观察图形的话就可以发现：只有在拼图块拼成一行时才能无限延伸，或者2\*2大小互相衔接，因为2\*2的话已经将这些缺口用完，所以就是极限情况了。

```c++

int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        int n, m;
        cin >> n >> m;
        if (n == 1 || m == 1)
        {
            cout << "YES" << endl;
        }
        else
        {
            if (n == 2 && m == 2)
                cout << "YES" << endl;
            else
                cout << "NO" << endl;
        }
    }
    return 0;
}

```

---

## B - Card Constructions

* 题意:  
给n个木棒，要搭成金字塔的形状，每次搭的金字塔要求是当前还剩余木棒能搭的最大高度，问最多能搭出来几个金字塔。

* 思路:  
2A。写了一波tle，以为是时间的问题，乱改了一波又交万万没想到居然re了。。。  
纯暴力的思路，直接把1e9+7能搭出来的高度先纯一下（大概26000个），然后暴力的将n带入，每次取大能搭的高度，直到n<=1。


```c++

ll vis[260000];
 
void init()
{
    ll cnt = 2, x = 2;
    ll t = 0;
    while (cnt <= 10000000007)
    {
        vis[t++] = cnt;
        cnt += (x * 2 + x - 1);
        x++;
    }
}
 
int main()
{
    IOS;
    ll T;
    cin >> T;
    init();
    while (T--)
    {
        ll n;
        cin >> n;
        ll flag = 1, ans = 0;
        while (flag)
        {
            for (ll i = 0;; i++)
            {
                if (vis[i] <= n && vis[i + 1] > n)
                {
                    ll t = n / vis[i];
                    n = n % vis[i];
                    ans += t;
                    break;
                }
                if (vis[i] > n)
                {
                    flag = 0;
                    break;
                }
            }
        }
        cout << ans << endl;
    }
    return 0;
}

```

---

## C - Hilbert's Hotel

* 题意:  
现在有很多顾客编号为【0，正无穷】，再给出一个数组An；然后对于每一个顾客k，把它移到 第(k+a[k%n]）的房间,问会不会出现有一个房间存在两个人，

* 思路:  
这题实属阅读理解题嗷，毒瘤死了  
对于非负整数k (0 <= k <= +∞)，对k做 (k+a[k%n]）的变换，判断变换后是否会出现相同的数字；
可以发现只要0 ~ n-1之内的k变换后没有出现相同的数字，那么大于k的数字变换后也不会出现相同的数字；所以只需要对0 ~ n-1的数进行模拟即可；

```c++

const int N = 2e5;
int a[N];
map<int, int> mp;
int main()
{
    int T;
    cin >> T;
    while (T--)
    {
        mp.clear();
        int flag = 1;
        int n;
        cin >> n;
        for (int i = 0; i < n; i++)
            cin >> a[i];
        for (int i = 0; i < n; i++)
        {
            int x = ((i + a[i]) % n + n) % n;
            if (mp[x])
            {
                flag = 0;
                break;
            }
            mp[x]++;
        }
        if (flag)
            cout << "YES" << endl;
        else
            cout << "NO" << endl;
    }
}

```

---

## D - Monopole Magnets

* 题意:  
给你一幅图，‘#’代表黑格，‘.’代表白格，让你在满足以下条件的情况下计算出最少的需要摆放的指北针数，若无解则输出-1

1：每行每列必须至少有一枚指南针

2：指北针能到达所有的黑格

3：指北针不能到达任何一个白格 

如果指北针所在的行和列存在指南针，指北针可以向指南针的方向移动；

* 思路:  
无解情况一：如果每行或每列中的任意两个黑格之间存在白格  
原因如下：
1. 若两个黑格所在的块中都有一枚指南针，那么当指北针在其中一块时，肯定会被另一块的指南针所吸引，从而到达白格，违背了条件3。
2. 若黑格中全都摆放指北针，如果不违背条件1，那么必须在白格摆放指南针，那么指北针还是能到达白格，违背条件3.
3. 若在一个黑块中全都摆放指北针，另一个黑块中摆放指南针，同样会违背条件.  

  无解情况二：有全为白格的行（列）但无全为白格的列（行）  
  
  这种情况下无论在空行白格处任何地方放指南针都会吸引黑色格子里的指北针到达白格  
  
  判断好这两种情况后，只需要dfs求联通块个数即可。


```c++

const int N = 1e3 + 5;
int n, m;
string maze[N];
int vis[N][N];
 
int check1()//查行/列是否存在两个黑格子直接有白格的
{
    int fl, fr, fb;
    for (int i = 0; i < n; i++)
    {
        fl = 0, fr = 0, fb = 0;
        for (int j = 0; j < m; j++)
        {
            if (!fl && maze[i][j] == '#')
                fl = 1;
            else if (!fb && fl && maze[i][j] == '.')
                fb = 1;
            else if (maze[i][j] == '#' && fl && fb)
                return 0;
        }
    }
    for (int i = 0; i < m; i++)
    {
        fl = 0, fr = 0, fb = 0;
        for (int j = 0; j < n; j++)
        {
            if (!fl && maze[j][i] == '#')
                fl = 1;
            else if (!fb && fl && maze[j][i] == '.')
                fb = 1;
            else if (maze[j][i] == '#' && fl && fb)
                return 0;
        }
    }
    return 1;
}
int flag2 = 0, flag3 = 0;//flag2为行 flag3为列
void check2()//查是否存在行/列有全为白格的
{
    int bh = 1, bl = 1;
    for (int i = 0; i < n; i++)
    {
        bh = 1;
        for (int j = 0; j < m; j++)
        {
            if (maze[i][j] == '#')
                bh = 0;
        }
        if (bh)
            flag2 = 1;
    }
    for (int i = 0; i < m; i++)
    {
        bl = 1;
        for (int j = 0; j < n; j++)
        {
            if (maze[j][i] == '#')
                bl = 0;
        }
        if (bl)
            flag3 = 1;
    }
}
int dx[] = {0, 0, 1, -1};
int dy[] = {1, -1, 0, 0};
void dfs(int x, int y)
{
    if (x < 0 || x >= n || y < 0 || y >= m || maze[x][y] == '.' || vis[x][y])
        return;
    vis[x][y] = 1;
    for (int i = 0; i < 4; i++)
    {
        int nx = x + dx[i], ny = y + dy[i];
        dfs(nx, ny);
    }
}
 
int main()
{
    cin >> n >> m;
    for (int i = 0; i < n; i++)
        cin >> maze[i];
    int flag1 = check1();
    check2();
    if (flag1 && flag2 == flag3)//满足两个条件，就查联通块的数量
    {
        int ans = 0;
        for (int i = 0; i < n; i++)
        {
            for (int j = 0; j < m; j++)
            {
                if (maze[i][j] == '#'&&!vis[i][j])
                        dfs(i, j), ans++;
            }
        }
        cout << ans;
    }
    else
        cout << "-1";
    return 0;
}

```

---