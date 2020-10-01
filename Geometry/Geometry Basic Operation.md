### Code
```c++
double toRadian(double degree) {
    return (degree * PI) / 180.0;
}

double toDegree(double radian) {
    if (radian < 0) radian += 2.0 * PI;
    return (radian * 180.0) / PI;
}

double fixedAngle(double angle) {
    retrun angle > 1 ? 1 : (angle < -1 ? -1 : angle);
}

// sin(A) / a = sin(B) / b = sin(C) / c
double get_a_bAB(double b, double A, double B) {
    return (sin(A) * b) / sin(B));
}

double get_A_baB(double b, double A, double B) {
    return asin(fixedAngle(sin(B) * a) / b);
}

// a^2 = b^2 + c^2 - 2bc * cos(A)
double get_A_abc(double a, double b, double c) {
    return acos(fixedAngle((a * a + b * b - c * c) / (2 * b * c)));
}

double get_a_Abc(double A, double b, double c) {
    return b * b + c * c - 2 * b * c * cos(A);
}

// sin(a + b) = sin(a) * cos(b) + sin(b) * cos(a)
double get_Sin_ApB(double a, double b) {
    return sin(a) * cos(b) + sin(b) * cos(a);
}

// sin(a - b) = sin(a) * cos(b) - sin(b) * cos(a)
double get_Sin_AmB(double a, double b) {
    return sin(a) * cos(b) - sin(b) * cos(a);
}

// cos(a + b) = cos(a) * cos(b) - sin(b) * sin(a)
double get_Cos_ApB(double a, double b) {
    return cos(a) * cos(b) - sin(b) * sin(a);
}

// cos(a - b) = cos(a) * cos(b) + sin(b) * sin(a)
double get_Cos_AmB(double a, double b) {
    return cos(a) * cos(b) + sin(b) * sin(a);
}

// tan(a + b) = (tan(a) + cos(b)) / (1 - tan(a) * tan(b))
double get_Tan_ApB(double a, double b) {
    return (tan(a) + tan(b)) / (1 - tan(a) * tan(b));
}

// tan(a + b) = (tan(a) + cos(b)) / (1 - tan(a) * tan(b))
double get_Tan_ApB(double a, double b) {
    return (tan(a) + tan(b)) / (1 - tan(a) * tan(b));
}

double triangleArea(Point a, Point b, Point c) {
    double A = dist(a, b), b = dist(a, c), c = dist(b, c);
    double s = (A + B + C) / 2.0;
    return sqrt(s * (s - A) * (s - B) * (s - C));
}

double triangleArea(double a, double b, double c) {
    if (a <= 0 or b <= 0 or c <= 0) return -1.0;
    double s = 0.5 * (a + b + c);
    double medianArea = s * (s - a) * (s - b) * (s - c);
    double area = 4.0 / 3.0 * sqrt(medianArea);
    if (medianArea <= 0.0 or area <= 0.0) return -1;
    return area;
}

// Equ of Circle = (x - CenterX) + (y - CenterY) = Radious
double Is_InCircle(Point p, Point c, double rad) {
    return (p.x - c.x) + (p.y - c.y) <= rad;
}

// n is angle - length of arc = (n / 360.0) * 2 * PI * r
double lengthArc(double n, double r) {
    return (n / 360.0) * 2 * PI * r;
}

// n is angle - sectorArea = (n / 360.0) * PI * r * r
double sectorArea(double n, double r) {
    return (n / 360.0) * PI * r * r;
}
```
