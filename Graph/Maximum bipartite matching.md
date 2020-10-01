### Explaination
https://www.geeksforgeeks.org/maximum-bipartite-matching/
### Code
```c++
const int N = 1e5 + 5;
int n, k, x, y, id, mt[N], vis[N];
vector<vector<int>> v(N);

bool dfs(int u, int id) {
    if (vis[u] == id) return 0;
    vis[u] = id;
    for (auto i : v[u])
        if (!(~mt[i]) || dfs(mt[i], id)) {
            mt[i] = u;
            return 1;
        }
    return 0;
}

int maxMatch() {
    int re = 0;
    memset(mt, -1, sizeof mt);
    for (int i = 1; i <= n; i++) {
        ++id;
        if (dfs(i, id))
            re++;
    }
    return re;
}
```