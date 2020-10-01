### Code
```c++
ll numberOfPrimeDivisors(ll x) {
    ll re = 0;
    for (ll i = 2; i * i <= x; i++) {
        if (x % i == 0) {
            while (x % i == 0) {
                re++;
                x /= i;
            }
        }
    }
    if (x != 1) re++;
    return re;
}
```