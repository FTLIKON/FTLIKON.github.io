---
layout: post
title:  "Codeforces Round #621 (Div. 1 + Div. 2) 题解"
date:   2020-2-18 14:21:37
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}

终于在一段没有比赛的空窗期把之前欠的博客补完了。爽了。
话说div1+2混合场还是难搞啊，wtcl。





---

## A - Cow and Haybales

* 题意:
有n堆干草排列，每一天可以把后一堆干草移到前一堆，给定m天，求第一堆干草的最大值。

* 思路:
最理想的做法是将干草从当前堆中移到最近的堆中，所以我们正向遍历数组，每一堆需要花s[i]*i天才能移动到第一堆，只需要判断s[i]能否移动完，如果不能移动那么只需要移动剩余天数/i堆

```c++

int s[500];

int main()
{
	IOS;
	int T;
	cin >> T;
	while (T--)
	{
		int n, m;
		cin >> n >> m;
		for (int i = 0; i < n; i++)
			cin >> s[i];

		for (int i = 1; i < n; i++)
		{
			int t = min(s[i], m / i);
            // m/i为最后一次放置的情况
			s[0] += t;
			m -= t * i;
		}
		cout << s[0] << endl;
	}
	return 0;
}

```

---

## B - Cow and Friend

* 题意:
一个兔子想从（0,0）跳到（x，0），每次可以跳的距离必须符合输入的n种情况之一，求兔子至少需要跳多少次才能达到。

* 思路:
1. 如果兔子到终点的距离在输入的情况之内，那么只需要跳一次。
2. 如果没有这种情况，那么求输入的情况的最大值，所以兔子需要跳的次数为max（2,距离/最大值），这是因为答案至少是[距离/最大值]，如果少了的话兔子无法达到终点。

```c++

int s[100005];

map<int, int> p;

int main()
{
	int T;
	cin >> T;
	while (T--)
	{
		p.clear();
		int n, x;
		cin >> n >> x;
		for (int i = 0; i < n; i++)
		{
			cin >> s[i];
			p[s[i]] = 1;
		}
		sort(s,s+n);
		if (p[x])
			cout << "1" << endl;
		else
			cout << max(2, ((x + s[n - 1] - 1) / (s[n - 1]))) << endl;
	}
}

```

---

## C - Cow and Message

* 题意:
给你一个字符串s，求其中下标间隔为等差数列的子串最多有多少。


* 思路:
1. 显然该子串只有可能是长度为1或者2的时候子串的数量能尽可能的多。
2. 因为能够成长度为的2以上的子串一定可以先构成长度为2的子串，显然还多了很多限制。
3. 所以我们先构造出符合给定字符串的所有子串，统计每一种长度为1和2的子串的数量，再求最大值。


```c++

ll vis[26], cnt[26][26];
int main()
{
	string s;
	cin >> s;
	int l = s.size();
	ll ans = 0;
	for (int i = 0; i < l; ++i)
	{
		for (int j = 0; j < 26; ++j)
		{
			if (vis[j])
				cnt[j][s[i] - 'a'] += vis[j];
		}
		vis[s[i] - 'a']++;
		ans = max(ans, vis[s[i] - 'a']);
	}
	for (int i = 0; i < 26; ++i)
	{
		for (int j = 0; j < 26; ++j)
		{
			ans = max(ans, cnt[i][j]);
		}
	}
	cout << ans;
}

```
