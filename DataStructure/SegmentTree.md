### Code
```c++
struct SegmentTree {
    int N;
    vector<int> T, L;

    SegmentTree(int _N) {
        N = _N;
        T = vector<int>((N + 5) << 2);
        L = vector<int>((N + 5) << 2);
    }

    void mrg(int i) {
        T[i] = max(T[i << 1], T[i << 1 | 1]);
    }

    void bld(int s, int e, int i, int a[]) {
        if (s == e) return void(T[i] = a[s]);

        int md = (s + e) >> 1;
        bld(s, md, i << 1, a);
        bld(md + 1, e, i << 1 | 1, a);

        mrg(i);
    }

    void push(int i, int s, int e) {
        if (!L[i]) return;
        T[i] += L[i];
        if (s != e) {
            L[i << 1] += L[i];
            L[i << 1 | 1] += L[i];
        }
        L[i] = 0;
    }

    void udt(int l, int r, int val, int s, int e, int i) {
        if (l > r) return;
        push(i, s, e);
        if (r < s || l > e) return;
        if (l <= s && r >= e) {
            L[i] += val;
            push(i, s, e);
            return;
        }

        int md = (s + e) >> 1;
        udt(l, r, val, s, md, i << 1);
        udt(l, r, val, md + 1, e, i << 1 | 1);

        mrg(i);
    }

    int qry(int p, int s, int e, int i) {
        push(i, s, e);
        if (s == e) return T[i];

        int md = (s + e) >> 1;
        if (p > md) return qry(p, md + 1, e, i << 1 | 1);
        return qry(p, s, md, i << 1);
    }

    int qry(int l, int r, int s, int e, int i) {
        push(i, s, e);
        if (r < s || l > e) return 0;
        if (l <= s && r >= e) return T[i];

        int md = (s + e) >> 1;

        return qry(l, r, s, md, i << 1) + qry(l, r, md + 1, e, i << 1 | 1);
    }
};
```
