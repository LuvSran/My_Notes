# 线段树
```cpp
template<int N,typename T,bool EnableGCD=false> struct Segment_tree {
    //建树 Segment_tree<200000(l~r),int(类型),true(是否需要gcd功能)> Tree;
    #define ls (p<<1)
    #define rs ((p<<1)|1)
    constexpr static T inf = numeric_limits<T>::max();
    struct Info{
        int l, r;
        T mx, mn, sum;
        T lz_add, lz_set;
        bool has_set;
        typename conditional<EnableGCD, T, bool>::type gcd;
    }tr[4 * N];
    void push_up(int p) {
        tr[p].mx = max(tr[ls].mx, tr[rs].mx);
        tr[p].mn = min(tr[ls].mn, tr[rs].mn);
        tr[p].sum = tr[ls].sum + tr[rs].sum;
        if constexpr (EnableGCD) tr[p].gcd = gcd(tr[ls].gcd, tr[rs].gcd);
    }
    void push_down(int p) {
        if (tr[p].has_set) {
            T tag = tr[p].lz_set;
            tr[ls].lz_set = tr[rs].lz_set = tag;
            tr[ls].mx=tr[rs].mx = tag;
            tr[ls].mn=tr[rs].mn = tag;
            tr[ls].sum=tag * (tr[ls].r - tr[ls].l + 1);
            tr[rs].sum=tag * (tr[rs].r - tr[rs].l + 1);
            if constexpr (EnableGCD) tr[ls].gcd = tr[rs].gcd =(tag>=0?tag:-tag);
            tr[ls].lz_add=tr[rs].lz_add = T(0);
            tr[ls].has_set=tr[rs].has_set = true;
            tr[p].lz_set=T(0);
            tr[p].has_set=false;
        }
        if (tr[p].lz_add != T(0)) {
            T tag = tr[p].lz_add;
            tr[ls].lz_add += tag;
            tr[ls].mx += tag;
            tr[ls].mn += tag;
            tr[ls].sum += tag * (tr[ls].r - tr[ls].l + 1);
            tr[rs].lz_add += tag;
            tr[rs].mx += tag;
            tr[rs].mn += tag;
            tr[rs].sum += tag * (tr[rs].r - tr[rs].l + 1);
            tr[p].lz_add = T(0);
        }
    }
    void Build(int p, int lo, int ro, const vector<T>& init = {}) { //建树
        tr[p].l = lo;
        tr[p].r = ro;
        tr[p].lz_add = T(0);
        tr[p].lz_set = T(0);
        tr[p].has_set = false;
        if (lo == ro) {
            T val = init.empty() ? T(0) : init[lo];
            tr[p].mx = tr[p].mn = tr[p].sum = val;
            if constexpr (EnableGCD) tr[p].gcd =(val>=0?val:-val); // GCD总是非负的
            return;
        }
        int mid = (lo + ro) >> 1;
        Build(ls, lo, mid, init);
        Build(rs, mid + 1, ro, init);
        push_up(p);
    }
    T Ask_mx(int p, int lo, int ro) {     //查区间最大
        if (lo <= tr[p].l && ro >= tr[p].r) return tr[p].mx;
        push_down(p);
        int mid = (tr[p].l + tr[p].r) >> 1;
        T res = -inf;
        if (lo <= mid) res = max(res, Ask_mx(ls, lo, ro));
        if (ro > mid) res = max(res, Ask_mx(rs, lo, ro));
        return res;
    }
    T Ask_mn(int p, int lo, int ro) { //查区间最小
        if (lo <= tr[p].l && ro >= tr[p].r) return tr[p].mn;
        push_down(p);
        int mid = (tr[p].l + tr[p].r) >> 1;
        T res = inf;
        if (lo <= mid) res = min(res, Ask_mn(ls, lo, ro));
        if (ro > mid) res = min(res, Ask_mn(rs, lo, ro));
        return res;
    }
    T Ask_sum(int p, int lo, int ro) {  //查区间和
        if (lo <= tr[p].l && ro >= tr[p].r) return tr[p].sum;
        push_down(p);
        int mid = (tr[p].l + tr[p].r) >> 1;
        T res = T(0);
        if (lo <= mid) res += Ask_sum(ls, lo, ro);
        if (ro > mid) res += Ask_sum(rs, lo, ro);
        return res;
    }
    template <bool E = EnableGCD>
    enable_if_t<E, T> Ask_gcd(int p, int lo, int ro) { //查区间gcd
        if (lo <= tr[p].l && ro >= tr[p].r) return tr[p].gcd;
        push_down(p);
        int mid = (tr[p].l + tr[p].r) >> 1;
        T res = T(0);
        if (lo <= mid) res = gcd(res, Ask_gcd(ls, lo, ro));
        if (ro > mid) res = gcd(res, Ask_gcd(rs, lo, ro));
        return res;
    }
    void Add(int p, int lo, int ro, T k) { //区间加
        if (lo <= tr[p].l && ro >= tr[p].r) {
            tr[p].lz_add += k;
            tr[p].mx += k;
            tr[p].mn += k;
            tr[p].sum += k * (tr[p].r - tr[p].l + 1);
            return;
        }
        push_down(p);
        int mid = (tr[p].l + tr[p].r) >> 1;
        if (lo <= mid) Add(ls, lo, ro, k);
        if (ro > mid) Add(rs, lo, ro, k);
        push_up(p);
    }
    void Set(int p, int lo, int ro, T k) { //区间赋值
        if (lo > ro) return;
        if (lo <= tr[p].l && ro >= tr[p].r) {
            tr[p].lz_set = k;
            tr[p].mx = k;
            tr[p].mn = k;
            tr[p].sum = k * (tr[p].r - tr[p].l + 1);
            if constexpr (EnableGCD) tr[p].gcd = (k>=0?k:-k);
            tr[p].lz_add = T(0);
            tr[p].has_set = true;
            return;
        }
        push_down(p);
        int mid = (tr[p].l + tr[p].r) >> 1;
        if (lo <= mid) Set(ls, lo, ro, k);
        if (ro > mid) Set(rs, lo, ro, k);
        push_up(p);
    }
    //建树 Segment_tree<200000(l~r),int(类型),true(是否需要gcd功能,默认false)> Tree;
};
/*************************************************************************/
/*
    Segment_tree<200000,int,false> tree; //(l~r范围,类型,是否需要gcd(不写默认false))
    建树:Build(1,l,r,vector_a)  //vector_a为传vector(类型必须与声明的类型一致),不写则默认所有位置为0
    区间加k: Add(1,l,r,k)
    区间赋值为k：Set(1,l,r,k)
    查区间最大/小值：Ask_mx(1,l,r) Ask_mn(1,l,r);
    查区间gcd: Ask_gcd(1,l,r);
    查区间和：Ask_sum(1,l,r)
*/
/************************************************************************/
```
# 最大子段和
```cpp
template<int N>struct Segment_tree{  
    struct Tree{  
        int l,r;  
        ll lmx=-INF,rmx=-INF,mx=-INF;  //注意2段-INF加起来就会爆ll
        ll sum=-INF;
    }tr[N*4];
    void push_up(Tree& pa,Tree& ls,Tree& rs){
        pa.mx=max(max(ls.mx,rs.mx),ls.rmx+rs.lmx);
        pa.lmx=max(ls.lmx,ls.sum+rs.lmx);
        pa.rmx=max(rs.rmx,rs.sum+ls.rmx);
        pa.sum=ls.sum+rs.sum;
        return;
    }
    void Build(int p,int lo,int ro, const vector<ll>& init = {}){  //传vector,不传默认为0
        tr[p].l=lo,tr[p].r=ro;  
        if(lo==ro){  
            ll _v = init.empty() ? ll(0) : init[lo];
            tr[p].lmx=tr[p].rmx=tr[p].mx=_v;  
            tr[p].sum=_v;
            return;  
        }  
        int mid=(lo+ro)>>1;  
        Build(lson,lo,mid,init);  
        Build(rson,mid+1,ro,init);  
        push_up(tr[p],tr[lson],tr[rson]);
    }  
    void Fix(int p,int k,ll x){ //k点单点修改为x
        if(tr[p].l==tr[p].r){
            tr[p].lmx=tr[p].rmx=tr[p].mx=tr[p].sum=x;
            return;
        }  
        int ndmid=(tr[p].l+tr[p].r)>>1;  
        if(k<=ndmid) Fix(lson,k,x);  
        else  Fix(rson,k,x);  
        push_up(tr[p],tr[lson],tr[rson]);
    }  
    Tree query(int p,int lo,int ro){  
        if(lo<=tr[p].l && ro>=tr[p].r) return tr[p];  
        Tree res,lres,rres;
        int ndmid=(tr[p].l+tr[p].r)>>1;  
        if(lo<=ndmid) lres=query(lson,lo,ro);
        if(ro>ndmid) rres=query(rson,lo,ro);  
        push_up(res,lres,rres);
        return res;
    }  
    ll Ask(int l,int r){ //查l~r的最大子段和
        return query(1,l,r).mx;
    }
};
```
# 区间加，求和
```
#define ls (p<<1)
#define rs ((p<<1)|1)
struct Segment_tree{
	struct{
		int l,r,len;
		ll sum;
		ll lz;
	}tr[4*maxn];
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
			tr[p].sum=a[lo];
			return;
		}
		int mid=(lo+ro)>>1;
		build(ls,lo,mid);
		build(rs,mid+1,ro);
		push_up(p);
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
};
Segment_tree T;
```
# 区间加乘，求和
```
#define lson (p<<1)
#define rson ((p<<1)|1)
const int mod=(1<<31);
int a[MAX];
struct segment_tree{  
	struct{  
		int l,r,len;  
		int add=0,mul=1;  
		int sum=0;  
	}tr[maxn*4];
	segment_tree(){}  
	void build(int p,int lo,int ro){  
		tr[p].l=lo,tr[p].r=ro,tr[p].len=ro-lo+1;  
		tr[p].sum=0; tr[p].add=0; tr[p].mul=1;  
		if(lo==ro){  
			tr[p].sum=a[lo]%mod;  
			return;  
		}  
		int mid=(lo+ro)>>1;  
		build(lson,lo,mid);  
		build(rson,mid+1,ro);  
		tr[p].sum=(tr[lson].sum+tr[rson].sum)%mod;  
	}  
	void push_down(int p){  
		if(tr[p].mul!=1){  
			int tag=tr[p].mul;  
			tr[lson].mul=(tr[lson].mul*tag)%mod;  
			tr[lson].add=(tr[lson].add*tag)%mod;  
			tr[lson].sum=(tr[lson].sum*tag)%mod;  
			tr[rson].mul=(tr[rson].mul*tag)%mod;  
			tr[rson].add=(tr[rson].add*tag)%mod;  
			tr[rson].sum=(tr[rson].sum*tag)%mod;  
			tr[p].mul=1;  
		}  
		if(tr[p].add!=0){  
			int tag=tr[p].add;  
			tr[lson].add=(tr[lson].add+tag)%mod;  
			tr[lson].sum=(tr[lson].sum+tag*tr[lson].len)%mod;  
			tr[rson].add=(tr[rson].add+tag)%mod;  
			tr[rson].sum=(tr[rson].sum+tag*tr[rson].len)%mod;  
			tr[p].add=0;  
		}  
		return;  
	}  
	void modify(int p,int lo,int ro,int plus,int mtp){ //plus,motiply
		if(lo<=tr[p].l && ro>=tr[p].r){  
			tr[p].sum=(tr[p].sum*mtp)%mod;  
			tr[p].sum=(tr[p].sum+plus*tr[p].len)%mod;  
			tr[p].mul=(tr[p].mul*mtp)%mod;  
			tr[p].add=(tr[p].add*mtp)%mod;  
			tr[p].add=(tr[p].add+plus)%mod;  
			return;  
		}  
		push_down(p);  
		int ndmid=(tr[p].l+tr[p].r)>>1;  
		if(lo<=ndmid) modify(lson,lo,ro,plus,mtp);  
		if(ro>ndmid) modify(rson,lo,ro,plus,mtp);  
		tr[p].sum=(tr[lson].sum+tr[rson].sum)%mod;  
	}  
	int query(int p,int lo,int ro){  
		if(lo<=tr[p].l && ro>=tr[p].r) return tr[p].sum%mod;  
		push_down(p);  
		int s=0;  
		int ndmid=(tr[p].l+tr[p].r)>>1;  
		if(lo<=ndmid) s=(s+query(lson,lo,ro))%mod;  
		if(ro>ndmid) s=(s+query(rson,lo,ro))%mod;  
		return s;  
	}  
};  
segment_tree sgt;

```
# 区间加，查区间最大值及其下标
```
template<int N>struct Segment_tree{
	struct{
		int l,r;
		int mx;
	    int ix;
        int lz;
	}tr[4*N];
	void push_up(int p){
        int& mx=tr[p].mx;
        int& ix=tr[p].ix;
		if(tr[ls].mx>tr[rs].mx) mx=tr[ls].mx,ix=tr[ls].ix;
        else mx=tr[rs].mx,ix=tr[rs].ix;
	}
	void push_down(int p){
		int& tag=tr[p].lz;
		if(tag==0) return;
		tr[ls].mx+=tag;
		tr[ls].lz+=tag;
		tr[rs].mx+=tag;
		tr[rs].lz+=tag;
		tag=0;
	}
	void Build(int p,int lo,int ro){
		tr[p].l=lo,tr[p].r=ro;
		tr[p].lz=tr[p].mx=0;
        tr[p].ix=lo;
		if(lo==ro)	return;
		int mid=(lo+ro)>>1;
		Build(ls,lo,mid);
		Build(rs,mid+1,ro);
		push_up(p);
	}
	void Fix(int p,int lo,int ro,int k){  
		if(lo<=tr[p].l && ro>=tr[p].r){
			tr[p].mx+=k;
			tr[p].lz+=k;
			return;
		}
		push_down(p);
		if(lo<=tr[ls].r) Fix(ls,lo,ro,k);
		if(ro>=tr[rs].l) Fix(rs,lo,ro,k);
		push_up(p);
	}
	pii Ask(int p,int lo,int ro){
		if(lo<=tr[p].l && ro>=tr[p].r) return {tr[p].mx,tr[p].ix};
		push_down(p);
		int mx=0,ix=0;
		if(lo<=tr[ls].r){
            auto t=Ask(ls,lo,ro);
            if(t.ft>mx) mx=t.ft,ix=t.sd;
        }
		if(ro>=tr[rs].l){
            auto t=Ask(rs,lo,ro);
            if(t.ft>mx) mx=t.ft,ix=t.sd;
        }
		return {mx,ix};
	}
};
```
# 单点修改，区间求最大/最小
```
struct segment_tree{
	struct{
		int l,r;
		int mx=0;
	}tr[4*MN];
	void build(int p,int lo,int ro){
		tr[p].l=lo,tr[p].r=ro,tr[p].mx=0;
		if(tr[p].l==tr[p].r){tr[p].mx=a[lo]; return;}
		int mid=(lo+ro)>>1;
		build(lson,lo,mid);
		build(rson,mid+1,ro);
		tr[p].mx=max(tr[lson].mx,tr[rson].mx);
	}
	void modify(int p,int x,int k){
		if(tr[p].l==tr[p].r){ tr[p].mx=k; return;}
		int ndmid=(tr[p].l+tr[p].r)>>1;
		if(x<=ndmid) modify(lson,x,k);
		else modify(rson,x,k);
		tr[p].mx=max(tr[lson].mx,tr[rson].mx);
	}
	int query(int p,int lo,int ro){
		if(tr[p].l>=lo && tr[p].r<=ro) return tr[p].mx;
		int rmx=0;
		int ndmid=(tr[p].l+tr[p].r)>>1;
		if(lo<=ndmid) rmx=max(rmx,query(lson,lo,ro));
		if(ro>ndmid) rmx=max(rmx,query(rson,lo,ro));
		return rmx;
	}
};
segment_tree sgt;
```
# 区间bool赋值，单点查询
```
struct segment_tree{    
    struct{  
        int l,r;  
        bool ok; //开局所有元素为false
        int lz; //-1表没有标记
    }tr[4*maxn];  
    void build(int p,int lo,int ro){  
        tr[p].l=lo,tr[p].r=ro;
        tr[p].lz=-1;  
        if(lo==ro){  
           tr[p].ok=false;
           return;  
        }  
        int mid=(lo+ro)>>1;  
        build(lson,lo,mid);  
        build(rson,mid+1,ro);  
    }  
    void push_down(int p){  
        if(tr[p].lz==-1) return;  
        int tag=tr[p].lz;  
        tr[p].lz=-1;
        tr[lson].ok=tag;
        tr[lson].lz=tag;
        tr[rson].lz=tag;
        tr[rson].ok=tag;
        return;  
    }  
    void modify(int p,int lo,int ro,bool k){  
        if(lo<=tr[p].l && ro>=tr[p].r){  
            tr[p].ok=k;  
            tr[p].lz=k;  
            return;  
        }  
        push_down(p);  
        int ndmid=(tr[p].l+tr[p].r)>>1;  
        if(lo<=ndmid) modify(lson,lo,ro,k);  
        if(ro>ndmid) modify(rson,lo,ro,k);  
    }  
    int query(int p,int pos){  
        if(tr[p].l==tr[p].r) return tr[p].ok;
        push_down(p);
        int ndmid=(tr[p].l+tr[p].r)>>1;
        if(pos<=ndmid) return query(lson,pos);
        else return query(rson,pos);
    }  
};
```
# 区间赋正/负，取相反
```
struct Segment_tree{
    struct{
        int l,r;
        int val; //-1负，1正
        int lz; //赋值,-1赋值负，1赋值正，0空
        int rev; //取反，1取反，0空
    }tr[4*maxn];
    void push_down(int p){
        if(tr[p].lz!=0){
            int& tag=tr[p].lz;
            tr[ls].val=tag;
            tr[ls].lz=tag;
            tr[ls].rev=0;
            tr[rs].val=tag;
            tr[rs].lz=tag;
            tr[rs].rev=0;
            tag=0;
        }
        int& r=tr[p].rev;
        tr[ls].rev^=r;
        tr[rs].rev^=r;
        if(r==1) tr[ls].val*=-1,tr[rs].val*=-1;
        r=0;
    }
    void build(int p,int lo,int ro){
        tr[p].l=lo,tr[p].r=ro,tr[p].lz=0,tr[p].rev=0;
        if(lo==ro){
            if(lo<=1e5+1) tr[p].val=-1;
            else tr[p].val=1;
            return;
        }
        int mid=(lo+ro)>>1;
        build(ls,lo,mid);
        build(rs,mid+1,ro);
    }
    ll query(int p,int x){
       if(tr[p].l==tr[p].r) return tr[p].val;
       push_down(p);
       int nmid=(tr[p].l+tr[p].r)>>1;
       if(x<=nmid) return query(ls,x);
       else return query(rs,x);
    }
    void modify(int p,int lo,int ro,int k){ //1赋值正，-1赋值负，2取反
        if(lo<=tr[p].l && ro>=tr[p].r){
            if(k==1 || k==-1){
                tr[p].val=k;
                tr[p].lz=k;
                tr[p].rev=0;
            }
            else if(k==2){
                tr[p].val*=-1;
                tr[p].rev^=1;
            }
            return;
        }
        push_down(p);
        int nmid=(tr[p].l+tr[p].r)>>1;
        if(lo<=nmid) modify(ls,lo,ro,k);
        if(ro>=nmid+1) modify(rs,lo,ro,k);
    }
}tree;
```
# 区间赋值
```
struct segment_tree{     
	struct{  
        int l,r;  
        int numb;  //-INF表杂  
        int lztag; //-INF表无  
        int sum;  
        int len;  
    }tr[4*maxn];  
    inline void push_up(int p){  
        tr[p].sum=tr[lson].sum+tr[rson].sum;  
        if(tr[lson].numb==-INF || tr[rson].numb==-INF) tr[p].numb=-INF;  
        else{  
            if(tr[lson].numb==tr[rson].numb) tr[p].numb=tr[lson].numb;  
            else tr[p].numb=-INF;  
        }  
    }  
    void build(int p,int lo,int ro){  
        tr[p].l=lo,tr[p].r=ro;  
        tr[p].numb=-INF,tr[p].lztag=-INF;  
        tr[p].len=ro-lo+1;  
        if(lo==ro){  
            tr[p].sum=a[lo],tr[p].numb=a[lo];  
            return;  
        }  
        int mid=(lo+ro)>>1;  
        build(lson,lo,mid);  
        build(rson,mid+1,ro);  
        push_up(p);  
    }  
    void push_down(int p){  
        if(tr[p].lztag==-INF) return;  
        int tag=tr[p].lztag;  
        tr[p].lztag=-INF;  
        tr[lson].sum=tag*tr[lson].len;  
        tr[lson].numb=tag;  
        tr[lson].lztag=tag;  
        /******/  
        tr[rson].sum=tag*tr[rson].len;  
        tr[rson].numb=tag;  
        tr[rson].lztag=tag;  
        return;  
    }  
    void modify(int p,int lo,int ro,int k){  
        if(lo<=tr[p].l && ro>=tr[p].r){  
            tr[p].numb=k;  
            tr[p].lztag=k;  
            tr[p].sum=k*tr[p].len;  
            return;  
        }  
        push_down(p);  
        int ndmid=(tr[p].l+tr[p].r)>>1;  
        if(lo<=ndmid) modify(lson,lo,ro,k);  
        if(ro>ndmid) modify(rson,lo,ro,k);  
        push_up(p);  
    }  
    int query(int p,int lo,int ro){  
        if(lo<=tr[p].l && ro>=tr[p].r) return tr[p].sum;  
        push_down(p);  
        int s=0;  
        int ndmid=(tr[p].l+tr[p].r)>>1;  
        if(lo<=ndmid) s+=query(lson,lo,ro);  
        if(ro>ndmid) s+=query(rson,lo,ro);  
        return s;  
    }  
};  segment_tree sgt;
```
# 区间开根
```
#define lson (p<<1)
#define rson ((p<<1)|1)
struct segment_tree{  
    struct{  
        int l,r;  
        int mn,mx;  
        int lz=0;  
        int sum=0;  
    }tr[4*maxn];        
    inline void push_up(int p){  
        tr[p].mx=max(tr[lson].mx,tr[rson].mx);  
        tr[p].mn=min(tr[lson].mn,tr[rson].mn);  
        tr[p].sum=tr[lson].sum+tr[rson].sum;  
    }  
    void build(int p,int lo,int ro){  
        tr[p].l=lo,tr[p].r=ro;  
        tr[p].lz=0;  
        if(lo==ro){  
            tr[p].mx=tr[p].mn=a[lo];  
            tr[p].sum=a[lo];  
            return;  
        }  
        int mid=(lo+ro)>>1;  
        build(lson,lo,mid);  
        build(rson,mid+1,ro);  
        push_up(p);  
    }  
    void push_down(int p){  
        if(tr[p].lz==0) return;  
        int tag=tr[p].lz;  
        tr[lson].lz+=tag;  
        tr[lson].sum-=tag*(tr[lson].r-tr[lson].l+1);  
        tr[lson].mx-=tag,tr[lson].mn-=tag;  
        tr[rson].lz+=tag;  
        tr[rson].sum-=tag*(tr[rson].r-tr[rson].l+1);  
        tr[rson].mx-=tag,tr[rson].mn-=tag;  
        tr[p].lz=0;  
    }  
    void modify_sqrt(int p,int lo,int ro){   //区间每个元素开根  
        if(lo<=tr[p].l && ro>=tr[p].r && (tr[p].mn-(int)sqrt(tr[p].mn))==(tr[p].mx-(int)sqrt(tr[p].mx))){  
            int tag=tr[p].mn-(int)sqrt(tr[p].mn);  
            tr[p].lz+=tag;  
            tr[p].sum-=tag*(tr[p].r-tr[p].l+1);  
            tr[p].mx-=tag;  
            tr[p].mn-=tag;  
            return;  
        }  
        push_down(p);  
        int ndmid=(tr[p].l+tr[p].r)>>1;  
        if(lo<=ndmid) modify_sqrt(lson,lo,ro);  
        if(ro>ndmid) modify_sqrt(rson,lo,ro);  
        push_up(p);  
    }  
    int query(int p,int lo,int ro){   //查询区间和  
        if(lo<=tr[p].l && ro>=tr[p].r) return tr[p].sum;  
        push_down(p);  
        int ndmid=(tr[p].l+tr[p].r)>>1;  
        int res=0;  
        if(lo<=ndmid) res+=query(lson,lo,ro);  
        if(ro>ndmid) res+=query(rson,lo,ro);  
        return res;  
    }  
};  
segment_tree sgt;
```
# 状压区间染色
```
bitset<MAX_color> ept;  
bitset<MAX_color> ans;  
void init(){  
    ans.reset(); ept.reset();  
}  
struct segment_tree{      
	struct{  
        int l,r;  
        bitset<MAX_color> color;  
        int lztag=0;  
    }tr[4*MAX];  
    void build(int p,int lo,int ro){  
        tr[p].color=ept;  //初始化将初始颜色位设为1
        tr[p].l=lo,tr[p].r=ro,tr[p].color[1]=1;  
        if(tr[p].r==tr[p].l) return;  
        int mid=(lo+ro)>>1;  
        build(lson,lo,mid);  
        build(rson,mid+1,ro);  
    }  
    void push_down(int p){  
        if(tr[p].lztag==0) return;  
        int tag=tr[p].lztag;  
        tr[lson].color=ept; tr[lson].color[tag]=1;  
        tr[rson].color=ept; tr[rson].color[tag]=1;  
        tr[lson].lztag=tag;  
        tr[rson].lztag=tag;  
        tr[p].lztag=0;  
    }  
    void modify(int p,int lo,int ro,int k){  
        if(tr[p].l>=lo && tr[p].r<=ro){  
            tr[p].color=ept;tr[p].lztag=k;  
            tr[p].color[k]=1;  
            return;  
        }  
        push_down(p);  
        int ndmid=(tr[p].l+tr[p].r)>>1;  
        if(lo<=ndmid) modify(lson,lo,ro,k);  
        if(ro>ndmid) modify(rson,lo,ro,k);  
        tr[p].color=tr[lson].color|tr[rson].color;  
    }  
    void query(int p,int lo,int ro){  
        if(tr[p].l>=lo && tr[p].r<=ro){  
            ans|=tr[p].color;  
            return;  
        }  
        push_down(p);  
        int ndmid=(tr[p].l+tr[p].r)>>1;  
        if(lo<=ndmid) query(lson,lo,ro);  
        if(ro>ndmid) query(rson,lo,ro);  
    }  
};  
segment_tree sgt;
```
# 01序列操作，区间赋值，取反，查询区间0/1数量，连续0/1最大长度
```
int a[maxn];  
struct segment_tree{  
    struct Tree{  
        int l,r,len;  
        int sum1=0;  
        int mx1=0,lmx1=0,rmx1=0;  //mx1最长连续1长度  
        int sum0=0;  
        int mx0=0,lmx0=0,rmx0=0;   //mx0最长连续0长度  
        int lz=-1;  //赋值  
        int xo=0;  //1取反  
    }tr[4*maxn];  
    void push_up(Tree &p,Tree& l,Tree& r){  
        p.sum1=l.sum1+r.sum1;  
        p.mx1=max(l.mx1,r.mx1);  
        p.mx1=max(p.mx1,l.rmx1+r.lmx1);  
        p.lmx1=l.lmx1+(l.lmx1==l.len)*r.lmx1;  
        p.rmx1=r.rmx1+(r.rmx1==r.len)*l.rmx1;  
        
        p.sum0=l.sum0+r.sum0;  
        p.mx0=max(l.mx0,r.mx0);  
        p.mx0=max(p.mx0,l.rmx0+r.lmx0);  
        p.lmx0=l.lmx0+(l.lmx0==l.len)*r.lmx0;  
        p.rmx0=r.rmx0+(r.rmx0==r.len)*l.rmx0;  
    }  
    void push_down(int p){  
        if(tr[p].lz==-1 && tr[p].xo==0) return;  
        int tag1=tr[p].lz,tag2=tr[p].xo;  
        tr[p].lz=-1,tr[p].xo=0;  
        if(tag1!=-1){  
            update(lson,tag1);  
            update(rson,tag1);  
        }  
        if(tag2==1){  
            rev(tr[lson]);  
            rev(tr[rson]);  
        }  
    }  
    inline void rev(Tree& p){  
        swap(p.sum1,p.sum0); swap(p.mx1,p.mx0);  
        swap(p.lmx1,p.lmx0); swap(p.rmx1,p.rmx0);  
        p.xo^=1;  
    }  
    void update(int p,int k){  
        Tree& t=tr[p];  
        t.mx0=t.lmx0=t.rmx0=t.sum0=t.len*(k==0);  
        t.mx1=t.lmx1=t.rmx1=t.sum1=t.len*(k!=0);  
        t.lz=k,t.xo=0;  
        return;  
    }  
    void build(int p,int lo,int ro){  
        tr[p].l=lo,tr[p].r=ro;  
        tr[p].len=ro-lo+1;  
        tr[p].xo=0,tr[p].lz=-1;  
        if(ro==lo){  
            tr[p].sum1=tr[p].mx1=tr[p].lmx1=tr[p].rmx1=a[lo];  
            tr[p].sum0=tr[p].mx0=tr[p].lmx0=tr[p].rmx0=a[lo]^1;  
            return;  
        }  
        int mid=(lo+ro)>>1;  
        build(lson,lo,mid);  
        build(rson,mid+1,ro);  
        push_up(tr[p],tr[lson],tr[rson]);  
    }  
    void modify(int p,int lo,int ro,int k){  //k==2表取反  
        if(lo<=tr[p].l && ro>=tr[p].r){  
            if(k==0) update(p,0);  
            else if(k==1) update(p,1);  
            else if(k==2) rev(tr[p]);  
            return;  
        }  
        push_down(p);  
        int ndmid=(tr[p].l+tr[p].r)>>1;  
        if(lo<=ndmid) modify(lson,lo,ro,k);  
        if(ro>ndmid) modify(rson,lo,ro,k);  
        push_up(tr[p],tr[lson],tr[rson]);  
    }  
    Tree query(int p,int lo,int ro){  
        if(lo<=tr[p].l && ro>=tr[p].r) return tr[p];  
        push_down(p);  
        Tree t,lt,rt;  
        int ndmid=(tr[p].l+tr[p].r)>>1;  
        if(lo<=ndmid) lt=query(lson,lo,ro);  
        if(ro>ndmid) rt=query(rson,lo,ro);  
        push_up(t,lt,rt);  
        return t;  
    }  
};  
segment_tree sgt;
```