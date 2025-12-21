*二分模板*
***整数1***
```cpp
int bsearch(int l,int r){
while(l<r){
int mid= l+r >> 1;
if(check(mid)) r=mid;
else l=mid+1;
}
return l;
}
```
***整数2***
```cpp
int bsearch(int l,int r){
while(l<r){
int mid= l+r+1 >> 1;
if(check(mid)) l=mid;
else r=mid-1;
}
return l;
}
```
***浮点二分（不考虑+1问题）***
```cpp
while(r-l>1e-8){
int mid=l+r >> 1;
if(check(mid))r=mid;
else l=mid;
}


```