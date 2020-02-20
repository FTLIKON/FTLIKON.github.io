---
layout: post
title:  "Codeforces Round #620 (Div. 2) 题解"
date:   2020-2-16 14:53:21
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}

2020-2-16 凌晨2:53 终于上蓝了啊啊啊啊啊啊啊啊！！！






# 赛后总结

---

终于！！我上蓝名了！！！第一次成功做了四题出来，算是圆满了寒假计划。 继续加油。

---

# 题解正文

---

> A - Two Rabbits

* 题意:
有两只兔子，它们的起始坐标是x，y，它们可以每次同时面向跳a，b的距离，问它们能否相遇。

* 思路:
水题。只需要判断两只兔子的距离/（a+b）能否整除即可。

```c++

int main()
{
	IOS;
	int T;
	cin >> T;
	while (T--)
	{
		ll x, y, a, b;
		cin >> x >> y >> a >> b;
		ll ans = (y - x) / (a + b);
		if ((y - x) % (a + b) != 0 || ans < 0)
		{
			cout << -1 << endl;
		}
		else
			cout << ans << endl;
	}
	return 0;
}

```

---

> B - Longest Palindrome

* 题意:
输入m个长度为n的字符串，问这些字符串能组成的最长回文串有多长。

* 思路:
1. 贪心的思想，我们只需要用当前字符串以及寻找该字符串的反向串是否存在，如果存在的话，就把该字符串与它的反向串添加进答案串的首尾。
2. 注意中间的那个字符串，如果我们输入的字符串中有回文串，且该串的反向串也不存在的话，可以把该串单独放在答案串的中间。
3. g++14有毒！！！本地调试能过结果交上去WA1.
```c++

bool judgeStr(const string &str)
{
	int len = str.size(), i, j;
	for (i = 0, j = len - 1; i <= len / 2; i++, j--)
	{
		if (str[i] != str[j])
			return 0;
	}
	return 1;
}
 
map<string, int> p;
 
string s[105];
 
int main()
{
	IOS;
	int n, m;
	cin >> n >> m;
	for (int i = 0; i < n; i++)
	{
		cin >> s[i];
 
		p[s[i]] = 1;
	}
	string ans, z, t;
	int flag = 0;
	for (int i = 0; i < n; i++)
	{
		if (judgeStr(s[i]))
		{
			z = s[i];
			flag = 1;
		}
		else
		{
			reverse(s[i].begin(), s[i].end());
			string h = s[i];
			reverse(s[i].begin(), s[i].end());
			if (p[h] == 1)
			{
				t += h;
				p[h]=0;
			}
		}
		p[s[i]]=0;
	}
	ans += t;
	reverse(t.begin(), t.end());
	if (flag)
		ans += z;
	ans += t;
	cout<<ans.size()<<endl;
	if(ans.size())
	cout << ans;
	return 0;
}

```

---

> C - Air Conditioner

* 题意:
输入n，再输入n个顾客的信息，分别为客人来的时间，客人需要的温度范围，每分钟可以进行一次三种操作之一：升温，降温和不变。问能否满足所有客人的需求。

* 思路:
暴力模拟题，要满足所有客人的需求，那么就用当前客人与下一个客人的时间差来取温度的最大（r）和最小值（l），当下一个客人来时再判断这个客人的需求是否在这个[l，r]之内。


```c++

struct node
{
	ll t, x, y;
} a[1005];
 
ll n, m;
void sol()
{
	ll cnt = 1, flag = 0;
	for (int i = 2; i <= n; i++)
	{
		if (a[i].t == a[cnt].t)
		{
			if (a[cnt].y < a[i].x || a[i].y < a[cnt].x)
			{
				flag = 1;
				break;
			}
			a[cnt].x = max(a[i].x, a[cnt].x);
			a[cnt].y = min(a[i].y, a[cnt].y);
		}
		else
			a[++cnt] = a[i];
	}
	if (flag)
	{
		cout << "NO" << endl;
		return;
	}
	flag = 0;
	ll l = m, r = m, t = 0;
	for (int i = 1; i <= cnt; i++)
	{
		ll ti = a[i].t - t;
		l -= ti;
		r += ti;
		if (r < a[i].x || a[i].y < l)
		{
			flag = 1;
			break;
		}
		l = max(l, a[i].x);
		r = min(r, a[i].y);
		t = a[i].t;
	}
	if (flag)
		cout << "NO" << endl;
	else
		cout << "YES" << endl;
}
 
int main()
{
	IOS;
	int T;
	cin >> T;
	while (T--)
	{
		cin >> n >> m;
		for (int i = 1; i <= n; i++)
			cin >> a[i].t >> a[i].x >> a[i].y;
		sol();
	}
	return 0;
}

```

---

> D - Shortest and Longest LIS

* 题意:
给定一个n，与构造规则，要求构造出符合构造规则的LIS最小和最大的串

* 思路:
直接贪心即可，最短序列的话，对于一串大于号，我们把当前未使用过的比较大的数尽可能的放在左边。最长序列就是反过来，尽可能的放未使用过的小的数放在左边即可


```c++

const int maxn = 2e5 + 7;
int n, m, a[maxn], b[maxn];
string s;

int main()
{
    int T;
    cin >> T;
    while (T--)
    {
        cin >> n >> s;
        m = n;
        for (int i = 0; i < n; i++)
        {
            int len = 1;
            while (i < s.size() && s[i] == '<')
            {
                len++;
                i++;
            }
            for (int j = i; j > i - len; j--)
            {
                a[j] = m;
                m--;
            }
        }
        for (int i = 0; i < n; i++)
        {
            cout << a[i] << " ";
        }
        cout << endl;
        m = 1;
        for (int i = 0; i < n; i++)
        {
            int len = 1;
            while (i < s.size() && s[i] == '>')
            {
                len++;
                i++;
            }
            for (int j = i; j > i - len; j--)
            {
                b[j] = m;
                m++;
            }
        }
        for (int i = 0; i < n; i++)
            cout << b[i] << " ";
        cout << endl;
    }
}

```