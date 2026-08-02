---
date: '2026-08-02T15:27:11+05:30'
draft: false
title: 'Round 1113'
---

## Contest

Contest: [Codeforces Round 1113 (Div. 2)](https://codeforces.com/contest/2248)

## Boilerplate Template

I consistently use this template for writing my Codeforces contests, so expect this to be at the start of every writeup. In almost every problem, I will only be writing the `solve()` function. If I modify the boilerplate to make it easier to solve a particular problem, or add helper functions that I don't usually use, I'll mention those changes in the relevant section.

```cpp
// mizoguchi's boilerplate template
#include <bits/stdc++.h>
using namespace std;

using ll  = long long;
using vi  = vector<int>;
using vll = vector<ll>;
using pii = pair<int,int>;
using pll = pair<ll,ll>;

#define pb push_back
#define ff first
#define ss second
#define all(x) (x).begin(), (x).end()
#define rall(x) (x).rbegin(), (x).rend()
#define sz(x) ((int)(x).size())
#define rep(i,n) for(int i=0;i<(n);i++)
#define rep1(i,a,b) for(int i=(a);i<=(b);i++)
#define per(i,a,b) for(int i=(a);i>=(b);i--)
#define each(x,a) for(auto &x : a)
#define read(v) for(auto &x : v) cin >> x
#define yes cout << "yes\n"
#define no cout << "no\n"
#define nl cout << '\n'

const ll inf = 1e18;
const int mod = 1e9 + 7;

template<typename t>
void print(vector<t>& v){
    for(auto &x : v) cout << x << ' ';
    cout << '\n';
}

void solve() { }

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int t;
    cin >> t;

    while(t--){
        solve();
    }

    return 0;
}
```

## Summary

I only managed to solve A, B, C and D this contest largely because I started late due to personal reasons. Nevertheless, I did lose rating in this lol. It went from 1548 to 1523, who cares. Although I must say the A, B and D were easier than C. Pretty uneventful contest; I will try to practice more.

---

## Problem A

**Problem:** [Problem 2248A](https://codeforces.com/contest/2248/problem/A)

### Explanation

Very straightforward problem that just wants Alice and Bob to play optimally, so we just make sure that Alice removes the first occurrence of `0` and Bob removes the first occurrence of `1`.

```cpp
void solve(){
    string s;
    cin >> s;

    int p0 = s.find('0');
    s.erase(p0, 1);

    int p1 = s.find('1');
    s.erase(p1, 1);

    cout << s << '\n';
}
```

---

## Problem B

**Problem:** [Problem 2248B](https://codeforces.com/contest/2248/problem/B)

### Explanation

Each merge takes 2 elements of a and turns them into 1, so if n < 2m, it's an instant NO. Otherwise, sort both arrays and check each b[i]: it needs at least i+1 smaller elements and m-i larger elements in a to be sandwiched properly. If any b[i] fails this, answer is NO.

```cpp
// ub is #define ub upper_bound 
// lb is #define lb lower_bound
void solve(){
int n;int m;
  cin >> n >> m;
  vector<int> a(n);
  vector<int> b(m);
  read(a);
  read(b);

    sort(a.begin(),a.end());
    sort(b.begin(),b.end());

  if (n<2*m){
    cout << "NO" << '\n';
  return;
  }




  bool ok=true;
rep(i,m){
  int less = lb(all(a),b[i]) - a.begin();
  if (less <i+1){
    ok = false ;
    break;
  }
  int great = n- (ub(all(a),b[i])- a.begin());
      if(great<m-i){
      ok =false;
      break;

      }
}
ok ? yes:no;
}
```

---

## Problem C

**Problem:** [Problem 2248C](https://codeforces.com/contest/2248/problem/C)

### Explanation

Deleting a value's full original range and scoring size^2 is always at least as good as any partial deletion, so the array splits into non-overlapping blocks (each block = one value's full span) plus leftover singletons scoring 1 each. Total score becomes 2n + Σ(size^2 - size) over the chosen blocks — so we just need to pick non-overlapping blocks maximizing that sum. Sort blocks by right endpoint, DP over them: skip or take (skipping to the last block that ends before this one starts, via binary search)

```cpp
//ub is upper_bound
//lb is lower_bound
void solve() {
 int n; 
 cin >> n;
 vector<int> a(2*n);
 read(a);
 vector<int>l(n+1,-1),r(n+1,-1);
 rep(i,2*n){
   int v=a[i];
   if(l[v]==-1) l[v]=i;
   r[v]=i;
 }
 
 vector<array<ll,3>> iv(n);
 rep1(v,1,n){
   ll s=r[v]-l[v]+1;
   ll w=s*s-s;
   iv[v-1]={(ll)r[v],(ll)l[v],w};
 }
 
sort(all(iv),[](const array<ll,3>&x,const array<ll,3>&y){return x[0]<y[0];});
 
vll rarr(n);
rep(i,n) rarr[i]=iv[i][0];
vll dp(n+1,0);
rep1(i,1,n){
  ll r=iv[i-1][0],l=iv[i-1][1], w=iv[i-1][2];
  int j=lb(rarr.begin(),rarr.begin()+(i-1),l)-rarr.begin();
  dp[i]=max(dp[i-1],w+dp[j]);
}
 
cout << 2LL*n+dp[n]<<'\n';
}
```

---

## Problem D

**Problem:** [Problem 2248D](https://codeforces.com/contest/2248/problem/D)

### Explanation

Count mismatches x (s=0,t=1) and y (s=1,t=0) in the range. Range is resolvable iff 2*max(x,y) <= len. Just prefix-sum the two mismatch counts and answer each query in O(1) — easily the shortest solution of the four (the rating dropped from 1750 to around 1000... lol).

```cpp
void solve() {
   int n,q;
   cin >>n>>q;
   string s,t;
   cin>>s>>t;
   vector<int> pa(n+1,0);
   
   vector<int> pb(n+1,0);
    rep1(i,1,n){
   pa[i]=pa[i-1]+(s[i-1]=='0' && t[i-1]=='1');
   pb[i]=pb[i-1]+(s[i-1]=='1' && t[i-1]=='0');
   }
  
  while(q--){
  	int l,r;
  	cin >> l>>r;
  	int len=r-l+1;
  	int x=pa[r]-pa[l-1];
  	int y=pb[r]-pb[l-1];
  	if(2*max(x,y)<=len) cout<<"YES" <<'\n';
  	else cout << "NO" <<'\n';
  	  }
   }
```

---

## Conclusion

Overall, this was a pretty standard contest. I was happy to solve four problems despite starting late, but there is definitely room for improvement. Hopefully the next contest goes a bit better.
