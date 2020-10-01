### Explaination
https://codeforces.com/blog/entry/53925
### Code
```c++
const int N = 1e7 + 5;
char mobius[N];
bool p[N];

void build()
{
    memset(mobius , -1 , sizeof mobius);
	for (ll i = 2; i < N; i++)
		if (!p[i])
        {
			mobius[i] = 1;
			for (ll j = i + i; j < N; j += i)
				p[j] = 1, mobius[j] = j % (i * i) == 0 ? 0 : -mobius[j];
		}
}
```