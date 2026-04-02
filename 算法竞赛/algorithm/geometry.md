判断三点是否共线
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
求两个矩形交集的最大正方形面积
```cpp
auto work=[&](int x1,int y1,int x2,int y2,int u1,int v1,int u2,int v2){
 //矩形1:左下角(x1,y1),右上角(x2,y2)。矩形2:左下角(u1,v1),右上角(u2,v2)
     if(x1>u1) swap(x1,u1),swap(y1,v1),swap(x2,u2),swap(y2,v2);
     if(x2<=u1) return 0ll;
     if(v1>=y2 || v2<=y1) return 0ll;
     int l=min(u2,x2)-u1;
     l=min(l,min(y2,v2)-max(y1,v1));
     return 1ll*l*l;
};
```