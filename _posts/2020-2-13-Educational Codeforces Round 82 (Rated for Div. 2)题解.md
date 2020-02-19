---
layout: post
title:  "Educational Codeforces Round 82 (Rated for Div. 2) 题解"
date:   2020-2-13 14:48:54
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}

继续补题解 这场当时是做了三个题。







# 题解正文

---

> A - Erasing Zeroes

* 题意:
给定一个只含有01的字符串，请问需要删除多少个0才能构成一个只有1的该字符串的子串

* 思路:
只需要遍历三次，判断第一个“1”与最后一个“1”之间有多少个“0”即可，因为需要删去这之间所有的“0”才能构成只含有“1”的子串。

```c++

int main()
{
	IOS;
	int T;
	cin >> T;
	while (T--)
	{
		string s;
		cin >> s;
		int t1, t2, i, l = s.size();
		for (i = 0; i < l; i++)
		{
			if (s[i] != '0')
				break;
		}
		t1 = i;
 
		for (i = l - 1; i >= 0; i--)
		{
			if (s[i] != '0')
				break;
		}
		t2 = i;
		int ans = 0;
		for (i = t1; i < t2; i++)
		{
			if (s[i] == '0')
				ans++;
		}
		cout<<ans<<endl;
	}
	return 0;
}

```

---

> B - National Project

* 题意:
有长度为n的公路，会循环有a天好天气，b天坏天气，好天气铺出来的是高质量的公路，只需要铺一半高质量的公路即可，问至少需要多少天

* 思路:
需要铺一半，即至少需要铺（n+1）/2，那么只需要考虑取余末端是好天气还是坏天气铺的，如果是坏天气就需要加上等同取余值的好天气的天数。

```c++

void sol()
{
	ll t, n, a, b, ans = 0;
	cin >> n >> a >> b;
	t = (n + 1) / 2; 
	t /= a; //t为需要多少个a天才能铺完一半
	if ((n + 1) / 2 % a == 0) //如果一半的长度/天数能取整
	{
		t--; 
		ans += a; 
	}
	else
	{
		ans += (n + 1) / 2 % a;//加上好天气能铺的
	}
	ans += t * (a + b);//好坏天气循环*t。
	if (ans <= n)
		cout << n << endl;
	else
		cout << ans << endl;
}
 
int main()
{
	IOS;
	int T;
	cin >> T;
	while (T--)
		sol();
	return 0;
}

```

---

> C - Perfect Keyboard

* 题意:
给定一个字符串，作为你想模拟输入的字符串，求键盘布局，要求键盘布局必须满足字符串中字符相邻。

* 思路:

1. 可以贪心解决该问题。我们将使用字符串中已经遇到的字母以及布局上的当前位置来维护键盘的当前布局。

2. 如果字符串的下一个字母已经在布局上，则它必须与当前字母相邻，否则输出“NO”。

3. 如果还没有这样的字母，我们可以将其添加到相邻的自由位置，如果两个字母都被占用，则输出“NO”。

4. 最后，依次添加字符串中还不存在的剩余字母 。

```c++

int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        string s;
        cin >> s;
        vector<bool> used(26);
        used[s[0] - 'a'] = true;
        string t(1, s[0]);
        int pos = 0;
        for (int i = 1; i < s.size(); i++)
        {
            if (used[s[i] - 'a'])
            {
                if (pos > 0 && t[pos - 1] == s[i])
                {
                    pos--;
                }
                else if (pos + 1 < t.size() && t[pos + 1] == s[i])
                {
                    pos++;
                }
                else
                {
                    cout << "NO" << endl;
                    return;
                }
            }
            else
            {
                if (pos == 0)
                {
                    t = s[i] + t;
                }
                else if (pos == t.size() - 1)
                {
                    t += s[i];
                    pos++;
                }
                else
                {
                    cout << "NO" << endl;
                    return;
                }
            }
            used[s[i] - 'a'] = true;
        }

        for (int i = 0; i < int(26); ++i)
        {
            if (!used[i])
                t += char(i + 'a');
        }
        cout << "YES" << endl;
        cout << t << endl;
    }
}

```

---

> D - Fill The Bag

* 题意:
给了一个数字n，然后给了m个数，这m个数都是2的幂次方。现在每次操作你可以把m个数中的一个一分为2，比如32 操作一次得到两个16，在操作一次就是4个8，这样是操作了两次，问最少要操作多少次，可以在操作后把这些数字恰好凑为n。

* 思路:
1. 显然 如果m个数总和＜n 自然就是输出-1

2. 如果可以的话我们考虑二进制，先把这m个数，用cnt[i]存下来 cnt[i]表示二进制从低到高 第i位有多少个数

3. 然后 我们对于n进二进制，如果对于n当前该二进制位i是1.，我们去看cnt[i]是不是有没有，有的话，自然就不用把其他数拆分为两个原来的一半，然后如果该为有剩余，每两个可以凑成一个cnt[i+1]。如果cnt[i]为0，我们就ans++，然后cnt[i+1]–-，因为我们需要实现一个拆分

```c++

ll cnt[70];
int main()
{
    int t;
    cin >> t;
    while (t--)
    {
        memset(cnt, 0, sizeof cnt);
        ll n, m;
        cin >> n >> m;
        ll sum = 0;
        for (int i = 0; i < m; i++)
        {

            int num = 0, x;
            cin >> x;
            sum += x;
            while (x)
                num++, x >>= 1;
            cnt[num - 1]++;
        }
        if (sum < n)
        {
            puts("-1");
            continue;
        }
        else
        {
            int ans = 0;
            for (int i = 0; i <= 63; i++)
            {

                int x = (n >> i) & 1;
                cnt[i] -= x;
                if (cnt[i] >= 2)
                    cnt[i + 1] += cnt[i] / 2;
                if (cnt[i] < 0)
                    ans++, cnt[i + 1]--;
            }
            cout << ans << endl;
        }
    }
    return 0;
}

```