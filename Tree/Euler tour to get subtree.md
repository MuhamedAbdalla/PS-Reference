### Code
```c++
const int N = 2e5 + 5;
int lvl[N], in[N], out[N], idx[N], ti;
vector<vector<int>> v(N);

void dfs(int u, int p, int l) {
    in[u] = ++ti;
    idx[ti] = u;
    lvl[u] = l;
    for (auto i : v[u])
        if (i != p) {
            dfs(i, u, l + 1);
        }
    out[u] = ti;
}
```