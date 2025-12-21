求单源最短路，边权可为负数，适用于求解起点到终点经过不超过k条边的最短路，含负权回路的图则不一定能求出，o(nm)
*思路*
 迭代k次，每次迭代遍历所有边并更新边的终点。
可用结构体存边。求起点到终点经过不超过k条边，就迭代k次。每次迭代遍历更新一次所有的边，遍历更新之前先将走到各点的距离的情况备份一次(防止此次迭代中后更新的基于前更新的情况而更新，违反不超过k条边)，然后开始。更新操作为`dist[b]=min(dist[b],backup[a]+w)`;(`backup[i]`为备份的未进行此次迭代更新时i点的情况)
*注意*
- 输入前初始化1到所有点的距离dist\[i]为无穷INF，再dist\[1]=0
- 判断1能否走到n可用`if(dist[n]>=INF/2)`,因为有负权边存在,dist\[n]经更新后可能不是INF
求含n个点m条边的有向图，经过不超过k条边从1走到n的最短距离。
```
const int INF=0x3f3f3f3f;  
const int N=520,M=1e4+20;  
struct Edge{  
    int a=0,b=0,w=0;  
};  
Edge edg[M];  
int dist[N];  
int bkp[N];  
inline void solve(){  
    memset(dist,0x3f,sizeof dist);  
    int n,m,k;  
    cin >> n >> m >> k;  
    for(int i=1;i<=m;i++) cin >> edg[i].a >> edg[i].b >> edg[i].w;  
    dist[1]=0;  
    int kk=k;  
    while(kk--){  
        memcpy(bkp,dist,sizeof dist);  //备份
        for(int i=1;i<=m;i++){  
            int go=edg[i].a,to=edg[i].b,l=edg[i].w;  
            dist[to]=min(dist[to],bkp[go]+l);  
        }  
    }  
    if(dist[n]>=INF/2) cout << -1;  
    else cout << dist[n];  
    return;  
}
```