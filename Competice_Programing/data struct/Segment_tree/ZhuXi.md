# 主席树
```cpp
#define lc(_x) tr[_x].ls
#define rc(_x) tr[_x].rs
int root[maxn];
struct ZX_Tree{
    int _n; //_n为a[i]值域上限，要使用rank()时记得对_n赋值
    int sit=0; //build()之前先把sit赋0
    struct{
        int sum,ls,rs;
    }tr[32*maxn];
    void build(int& ts,int nl,int nr){
        ts=++sit;
        if(nl==nr){
            tr[sit].sum=0;
            return;
        }
        int nmid=(nl+nr)>>1;
        build(lc(ts),nl,nmid);
        build(rc(ts),nmid+1,nr);
    }
    void Build(int l_lim,int r_lim){ //a[i]值域左边界，右边界
	    _n=r_lim;
	    sit=0;
	    build(root[0],l_lim,r_lim);
    }
    void add(int& ts,int his,int node_l,int node_r,int x,int val){ //复制一个his版本，并在x处增加val
        ts=++sit;
        tr[ts]=tr[his];
        tr[ts].sum+=val;
        if(node_l==node_r) return;
        int nmid=(node_l+node_r)>>1;
        if(x<=nmid) add(lc(ts),lc(his),node_l,nmid,x,val);
        else add(rc(ts),rc(his),nmid+1,node_r,x,val);
    }
    int Ask(int id1,int id2,int node_l,int node_r,int k){
        if(node_l==node_r) return node_l;
        int s=tr[lc(id2)].sum-tr[lc(id1)].sum;
        int nmid=(node_l+node_r)>>1;
        if(k<=s) return query(lc(id1),lc(id2),node_l,nmid,k);
        else return query(rc(id1),rc(id2),nmid+1,node_r,k-s);
    }
    int Kth(int l,int r,int k){ //查询l~r区间内第k小
        return Ask(root[l-1],root[r],1,_n,k);
    }
    int ask_sum(int id1,int id2,int node_l,int node_r,int lo,int ro){ //查询lo~ro值域内的数的个数
        if(lo<=node_l && ro>=node_r) return tr[id2].sum-tr[id1].sum;
        int nmid=(node_l+node_r)>>1;
        int s=0;
        if(lo<=nmid) s+=ask_sum(lc(id1),lc(id2),node_l,nmid,lo,ro);
        if(ro>=nmid+1) s+=ask_sum(rc(id1),rc(id2),nmid+1,node_r,lo,ro);
        return s;
    }
    int Rank(int l,int r,int k){    //查询k在l~r内的排名
        return 1+ask_sum(root[l-1],root[r],1,_n,1,k-1); //注意是0~k-1还是1~k-1
    }
};
ZX_Tree T;
solve(){
	T.Build(1,100000)     //a[i]值域范围
    for(int i=1;i<=n;++i){
        T.add(root[i],root[i-1],1,n,a[i],1); //a[i]可能要离散化
    }
}
```