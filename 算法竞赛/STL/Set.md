```
struct Z{
    int x,y;
    bool operator<(const Z& t)const{
        return x<t.x; //this排在t之前，即x小的接近begin()
    }
};
```