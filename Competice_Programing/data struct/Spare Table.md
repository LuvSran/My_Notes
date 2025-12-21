# ST表(o(nlogn)预处理，o(1)查询)
```
template<typename T,int N> struct STB{
	T f[N][25];
	void Build(T t[],int _n){
		for(int i=1;i<=_n;++i) f[i][0]=t[i];
		for(int j=1;(1ll<<j)<=_n;++j){
			for(int i=1;i+(1<<j)-1<=_n;++i){
				f[i][j]=min(f[i][j-1],f[i+(1<<(j-1))][j-1]);
			}
		}
	}
	T Ask(int l,int r){
		int k=__lg(r-l+1);
		return min(f[l][k],f[r-(1ll<<k)+1][k]);
	}
};
STB<200005> T;
```
用于查询具有结合律且可重复贡献的区间信息，例如区间最大/最小值，区间最大公因数/最小公倍数，区间按位与/按位或
`f[i][j]`表示以第`i`个数为起点，长度为`2^j`区间的信息
预处理时f(i)(j)用区间`i~i+2^(j-1)`和区间`i+2^(j-1)~i+2^(j-1)+2^(j-1)`拼凑
*ST表预处理：求f\[i]\[j]*
```
for(int i=1;i<=n;i++) f[i][0]=a[i]; //f[i][0]:区间长度为1
for(int j=1;(1<<j)<=n;++j){ //枚举区间长度
	for(int i=1;i+(1<<j)-1<=n;++i) //枚举区间起点
		f[i][j]=   //具体状态转移
}
```
##### 状态转移
`f[i][j]=max(f[i][j-1],f[i+(1<<(j-1))][j-1]); //区间最大值`
`f[i][j]=min(f[i][j-1],f[i+(1<<(j-1))][j-1]); //区间最小值`
`f[i][j]=f[i][j-1]|f[i+(1<<(j-1))][j-1]; //区间或`
`f[i][j]=f[i][j-1]&f[i+(1<<(j-1))][j-1]; //区间与`
*查询区间信息*
对于区间`[l][r]`，发现`2^k<=r-l+1<=2*2^k` ,因此可用两段长度为`2^k` 的区间重叠拼凑
```
int len=r-l+1;
int k=logg[len];
int res;
res=max(f[l][k],f[r-(1<<k)+1][k])); //区间最大值,r=l+len-1
res=f[l][k]|f[r-(1<<k)+1][k]; //区间或
```
*logg预处理*
重复计算`log2(n)`较慢，开一个`logg[i]`数组表示`log2(i)`，递推：`logg[i]=logg[i/2]+1`
```
int logg[MAXN]; // 存储 log2(i) 的值 
void preprocess() { 
	logg[1] = 0; 
	for (int i = 2; i < MN; i++)
		logg[i] = logg[i / 2] + 1; 
}
```
