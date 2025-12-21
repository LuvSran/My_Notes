# 二维数点
```cpp
const int maxn=1e6+20;
template<int N> struct Fenwick{
    ll f[N+5];  
    int _n;
    int s0=0;
    void Init(int _N){
        s0=0;          //特判0下标的值
        _n=_N;
        fill(f,f+_N,0);
    }  
    void Fix(int ps,int k){  //ps单点加k
        if(ps==0){
            s0+=k;
            return;
        }
        while(ps<=_n){  
            f[ps]+=k;  
            ps+=lowbit(ps);  
        }  
    }  
    ll Ask(int ps){   //查询第1~ps元素之和
        if(ps==0){
            return s0;
        }
        int res=0;  
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
Fenwick<maxn> T;
struct Z{
    int x,y,id,sgn; //id为-1表(x,y)+1
};
bool cmp(Z& t1,Z& t2){
    if(t1.x!=t2.x) return t1.x<t2.x;
    return t1.id<t2.id; //x相同时，加点需排在查询前面
}
void solve(){    
    int n,m;
    cin >> n >> m; //n*n的图，m个点
    T.Init(n+10);
    vector<Z> vc;
    for(int i=1;i<=m;++i){
        int u,v;
        cin >> u >> v;
        vc.pb({u,v,-1,1}); //点(x,y)+1
    }
    auto Push=[&](int x1,int x2,int y1,int y2,int ix,int lim){ //(x1,x2)~(y1,y2)范围的矩形询问，询问编号，平面边界0/1
        vc.pb({x2,y2,ix,1});
        if(x1-1>=lim) vc.pb({x1-1,y2,ix,-1});
        if(y1-1>=lim) vc.pb({x2,y1-1,ix,-1});
        if(x1-1>=lim && y1-1>=lim) vc.pb({x1-1,y1-1,ix,1});
    };
    int q;
    cin >> q;
    vector<int> ans(q+3,0);
    for(int i=1;i<=q;++i){
        int x1,x2,y1,y2;
        cin >> x1 >> x2 >> y1 >> y2; //询问(x1~x2),(y1~y2)矩形内有多少个点
        if(x1>x2) swap(x1,x2);
        if(y1>y2) swap(y1,y2);
        Push(x1,x2,y1,y2,i,1);
    }
    sort(vc.begin(),vc.end(),cmp);
    for(auto& [x,y,id,sgn]:vc){
        if(id==-1) T.Fix(y,1);
        else if(id!=0) ans[id]+=sgn*T.Ask(y);
    }
    for(int i=1;i<=q;++i) cout << ans[i] << '\n';
}
```