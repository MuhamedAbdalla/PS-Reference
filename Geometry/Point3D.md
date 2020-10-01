### Code
```c++
struct Point3D {
    typedef double T;
    typedef Point3D P;
    typedef const P &R;
    T x, y, z;

    explicit Point3D(T x = T(), T y = T(), T z = T()) : x(x), y(y), z(z) {}

    bool operator==(R p) const { return x == p.x && y == p.y && z == p.z; }

    P operator+(R p) const { return P(x + p.x, y + p.y, z + p.z); }

    P operator-(R p) const { return P(x - p.x, y - p.y, z - p.z); }

    P operator-() { return P(-x, -y, -z); }

    P operator*(T d) const { return P(x * d, y * d, z * d); }

    P operator/(T d) const { return P(x / d, y / d, z / d); }

    T dot(R p) const { return x * p.x + y * p.y + z * p.z; }

    P cross(R p) const {
        return P(y * p.z - z * p.y, z * p.x - x * p.z, x * p.y - y * p.x);
    }

    T dist2() const { return x * x + y * y + z * z; } //distance^2
    double dist() const { return sqrt((double) dist2()); }

    P unit() const { return *this / (T) dist(); } //makes dist()=1
    //returns unit vector normal to *this and p
    P normal(P p) const { return cross(p).unit(); }

    T angle(P p) { return acos(dot(p)); }
};
```