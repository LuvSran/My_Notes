*归并排序模板*
```cpp
const int N=1e5+10;
int a[N];
int tmp[N];
void merge_sort(int a[],int l,int r){
if(l>=r) return;
int mid= l+r >> 1,i=l,j=mid+1;
merge_sort(a[],l,mid);
merge_sort(a[],mid+1,r);
int k=0;
while(i<=mid && j=<r){
if(a[i]<=a[mid]) tmp[k++]=a[i++];
else tmp[k++]=a[j++];
}
while(i<=mid) tmp[k++]=a[i++];
while(j<=r)   tmp[k++]=a[j++];
for(int i=l,j=0;i<=r;i++) a[i]=tmp[j];
return;
}
```