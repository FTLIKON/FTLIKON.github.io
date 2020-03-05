---
layout: post
title:  "Ozon Tech Challenge 2020 (Div.1 + Div.2, Rated, T-shirts + prizes!) 题解"
date:   2020-3-4 14:22:58
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}

又是掉分白给场....神奇的prize round，有生之年我也想拿一次抽奖周边哈哈哈哈（做梦ing）






---

## A - Kuroni and the Gifts

* 题意:  
给定x，y两种物品各有n个，每个x1,x2,...xn和y1，y2,....yn的价值都不同，现在需要将x，y两两组成n对，要求所有成对的x1+y1，x2+y2，....xn+yn的价值都不同。

* 思路:  
因为所有的xi,yi都不重复，那么只需要将这两个数组排个序依次取即可得到不重复的答案。

```c++

int a[105],b[105];
int main() 
{
    IOS;
    int T;
    cin>>T;
    while(T--)
    {
        int n;
        cin>>n;
        for(int i=0;i<n;i++)
        cin>>a[i];
        for(int i=0;i<n;i++)
        cin>>b[i];
        sort(a,a+n);
        sort(b,b+n);
        for(int i=0;i<n;i++)
        cout<<a[i]<<" ";
        cout<<endl;
        for(int i=0;i<n;i++)
        cout<<b[i]<<" ";
        cout<<endl;
    }
    return 0;
}

```

---

## B - Kuroni and Simple Strings

* 题意:  
一位小朋友需要一个特定序列括号做生日礼物（这个括号必须是最简单的无法继续操作的括号），最简单的括号指：比如“（）”这种就是最简单无法分割，而：“）（”这种就是不简单的，给了一个长序列的括号串，让你删除其中的子序列达到生日礼物的要求，输出要操作的最小次数，以及所要删的数目，以及要删的括号下标。

* 思路:  
1. 如果一开始给的长序列里面子序列没有一个是简单括号，例如：“）（”这种答案即为0。
2. 这里讲了最小操作数一个坑，然后如果有简单括号序列，那么他的最小操作次数绝对是1（因为你们每次正常操作次数肯定超过或等于1的）
3. 那么只要把每一个左括号和右括号匹配，将其放在一个列表中，说明其是要删掉的，然后当删除后的序列中没有简单括号的时候，就完成了，如果完成，继续寻找匹配括号，将其放在列表中。

```c++

int a[1100],b[1100];//分别记录右括号和左括号的下标 
int ta=0,tb=0;//记录右括号和左括号的数目 
int c1[1100];//储存匹配的括号 
int main()
{
	string s;
	cin >> s;//得到长序列 
	for(int i=0;i<s.length();i++)
	{
		if(s[i]=='(')//记录 右括号下标 
		{
			a[ta++]=i; 
		}
		else//记录左括号下标 
		{
			b[tb++]=i;
		}
	}
	int la=tb-1,lb=0,t=0;//t为列表中所匹配的数目 
	while(la>=0 && lb<ta && a[lb]<b[la])//匹配括号字符串 
	{
		 c1[t++]=a[lb++];
		 c1[t++]=b[la--];		
	}
	if(t)//如果列表中无匹配元素，说明长序列都是不简单括号 
	{
		
        cout<<"1"<<endl<<t<<endl;
		for(int i=0;i<t;i++)//输出所要删除的下标，这里的下标从0开始，所以要加1 
		{
			cout<<c1[i]+1<<" ";
		}
	}
	else
	cout<<"0"<<endl;
}

```

---

## C - Kuroni and Impossible Calculation

* 题意:  
给定一个序列a,要求计算|a1−a2|⋅|a1−a3|⋅ … ⋅|a1−an|⋅|a2−a3|⋅|a2−a4|… ⋅|a2−an|⋅ … ⋅|a(n−1)−an|

* 思路:  
真没想到混合场的c题这么水。。。本来想着一发暴力过不去就睡觉，结果真就让我过了。。我佛了  
1. 如果输入的数有取余后相同的情况，那么也就是两数之差为0,0的任意积也是0，答案则必为0。
2. 两层循环纯暴力，根据题意模拟。


```c++

ll a[200005];
ll s[10005];
void sol()
{
    IOS;
    ll n, mod;
    cin >> n >> mod;
    ll flag = 0;
    for (ll i = 1; i <= n; i++)
        cin >> a[i];
    for (ll i = 1; i <= n; i++)
    {
        ++s[a[i] % mod];
        if (s[a[i] % mod] > 1)
        {
            flag = 1;
            break;
        }
    }
    ll ans = 1;
    if (flag)
    {
        cout << "0" << endl;
        return;
    }
    for (ll i = 1; i <= n; i++)
        for (ll j = i + 1; j <= n; j++)
            ans = ans * (abs(a[i] - a[j])) % mod;
    cout << ans << endl;
}
int main()
{
    sol();
    return 0;
}

```

---