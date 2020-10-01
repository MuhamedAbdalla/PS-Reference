### Code
```c++
struct Trie {
    Trie *st[2];
    int cnt[2];
 
    Trie() {
        memset(st, 0, sizeof st);
        memset(cnt, 0, sizeof cnt);
    }
 
    void ins(int x, int msk) {
        if (!(~msk)) return;
        if (!st[x >> msk]) st[x >> msk] = new Trie();
        cnt[x >> msk]++;
        st[x >> msk]->ins(x & ((1 << msk) - 1), msk - 1);
    }
 
    int getMn(int msk) {
        if (!(~msk)) return 0;
 
        if (!st[0]) return st[1]->getMn(msk - 1);
        if (!st[1]) return st[0]->getMn(msk - 1);
 
        int mn0 = INT_MAX, mn1 = INT_MAX;
        mn0 = st[0]->getMn(msk - 1);
        mn1 = st[1]->getMn(msk - 1);
 
        return min(mn0, mn1) + (1 << msk);
    }
 
    void del(int x, int msk) {
        if (!(~msk)) return;
        cnt[x >> msk]--;
        if (cnt[x >> msk] == 0) {
            st[x >> msk] = nullptr;
            return;
        }
        st[x >> msk]->del(x & ((1 << msk) - 1), msk - 1);
    }
} T;
```