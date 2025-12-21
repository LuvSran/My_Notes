
求MST(Minimum Spanning Tree，最小生成树)，边权可为负，o(n^2)
*思路*
每次迭代选取距离树最近的点`t`，此时确定了`t`通过该距离的边连到树上是最优的，然后通过`t`更新其余所有还未确定的点,`dist[j]=min(dist[j],g[t][j]);`(意义为断开原来的边连到点`t`上是否更优)。不断迭代直到所有点确定(循环n(点数)次)。
###### 注意
- 将所有`dist[i]`(表示点`i`到树的最短距离)和`g[][]`初始化为INF，`dist[1]=0`,然后输入时保留最小`g[][]`,即`g[u][v]=g[v][u]=min(g[v][u],w);`
- 若某次迭代选取的`t`有`dist[t]>=INF`，说明所有点不连通，不能用所有点构造最小生成树
*输入n个点m条边的无向图，求最小生成树各边权之和，不存在则输出“No”*
```
const int INF=0x3f3f3f3f;  
const int M=1e5+20,N=520;  
int g[N][N];  
int dt[N];  
bool st[N];  
inline void solve(){  
    memset(g,0x3f,sizeof g);  
    memset(dt,0x3f,sizeof dt);  
    int n,m,sum=0;  
    cin >> n >> m;  
    for(int i=1;i<=m;i++){  
        int u,v,w;  
        cin >> u >> v >> w;  
        g[u][v]=g[v][u]=min(g[v][u],w);  
    }  
    dt[1]=0;  
    for(int i=1;i<=n;i++){  
        int t=-1;  
        for(int j=1;j<=n;j++){  
            if((st[j]==false)&&(t==-1||dt[j]<dt[t])) t=j;  
        }  
        if(dt[t]>=INF){  
            cout << "No";  
            return;  
        }  
        sum+=dt[t];  
        st[t]=true;  
        for(int j=1;j<=n;j++){  
            if(st[j]==false)    dt[j]=min(dt[j],g[t][j]);  //更新
        }  
    }  
    cout << sum;  
    return;  
}
```