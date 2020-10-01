### Explaination
https://www.geeksforgeeks.org/euclidean-algorithms-basic-and-extended/
### Code
```
ll EEA(ll a , ll b , ll &x , ll &y)
{
    if(a < 0)
    {
        ll r = EEA(-a , b , x , y);
        x *= -1;
        return r;
    }

    if(b < 0)
    {
        ll r = EEA(a , -b , x , y);
        y *= -1;
        return r;
    }

    if(b == 0)
    {
        x  = 1 , y = 0;
        return a;
    }
    ll g = EEA(b , a % b , y , x);
    y -= (a / b) * x;
    return g;
}
```
