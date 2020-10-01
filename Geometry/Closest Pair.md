### Code
```c++
struct PT {
    ll x, y;

    PT() {};

    PT(ll _x, ll _y) : x(_x), y(_y) {}

    PT(const PT &p) : x(p.x), y(p.y) {}

    PT operator+(const PT &p) const { return PT(x + p.x, y + p.y); }

    PT operator-(const PT &p) const { return PT(x - p.x, y - p.y); }

    PT operator*(long long c) const { return PT(x * c, y * c); }

    bool operator<(const PT &p) const { return make_pair(x, y) < make_pair(p.x, p.y); }
};

ll dot(PT a, PT b) {
    return a.x * b.x + a.y * b.y;
}

ll dist(PT p, PT q) {
    return dot(p - q, p - q);
}

ll ClosestPair(vector<PT> &p, int l, int r) {
    if (l == r) {
        return LLONG_MAX;
    }
    if (l + 1 == r) {
        return dist(p[l], p[r]);
    }
    int mid = (l + r) >> 1;
    ll a = ClosestPair(p, l, mid), b = ClosestPair(p, mid + 1, r);
    ll res = min(a, b);
    vector<PT> L, R;
    for (int i = l; i <= r; i++) {
        if ((mid - i) * (mid - i) < res) {
            if (i <= mid) {
                L.push_back(PT(p[i].y, p[i].x));
            } else {
                R.push_back(PT(p[i].y, p[i].x));
            }
        }
    }
    sort(L.begin(), L.end());
    sort(R.begin(), R.end());
    int st = 0, en = 0;
    for (auto i : L) {
        while (st < R.size() && R[st].x <= i.x && dist(R[st], i) >= res) {
            st++;
        }
        en = max(en, st);
        while (en < R.size() && (R[en].x <= i.x || dist(R[en], i) < res)) {
            en++;
        }
        for (int j = st; j < en; j++) {
            res = min(res, dist(i, R[j]));
        }
    }
    return res;
}
```