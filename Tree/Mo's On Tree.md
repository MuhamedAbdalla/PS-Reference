### Explanation
https://codeforces.com/blog/entry/43230
### Code
```c++
const int N = 1e6 + 5, BLOCK_SIZE = 320, LG = 20;
int n, x, y, st[N], en[N], t, dp[N][LG], dep[N], ID[N], ANS[N];
bool vis[N];
vector<int> v[N];

struct Query {
    int l, r, idx, LCA;

    bool operator<(Query other) const {
        if (l / BLOCK_SIZE != other.l / BLOCK_SIZE)
            return make_pair(l / BLOCK_SIZE, r) <
                   make_pair(other.l / BLOCK_SIZE, other.r);
        return (l / BLOCK_SIZE & 1) ? (r < other.r) : (r > other.r);
    }
} Q[N];

void check(int x, ll &res) {
    x = ID[x];
    // Add
    if (vis[x]) {
       
    } 
    // Delete
    else {
       
    }
    vis[x] ^= 1;
}

void MO() {
    sort(Q, Q + q);

    int cur_l = Q[0].l;
    int cur_r = Q[0].l - 1;
    ll cur = 1;
    for (int i = 0; i < q; i++) {
        while (cur_l < Q[i].l) check(cur_l++, cur);
        while (cur_l > Q[i].l) check(--cur_l, cur);
        while (cur_r < Q[i].r) check(++cur_r, cur);
        while (cur_r > Q[i].r) check(cur_r--, cur);

        if (Q[i].LCA != -1) check(st[Q[i].LCA], cur);
        ANS[Q[i].idx] = cur;
        if (Q[i].LCA != -1) check(st[Q[i].LCA], cur);
    }
}

void dfs(int u, int p) {
    st[u] = ++t;
    ID[st[u]] = u;
    dp[u][0] = p;
    for (auto i  : v[u])
        if (i != p) {
            dep[i] = dep[u] + 1;
            dfs(i, u);
        }
    en[u] = ++t;
    ID[en[u]] = u;
}

void initLCA() {
    for (int j = 1; j < LG; j++)
        for (int i = 1; i <= n; i++)
            dp[i][j] = dp[dp[i][j - 1]][j - 1];
}

int LCA(int a, int b) {
    if (dep[a] > dep[b])
        swap(a, b);

    for (int i = LG - 1; i >= 0; i--)
        if (dep[b] - (1 << i) >= dep[a])
            b = dp[b][i];

    if (a == b)
        return a;

    for (int i = LG - 1; i >= 0; i--)
        if (dp[b][i] != dp[a][i])
            b = dp[b][i], a = dp[a][i];

    return dp[a][0];
}
```