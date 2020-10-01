### Code
```c++
for (int i = 0; i < n; i++) {
   path[i][i] = 0;
}
for (int k = 0; k < n; k++) {
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (path[i][k] + path[k][j] < path[i][j]){
                path[i][j] = path[i][k] + path[k][j];
            }
        }
    }
}
```