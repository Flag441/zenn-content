---
title: "【ABC485 D】ABC|ABC|ABC"
emoji: "🌟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["atcoder","競技プログラミング","cpp"]
published: true
---

## 問題

https://atcoder.jp/contests/abc485/tasks/abc485_d

## コンテスト中に考えたこと

左の区切りの位置は愚直に, それに対して条件を満たす右の区切りの位置を高速で求めることができるかを考えた.

$i$を固定して, 分割した一番左の文字列を$S_0S_1...S_i$とする. これは条件を満たすとする. 真ん中の文字列を $S_iS_{i+1}...S_{j}$ とする. $i+1$文字以降でA,B,Cが最初に出てくるところをそれぞれ $mn_A, mn_B, mn_C$ とすると, 真ん中の文字列が条件を満たすには $j$ は $\max(mn_A,mn_B,mn_C) \le j$ となる必要がある.

また, $S$全体で最後にA,B,Cが出てくるところをそれぞれ $mx_A, mx_B, mx_C$ とすると, 右端の文字列が条件を満たすには $j$ は $j < \min(mx_A,mx_B,mx_C)$ となる必要がある.

逆に, $j$が上の2つを満たすなら$(i,j)$は条件を満たすような分割になる. したがって, 各 $i=0,1,...,N-1$に対して $\min(mx_A,mx_B,mx_C) - \max(mn_A,mn_B,mn_C)$ を, (負になる場合を除いて) 足し合わせればよい.

## 実装

```cpp
#include <bits/stdc++.h>

using namespace std;

using ll = long long;

#define rep(i,n) for (ll i=0;i < (n); i++)

int main()
{
    ll n; cin >> n;
    string s; cin >> s;
    ll R = n-1;
    ll a=0,b=0,c=0;
    while(a==0 or b==0 or c==0)
    {
        if(R==-1)
        {
            cout << 0 << endl;
            return 0;
        }

        if(s[R]=='A') a++;
        else if(s[R]=='B') b++;
        else c++;
        R--;
    }

    R++;

    deque<ll> dqA,dqB,dqC;
    rep(i,n)
    {
        if(s[i]=='A') dqA.push_back(i);
        else if(s[i]=='B') dqB.push_back(i);
        else dqC.push_back(i);
    }

    ll ans=0;
    a=b=c=0;
    rep(i,n)
    {
        if(s[i]=='A') a++;
        else if(s[i]=='B') b++;
        else c++;

        while(!dqA.empty() and dqA.front()<=i) dqA.pop_front();
        while(!dqB.empty() and dqB.front()<=i) dqB.pop_front();
        while(!dqC.empty() and dqC.front()<=i) dqC.pop_front();

        if(dqA.empty() or dqB.empty() or dqC.empty()) break;
        if(a==0 or b==0 or c==0) continue;

        ans += max((ll)0,R-max({dqA.front(),dqB.front(),dqC.front()}));
    }

    cout << ans << endl;
}
```

計算量: $O(N)$
