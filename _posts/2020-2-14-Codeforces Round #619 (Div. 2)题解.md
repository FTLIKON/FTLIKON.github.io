---
layout: post
title:  "Codeforces Round #618 (Div. 2) 题解"
date:   2020-2-13 14:48:54
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}

又是三题险掉分。。。差一点做出D题 奈何手速跟不上脑子







# 题解正文

---

> A - Three Strings

* 题意:
给定三个长度相同字符串，A串与B串上的字符可以同时与C串上同位置的字符进行交换，每次交换必须A串、B串同时与C串交换。

* 思路:
如果在每一位上都有A串与C串相同或者B串与C串相同，那么就可以最终使得A串等于B串；其余情况均不可能满足。

```c++

int main()
{
	IOS;
	string a,b,c;
	int T;
	cin>>T;
	while(T--)
	{
		cin>>a;
		cin>>b;
		cin>>c;
		int l=a.size(),flag=1;
		for(int i=0;i<l;i++)
		{
			flag=1;
			if(a[i]==c[i]||b[i]==c[i])
			flag=0;
			else
			break;
		}
		if(flag)
		cout<<"NO"<<endl;
		else
		cout<<"YES"<<endl;
 
	}
	return 0;
}

```

---

> B - Motarack's Birthday

* 题意:
给定一个数组，求一个k，将k替换所有的-1，使得max(ai-ai-1)最大

* 思路:
只要找到贴着-1的最大值和最小值，求其平均值即可，注意边界问题和两个-1在一起的特殊情况

```c++

ll s[100005];
 
void sol()
{
	ll n;
	cin >> n;
	ll maxn = 0,minn = 1e9 + 5;
	for (ll i = 1; i <= n; i++)
		cin >> s[i];
	for (int i = 1; i <= n; i++)
	{
		if (s[i] == -1)
		{
			if (s[i + 1] != -1 && (i + 1) <= n)
			{
				maxn = max(maxn, s[i + 1]);
				minn = min(minn, s[i + 1]);
			}
			if (s[i - 1] != -1 && (i - 1) >= 1)
			{
				maxn = max(maxn, s[i - 1]);
				minn = min(minn, s[i - 1]);
			}
		}
	}
	ll ans = (maxn + minn) / 2;
	ll res = max(abs(ans - maxn), abs(ans - minn));
	for (int i = 1; i < n; i++)
	{
		if (s[i] != -1 && s[i + 1] != -1)
		{
			res = max(res, abs(s[i] - s[i + 1]));
		}
	}
	if (maxn == 0 && minn == 1e9 + 5)
	{
		cout << "0 0" << endl;
		return;
	}
	else
	{
		cout << res << " " << ans << endl;
		return;
	}
}
 
int main()
{
	IOS;
	ll T;
	cin >> T;
	while (T--)
		sol();
}

```

---

> C - Ayoub's function

* 题意:
长度n的01串中有m个1，一个合法子串当且仅当子串里存在至少一个1，让你求出所有满足这样条件的01串中的合法子串最多有多少

* 思路:

正难则反，不要只想 1 的位置该怎么摆放，可以思考一下 0 的位置该怎么放才能使整个字符串只含 0 的子串最少，这里想到的就是所有 0 尽量均分成（m+1）块，这样只含 0 的子串会最少。

```c++
 
void sol()
{
	IOS;
	cin >> n >> m;
	ans = (n + 1) * n / 2, t = n - m;
    // ans为子串的数量 t为0的数量
	a = t / (m + 1), b = t % (m + 1);
    // a为每个区间放多少个0 b表示有多少区间放置（a+1）个0
	ans -= (a * (a + 1) / 2) * (m + 1);
    // 减去只包含0的子串的数量
    ans -= b * (a + 1);
    // 减去多余的单独0的子串
	cout << ans << endl;
	return;
}
 
int main()
{
	IOS;
	ll T;
	cin >> T;
	while (T--)
		sol();
}

```

---

> D - Time to Run

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