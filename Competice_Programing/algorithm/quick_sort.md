*快排模板*
```cpp
void quick_sort(int a[],int l,int r){
if(l>=r) return;
int mid= l+r >> 1,i=l-1,j=r+1;
while(i<j){
do i++;while(a[i]<a[mid]);
do j--;while(a[j]>a[mid]);
if(i<j) swap(a[i],a[j]);
}
quick_sort(a[],l,j);
quick_sort(a[],j+1,r);

}

```