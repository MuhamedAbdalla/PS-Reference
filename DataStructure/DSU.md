### Explaination
https://www.geeksforgeeks.org/disjoint-set-union-trees-set-2/?ref=rp
### Code
```c++
const int N = 2e5 + 5;
int par[N], sz[N];
 
void init(int n) {
    iota(par, par + N, 0);
    fill(sz, sz + N, 1);
}
 
int get(int idx) {
    while (idx != par[idx]) {
        par[idx] = par[par[idx]], idx = par[idx];
    }
    return idx;
}
 
void Union(int a, int b) {
    int i = get(a), j = get(b);
    if (i != j) {
        if (sz[i] >= sz[j]) {
            par[j] = i, sz[i] += sz[j];
            sz[j] = 0;
        }
        else {
            par[i] = j, sz[j] += sz[i];
            sz[i] = 0;
        }
    }
}
```
