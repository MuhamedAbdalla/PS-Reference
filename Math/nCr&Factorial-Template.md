### Explanation
problem : https://atcoder.jp/contests/abc172/tasks/abc172_e
### Code
```c++
const int N = 5e5 + 5, MOD = 1e9 + 7;

struct mi {
    ll v;

    explicit operator ll() const { return v; }

    mi() { v = 0; }

    mi(ll _v) {
        v = (-MOD < _v && _v < MOD) ? _v : _v % MOD;
        if (v < 0) v += MOD;
    }

    friend bool operator==(const mi &a, const mi &b) {
        return a.v == b.v;
    }

    friend bool operator!=(const mi &a, const mi &b) {
        return !(a == b);
    }

    friend bool operator<(const mi &a, const mi &b) {
        return a.v < b.v;
    }

    mi &operator+=(const mi &m) {
        if ((v += m.v) >= MOD) v -= MOD;
        return *this;
    }

    mi &operator-=(const mi &m) {
        if ((v -= m.v) < 0) v += MOD;
        return *this;
    }

    mi &operator*=(const mi &m) {
        v = v * m.v % MOD;
        return *this;
    }

    mi &operator/=(const mi &m) { return (*this) *= inv(m); }

    friend mi pow(mi a, ll p) {
        mi ans = 1;
        assert(p >= 0);
        for (; p; p /= 2, a *= a) if (p & 1) ans *= a;
        return ans;
    }

    friend mi inv(const mi &a) {
        assert(a.v != 0);
        return pow(a, MOD - 2);
    }

    mi operator-() const { return mi(-v); }

    mi &operator++() { return *this += 1; }

    mi &operator--() { return *this -= 1; }

    mi operator++(int) {
        mi temp;
        temp.v = v++;
        return temp;
    }

    mi operator--(int) {
        mi temp;
        temp.v = v--;
        return temp;
    }

    friend mi operator+(mi a, const mi &b) { return a += b; }

    friend mi operator-(mi a, const mi &b) { return a -= b; }

    friend mi operator*(mi a, const mi &b) { return a *= b; }

    friend mi operator/(mi a, const mi &b) { return a /= b; }

    friend ostream &operator<<(ostream &os, const mi &m) {
        os << m.v;
        return os;
    }

    friend istream &operator>>(istream &is, mi &m) {
        ll x;
        is >> x;
        m.v = x;
        return is;
    }
};

mi facs[N], facInvs[N];

mi nCr(mi a, mi b) {
    if ((ll) b > (ll) a) return 0;
    if ((ll) a < 0) return 0;
    if ((ll) b < 0) return 0;
    mi cur = facs[(ll) a];
    cur = cur * facInvs[(ll) b];
    cur = cur * facInvs[(ll) a - (ll) b];
    return cur;
}

void initFacs() {
    facs[0] = 1;
    facInvs[0] = 1;
    for (int i = 1; i < N; i++) {
        facs[i] = (facs[i - 1] * i);
        facInvs[i] = inv(facs[i]);
    }
}

```