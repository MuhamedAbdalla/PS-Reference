### Explaination
https://www.geeksforgeeks.org/union-find-algorithm-set-2-union-by-rank/
### Code
```c++
const int N = 2 * 1e5 + 5;
int n, k, a, b, lev[N], par[N];
 
int find(int node) {
    return node == par[node] ? node : par[node] = find(par[node]);
}
 
void Uni(int x, int y) {
    x = find(x), y = find(y);
    if (x == y) return;
    if (lev[x] <= lev[y]) swap(x, y);
    par[y] = x;
    lev[x] += lev[x] == lev[y];
}
 
void init() {
    iota(par, par + N, 0);
}
```
