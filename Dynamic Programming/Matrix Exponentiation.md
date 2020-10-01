### Explaination
https://www.geeksforgeeks.org/matrix-exponentiation/
### Code
```c++
typedef ll mType;
typedef vector<vector<mType>> mat;
const mType N = 3, mod = 1e9 + 7;

mat init() {
    return vector<vector<mType> >(N, vector<mType>(N));
}

void mul(mat &a, mat &b, mat &re, mType m) {
    for (int i = 0; i < N; i++)
        for (int j = 0; j < N; j++) {
            re[i][j] = 0;
            for (int k = 0; k < N; k++)
                re[i][j] = (re[i][j] + a[i][k] * b[k][j]) % m;
        }
    swap(a, re);
}

mat mfp(mat &a, mType p, mType m) {
    mat re = init(), tmp = init();
    for (int i = 0; i < N; i++)
        re[i][i] = 1;

    while (p) {
        if (p & 1) mul(re, a, tmp, m);
        p >>= 1;
        mul(a, a, tmp, m);
    }
    return re;
}
```