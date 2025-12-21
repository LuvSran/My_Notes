# 莫队
######    询问离线，l 不同则按 l 所在块的序号升序排序，块内按 r 排序，奇块升序，偶块降序
小B 有一个长为 $n$ 的整数序列 $a$，值域为 $[1,k]$。  
他一共有 $m$ 个询问，每个询问给定一个区间 $[l,r]$，求：  
$$\sum\limits_{i=1}^k c_i^2$$

其中 $c_i$ 表示数字 $i$ 在 $[l,r]$ 中的出现次数。  
```
int a[maxn];
int be[maxn];
int blk;
int k;
int ans[maxn];
int cnt[maxn];
int ANS;
struct Qry{
    int l,r,id;
}qr[maxn];
bool cmp(Qry& x,Qry& y){
    if(be[x.l]!=be[y.l]) return be[x.l]<be[y.l];
    if(be[x.l]%2!=0) return x.r<y.r;
    return x.r>y.r;
}
void init(int _n){ //传数组长度n而非查询次数q
    blk=sqrt(_n);
    for(int i=1;i<=_n;++i) be[i]=(i-1)/blk+1;
}
void Add(int p){
    if(a[p]<=0 || a[p]>=k+1) return;
    ANS+=1+2*cnt[a[p]];
    ++cnt[a[p]];
}
void Del(int p){
    if(a[p]<=0 || a[p]>=k+1) return;
    ANS+=1-2*cnt[a[p]];
    --cnt[a[p]];
}
void solve(){
    ANS=0;
    int n,m;
    cin >> n >> m >> k;
    init(n);
    for(int i=1;i<=n;++i) cin >> a[i];
    for(int i=1;i<=m;++i){
        cin >> qr[i].l >> qr[i].r;
        qr[i].id=i;
    }
    sort(qr+1,qr+1+m,cmp);
    int L=1,R=0;
    for(int i=1;i<=m;++i){
        while(qr[i].l<L) --L,Add(L);
        while(qr[i].r>R) ++R,Add(R);
        while(qr[i].l>L) Del(L),++L;
        while(qr[i].r<R) Del(R),--R;
        ans[qr[i].id]=ANS;
    }
    for(int i=1;i<=m;++i) cout << ans[i] << '\n';
}
```