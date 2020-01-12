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



