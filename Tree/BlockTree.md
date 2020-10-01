### Explanation
https://codeforces.com/blog/entry/46843
### Problem
https://atcoder.jp/contests/abc133/tasks/abc133_f
### Code
```c++
#include <bits/stdc++.h>

using namespace std;
using ll = long long;

const int N = 1e5 + 5;
int n, q, dis[N], lev[N], par[N], parC[N];
bool spi[N], vis[N];
vector<pair<int, int>> e[N], cl;

void dfs1(int node, int pre) {
    for (auto i : e[node]) {
        if (i.first != pre) {
            lev[i.first] = lev[node] + 1;
            par[i.first] = node;
            parC[i.first] = i.second;
            dis[i.first] = dis[node] + cl[i.second].second;
            dfs1(i.first, node);
        }
    }
}

vector<map<int, pair<int, int>>> colorP(N);
map<int, pair<int, int>> color;

void dfs2(int node, int pre) {
    if (spi[node]) {
        colorP[node] = color;
    }
    for (auto i : e[node]) {
        if (i.first != pre) {
            int c = cl[i.second].first, di = cl[i.second].second;
            color[c].first++;
            color[c].second += di;
            dfs2(i.first, node);
            color[c].first--;
            color[c].second -= di;
            if (color[c].first == 0) color.erase(c);
        }
    }
}

const int LG = 20;
int dp[N][LG];

void dfs3(int u, int p) {
    dp[u][0] = p;
    for (auto i : e[u])
        if (i.first != p) {
            dfs3(i.first, u);
        }
}

void initLCA() {
    for (int j = 1; j < LG; j++)
        for (int i = 1; i <= n; i++)
            if (dp[i][j - 1] != -1)
                dp[i][j] = dp[dp[i][j - 1]][j - 1];
}

int LCA(int a, int b) {
    if (lev[a] > lev[b])
        swap(a, b);

    for (int i = LG - 1; i >= 0; i--)
        if (lev[b] - (1 << i) >= lev[a])
            b = dp[b][i];

    if (a == b)
        return a;

    for (int i = LG - 1; i >= 0; i--)
        if (dp[a][i] != -1 && dp[b][i] != -1 && dp[b][i] != dp[a][i])
            b = dp[b][i], a = dp[a][i];

    return dp[a][0];
}

pair<int, int> get2NE1(int nc, int c) {
    if (spi[nc]) {
        if (!colorP[nc].count(c)) {
            return {0, 0};
        } else {
            return colorP[nc][c];
        }
    }
    pair<int, int> ret = get2NE1(par[nc], c);
    if (cl[parC[nc]].first == c) {
        ret.first++, ret.second += cl[parC[nc]].second;
    }
    return ret;
}

int get(int nc, int d, int c) {
    pair<int, int> ne1 = get2NE1(nc, c);
    return dis[nc] - ne1.second + ne1.first * d;
}

int main() {
    ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);
    cin >> n >> q;
    for (int i = 0; i < n - 1; i++) {
        int a, b, c, l;
        cin >> a >> b >> c >> l;
        e[a].emplace_back(b, i);
        e[b].emplace_back(a, i);
        cl.emplace_back(c, l);
    }

    dfs1(1, -1);

    priority_queue<pair<int, int>> pq;
    for (int i = 1; i <= n; i++) {
        pq.push({lev[i], i});
    }

    par[1] = -1;
    int sq = ceil(sqrtl((long double) n));
    while (pq.size()) {
        pair<int, int> pa = pq.top();
        pq.pop();
        if (vis[pa.second]) continue;
        int temp = pa.second;
        for (int i = 0; i <= sq && par[temp] != -1; i++) {
            if (spi[temp]) {
                goto no;
            }
            vis[temp] = 1;
            temp = par[temp];
        }
        spi[temp] = 1;
        no:;
    }

    dfs2(1, -1);

    memset(dp, -1, sizeof(dp));

    dfs3(1, -1);

    initLCA();

    while (q--) {
        int c, d, u, v;
        cin >> c >> d >> u >> v;
        int lc = LCA(u, v);
        int data1 = get(u, d, c);
        int data2 = get(v, d, c);
        int data3 = get(lc, d, c);

        cout << data1 + data2 - 2 * data3 << "\n";

    }

    return 0;
}
```
