### Explanation
https://cp-algorithms.com/string/aho_corasick.html
### Code
```c++
struct Aho {
    struct Vertex {
        map<char, int> next, go;
        int link = -1, p = -1;
        vector<int> patIdx;
        char pch;

        Vertex(int p = -1, char ch = '$') : p(p), pch(ch) {
        }
    };

    vector<Vertex> T;

    Aho() {
        T.emplace_back();
    }

    void add_string(string const &s, int i) {
        int v = 0;
        for (char ch : s) {
            int c = ch - 'a';
            if (T[v].next.count(c) == 0) {
                T[v].next[c] = T.size();
                T.emplace_back(v, ch);
            }
            v = T[v].next[c];
        }
        T[v].patIdx.push_back(i);
    }

    int get_link(int v) {
        if (T[v].link == -1)
            if (v == 0 || T[v].p == 0) {
                T[v].link = 0;
            } else {
                T[v].link = go(get_link(T[v].p), T[v].pch);
                T[v].patIdx.insert(T[v].patIdx.end(), T[T[v].link].patIdx.begin(), T[T[v].link].patIdx.end());
            }
        return T[v].link;
    }

    int go(int v, char ch) {
        int c = ch - 'a';
        if (T[v].go.count(c) == 0)
            if (T[v].next.count(c) != 0) {
                T[v].go[c] = T[v].next[c];
            } else {
                T[v].go[c] = v == 0 ? 0 : go(get_link(v), ch);
            }
        return T[v].go[c];
    }

    void build() {
        for (int i = 0; i < T.size(); i++) get_link(i);
    }
} A;
```