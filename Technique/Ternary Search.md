### Explanation
https://www.geeksforgeeks.org/ternary-search/
### Code
```c++
const double ESP = 1e-6, MX = 200, MNVAL = -1e4, MXVAL = 1e4;

double Ternary(double lo, double hi) {
    double x1, x2;
    while (abs(hi - lo) > ESP) {
        x1 = lo + (hi - lo) / 3;
        x2 = hi - (hi - lo) / 3;
        double f1 = calc(x1);
        double f2 = calc(x2);
        if (f1 > f2) {
            lo = x1;
        } else {
            hi = x2;
        }
    }
    return calc(hi);
}
```