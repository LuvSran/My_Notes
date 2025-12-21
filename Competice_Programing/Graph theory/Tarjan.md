`low[u]`:  DFS树中，u能通过u为根的子树树边以及最多一条非树边，所能到达的节点中的最小dfn值
# 强连通分量 
有向图中的一个​​极大子图​，其中任意两个节点 u和 v都​​互相可达​（即存在 u→v和 v→u的路径）
```cpp
template<int N> struct SCC{
    vector<int> scc;
    vector<int> stk;
    int dfn[N],low[N];
    const vector<int>* adj;
    int n,clk,tot; //tot:scc数量
    void dfs(int u){
        ++clk;
        dfn[u]=low[u]=clk;
        stk.push_back(u);
        for(int to:adj[u]){
            if(dfn[to]==-1){
                dfs(to);
                low[u]=min(low[u],low[to]);
            }
            else if(scc[to]==-1) low[u]=min(low[u],dfn[to]);
        }
        if(low[u]==dfn[u]){
            int v=-1;
            ++tot;
            do{
                v=stk.back();
                stk.pop_back();
                scc[v]=tot;
            }while(v!=u);
        }
    }
    void Run(int _n,const vector<int>adj[]){
        n=_n;
        clk=tot=0;
        stk.clear();
        fill(low,low+3+n,-1);
        fill(dfn,dfn+3+n,-1);
        scc.assign(n+3,-1);
        this->adj=adj;
        for(int i=1;i<=n;++i) if(scc[i]==-1) dfs(i);
    }
    vector<int> Get(){
        return scc;
    }
};
SCC<maxn> T;
```
# 割点
无向图中，如果删除该顶点及其相连的边后，图的​​连通分量数量增加​，则该顶点称为​割点​​
```cpp
template<int N> struct Cut_vertex{
    vector<bool> cut; //cut[i]==true,i是割点
    int dfn[N],low[N];
    const vector<int>* adj;
    int n,clk,root;
    void dfs_t(int u){
        dfn[u]=low[u]=++clk;
        int cnt=0; //DFS树的u子树中,去掉u能新增的连通块数
        for(int to:adj[u]){
            if(dfn[to]==0){
                dfs_t(to);
                low[u]=min(low[u],low[to]);
                if(low[to]>=dfn[u]) ++cnt;
            }
            else low[u]=min(low[u],dfn[to]);
        }
        if((u!=root && cnt>=1) || cnt>=2) cut[u]=true;
        else cut[u]=false;
    }
    void Run(int _n,const vector<int>adj[]){
        n=_n;
        clk=0;
        cut.assign(n+3,false);
        fill(low,low+3+n,0);
        fill(dfn,dfn+3+n,0);
        this->adj=adj;
        for(int i=1;i<=n;++i) if(dfn[i]==0) {root=i,dfs_t(i);}
    }
    vector<bool> Get(){ return cut; }
};
Cut_vertex<maxn> T;
```
# 割边(桥)
无向图中，如果删除该边后，图的​连通分量数量增加​​（即图变得更不连通），则该边称为​割边​/桥
限定：`low[u]`通过的非树边不能是子节点到父节点的反向边
```cpp
template<int N> struct Cut_edge{ //不含重边
    const vector<int>* adj;
    vector<pair<int,int>> bridge;
    int dfn[N],low[N];
    int clk;
    void dfs(int u,int father){
        dfn[u]=low[u]=++clk;
        for(int to:adj[u]){
	        if(to==father) continue; //不能走回父亲的边
            if(dfn[to]==0){
                dfs(to,u);
                low[u]=min(low[u],low[to]);
                if(low[to]>dfn[u]) bridge.push_back({u,to});
            }
            else low[u]=min(low[u],dfn[to]);
        }
    }
    void Run(int _n,vector<int> adj[]){
        this->adj=adj;
        clk=0;
        bridge.clear();
        fill(dfn,dfn+5+_n,0);
        fill(low,low+5+_n,0);
        for(int i=1;i<=_n;++i) if(dfn[i]==0) dfs(i,-1);
    }
    vector<pair<int,int>> Get(){ return bridge;}
};
Cut_edge<maxn> T;
```
# 边双连通分量
```cpp
template<int N> struct eDCC{ //考虑含重边
    vector<pair<int,int>> adj[N];
    vector<int> stk;
    vector<int> bel;
    vector<pair<int,int>> bridge;
    int dfn[N],low[N];
    int clk,eid,tot,n; //时间，边编号,边双数量
    void Add(int u,int v){ //u,v之间连一条无向边
        ++eid;
        adj[u].push_back({v,2*eid});
        adj[v].push_back({u,2*eid+1});
    }
    void dfs(int u,int uk){
        dfn[u]=low[u]=++clk;
        stk.push_back(u);
        for(auto& [to,k]:adj[u]){
            if(k==(uk^1)) continue; //不能走回父亲的边
            if(dfn[to]==0){
                dfs(to,k);
                low[u]=min(low[u],low[to]);
                if(low[to]>dfn[u]) bridge.push_back({u,to});
            }
            else low[u]=min(low[u],dfn[to]);
        }
        if(dfn[u]==low[u]){
            ++tot;
            int v=-1;
            do{
                v=stk.back();
                stk.pop_back();
                bel[v]=tot;
            }while(v!=u);
        }
    }
    void Clear(int _n){ //开始Add()之前先Clear()
        n=_n;
        for(int i=0;i<=_n+3;++i) adj[i].clear();
        stk.clear();
        bridge.clear();
        bel.assign(n+3,0);
        tot=eid=clk=0;
        fill(dfn,dfn+5+_n,0);
        fill(low,low+5+_n,0);
    }
    void Run(){ for(int i=1;i<=n;++i) if(dfn[i]==0) dfs(i,-1); }
    vector<int> Get(){ return bel;}
};
eDCC<maxn> T;
```
# 点双连通分量
```cpp
template<int N> struct vDCC {
    int dfn[N],low[N];          
    const vector<int>* adj;        
    vector<int> stk,cut;                
    vector<int> Dcc[N]; //1~tot,Dcc[i]，编号为i的vDcc内的点.
    int tot; //vDcc数量      
    int n,clk,root;              
    void dfs(int u) {
        dfn[u]=low[u]=++clk;
        stk.push_back(u);
        int cnt=0;
        for (int to:adj[u]) {
            if (dfn[to]==0) {          
                dfs(to);
                low[u]=min(low[u], low[to]);
                if (low[to]>=dfn[u]){
                    ++cnt;
                    ++tot;
                    int v;
                    do{
                        v=stk.back();
                        stk.pop_back();
                        Dcc[tot].emplace_back(v);
                    }while(v!=to);
                    Dcc[tot].emplace_back(u);
                }
            }
            else low[u]=min(low[u],dfn[to]);  
        }
        if((u!=root && cnt>=1) || cnt>=2) cut[u]=true;
        if(cnt==0 && u==root) Dcc[++tot].pb(u);
    }
    void Run(int _n,vector<int> adj[]) {
        n=_n;
        this->adj=adj;
        clk=tot=0;
        fill(dfn,dfn+n+3,0);
        fill(low,low+n+3,0);
        stk.clear();
        cut.assign(n+3,false);
        for(int i=0;i<=n+3;++i) Dcc[i].clear();
        for (int i=1;i<=n;++i){
            if(dfn[i]==0) {root=i; dfs(i);}
         }
    }
};
vDCC<maxn> T;
```