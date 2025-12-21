判断三点是否贡献
```cpp
bool same_line(ll x1,ll y1,ll x2,ll y2,ll x3,ll y3){
    return (x2-x1)*(y3-y1)-(x3-x1)*(y2-y1)==0;  //叉积为0共线
}
```
由`(x1,y1),(x2,y2)`得`ax+by+c=0`，求`a,b,c`
```cpp
 ll a=y2-y1,b=x1-x2,c=x2*y1-x1*y2;
```
由`(x1,y1),(x2,y2),(x3,y1)`组成的三角形面积
```cpp
ll Area(ll x1,ll y1,ll x2,ll y2,ll x3,ll y3){
	 return ABS((x2-x1)*(y3-y1)-(x3-x1)*(y2-y1))/2;
}
```