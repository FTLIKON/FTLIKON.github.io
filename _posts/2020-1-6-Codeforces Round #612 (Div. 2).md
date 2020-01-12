---
layout: post
title:  "Codeforces Round #612 (Div. 2)题解"
date:   2020-1-6 9:20:54
categories: CodeForces 题解
tags: MyBlog CF 
---

* content
{:toc}

一场深夜和学长们连麦的CF 虽然依旧还是这么菜 
但是和学长连麦打CF的快乐还是很爽哎嘿嘿

---

# 赛后总结

---

只做了一道签到题...我还是太菜了。A题就做了快40分钟，B题做的时候理解题意就花了快20分钟，C题直接爆炸，完全没看懂。B题当时和学长们讨论了一下 没太听懂学长们的意思。第二天早上起来看大佬们写的代码，我人傻了。。。

---
---

> A - Angry Students

* 题意:
一排小人站一起互相扔雪球 每次扔给右边一名小人并使下一名小人扔雪球 扔到不能再扔要花多少时间
* 思路:
处理小人能传播到的小人数量的最大值,暴力遍历即可

```c++

#include <iostream>
#include <cstdio>
#include <algorithm>
#include <vector>
#include <queue>
#include <stack>
#include <map>
#include <stdlib.h>
#include <math.h>
#include <string.h>
// *start on @date: 2020-01-05 19:21
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
 
int minn = 999999;
 
int main()
{
    int T;
    cin >> T;
    while (T--)
    {
        int maxn = -999999;
        int n;
        cin >> n;
        char s[n];
        getchar();
        scanf("%s", s);
        int flag=0;
        for (int i = 0; i < n; i++)
        {
            int ans = 0;
            if (s[i] == 'A')
            {
                flag=1;
                if (s[i + 1] == 'A')
                    i++;
                for (int j = i + 1; j < n; j++)
                {
 
                    if (s[j] == 'A')
                        break;
                    ans++;
                }
                if (ans > maxn)
                    maxn = ans;
            }
        }
        if(flag==0)
        cout<<"0"<<endl;
        else
        cout << maxn << endl;
    }
    return 0;
}

```
---

>　B - Hyperset

* 题意:三个字符串中,同一位置的字符都相同或者都不同,为一个"三元组".现在给定n个字符串,问能组成多少个三元组?

* 思路:根据大佬的代码,我的分析如下:

1. 有题义可知时间复杂度被限定在了O(n^2),那么需要降低时间复杂度,暴力不可取.
2. 我们需要用一个 map<string,int> 将已经出现的字符串给储存起来便于查询 
3. 降低时间复杂度的关键在于: 构建一个新的字符串满足已经出现的第一与第二字符串,
查看该字符串在map中是否存在
4. 注意关键是不能有重复的结果 所以需要通过构建第三个字符串来查询.


```c++

#include <iostream>
#include <cstdio>
#include <map>
using namespace std;
const int maxn = 1505;
string x[maxn];
int n, k;
int main()
{
    cin >> n >> k;
    for (int i = 0; i < n; i++)
        cin >> x[i];

    int ans = 0;
    std::map<string, int> cnt;
    for (int i = 0; i < n; i++)
    {
        for (int j = i + 1; j < n; j++)
        {
            string temp;
            for (int z = 0; z < k; z++)
            {
                if (x[i][z] == x[j][z])
                {
                    temp += x[j][z];
                }
                else
                {
                    if (x[i][z] != 'S' && x[j][z] != 'S')
                        temp += 'S';
                    else if (x[i][z] != 'E' && x[j][z] != 'E')
                        temp += 'E';
                    else
                        temp += 'T';
                }
            }
            ans += cnt[temp];
        }
        cnt[x[i]]++;//如果不这样而提前赋值的话会出现重复的情况
    }
    cout << ans << endl;
    return 0;
}

```


