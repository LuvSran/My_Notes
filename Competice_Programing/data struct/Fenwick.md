# 树状数组
## 点修区查
```cpp
#define lowbit(x) (x&(-x))
template<int N> struct Fenwick{
    ll f[N+5];  
    int _n;
    ll s0=0;
    void Init(int _N){
        s0=0;          //特判0下标的值
        _n=_N;
        fill(f,f+_N+3,0);
    }  
    void Fix(int ps,ll k){  //ps单点加k
        if(ps==0){
            s0+=k;
            return;
        }
        while(ps<=_n){  
            f[ps]+=k;  
            ps+=lowbit(ps);  
        }  
    }  
    ll Ask(int ps){   //查询第0~ps元素之和
        if(ps==0){
            return s0;
        }
        ll res=0;  
        while(ps>0){   //注意>0而非>=0
            res+=f[ps];  
            ps-=lowbit(ps);  
        }  
        return res+s0;
    }  
    ll range_Ask(int lo,int ro){  
	    if(lo==0) return Ask(ro);
        return Ask(ro)-Ask(lo-1);  
    }
};
```



# 区修点查
```
int a[maxn];
struct Fenwick{
    int ft[maxn];  
    int _n;  //注意_n赋值
    void init(int n){
	    _n=n;
        fill(ft,ft+3+_n,0);
        for(int i=1;i<=_n;++i) fix(i,a[i]-a[i-1]);
    }
    void fix(int ps,int k){  
        while(ps<=_n){  
            ft[ps]+=k;  
            ps+=lowbit(ps);  
        }  
    }  
    void range_modify(int p1,int p2,int k){
        fix(p1,k);
        fix(p2+1,-k);
    }
    int query(int ps){   //查询点ps
        int res=0;  
        while(ps>0){   //注意>0而非>=0
            res+=ft[ps];  
            ps-=lowbit(ps);  
        }  
        return res;  
    }  
};
```