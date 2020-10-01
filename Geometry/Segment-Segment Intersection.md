### Explanation
https://www.geeksforgeeks.org/check-if-two-given-line-segments-intersect/
### Code
```c++
set<pair<ll, ll>> se;
const double PI = acos(-1.0);
const double EPS = 1e-9;
typedef complex<double> point;

#define X real()
#define Y imag()
#define angle(a)                (atan2((a).imag(), (a).real()))
#define vec(a,b)                ((b)-(a))
#define same(p1,p2)             (dp(vec(p1,p2),vec(p1,p2)) < EPS)
#define dp(a,b)                 ( (conj(a)*(b)).real() )	// a*b cos(T), if zero -> prep
#define cp(a,b)                 ( (conj(a)*(b)).imag() )	// a*b sin(T), if zero -> parllel
#define length(a)               (hypot((a).imag(), (a).real()))
#define normalize(a)            (a)/length(a)
//#define polar(r,ang)            ((r)*exp(point(0,ang)))  ==> Already added in c++11
#define reflectO(v,m)  (conj((v)/(m))*(m))

struct PT {
    ll x, y;

    PT() {};

    PT(ll _x, ll _y) : x(_x), y(_y) {}

    PT(const PT &p) : x(p.x), y(p.y) {}

    PT operator+(const PT &p) const { return PT(x + p.x, y + p.y); }

    PT operator-(const PT &p) const { return PT(x - p.x, y - p.y); }

    PT operator*(long long c) const { return PT(x * c, y * c); }

    bool operator<(const PT &p) const { return make_pair(x, y) < make_pair(p.x, p.y); }
};

PT rotate_point(PT p, PT px, double angle, bool clockwise){
    double s = sin(angle); // angle is in radians
    double c = cos(angle); // angle is in radians
    
    // translate point back to origin:
    p -= px;
    
    PT PTnew;
    
    if (clockwise) {
        PTnew.x = p.x * c + p.y * s;
        PTnew.y = -p.x * s + p.y * c;
    } else {
        PTnew.x = p.x * c - p.y * s;
        PTnew.y = p.x * s + p.y * c;
    }
    
    PTnew += px;
    
    return PTnew;
}

ll IntegerPointsOnBetweenTwoPoints(PT a, PT b) {
    return __gcd(abs(a.x - b.x), abs(a.y - b.y)) + 1;
}

ll dot(PT a, PT b) {
    return a.x * b.x + a.y * b.y;
}

ll dist(PT p, PT q) {
    return dot(p - q, p - q);
}

PT reflect(PT p, PT p0, PT p1) {
  PT z = p - p0, w = p1 - p0;
  return conj(z / w) * w + p0;
}

ll cross(PT a, PT b) {
    return a.x * b.y - a.y * b.x;
}

ll orient(PT &a, PT &b, PT &c) {
    return cross(b - a, c - a);
}

bool inDisk(PT &a, PT &b, PT &p) {
    return dot(a - p, b - p) <= 0;
}

bool onSegment(PT &a, PT &b, PT &p) {
    return orient(a, b, p) == 0 && inDisk(a, b, p);
}

bool properInter(PT &a, PT &b, PT &c, PT &d) {
    ll oa = orient(c, d, a),
            ob = orient(c, d, b),
            oc = orient(a, b, c),
            od = orient(a, b, d);

    if (((oa < 0 && ob > 0) || (oa > 0 && ob < 0)) && ((oc < 0 && od > 0) || (oc > 0 && od < 0))) {
        PT cur = a * ob - b * oa;
        ll df = ob - oa;

        cur = cur / df;
        se.insert({cur.x, cur.y});
        
        return 1;
    }

    return 0;
}

void inters(PT &a, PT &b, PT &c, PT &d) {
    if (properInter(a, b, c, d)) return;

    if (onSegment(c, d, a)) se.insert({a.x, a.y});
    if (onSegment(c, d, b)) se.insert({b.x, b.y});
    if (onSegment(a, b, c)) se.insert({c.x, c.y});
    if (onSegment(a, b, d)) se.insert({d.x, d.y});
}
```
