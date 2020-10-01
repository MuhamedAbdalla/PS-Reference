# Baby step Giant Step Algorithm

### Explaination

Given a prime n and two integers a and k, find all x for which

![](https://latex.codecogs.com/png.latex?x^k&space;\equiv&space;a&space;\pmod&space;n)

Let's apply the concept of a primitive root modulo **n**. Let **g** be a primitive root modulo **n**. Note that since n is prime, it must exist, and it can be found in **O(Ans⋅logϕ(n)⋅logn)=O(Ans⋅log2n)** plus time of factoring **ϕ(n)**.

We can easily discard the case where **a=0**. In this case, obviously there is only one answer: **x=0**.

Since we know that **n** is a prime and any number between **1** and **n−1** can be represented as a power of the primitive root, we can represent the discrete root problem as follows:

![](https://latex.codecogs.com/png.latex?(g^y)^k&space;\equiv&space;a&space;\pmod&space;n)

**where:**

![](https://latex.codecogs.com/png.latex?x&space;\equiv&space;g^y&space;\pmod&space;n)

**This, in turn, can be rewritten as:**

![](https://latex.codecogs.com/png.latex?(g^k)^y&space;\equiv&space;a&space;\pmod&space;n)

![](https://latex.codecogs.com/png.latex?L=\sqrt{n})

![](https://latex.codecogs.com/png.latex?(g^k)^{i*L-j}\equiv&space;a&space;\pmod&space;n)

![](https://latex.codecogs.com/png.latex?(g^k)^{i*L}\equiv&space;(g^k)^{j}a&space;\pmod&space;n)

The solution can be found using Shanks' baby-step giant-step algorithm in **O(√n⋅log(n))**

### Code
```c++
#define ll long long
#define ull unsigned long long
typedef vector<vector<ll>> mat;
    
ll phi(ll X) {
	ll res = X;
	for (ll i = 2; i * i <= X; i++) 
	{
		if (X % i == 0) 
		{
			res -= res / i;
			while (X % i == 0)
				X /= i;
		}
	}
	if (X != 1)
		res -= res / X;
	return res;
}

ll generator(ll p)
{
	vector<ll> fact;
	int phi = p - 1, n = phi;
	for (int i = 2; i * i <= n; i++)
		if (n % i == 0) 
		{
			fact.push_back(i);
			while (n % i == 0)
				n /= i;
		}
	if (n > 1)
		fact.push_back(n);
	for (int res = 2; res <= p; ++res)
	{
		bool ok = true;
		for (int factor : fact)
		{
			if (fp(res, phi / factor, p) == 1) 
			{
				ok = false;
				break;
			}
		}
		if (ok)
			return res;
	}
	return -1;
}

ll bsgs(ll k , ll a , ll mod)
{
    //x^k == a % mod
    if(!a) return 0;

    a %= mod;
    ll g = generator(mod);
    ll sq = (ll) sqrt(mod + .0) + 1;
    ll cur = fp(g , sq * k % (mod - 1) , mod) , org = cur;

    vector<pair<ll , ll> > dec;
    for(int i = 1 ; i <= sq ; i++)
        dec.push_back({cur , i}) , cur = org * cur % mod;
    sort(dec.begin() , dec.end());

    cur = 1;
    org = fp(g , k % (mod - 1) , mod);
    for(int i = 0 ; i < sq ; i++)
    {
        ll my = cur * a % mod;
        cur = cur * org % mod;

        auto it = lower_bound(dec.begin() , dec.end() , make_pair(my , 0ll));
        if(it != dec.end() && it->first == my)
            return fp(g , it->second * sq - i , mod);
    }
    return -1;
}
```
