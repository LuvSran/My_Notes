Krusal重构树
```
using ll = long long;
const int maxn=1e5+20;
template<int N> struct LCA{
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
    void Run(int _n,vector<int>adj[],vector<int> root={_n}){ //总点数，邻接表，森林的根集合
        nn=_n;
        clk=0;
        this->adj=adj;
        for(int rt:root) dfs_init(rt,0,0);
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
};
struct E{
    int v1,v2;
    ll w;
};
template<typename comp>struct Krusal_Rebuild{ 
//greater<>:重构树的 lca(u,v)点权 为原图u->v所有路径中,取最小边权最大的路径,的最小边权。less<>,原图u->v所有路径中,取最大边权最小的路径,的最大边权
    int nn;
    vector<int> p,fa;
    vector<ll> val;
    vector<bool> isRoot;
    int sz;
    comp cmp;
    int find(int x){
        if(p[x]!=x) p[x]=find(p[x]);
        return p[x];
    }
    void Run(int _n,vector<E>& edg){ //_n为点数，edg为边集合，注意edg是否引用
        nn=_n;
        p.resize(2*nn+3);  
        iota(p.begin(),p.end(),0);  
        fa.assign(2*nn+3,0);
        isRoot.assign(2*nn+3,1);
        val.assign(nn+ 1,0);
        sort(edg.begin(),edg.end(),[&](E& t1,E& t2){
            return cmp(t1.w,t2.w);
        });
        int ix=nn;
        for(auto& [v1,v2,w]:edg){
            int f1=find(v1),f2=find(v2);
            if(f1==f2) continue;
            p[f1]=p[f2]=++ix;
            val.push_back(w);
            fa[f1]=fa[f2]=ix;
            isRoot[f1]=isRoot[f2]=0;
        }
        fa.resize((int)val.size());
        sz=(int)val.size()-1;
    }
    bool Same(int v1,int v2){ //原图中v1和v2是否连通,true连通
        return find(v1)==find(v2);
    }
    pair<vector<int>,vector<ll>> Get(){ //返回fa[i]：i点的父节点。val[i]: i点点权
        return {fa,val};
    }
    vector<int> Root_set(){ //重构树森林的根集合
        vector<int> res;
        for(int i=1;i<=2*nn;++i) if(isRoot[i]==1) res.push_back(i);
        return res;
    }
};
Krusal_Rebuild<greater<ll>> K; //greater<>,sort()降序，最大生成树的重构树。less<>，最小
LCA<maxn> T;
vector<int> e[maxn];
void solve(){
    int n,m;
    cin >> n >> m; //n个点m条边无向图，q次询问。问u->v所有路径中,最小边权最大的路径的最小边权是多少
    vector<E> edg;
    for(int i=1;i<=m;++i){
        int u,v,w;
        cin >> u >> v >> w;
        edg.push_back(E{u,v,w});
    }
    K.Run(n,edg);
    auto [fa,val]=K.Get();
    int _n=val.size()-1; 
    for(int i=1;i<=_n;++i)  e[fa[i]].push_back(i);
    T.Run(_n,e,K.Root_set());
    int q;
    cin >> q;
    while(q--){
        int u,v;
        cin >> u >> v;
        if(K.Same(u,v)==false) cout << -1 << '\n';
        else cout << val[T.Lca(u,v)] << '\n';
    }
}
```