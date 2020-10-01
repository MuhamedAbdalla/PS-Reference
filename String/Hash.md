### Explanation
https://codeforces.com/topic/60789/en18
### Code
```c++
const int M = 1e6 + 5, md1 = 1e9 + 7, md2 = 1e9 + 9, St1 = 30, St2 = 30;
int bs1, bs2, bs1p[M], bs2p[M];

struct H {
    int v1, v2;

    H() : v1(0), v2(0) {}

    void operator+=(char c) {
        v1 = ((ll) v1 * bs1 + (c - 'a' + 1)) % md1;
        v2 = ((ll) v2 * bs2 + (c - 'a' + 1)) % md2;
    }

    H operator-(H const &o) const {
        H an;
        an.v1 = v1 - o.v1;
        if (an.v1 < 0)an.v1 += md1;
        an.v2 = v2 - o.v2;
        if (an.v2 < 0)an.v2 += md2;
        return an;
    }

    H operator<<(int x) {
        H an;
        an.v1 = (ll) v1 * bs1p[x] % md1;
        an.v2 = (ll) v2 * bs2p[x] % md2;
        return an;
    }

    bool operator==(H const &o) const { return v1 == o.v1 && v2 == o.v2; }
} h[M];

H getH(int l, int r) { return h[r] - (h[l - 1] << r - l + 1); }

void initH(string &s) {
    srand(time(0));
    bs1 = rand() % 30 + St1;
    bs1p[0] = 1;
    for (int i = 1; i < M; i++) bs1p[i] = (ll) bs1p[i - 1] * bs1 % md1;

    bs2 = rand() % 30 + St2;
    bs2p[0] = 1;
    for (int i = 1; i < M; i++) bs2p[i] = (ll) bs2p[i - 1] * bs2 % md2;

    for (int i = 1; i <= s.size(); i++) h[i] = h[i - 1], h[i] += s[i - 1];
}
```