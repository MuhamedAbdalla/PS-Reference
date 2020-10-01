### Code
```c++
const int M = 1e9 + 7;
int C[N][N];

void init() {
    C[0][0] = 1;
    for (int i = 1; i < N; i++) {
        C[i][0] = 1;
        for (int j = 1; j <= i; j++) {
            C[i][j] = (C[i - 1][j] + C[i - 1][j - 1]) % M;
        }
    }
}
```