### Explanation
https://www.youtube.com/watch?v=vjxLlFTKhrU&t=1244s
### Code
```c++
void kmpPreprocess(string &P, int b[], int len) {
    int i = 0, j = -1;
    b[0] = -1;
    while (i < len) {
        while (j >= 0 && P[i] != P[j]) j = b[j];
        i++, j++;
        b[i] = j;
    }
}

void kmpSearch(string &T, string &P, int b[], int tLen, int pLen) {
    int i = 0, j = 0;
    while (i < tLen) {
        while (j >= 0 && T[i] != P[j]) j = b[j];
        i++;
        j++;
        if (j == pLen) {
            printf("P is found at index %d in T\n", i - j);
            j = b[j];
        }
    }
}
```