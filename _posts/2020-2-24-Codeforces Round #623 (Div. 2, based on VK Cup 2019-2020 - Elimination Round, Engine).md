---
layout: post
title:  "Codeforces Round #623 (Div. 2, based on VK Cup 2019-2020 - Elimination Round, Engine) 题解"
date:   2020-2-24 14:21:37
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}

依旧是常规CFround。这场ABC相对比较简单 但是D题难得出奇- -.....





---

# 题解正文

---

![](https://github.com/FTLIKON/FTLIKON.github.io/blob/master/_posts/pic/t1.jpg?raw=true)

> A - Dead Pixel

* 题意:
给定a*b大小的屏幕，在这个屏幕上的（x,y）坐标处有一个坏点，问怎么裁切屏幕使新屏幕没有坏点。

* 思路:
以这个坏点为边界，求上下左右能取的最大矩形面积，求这四个矩形的最大值即可。

```c++

int main()
{
    IOS;
    int T;
    cin>>T;
    while(T--)
    {
        ll a,b,x,y;
        cin>>a>>b>>x>>y;
        ll s1,s2;
        s1=max((a-x-1)*b,x*b);
        s2=max((b-y-1)*a,a*y);
        cout<<max(s1,s2)<<endl;
    }
    return 0;
}

```

---

> B - Homecoming

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

> C - Cow and Message

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
