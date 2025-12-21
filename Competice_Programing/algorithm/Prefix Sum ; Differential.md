# 等差数列二阶差分
静态
```cpp
struct Dif{
    vector<ll> res;
    int nn;
    Dif(int _n){ nn=_n;res.assign(_n+5,0);}
    void Add(int32_t l,int32_t r,ll s,ll d){ //l~r范围内加上首项s,公差d的等差数列
        ll e=s+1ll*(r-l)*d;
        res[l]+=s;
        res[l+1]+=d-s;
        res[r+1]-=d+e;
        res[r+2]+=e;
    }
    vector<ll> Get(){
        for(int i=1;i<=nn;++i) res[i]+=res[i-1];
        for(int i=1;i<=nn;++i) res[i]+=res[i-1];
        return res;
    }
};
```
动态(线段树维护)(log修log查单点)
```cpp
template<int N>struct Dmc_Dif{
    vector<ll> dif;
    int nn;
    struct{
        int l,r,len;
        ll sum;
        ll lz;
    }tr[4*N];
    void push_up(int p){
        tr[p].sum=tr[ls].sum+tr[rs].sum;
    }
    void push_down(int p){
        ll& tag=tr[p].lz;
        if(tag==0) return;
        tr[ls].sum+=tag*tr[ls].len;
        tr[ls].lz+=tag;
        tr[rs].sum+=tag*tr[rs].len;
        tr[rs].lz+=tag;
        tag=0;
    }
    void build(int p,int lo,int ro){
        tr[p].l=lo,tr[p].r=ro;
        tr[p].lz=0,tr[p].len=ro-lo+1;
        if(lo==ro){
            tr[p].sum=0;
            return;
        }
        int mid=(lo+ro)>>1;
        build(ls,lo,mid);
        build(rs,mid+1,ro);
    }
    void Fix(int p,int lo,int ro,ll k){  
        if(lo<=tr[p].l && ro>=tr[p].r){
            tr[p].sum+=k*tr[p].len;
            tr[p].lz+=k;
            return;
        }
        push_down(p);
        if(lo<=tr[ls].r) Fix(ls,lo,ro,k);
        if(ro>=tr[rs].l) Fix(rs,lo,ro,k);
        push_up(p);
    }
    ll Ask(int p,int lo,int ro){
        if(lo<=tr[p].l && ro>=tr[p].r) return tr[p].sum;
        push_down(p);
        int ndmid=(tr[p].l+tr[p].r)>>1;
        ll s=0;
        if(lo<=tr[ls].r) s+=Ask(ls,lo,ro);
        if(ro>=tr[rs].l) s+=Ask(rs,lo,ro);
        return s;
    }
    void Init(int _n){ //初始化一个长n的全0序列
        nn=_n;
        build(1,1,nn+5);
        dif.assign(nn+5,0);
    }
    void Add(i32 l,i32 r,ll s,ll d){ //l~r范围内加上首项s，末项e,公差d的等差数列
        Fix(1,l,l,s);
        Fix(1,l+1,r,d);
        Fix(1,r+1,r+1,-s-(r-l)*d);
    }
    ll Point(int _x){return Ask(1,1,_x);} //查询_x单点值
    vector<ll> Get(){ //查询整个序列
        for(int i=1;i<=nn;++i) dif[i]=Ask(1,1,i);
        return dif;
    }
};
Dmc_Dif<maxn> T;
```
# 二维前缀和
求s，可以一维一维加
```cpp
for(int i=1;i<=n;++i)          //求二维s
	for(int j=1;j<=n;++j) s[i][j]=a[i][j];
for(int i=1;i<=n;++i)
	for(int j=1;j<=n;++j) s[i][j]+=s[i-1][j];
for(int i=1;i<=n;++i)
	for(int j=1;j<=n;++j) s[i][j]+=s[i][j-1];
```

# 三维
```cpp
s[i][j][k]=s[i-1][j][k]
+s[i][j-1][k]
+s[i][j][k-1]
-s[i-1][j-1][k]
-s[i-1][j][k-1]
-s[i][j-1][k-1]
+s[i-1][j-1][k-1]
+a[i][j][k];

sum[l1~r1][l2~r2][l3~r3]=s[r1][r2][r3]
-s[r1][r2][l3-1]
-s[r1][l2-1][l3]
-s[l1-1][r2][r3]
+s[l1-1][l2-1][r3]
+s[l1-1][r2][l3-1]
+s[r1][l2-1][l3-1]
-s[l1-1][l2-1][l3-1];
```
# 二维
求左上顶点(x1,y1)到右下顶点(x2,y2)的矩阵元素和sum函数：
```cpp
int sum(int x1,int y1,int x2,int y2){ //左上角，右下角
	return s[x2][y2]-s[x1-1][y2]-s[x2][y1-1]+s[x1-1][y1-1];
}
```
求s(i,j)
```cpp
for(int i=1;i<=n;i++){
		for(int j=1;j<=m;j++) s[i][j]=mat[i][j]+s[i-1][j]+s[i][j-1]-s[i-1][j-1];
	}
```
![[06c99ca0f54d2503ad3b54e158cc9c2 1.jpg]]
# 二维差分
add函数，将原矩阵mat中左上顶点(x1,y1)到右下顶点(x2,y2)的元素加上值k
```cpp
void add(int x1,int y1,int x2,int y2,int k){
	c[x1][y1]+=k;
	c[x2+1][y1]-=k;
	c[x1][y2+1]-=k;
	c[x2+1][y2+1]+=k;
}
```
求差分矩阵c(i,j)，`add(i,j,i,j,mat[i,j])`，即将mat(i,j)看作0，调整差分矩阵使其变为原mat(i,j)
```cpp
for(int i=1;i<=n;i++){
       for(int j=1;j<=m;j++){
           cin >> mat[i][j];
           add(i,j,i,j,mat[i][j]);
       }
   }
```
用差分矩阵c还原矩阵mat: `mat[i][j]=mat[i-1][j]+mat[i][j-1]-mat[i-1][j-1]+c[i][j];`,即差分矩阵除去(i,j)点的前缀和加上差分c(i,j)
```cpp
for(int i=1;i<=n;i++){
		for(int j=1;j<=n;j++){
			mat[i][j]=mat[i-1][j]+mat[i][j-1]-mat[i-1][j-1]+c[i][j];
			cout << mat[i][j] << ' ';
		}
		cout << '\n';
	}
```
![[8a8c092ff1df8e7f8f2feb34c557975.jpg]]