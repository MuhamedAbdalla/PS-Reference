### Explaination
https://www.geeksforgeeks.org/multiplicative-inverse-under-modulo-m/
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
```