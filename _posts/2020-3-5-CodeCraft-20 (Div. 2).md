---
layout: post
title:  "CodeCraft-20 (Div. 2) 题解"
date:   2020-3-5 14:57:23
categories: CodeForces 题解
tags: CF 数学 搜索 div.2
---

* content
{:toc}

本周的最后一场cf，学长们都上蓝了~~我也成功的结束了刚上蓝名就连续掉分的诅咒（差点蓝名体验劵）   
话说又鸽了几天博客了（我鸽我自己），要加油补题鸭！






---

## A - Grade Allocation

* 题意:  
给定n个数，想要使第一个数的最大值达到m，问第一个数能达到的最大值。

* 思路:  
对除了第一个数以外的求和，如果总和>m就输出m，否则输出总和.

```c++

int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        ll n, m;
        cin >> n >> m;
        n--;
        ll a;
        cin >> a;
        ll num = 0;
        while (n--)
        {
            ll temp;
            cin >> temp;
            num += temp;
        }
        if (a + num >= m)
            cout << m << endl;
        else
            cout << a + num << endl;
    }
    return 0;
}

```

---

## B - String Modification

* 题意:  
给定长为n的字符串，查找一个数k，使得整个原字符串以k为长度的子串进行翻转，求k取多少的时候得到的反转后的字符串字典序最小。

* 思路:  
1. 找规律。观察k为奇数时即为将前(k-1)个字符移到后面，为偶数使就将前(k-1)个字符反转后移到后面。
2. 那么就只需要枚举1-n，求出字典序最小的k与字符串即可。

```c++

int main()
{
    int T;
    cin >> T;
    while (T--)
    {
        int n;
        string s;
        cin >> n >> s;
        string mn = s;
        int lc = 1;
        for (int i = 0; i < n; i++)
        {
            string x = s.substr(i);
            string l = s.substr(0, i);
            if ((n - i) % 2)
                reverse(l.begin(), l.end());
            x += l;
            if (mn > x)
            {
                lc = i + 1;
                mn = x;
            }
        }
        cout << mn << '\n';
        cout << lc << '\n';
    }
    return 0;
}

```

---

## C - Primitive Primes

* 题意:  
f(x) = a0+a1\*x+  +an\*x^(n-1) , g(x) = b0+b1\*x+  +bn\*x^(n-1)，h(x) = f(x)\*g(x)，问h（x）的哪一项系数模p!=0

* 思路:  
1. 如果数a，b%p!=0，那么a\*b%p = a%p \* b%p ！=0
2. 输入时判断一下是第几项%p不为0即可。

```c++

int main()
{
    IOS;
    ll n, m, p, x;
    cin >> n >> m >> p;
    ll ans1 = -1;
    for (ll i = 1; i <= n; i++)
    {
        cin >> x;
        if (ans1 == -1 && x % p)
            ans1 = i - 1;
    }
    ll ans2 = -1;
    for (ll i = 1; i <= m; i++)
    {
        cin >> x;
        if (ans2 == -1 && x % p)
            ans2 = i - 1;
    }
    cout << ans1 + ans2;
    return 0;
}

```

---

## D - Nash Matrix

* 题意:  
给出n*n的网格，再给出每个点的终点，-1 -1为永不停止的运动，问你是否能构造出这样的地图。

* 思路:  
1. 可知如果一个点的值就是它自己的终点的话，即为X。
2. 那么从每一个X出发跑dfs，把所有相邻的且终点为这个X的点求出来
3. 难点在于处理-1死循环的情况，考虑将所有相邻的-1两两成对的处理，通过判断上下左右的关系将这两个-1连起来。
4. 如果最后这个地图还有还没处理完的地方，就输出INVALID。

```c++

const int M = 1005;
char mat[M][M];
int x[M][M], y[M][M];
int n;

bool connect(int p, int q, int r, int s, char d1, char d2)
{
	if (x[r][s] == -1)
	{
		mat[p][q] = d1;
		if (mat[r][s] == '\0')
			mat[r][s] = d2;
		return 1;
	}
	else
		return 0;
}

void dfs(int p, int q, char d)
{
	if (mat[p][q] != '\0')
		return;
	mat[p][q] = d;

	if (x[p][q] == x[p + 1][q] && y[p][q] == y[p + 1][q])
		dfs(p + 1, q, 'U');
	if (x[p][q] == x[p - 1][q] && y[p][q] == y[p - 1][q])
		dfs(p - 1, q, 'D');
	if (x[p][q] == x[p][q + 1] && y[p][q] == y[p][q + 1])
		dfs(p, q + 1, 'L');
	if (x[p][q] == x[p][q - 1] && y[p][q] == y[p][q - 1])
		dfs(p, q - 1, 'R');
}

int main()
{
	int i, j;
	cin >> n;
	for (i = 1; i <= n; ++i)
		for (j = 1; j <= n; ++j)
			cin >> x[i][j] >> y[i][j];

	for (i = 1; i <= n; ++i)
		for (j = 1; j <= n; ++j)
			if (x[i][j] == -1)
			{
				bool res = (mat[i][j] != '\0');
				if (res == 0)
					res = connect(i, j, i + 1, j, 'D', 'U');
				if (res == 0)
					res = connect(i, j, i, j + 1, 'R', 'L');
				if (res == 0)
					res = connect(i, j, i - 1, j, 'U', 'D');
				if (res == 0)
					res = connect(i, j, i, j - 1, 'L', 'R');
				if (res == 0)
				{
					cout << "INVALID" << endl;
					return 0;
				}
			}
			else if (x[i][j] == i && y[i][j] == j)
				dfs(i, j, 'X');

	for (i = 1; i <= n; ++i)
		for (j = 1; j <= n; ++j)
			if (mat[i][j] == '\0')
			{
				cout << "INVALID" << endl;
				return 0;
			}

	cout << "VALID" << endl;

	for (i = 1; i <= n; ++i)
	{
		for (j = 1; j <= n; ++j)
			cout << mat[i][j];
		cout << endl;
	}

	return 0;
}

```

---