### Explanation
https://www.geeksforgeeks.org/lowest-common-ancestor-binary-tree-set-1/
### Code
```c++
const int N = 1e6 + 5, LG = 20;
int n, x, y, dp[N][LG], dep[N];
vector<int> v[N];

void init() {
    memset(dp, -1, sizeof(dp));
}

void dfs(int u, int p) {
    dp[u][0] = p;
    for (auto i  : v[u])
        if (i != p) {
            dep[i] = dep[u] + 1;
            dfs(i, u);
        }
}

void initLCA() {
    for (int j = 1; j < LG; j++)
        for (int i = 1; i <= n; i++)
            if (dp[i][j - 1] != -1)
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
        if (dp[a][i] != -1 && dp[b][i] != -1 && dp[b][i] != dp[a][i])
            b = dp[b][i], a = dp[a][i];

    return dp[a][0];
}
```