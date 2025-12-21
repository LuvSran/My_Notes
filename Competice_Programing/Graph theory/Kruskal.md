求MST,o(mlogm)，(常数小，实际很快)
*思路*(并查集+贪心)
建一个并查集和cnt，`cnt`统计树的边数，边可用结构体存。对所有边按边权进行升序排序，然后遍历所有边，(开始时默认所有点不连通)，如果某条边的两点不在一个连通块中，就加到一个连通块`p[find(a)]=find(b);`，同时边权总和增加并且边数+1:`sum+=w`,`cnt++`。遍历结束后如果`cnt<n-1`，则不存在最小生成树
###### 注意
- 结构体重载小于号
- 可对边降序排序构建最大生成树
```
struct Edge{  
    int a=0,b=0,w=0;  
    bool operator<(const Edge &t) const{  
        return w<t.w;  
    }  
}; 
```
*输入n个点m条边的无向图，求最小生成树各边权之和，不存在则输出“No”*
```
const int M=2*1e5+20,N=1e5+20;  
struct Edge{  
    int a=0,b=0,w=0;  
    bool operator < (const Edge &t) const{  
        return w<t.w;  
    }  
};  
int p[N];  
Edge edg[M];  
int find(int x){  
    if(p[x]!=x) p[x]=find(p[x]);  
    return p[x];  
}  
inline void solve(){  
    int n,m,sum=0,cnt=0;  
    cin >> n >> m;  
    for(int i=1;i<=m;i++) cin >> edg[i].a >> edg[i].b >> edg[i].w;  
    for(int i=1;i<=n;i++) p[i]=i;  
    sort(edg+1,edg+1+m);  
    for(int i=1;i<=m;i++){  
        int a=edg[i].a, b=edg[i].b, w=edg[i].w;  
        if(find(a)!=find(b)){  
            p[find(a)]=find(b);  
            sum+=w;  
            cnt++;  
        }  
    }  
    if(cnt<n-1) cout << "No";  
    else cout << sum;  
    return;  
}
```