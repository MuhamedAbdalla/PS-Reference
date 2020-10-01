### Explaination
https://cp-algorithms.com/data_structures/sqrt_decomposition.html#toc-tgt-4
### Code
```c++
const int BLOCK_SIZE = 320;

struct Query {
    int l, r, idx;

    bool operator<(Query other) const {
        if (l / BLOCK_SIZE != other.l / BLOCK_SIZE)
            return make_pair(l / BLOCK_SIZE, r) <
                   make_pair(other.l / BLOCK_SIZE, other.r);
        return (l / BLOCK_SIZE & 1) ? (r < other.r) : (r > other.r);
    }
};

vector<Query> qry;

void add(int idx) {
    cur += cnt[a[idx] ^ k];
    cnt[a[idx]]++;
}

void del(int idx) {
    cnt[a[idx]]--;
    cur -= cnt[a[idx] ^ k];
}

ll getAns() {
    return cur;
}

vector<ll> MO(vector<Query> queries) {
    vector<ll> answers(queries.size());
    sort(queries.begin(), queries.end());

    int cur_l = 0;
    int cur_r = -1;
    for (Query q : queries) {
        while (cur_l > q.l) {
            cur_l--;
            add(cur_l);
        }
        while (cur_r < q.r) {
            cur_r++;
            add(cur_r);
        }
        while (cur_l < q.l) {
            del(cur_l);
            cur_l++;
        }
        while (cur_r > q.r) {
            del(cur_r);
            cur_r--;
        }
        answers[q.idx] = getAns();
    }
    return answers;
}
```