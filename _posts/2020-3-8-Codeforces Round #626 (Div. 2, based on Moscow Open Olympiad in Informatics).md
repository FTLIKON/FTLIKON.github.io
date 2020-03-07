---
layout: post
title:  "CodeCraft-20 (Div. 2) 题解"
date:   2020-3-5 14:57:23
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}

又双叒叕掉分了！！！  
好好的div.2硬是变成了手速场（手残患者自闭中）。。。来自atcoder的恐惧.jpg






---

## A - A - Even Subset Sum Problem

* 题意:  
给定n个数的集合，求该集合总和为偶数的非空子集。

* 思路:  
1. 如果改序列有偶数出现，则该偶数可以单独成为大小为1的子集。
2. 如果改序列没有偶数，但是该序列出现了两个奇数，则这两个奇数构成大小为2的子集。
3. 以上情况都不存在，输出-1.

```c++

ll s[1005];
int main()
{
	IOS;
	int T;
	cin >> T;
	while (T--)
	{
		int n;
		cin >> n;
		ll a = 0, b = 0;
		ll t1, t2, t3;
		for (int i = 0; i < n; i++)
		{
			cin >> s[i];
			if (s[i] % 2 == 0)
			{
				a++;
				t1 = i + 1;
			}
			else
			{
				if(b==0)
				t2=i+1;
				else if(b==1)
				t3=i+1;
				b++;
			}
		}
		if (a > 0)
			cout<<"1"<<endl<<t1<<endl;
		else if(a==0&&b>1)
		cout<<"2"<<endl<<t2<<" "<<t3<<endl;
		else
		cout<<"-1"<<endl;
		
	}
	return 0;
}

```

---

## B - Count Subrectangles

* 题意:  
给一个大小为n的a数组，一个大小为m的b数组，两个数组构成矩阵c，规则为c[i][j]=a[i]*b[j]，问面积为k的矩形有几个。

* 思路:  
1. 求a,b的连续为1的子序列，所有存在的a，b连续为1长度的乘积为k即为符合要求的情况，将所有情况求和即为答案。
2. 枚举所有可能纯在的k的两个因数，即为（i , k/i）,再分别在a，b序列中查询（i , k/i）的数量，乘积即为答案。
3. 要注意需要去重，当 i = k/i 时只考虑一种情况。

```c++

ll a[40005];
ll b[40005];
ll n, m, k;
ll sol(ll x, ll y)
{
	ll aa = 0, bb = 0;
	ll t = 0;
	for (int i = 1; i <= n; i++)
	{
		if (a[i])
			t++;
		else
			t = 0;
		if (t >= x)
			aa++;
	}
	t = 0;
	for (int i = 1; i <= m; i++)
	{
		if (b[i])
			t++;
		else
			t = 0;
		if (t >= y)
			bb++;
	}
	return aa * bb;
}

int main()
{
	IOS;
	cin >> n >> m >> k;
	for (int i = 1; i <= n; i++)
		cin >> a[i];
	for (int i = 1; i <= m; i++)
		cin >> b[i];
	ll ans = 0;
	for (ll i = 1; i <= sqrt(k); i++)
	{
		if (k % i == 0)
		{
			ans += sol(i, k / i);
			if (i != k / i)
				ans += sol(k / i, i);
		}
	}
	cout << ans << endl;
	return 0;
}

```

---

## C - Unusual Competitions

* 题意:  
给定一个括号序列，每次操作可以将某一子段进行排序，花费为（长度 / 2）.问至少需要多少花费才能将序列变为只含()的简单序列。

* 思路:  
zn学长nb！！（破音） 我B题还没思路，学长就把C给过了，TQL！  
1. 如果序列长度为奇数，或者左括号和右括号数量不相等，则无法形成简单序列，答案为-1。
2. 那么只需要处理形为 )))((( 的序列即可，如果序列已经为()()则跳过，使用标记遍历序列，最后求和即可。

```c++

int main()
{
	IOS;
	ll n;
	string s;
	cin >> n >> s;
	ll l = 0, r = 0, sum = 0, flag = 0;
	ll zu = 0, yo = 0;
	for (int i = 0; i < n; i++)
	{
		if (s[i] == '(')
		{
			zu++;
			l++;
			if (flag == 0)
				flag = 2;
		}
		if (s[i] == ')')
		{
			yo++;
			r++;
			if (flag == 0)
				flag = 1;
		}
		if (l == r && flag == 1)
		{
			sum += l * 2;
			l = 0;
			r = 0;
			flag = 0;
		}
		if (l == r && flag == 2)
		{
			flag = 0;
			l = 0;
			r = 0;
		}
	}
	if (zu != yo || n % 2 == 0)
		cout << "-1";
	else
		cout << sum;
	return 0;
}

```

---
