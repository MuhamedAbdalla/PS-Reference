### Explaination
https://www.geeksforgeeks.org/chinese-remainder-theorem-set-2-implementation/
https://en.wikipedia.org/wiki/Chinese_remainder_theorem
https://codeforces.com/blog/entry/61290
### Code
```c++
ll mul_inv(int a , int b)
{
    ll b0 = b , t , q;
    ll x0 = 0 , x1 = 1;
    if(b == 1) return 1;
    while(a > 1)
    {
        q = a / b;
        t = b , b = a % b , a = t;
        t = x0 , x0 = x1 - q * x0 , x1 = t;
    }
    if(x1 < 0) x1 += b0;
    return x1;
}

ll chinese_remainder(vector<ll> &a , vector<ll> &n)
{
    ll p , prod = 1 , sum = 0;
    for(int i = 0 ; i < n.size() ; i++) prod *= n[i];
    for(int i = 0 ; i < n.size() ; i++)
    {
        p = prod / n[i];
        sum += a[i] * p * mul_inv(p , n[i]);
    }
    return sum % prod;
}

```