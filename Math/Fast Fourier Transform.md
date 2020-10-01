### Explanation
https://cp-algorithms.com/algebra/fft.html
### Code
```c++
typedef complex<double> base;
const double PI = acos(-1);

void fft(vector<base> &a, bool inv) {
    int n = (int) a.size();

    for (int i = 1, j = 0; i < n; i++) {
        int bit = n >> 1;

        for (; j & bit; bit >>= 1) j ^= bit;
        j ^= bit;

        if (i < j) swap(a[i], a[j]);
    }

    for (int len = 2; len <= n; len <<= 1) {
        double ang = 2 * PI / len * (inv ? -1 : 1);
        base wlen(cos(ang), sin(ang));
        for (int i = 0; i < n; i += len) {
            base w(1);
            for (int j = 0; j < len / 2; j++) {
                base u = a[i + j], v = a[i + j + len / 2] * w;
                a[i + j] = u + v;
                a[i + j + len / 2] = u - v;
                w *= wlen;
            }
        }
    }

    if (inv) {
        for (base &x : a) x /= n;
    }
}


vector<int> multiply(const vector<int> &a, const vector<int> &b) {
    vector<base> fa(a.begin(), a.end()), fb(b.begin(), b.end());

    int n = 1;
    while (n < a.size() + b.size()) n <<= 1;
    n = min(n, 1 << 20);

    fa.resize(n), fb.resize(n);

    fft(fa, 0), fft(fb, 0);
    for (int i = 0; i < n; i++) fa[i] *= fb[i];
    fft(fa, 1);

    vector<int> res(n);
    for (int i = 0; i < n; i++) res[i] = round(fa[i].real());

    return res;
}
```