## 有依赖的背包
有 N个物品和一个容量 V的背包。
物品之间具有依赖关系，且依赖关系组成一棵树的形状。如果选择一个物品，则必须选择它的父节点。
```cpp
const int maxn=200+20;
const int maxV=135;  //o(n*V^2)
int v[maxn],w[maxn]; //体积，价值
vector<int> e[maxn];
int dp[maxn][maxV]; //dp[u][v]，节点u在容量为V的情况下的最大价值
int n,V;
int root;
void dfs(int u){  
    for(int i=v[u];i<=V;++i) dp[u][i]=w[u];
    for(int to:e[u]){
        dfs(to);
        for(int i=V;i>=v[u];--i){
            for(int j=0;j+v[u]<=i;++j){
                dp[u][i]=max(dp[u][i],dp[u][i-j]+dp[to][j]);
            }
        }
    }
}
void solve(){
    cin >> n >> V;
    for(int i=1;i<=n;++i){
        cin >> v[i] >> w[i];
        int pi;
        cin >> pi;
        if(pi==-1) root=i;
        else e[pi].pb(i);
    }
    dfs(root);
    KILL(dp[root][V]);
}
```