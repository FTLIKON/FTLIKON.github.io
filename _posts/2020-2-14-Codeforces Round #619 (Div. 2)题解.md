---
layout: post
title:  "Codeforces Round #619 (Div. 2) 题解"
date:   2020-2-14 15:26:47
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}

又是三题险掉分。。。差一点做出D题 奈何手速跟不上脑子







---

## A - Three Strings

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

## B - Motarack's Birthday

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

## C - Ayoub's function

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

## D - Time to Run

* 题意:
给定一个n*m大小的图，从起点开始走，每一条边只能正反各穿过一次，问有K次机会，怎么走才能走更多的路。

* 思路:
1. 可以发现通过合适的方法，我们可以将所有边都走遍，那么这个方法就是先不停向右再不停向左然后向下直到走到最后一行的最右边，然后开始向上走再向下走再向左走就可以将整张图走完

2. 比较复杂的就是如何记录路径，可以开一个vector里面存放的是pair<int,string>,分别是重复的次数与方向

3. 之后再不停答案存到另一个vector中，然后累计x直到k位置就跳出，最后再输出即可

```c++

int main()
{
    int n, m, k;
    vector<pair<int, string>> V, ans;
    cin >> n >> m >> k;
    for (int i = 0; i < n - 1; i++)
    {
        if (m - 1 != 0)// 先把左右全部跑完，每次跑完一层D一下
        {
            V.push_back({m - 1, "R"});
            V.push_back({m - 1, "L"});
        }
        V.push_back({1, "D"});
    }
    if (m - 1 != 0)// 最后一层只去不回
    {
        V.push_back({m - 1, "R"});
    }
    for (int i = 0; i < m - 1; i++)
    {
        if (n - 1 != 0)// 再把上下全部跑完，每次跑完一层L一下
        {
            V.push_back({n - 1, "U"});
            V.push_back({n - 1, "D"});
        }
        V.push_back({1, "L"});
    }
    if (n - 1 != 0)
    {
        V.push_back({n - 1, "U"});// 回到起点
    }
    for (int i = 0; i < V.size(); i++)// 处理答案
    {
        if (k >= V[i].first)
        {
            k -= V[i].first;
            ans.push_back(V[i]);
        }
        else if (k != 0 && V[i].first > k)
        {
            ans.push_back({k, V[i].second});
            k = 0;
        }
    }
    if (k > 0)
    {
        cout << "NO" << endl;
    }
    else
    {
        cout << "YES" << endl;
        cout << ans.size() << endl;
        for (int i = 0; i < ans.size(); i++)
        {
            cout << ans[i].first << " " << ans[i].second << endl;
        }
    }
}

```