树上log修改点权，log查询两点之间点权和
```cpp
template<int N> struct Tree_theory{
    const vector<int>* adj;
    int n;
    int dfn[N], out[N],dep[N],fa[N],clk;
    int st[N * 2][22],pos[N * 2], tot; 
    ll val[N], bit[N + 5]; 
    void update(int x,ll d) {
        for (; x <= n; x += x & -x) bit[x] += d;
    }
    ll query(int x) {
        ll res = 0;
        for (; x > 0; x -= x & -x) res += bit[x];
        return res;
    }
    void dfs(int u, int p, int d) {
        dfn[u]=++clk;
        fa[u]=p;
        dep[u]=d;
        pos[u]=++tot;
        st[tot][0]=u;
        for (int v :adj[u]) {
            if (v==p) continue;
            dfs(v, u, d + 1);
            st[++tot][0] = u; 
        }
        out[u]=clk;
    }
    void Init(int _n, const vector<int> _adj[]) { //初始化，默认所有点权为1
        n= _n; adj= _adj;
        clk=tot=0;
        for (int i=0; i<=n+1;++i) bit[i]=0,val[i]=0;
        dfs(1, 0, 0);
        for (int j = 1; j <= 20; j++) {
            for (int i = 1; i + (1 << j) - 1 <= tot; i++) {
                int a = st[i][j - 1], b = st[i + (1 << (j - 1))][j - 1];
                st[i][j] = (dep[a] < dep[b] ? a : b);
            }
        }
    }
    int Lca(int u, int v) { //查u,v的lca
        int l = pos[u], r = pos[v];
        if (l > r) swap(l, r);
        int k =__lg(r-l+1);
        int a = st[l][k], b = st[r - (1 << k) + 1][k];
        return dep[a] < dep[b] ? a : b;
    }
    void Fix(int u, int k) { //将u点点权修改为k
        ll delta = k - val[u];
        val[u] = k;
        update(dfn[u], delta);
        update(out[u] + 1, -delta);
    }
    ll Ask(int u, int v) { //查询u,v路径的点权和
        int lca = Lca(u, v);
        return query(dfn[u]) + query(dfn[v]) - 2 * query(dfn[lca]) + val[lca];
    }
};
```