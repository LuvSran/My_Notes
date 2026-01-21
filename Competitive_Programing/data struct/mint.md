```cpp
struct mint {
    int x;
    inline mint(int o = 0) { x = o; }
    inline mint & operator = (int o) { return x = o, *this; }
    inline mint & operator += (mint o) { return (x += o.x) >= mod && (x -= mod), *this; }
    inline mint & operator -= (mint o) { return (x -= o.x) < 0 && (x += mod), *this; }
    inline mint & operator *= (mint o) { return x = (ll) x * o.x % mod, *this; }
    inline mint & operator ^= (int b) {
        mint w = *this;
        mint ret(1);
        for(; b; b >>= 1, w *= w) if(b & 1) ret *= w;
        return x = ret.x, *this;
    }
    inline mint & operator /= (mint o) { return *this *= (o ^= (mod - 2)); }
    friend inline mint operator + (mint a, mint b) { return a += b; }
    friend inline mint operator - (mint a, mint b) { return a -= b; }
    friend inline mint operator * (mint a, mint b) { return a *= b; }
    friend inline mint operator / (mint a, mint b) { return a /= b; }
    friend inline mint operator ^ (mint a, int b) { return a ^= b; }
};
```