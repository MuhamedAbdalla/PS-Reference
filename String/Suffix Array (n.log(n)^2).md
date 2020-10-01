### Explaination
https://www.youtube.com/watch?v=maBr777ZRhw&list=PLPt2dINI2MIYrtHBahPW16S-Wz9wx24Nc&index=5
### Code
```
namespace SuffixArray {
    string S;
    int N, gap;
    vector<int> sa, pos, tmp, lcp;

    bool sufCmp(int i, int j) {
        if (pos[i] != pos[j])
            return pos[i] < pos[j];
        i += gap;
        j += gap;
        return (i < N && j < N) ? pos[i] < pos[j] : i > j;
    }

    void buildSA() {
        N = S.size();
        sa.resize(N);
        pos.resize(N);
        tmp.clear();
        tmp.resize(N);
        for (int i = 0; i < N; i++)
            sa[i] = i, pos[i] = S[i];
        for (gap = 1; tmp[N - 1] != N - 1; gap <<= 1) {
            sort(sa.begin(), sa.end(), sufCmp);
            for (int i = 0; i + 1 < N; i++)
                tmp[i + 1] = tmp[i] + sufCmp(sa[i], sa[i + 1]);
            for (int i = 0; i < N; i++)
                pos[sa[i]] = tmp[i];
        }
    }

    void buildLCP() {
        lcp.resize(N);
        for (int i = 0, k = 0; i < N; ++i)
            if (pos[i] != N - 1) {
                for (int j = sa[pos[i] + 1]; S[i + k] == S[j + k]; ++k);
                lcp[pos[i]] = k;
                if (k) --k;
            }
    }
}
```