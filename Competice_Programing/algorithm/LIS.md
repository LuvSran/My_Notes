求最长上升子序列长度
```cpp
int a[MN];
int f[MN];
inline void solve(){
	int n;
	cin >> n;
	for(int i=1;i<=n;i++) cin >> a[i];
	f[0]=-2*1e9;
    int len=0;
    for(int i=1;i<=n;i++){
        int l=0,r=len;
        while(l<r){
            int mid=(l+r+1)>>1;
            if(f[mid]<a[i]) l=mid; //若是求最长不降子序列，则f[mid]<=a[i]
            else r=mid-1;
        }
        len=max(len,l+1);
        f[l+1]=a[i];
    }
    cout << len;
	return;
}
```
保存最长上升子序列,加一个`ans[]`
```cpp
int a[MN];
int f[MN];
string ans[MN];
inline void solve(){
	int n;
	cin >> n;
	for(int i=1;i<=n;i++) cin >> a[i];
	f[0]=-2*1e9;
    int len=0;
    for(int i=1;i<=n;i++){
        int l=0,r=len;
        while(l<r){
            int mid=l+r+1 >> 1;
            if(f[mid]<a[i]) l=mid;
            else r=mid-1;
        }
        len=max(len,l+1);
        f[l+1]=a[i];
        char ch=a[i]+'0';
        ans[l+1]=ans[l]+ch;
    }
    cout << len;
    cout << '\n' << ans[len];
	return;
}
```