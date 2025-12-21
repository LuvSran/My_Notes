# 最近公共祖先
# DFS序+ST表（o(1)查询）
```
template<int N> struct Tree_theory{
    const vector<int>* adj;
    int dfn[N]; 
    int fa[N];
    int dep[N];
    int f[N][25];
    int clk=0,nn=0;
    void dfs_init(int u,int father,int deep){
        dfn[u]=++clk;
        dep[clk]=deep;
        fa[clk]=father;
        for(int to:adj[u]){
            if(to==father) continue;
            dfs_init(to,u,deep+1);
        }
    }
    int ask(int lo,int ro){
        int k=__lg(ro-lo+1);
        if(dep[f[lo][k]]<dep[f[ro-(1<<k)+1][k]]) return f[lo][k];
        else return f[ro-(1<<k)+1][k];
    }
    void Run(int _n,vector<int>adj[]){
        nn=_n;
		clk=0;
	    this->adj=adj;
        dfs_init(1,0,0);
        for(int i=1;i<=_n;++i) f[i][0]=i;
        for(int j=1;(1<<j)<=_n;++j){
            for(int i=1;i+(1<<j)-1<=_n;++i){
                if(dep[f[i][j-1]]<dep[f[i+(1<<(j-1))][j-1]]) f[i][j]=f[i][j-1];
                else f[i][j]=f[i+(1<<(j-1))][j-1];
            }
        }
    }
    int Lca(int u,int v){ //查u,v的lca
        if(u==v) return u; 
        int l=min(dfn[u],dfn[v]),r=max(dfn[u],dfn[v]);
        int t=ask(l+1,r);
        return fa[t];
    }
    int Dis(int u,int v){//查u,v的距离(之间的边数)
        return dep[dfn[u]]+dep[dfn[v]]-2*dep[dfn[Lca(u,v)]];
    }
    pair<int,int> Diameter(){ //求树的直径两个端点
        int ver1=1,ver2=1,mx=0;
        for(int i=1;i<=nn;++i){
            int t=Dis(1,i);
            if(t>mx) mx=t,ver1=i;
        }
        mx=0;
        for(int i=1;i<=nn;++i){
            int t=Dis(ver1,i);
            if(t>mx) mx=t,ver2=i;
        }
        return {ver1,ver2};
    }
};
Tree_theory<maxn> T;
```
$lca(u,v)$，为 $dfn(u)+1-dfn(v)$ 之间，深度最小的点的父节点
```
vector<int> e[maxn];
int dfn[maxn]; //i的时间戳
int fa[maxn]; //时间戳i的父节点
int dep[maxn];//时间戳i的深度
int clk=0;
struct St_table{
    int f[maxn][25];
    void init(int _n){
        for(int i=1;i<=_n;++i) f[i][0]=i;
        for(int j=1;(1<<j)<=_n;++j){
            for(int i=1;i+(1<<j)-1<=_n;++i){
                if(dep[f[i][j-1]]<dep[f[i+(1<<(j-1))][j-1]]) f[i][j]=f[i][j-1];
                else f[i][j]=f[i+(1<<(j-1))][j-1];
            }
        }
    }
    int query(int lo,int ro){
        int k=__lg(ro-lo+1);
        if(dep[f[lo][k]]<dep[f[ro-(1<<k)+1][k]]) return f[lo][k];
        else return f[ro-(1<<k)+1][k];
    }
};
St_table st;
void dfs_init(int u,int father,int deep){
    dfn[u]=++clk;
    dep[clk]=deep;
    fa[clk]=father;
    for(int to:e[u]){
        if(to==father) continue;
        dfs_init(to,u,deep+1);
    }
}
int lca(int u,int v){
    if(u==v) return u;  //一定要特判
    int l=min(dfn[u],dfn[v]),r=max(dfn[u],dfn[v]);
    int t=st.query(l+1,r);
    return fa[t];
}
```

## 倍增(nlogn预处理，logn查询)
```
vector<int> e[maxn];  
int dep[maxn],pa[maxn][30];  
int n,m,s;  
void dfs_init(int u,int father){  
    dep[u]=dep[father]+1;  
    pa[u][0]=father;  
    for(int i=1;i<=31;++i) pa[u][i]=pa[pa[u][i-1]][i-1];  
    for(int to:e[u]){  
        if(to==father) continue;  
        dfs(to,u);  
    }  
}  
int lca(int u,int v){  
    if(dep[u]<dep[v]) swap(u,v);  
    for(int i=31;i>=0;--i){  
        if(dep[pa[u][i]]>=dep[v])  
            u=pa[u][i];  
    }  
    if(u==v) return u;  
    for(int i=31;i>=0;--i){  
        if(pa[u][i]!=pa[v][i])  
            u=pa[u][i],v=pa[v][i];  
    }  
    return pa[u][0];  
}
```
## tarjan(离线，o(n+m(询问数))
```
vector<int> e[maxn];  
vector<pii> query[maxn];  
int ans[maxn];  
bitset<maxn> vis; int p[maxn];  
int n,m,s;  
void init(int _n){ for(int i=0;i<=_n;++i) p[i]=i;}  
int find(int x){  
    if(p[x]!=x) p[x]=find(p[x]);  
    return p[x];  
}  
void tarjan(int u){  
    vis[u]=1;        
    for(int to:e[u]){  
        if(vis[to]) continue;  
        tarjan(to);  
        p[to]=u;  
    }  
    for(auto [v,id]:query[u]){  
        if(vis[v]==0) continue;  
        ans[id]=find(v);  
    }  
}  
void solve(){  
    cin >> n >> m >> s;  
    init(n);  
    for(int i=1;i<=n-1;++i){  
        int x,y;  
        cin >> x >> y;  
        e[x].pb(y); e[y].pb(x);  
    }  
    for(int i=1;i<=m;++i){  
        int a,b;  
        cin >> a >> b;  
        query[a].pb(make_pair(b,i));  
        query[b].pb(make_pair(a,i));  
    }  
    tarjan(s);  
    for(int i=1;i<=m;++i) cout << ans[i] << '\n';  
    return;  
}
```