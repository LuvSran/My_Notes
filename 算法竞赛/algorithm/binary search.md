*二分模板*
***整数1***
```cpp
int bsearch(int l,int r){
while(l<r){
	int mid=(l+r)>> 1;
	if(check(mid)==true) r=mid;
	else l=mid+1;
}
return l;
}
```
***整数2***
```cpp
int bsearch(int l,int r){
	while(l<r){
		int mid=(l+r+1) >> 1;
		if(check(mid)==true) l=mid;
		else r=mid-1;
	 }
	return l;
}
```
***浮点一(值域较小)***
```cpp
while(r-l>1e-8){
	int mid=l+r >> 1;
	if(check(mid))r=mid;
	else l=mid;
}
```
*浮点二(值域较大)*
```cpp
using ld = long double;
ld l=0,r=R;
for(int k=1;k<=50;++k){ //先算出循环次数至少为几次,50?60?
     ld mid=(l+r)/2.0;
     if(check(mid)==true) r=mid;
     else l=mid;
}
```