# 分块
区间加，区间求和
```cpp
const int maxn=1e5+20;
int L[maxn],R[maxn]; //第i块的左/右端点
int tag[maxn]; //第i块的标记
int sum[maxn]; //第i块的总和
int be[maxn]; //a[i]属于哪一块
int blk,tot; //块长,总块数
int a[maxn];
void init(int _n){
    blk=sqrt(_n);
    tot=_n/blk+(_n%blk!=0);
    for(int i=1;i<=tot;++i){
        L[i]=(i-1)*blk+1;
        R[i]=min(_n,i*blk);
        for(int j=L[i];j<=R[i];++j) be[j]=i;
    }
    for(int i=1;i<=tot;++i){
        sum[i]=0; tag[i]=0;
        for(int j=L[i];j<=R[i];++j) sum[i]+=a[j];
    }
}
void fix(int lo,int ro,int x){
    int p=be[lo],q=be[ro];
    if(p==q){
        for(int i=lo;i<=ro;++i) a[i]+=x;
        sum[p]+=(ro-lo+1)*x;
        return;
    }
    else{
        for(int i=p+1;i<=q-1;++i) tag[i]+=x;
        for(int i=lo;i<=R[p];++i) a[i]+=x;
        sum[p]+=(R[p]-lo+1)*x;
        for(int i=L[q];i<=ro;++i) a[i]+=x;
        sum[q]+=(ro-L[q]+1)*x;
    }
}
ll ask(int lo,int ro){
    int p=be[lo],q=be[ro];
    if(p==q){
        ll res=0;
        for(int i=lo;i<=ro;++i) res+=a[i];
        res+=tag[p]*(ro-lo+1);
        return res;
    }
    else{
        ll res=0;
        for(int i=p+1;i<=q-1;++i) res+=sum[i]+tag[i]*(R[i]-L[i]+1);
        for(int i=lo;i<=R[p];++i) res+=a[i];
        res+=(R[p]-lo+1)*tag[p];
        for(int i=L[q];i<=ro;++i) res+=a[i];
        res+=(ro-L[q]+1)*tag[q];
        return res;
    }
}
```
区间加乘，区间求和
```
int L[maxn],R[maxn];
ll add[maxn],mul[maxn];
ll sum[maxn];
int be[maxn];
int blk,tot;
ll a[maxn];
ll mod=571373;
void init(int _n){
    blk=sqrtl(_n);
    tot=_n/blk+(_n%blk!=0);
    for(int i=1;i<=tot;++i){
        L[i]=(i-1)*blk+1,R[i]=min(_n,i*blk);
        mul[i]=1;
        for(int j=L[i];j<=R[i];++j){sum[i]+=a[j],be[j]=i;}
    }
}
void cg(int id){  //整块直接改tag和sum,碎块重构a[i]并获取新sum
    for(int i=L[id];i<=R[id];++i) a[i]=(a[i]*mul[id])%mod,a[i]=(a[i]+add[id])%mod;
    mul[id]=1,add[id]=0;
    sum[id]=0;
    for(int i=L[id];i<=R[id];++i) sum[id]=(sum[id]+a[i])%mod;
}
void fix_mul(int lo,int ro,int k){
    int p=be[lo],q=be[ro];
    if(p==q){
        cg(p);
        for(int i=lo;i<=ro;++i) sum[p]=(sum[p]+(k-1)*a[i])%mod,a[i]=(a[i]*k)%mod;
        return;
    }
    else{
        cg(p);
        for(int i=lo;i<=R[p];++i) sum[p]=(sum[p]+(k-1)*a[i])%mod,a[i]=a[i]*k%mod;
        cg(q);
        for(int i=L[q];i<=ro;++i) sum[q]=(sum[q]+(k-1)*a[i])%mod,a[i]=a[i]*k%mod;
        for(int i=p+1;i<=q-1;++i){
            mul[i]=mul[i]*k%mod;
            add[i]=add[i]*k%mod; //err
            sum[i]=sum[i]*k%mod;
        }
        return;
    }
}
void fix_add(int lo,int ro,int k){
    int p=be[lo],q=be[ro];
    if(p==q){
        cg(p);
        for(int i=lo;i<=ro;++i) a[i]=(a[i]+k)%mod;
        sum[p]=(sum[p]+k*(ro-lo+1))%mod;
        return;
    }
    else{
        cg(p);
        for(int i=lo;i<=R[p];++i) a[i]=(a[i]+k)%mod;
        sum[p]=(sum[p]+(R[p]-lo+1)*k)%mod;
        cg(q);
        for(int i=L[q];i<=ro;++i) a[i]=(a[i]+k)%mod;
        sum[q]=(sum[q]+(ro-L[q]+1)*k)%mod;
        for(int i=p+1;i<=q-1;++i){
            add[i]=(add[i]+k)%mod;
            sum[i]=(sum[i]+k*(R[i]-L[i]+1))%mod;
        }
    }
}
ll ask(int lo,int ro){
    int p=be[lo],q=be[ro];
    if(p==q){
        cg(p);
        ll res=0;
        for(int i=lo;i<=ro;++i) res=(res+a[i])%mod;
        return res;
    }
    else{
        ll res=0;
        cg(p);
        for(int i=lo;i<=R[p];++i) res=(res+a[i])%mod;
        cg(q);
        for(int i=L[q];i<=ro;++i) res=(res+a[i])%mod;
        for(int i=p+1;i<=q-1;++i) res=(res+sum[i])%mod;
        return res;
    }
}
```
区间加，区间查询多少数大于C
```
int L[maxn],R[maxn];
int add[maxn];
int be[maxn];
int blk,tot;
int raw[maxn],a[maxn]; //原始数组，副本
void init(int _n){
    blk=sqrt(_n); tot=_n/blk+(_n%blk!=0);
    for(int i=1;i<=tot;++i){
        L[i]=(i-1)*blk+1,R[i]=min(_n,i*blk);
        add[i]=0;
        for(int j=L[i];j<=R[i];++j) be[j]=i,a[j]=raw[j];
    }
}
void cg(int id){
    for(int i=L[id];i<=R[id];++i) raw[i]+=add[id];
    for(int i=L[id];i<=R[id];++i) a[i]=raw[i];
    add[id]=0;
}
void fix(int lo,int ro,int k){
    int p=be[lo],q=be[ro];
    if(p==q){
        cg(p);
        for(int i=lo;i<=ro;++i) raw[i]+=k,a[i]+=k; 
        sort(a+L[p],a+R[p]+1);
        return;
    }
    else{
        cg(p);
        for(int i=lo;i<=R[p];++i) raw[i]+=k,a[i]+=k;
        sort(a+L[p],a+R[p]+1);
        cg(q);
        for(int i=L[q];i<=ro;++i) raw[i]+=k,a[i]+=k;
        sort(a+L[q],a+R[q]+1);
        for(int i=p+1;i<=q-1;++i) add[i]+=k;
    }
}
int ask(int lo,int ro,int c){
    int p=be[lo],q=be[ro];
    if(p==q){
        int res=0;
        for(int i=lo;i<=ro;++i) res+=(raw[i]+add[p]>=c);
        return res;
    }
    else{
        int res=0;
        for(int i=lo;i<=R[p];++i) res+=(raw[i]+add[p]>=c);
        for(int i=L[q];i<=ro;++i) res+=(raw[i]+add[q]>=c);
        for(int i=p+1;i<=q-1;++i){
            int pos=lower_bound(a+L[i],a+R[i]+1,c-add[i])-(a+L[i])+1;
            res+=R[i]-L[i]+1-pos+1;
        }
        return res;
    }
}
```