# 扫描线
求矩形面积并
```cpp
const int maxn=1e5+20;
struct Line{
    int x1,x2,y,mk;
    bool operator<(Line& t){
        return y<t.y;
    }
};
Line line[2*maxn];
vector<int> X;
struct Segment_tree{
    struct{
        int l,r,len,val;
    }z[8*maxn];
    void push_up(int p){
        if(z[p].val>0) z[p].len=X[z[p].r+1]-X[z[p].l];
        else z[p].len=(z[p].l==z[p].r)?0:z[ls].len+z[rs].len;
    }
    void build(int p,int lo,int ro){
        auto& [l,r,len,val]=z[p];
        l=lo,r=ro,len=0,val=0;
        if(lo==ro) return;
        int mid=(lo+ro)>>1;
        build(ls,lo,mid);
        build(rs,mid+1,ro);
    }
    void Fix(int p,int lo,int ro,int k){
        if(lo<=z[p].l && ro>=z[p].r){
            z[p].val+=k;
            push_up(p);
            return;
        }
        if(lo<=z[ls].r) Fix(ls,lo,ro,k);
        if(ro>=z[rs].l) Fix(rs,lo,ro,k);
        push_up(p);
    }
};
Segment_tree T;
int Inx(int _x){
    return lower_bound(X.begin()+1,X.end(),_x)-X.begin();
}
void solve(){  
    X.assign(1,0);
    int n;
    cin >> n;
    for(int i=1;i<=n;++i){
        int x1,y1,x2,y2; //(x1,y1)左下角，(x2,y2)右上角
        cin >> x1 >> y1 >> x2 >> y2;
        X.pb(x1); X.pb(x2);
        line[i]={x1,x2,y1,1};
        line[i+n]={x1,x2,y2,-1};
    }
    sort(line+1,line+1+2*n);
    sort(X.begin()+1,X.end());
    X.erase(unique(X.begin()+1,X.end()),X.end());
    int _n=Size(X)-1;
    T.build(1,1,_n+5);
    ll ans=0;
    for(int i=1;i<=2*n-1;++i){
        auto& [x1,x2,y,mk]=line[i];
        T.Fix(1,Inx(x1),Inx(x2)-1,mk);
        ans+=T.z[1].len*(line[i+1].y-y);
    }
    cout << ans << '\n';
}
```