### Explanation
https://codeforces.com/blog/entry/44351
### Code
```c++
const int N = 1e5 + 5;
int n, m, x, y, col[N], cnt[N], out[N], sz[N], st[N], en[N], tToN[N], ti;
OS<pair<int, int>> se;
vector<vector<int>> v(N);
vector<vector<pair<int, int>>> q(N);
 
void ad(int u) {
    se.erase({cnt[col[u]], col[u]});
    cnt[col[u]]++;
    se.insert({cnt[col[u]], col[u]});
}
 
void rm(int u) {
    se.erase({cnt[col[u]], col[u]});
    cnt[col[u]]--;
}
 
void dfsInit(int u, int p) {
    st[u] = ++ti;
    tToN[ti] = u;
    sz[u] = 1;
    for (auto i : v[u])
        if (i != p) {
            dfsInit(i, u);
            sz[u] += sz[i];
        }
    en[u] = ti;
}
 
void dfs(int u, int p, bool keep) {
    int mx = -1, bigChild = -1;
    for (auto i : v[u])
        if (i != p && sz[i] > mx) {
            mx = sz[i];
            bigChild = i;
        }
 
    for (auto i : v[u])
        if (i != p && i != bigChild) {
            dfs(i, u, 0);
        }
 
    if (bigChild != -1) {
        dfs(bigChild, u, 1);
    }
 
    for (auto i : v[u])
        if (i != p && i != bigChild)
            for (int j = st[i]; j <= en[i]; j++) {
                ad(tToN[j]);
            }
    ad(u);
 
    for (auto j : q[u]) {
        int x = se.order_of_key({j.first, -1e9});
        out[j.second] = (int) se.size() - x;
    }
 
    if (!keep)
        for (int i = st[u]; i <= en[u]; i++) {
            rm(tToN[i]);
        }
}
```