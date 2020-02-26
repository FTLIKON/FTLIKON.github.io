---
layout: post
title:  "Codeforces Round #624 (Div. 3) 题解"
date:   2020-2-26 15:28:25
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}

久违的div3。因为蓝名以上打div3不计分，所以乘机练了个小号美滋滋~~~





---

# 题解正文

---

> A - Add Odd or Subtract Even

* 题意:  
输入x，y两个数，每次操作可以使x加任意奇数或减任意偶数，问至少需要操作几次使x y相等。

* 思路:
1. 如果x = y，那么答案就是操作0次。
2. 我们知道奇数+奇数=偶数，奇数+偶数=奇数。那么分情况考虑这个问题。
3. 当y > x时，如果差值为偶数，那么就需要进行a加一次奇数-1，共两次操作；如果差值为奇数，那么直接加一次奇数即可。
4. 当y < x的时候，如果差值为奇数，那么就需要减一个偶数再+1共两次操作；如果差值为偶数，那么直接减一个偶数即可。

```c++

int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        int x, y, c;
        cin >> x >> y;
        if (x == y)
        {
            cout << "0" << endl;
            continue;
        }
        c = y - x;
        if (y > x)
        {
            if (c % 2 == 0)
                cout << "2" << endl;
            else
                cout << "1" << endl;
            continue;
        }
        c *= -1;
        if (c % 2 == 0)
            cout << "1" << endl;
        else
            cout << "2" << endl;
    }
    return 0;
}

```

---

> B - WeirdSort

* 题意:  
给定一个序列a，和该序列的可排序规则p，仅当pi存在时可以交换a[pi]和a[pi+1].问能否使该序列实现按照从小到大排序。

* 思路:  
这个题最开始想的是模拟冒泡。。。然后一发WA3才发现小看了这个题。  

这个题需要这样来想，当一段连续的pi存在时，就可以直接对这一段a[pi+i~pi+j]进行排序，那么我们只需要对于每一个存在的pi枚举所有可连续最长区间的情况，最后再对这些区间进行排序，最后再对处理完后的a序列遍历看是否符合从小到大排序即可。

```c++

int m, n;
int a[105];
bool vis[105];
int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        memset(vis, 0, sizeof(vis));
        cin >> n >> m;
        for (int i = 1; i <= n; i++)
            cin >> a[i];
        for (int i = 1; i <= m; i++)
        {
            int t;
            cin >> t;
            vis[t] = 1;
        }
        for (int i = 1; i <= n; i++)
        {
            if (!vis[i])
                continue;
            int j = i;
            while (j + 1 <= n && vis[j + 1])
                j++;
            sort(a + i, a + j + 2);
            i = j;
        }
        int flag = 0;
        for (int i = 1; i <= n - 1; i++)
        {
            if (a[i] > a[i + 1])
            {
                flag = 1;
            }
        }
        if (flag)
            cout << "NO" << endl;
        else
            cout << "YES" << endl;
    }
    return 0;
}

```

---

> C - Perform the Combo

* 题意:
给一个长度为n的由小写字母组成的字符串，要把这个字符串取m次，每次的长度给定，问每种字母取过多少次。

* 思路:
应该比A题简单吧，不过题意理解起来是有点吃屎。因为要取m次，那么我们可以知道第一个字符至少要取m次，这时候到下一个字符，如果我们取的长度中有“1”的话，那么就要少取一次第二个字符，依次类推，可知只需要存一下到当前长度时有多少个长度已经不满足即可。

```c++

int nums[500005];
string s;
int ans[100];
int main()
{   
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        int len, m;
        cin >> len >> m;
        cin >> s;
        memset(nums, 0, sizeof(nums));
        memset(ans, 0, sizeof(ans));
        for (int i = 0; i < m; i++)
        {
            int temp;
            cin >> temp;
            nums[0]++;
            nums[temp]--;
        }

        int cnt = 0;
        for (int i = 0; i < len; i++)
        {
            cnt += nums[i];
            ans[s[i] - 'a'] += cnt;
            ans[s[i] - 'a']++;
        }

        for (int i = 0; i < 26; i++)
        {
            cout << ans[i] << " ";
        }
        cout << endl;
    }
    return 0;
}


```

---

> D - Three Integers

* 题意:
输入三个整数a，b，c，可以把a，b，c的值减小，要能够满足a，b，c能互相整除，问怎么处理才能使处理后的a，b，c的总和最大。

* 思路:  
纯暴力，因为要满足a是b的因数，b是c的因数，故可以枚举所有情况，详情见代码。  
时间复杂度：n*log(n)*log(log(n)) ,故不会超时。  
话说赛后好多HACK成功的。。当时看的背后发凉。不过还好我的代码比较稳啊哈哈哈（小膨胀）。

```c++

int main()
{
    IOS;
    int T;
    cin >> T;
    while (T--)
    {
        ll a, b, c, a1, a2, a3;
        ll ans = 99999999;
        cin >> a >> b >> c;
        for (int i = 1; i <= 10000; i++)
        for (ll j = 1; j <= 10000 / i + 1; j++)
        for (int k = 1; k <= 10000 / (i * j) + 2; k++)
            if (ans > abs(i - a) + abs(i * j - b) + abs(i * j * k - c))
            {
                ans = abs(i - a) + abs(i * j - b) + abs(i * j * k - c);
                a1 = i;
                a2 = i * j;
                a3 = i * j * k;
            }
        cout << ans << endl;
        cout << a1 << " " << a2 << " " << a3 << endl;
    }
    return 0;
}

```

---

