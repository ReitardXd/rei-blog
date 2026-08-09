+++
date = '2026-08-09T12:04:01+05:30'
draft = false
title = 'Round 1114'
+++

## Contest

Contest: [Codeforces Round 1114 (Div. 3)](https://codeforces.com/contest/2254)

## Boilerplate

Refer to this article for the boilerplate
[Boilerplate](https://reitardxd.tech/codeforces/round-1113/)

## Summary

I managed to solve A,B,C1,C2,D,E and F(at the end of the contest) although i partially solved F but  got a failed test case 2 error when i submitted it regardless i did manage to solve it later after the contest ended. Gained about a +40 rating from 1523 to 1563 so i am happy although i was hoping to solve both F and G also i started the contest a little late so maybe i could have solved F and submitted it . This blog is going to be long so please open a code editor if you want to solve it while reading this

---

## Problem A

**Problem:** [Problem 2254A](https://codeforces.com/contest/2254/problem/A)

### Explanation
Pretty simple question wherein we are given a array with only 3 elements and if any of those elements are equal the round ends 
so compare the elements with each other using the mx and mn integers we can do the second activity that we are allowed to do 


```cpp
void solve() {
  vector<int> n(3);
  read(n);
 
  int r=0;
  while(true){
    if (n[0]== n[1]||n[1]==n[2]||n[2]==n[0]){
      cout << r << '\n';
      return;
    }
    int mx = max_element(all(n)) - n.begin();
    int mn = min_element(all(n)) - n.begin();
    n[mx]--;
    n[mn]++;
    r++;
  }
}

```

---

## Problem B

**Problem:** [Problem 2254B](https://codeforces.com/contest/2254/problem/B)

### Explanation
Take n and string s as inputs declare int r as 1 since its the lowest value it can be then run a for loop to check for duplicate elements and update r by 1 and declare ans = r. running another for loop we can find the difference by utilizing similar checks for i-1 and i+1 and increasing diff value when they are unequal and returning the answer as the lowest value out of ans and r+diff.
```cpp

void solve() {
   int n;
   cin >> n;
   string s;
   cin >> s;
   int r=1;
for (int i=1;i<n;i++)
    if (s[i]!=s[i-1]) r++;
    int ans = r;
 for (int i =1;i<n-1;i++){
   int diff=0;
  diff-=(s[i-1]!=s[i]);
  diff-=(s[i]!=s[i+1]);
  diff+=(s[i-1]!=s[i+1]);
 ans =min(ans,r+diff);
    }
  cout << ans << '\n';
  
}
 
```

---

## Problem C1

**Problem:** [Problem 2254C1](https://codeforces.com/contest/2254/problem/C1)

### Explanation

This one is a very common problem, i am glad they put this because of how recognizable it is, making it quite easy for someone who has done string transformation problems a lot.The allowed operation only swaps a '1' where the same parity exists , the count of 1s at even indices and the count of 1s at odd indices are both invariant so we just need to check whether these counts match between the two strings.

Like always take input then for each index i count the 1s in a and b separately based on whether i is even or odd (oa/ea for string a, ob/eb for string b), and compare oa with ob and ea with eb.If both match then  transformation is possible and output Yes otherwise output no+.

```cpp
void solve() {
   int n;
   cin >> n;
   string a,b;
   cin >> a >> b;
   int oa =0,ea=0,ob=0,eb=0;
   rep(i,n){
   	if (a[i]=='1'){
   		if(i%2==0)oa++;
   		else ea++;
   	}
   	if (b[i]=='1'){
   		if(i%2==0)ob++;
   		else eb++;
   		}
   
   }
  if(oa==ob && ea==eb)
  cout << "Yes" <<'\n';
  else cout << "No" << '\n';
}
 
```

---

## Problem C2

**Problem:** [Problem 2254C2](https://codeforces.com/contest/2254/problem/C2)

### Explanation
This one is similar to B due to the parity reasoning , but now we need the actual minimum cost instead of just a baisc yes or no . Since a swap can only move a '1' two positions at a time within the same parity class, we track the actual positions of 1s at even and odd indices separately for both strings. If the counts of even-1s or odd-1s differ between a and b it is impossible so we printout -1 .Otherwise we   sort the positions within each parity class and pair them up in order matching sorted positions minimizes total movement and the cost to move a 1 from one position to another is half the absolute distances summing these costs over both parity classes gives us the answer .

```cpp

void solve() {
   int n;
   cin >> n;
   string a,b;
   cin >> a >> b;
   vector<int> oa,ea,ob,eb;
   rep(i,n){
   	if (a[i]=='1'){
   		if(i%2==0) ea.push_back(i);
   		else oa.push_back(i);
   	}
   	if (b[i]=='1'){
   		if(i%2==0)eb.push_back(i);
   		else ob.push_back(i);
   		}
   
   }
   
   
   if (ea.size()!=eb.size()||oa.size()!=ob.size()){
   	cout<< -1 <<'\n';
   	return;
   	
   }
   ll ans=0;
 for(int i=0;i<(int)ea.size();i++)
 ans +=abs(ea[i]-eb[i])/2;
  for(int i=0;i<(int)oa.size();i++)
 ans +=abs(oa[i]-ob[i])/2;
 
cout << ans <<'\n';
}
 
```

---

## Problem D

**Problem:** [Problem 2254D](https://codeforces.com/contest/2254/problem/D)

### Explanation
We are given the array b and need to reconstruct a . First pair each b[i] with its original index and sort by value, then group equal values together gb essentially  stores the distinct sorted values, gcnt stores how many indices share that value, and gr stores the index range of each group. If the smallest value in gb isn't 0, it's immediately impossible.

Then for each consecutive pair of groups, the gap S between their values must be evenly divisible by the count of the earlier group (gcnt[m]) — this gives the a-value for that group, a[m] = S / gcnt[m]. This value must be at least 1, and it must be strictly increasing compared to the previous group's a-value, otherwise it's invalid. Once all groups pass these checks, the last group's a-value is fixed as one more than the second-to-last (or just 1 if there's only one group). Finally, we scatter these a-values back to their original positions and print the result and  printing -1 if  the checks fail(if this sounds ai generated because it is i put my own explanation through claude to format it since i am quite tired and i am writing this on sunday ill try to reduce my ai usage as my energy levels improve).


```cpp
void solve() {
int n; 
cin >> n;
 vector<ll> b(n);	
 for(auto &x:b ) cin >> x;
 vector<pair<ll,int>> arr(n);
 rep(i,n) arr[i]={b[i],i};
 sort(arr.begin(),arr.end());
 
vector<ll> gb,gcnt;
vector<pair<int,int>> gr;
int i=0;
while(i<n){
	int j =i;
	
	while(j<n && arr[j].first==arr[i].first) j++;
	gb.push_back(arr[i].first);
	gcnt.push_back(j-i);
	gr.push_back({i,j});
	i=j;
}
int k=gb.size();
if (gb[0]!=0) 
{cout<< -1 <<'\n'; 
return;}
 
 
vector<ll> a(k);
 bool ok =true;
  for (int m=0;m<=k-2;m++) {
        ll S=gb[m+1]-gb[m];
        if(S%gcnt[m]!=0)
      {ok=false;break;}
        a[m]=S/gcnt[m];
        if(a[m] <1) {ok=false;break;}
        if(m>0 && a[m]<=a[m-1]) 
        {ok=false;break; }
    }
    if (ok) {
        if (k==1) a[0] = 1;
        else a[k-1] = a[k-2] + 1;
    }
    if (!ok) {cout<< -1 <<'\n';
    return; }
    vector<ll> ans(n);
    for(int m=0;m<k;m++)
        for(int idx = gr[m].first; idx < gr[m].second; idx++)
            ans[arr[idx].second] = a[m];
    for(int idx=0;idx<n;idx++)
        cout << ans[idx] << " \n"[idx==n-1];
}
 
```
---

## Problem E
**Problem:** [Problem 2254E](https://codeforces.com/contest/2254/problem/E)

### Explanation
Take input as int and use a vector<ll> since we're working with a long long datatype here . Compute the total sum of b  if it's less than 1 print -1.
Otherwise chuck everything into a multiset and greedily build the array: each step we need s+x to stay at least 1, so figure out the minimum needed value h = 1-s and grab the smallest thing in the multiset that's >= h using lower_bound. Erase it, add it to s, store s as a[i], repeat till done. Print it all out at the end.
```cpp
void solve() {
	int n;
	cin >> n;
	vector<ll> b(n);
	read(b);
	
	
	ll total=0;
	each(x,b) total+=x;
	if (total < 1) {
		cout << -1 << '\n';
		return;
	}
	multiset<ll> ms(all(b));
	ll s=0;
	vll a(n);
	rep(i,n){
		ll h=1-s;
		auto it = ms.lower_bound(h);
		
		ll x =*it;
		ms.erase(it);
		s+=x;
		a[i]=s;
		
	}
	rep(i,n)cout<<a[i]<<" \n"[i==n-1];
}

```
---

## Problem F

**Problem:** [Problem 2254F](https://codeforces.com/contest/2254/problem/F)

### Explanation
Had to use the editorial for this one since  my first submission passed the first test case but failed at test case 2, and I could not spot the gap myself until reading through it. First sort both arrays and check if they're already equal, in which case the answer is immediately yes. Otherwise compute x. The XOR of every element across both arrays combined. If a matching transformation exists, x must appear somewhere in a so we search for an index in a whose value equals x. If no such index exists, the answer is no. If it does exist then we  XOR every other element of a (all except that index) with x, re-sort a, and check if it now matches b if so output yes other wise we output no.

```cpp

void solve(){
    int n;
    cin >> n;

    vi a(n);
    vi b(n);

    read(a);
    read(b);

    sort(all(a));
    sort(all(b));

    if(a == b){

        cout << "Yes" << '\n';
        return;
    }

    int x = 0;

    each(it, a) x ^= it;
    each(it, b) x ^= it;

    int idx = -1;

    rep(i, n){
        if(a[i] == x){
            idx = i;
            break;
        }
    }

    if(idx == -1){
        cout << "NO"<<  '\n';
        return;
    }

    rep(i, n){
        if(i == idx)
            continue;

        a[i] ^= x;
    }

    sort(all(a));

    if(a == b)
        cout << "YES" << '\n';
    else
        cout << "NO" << '\n';
}

```

---

## Conclusion
The contest was easy overall since it was a Div 3, but i am quite bummed out that i wasn't able to solve F in time. Found D to be easier than C1 and C2 which was kind of weird, usually it goes the other way. I also plan to start grinding the CSES problemset religiously and maybe throw in some AtCoder contests too, lol.
