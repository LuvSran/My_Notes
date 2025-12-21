树的直径
```cpp
vector<pair<int,int>> e[maxn];  
int tR=0;  
int dfs_R(int u,int father){  
    int d1=0,d2=0;  //经过u点的最长边，次长边
    for(auto [to,wt]:e[u]){  
        if(to==father) continue;  
        int res=dfs_R(to,u)+wt;  
        if(res>=d1) d2=d1,d1=res;  
        else if(res>=d2 && res<d1) d2=res;  
    }  
    tR=max(tR,d1+d2);  
    return d1;  
}
```
树的中心
```
vector<pii> e[maxn];
int d1[maxn],d2[maxn]; //向下最长链，次长链
int d3[maxn]; //向上最长链
int rec[maxn]; //向下最长链的儿子
int ans[maxn];
void dfs_1(int u,int father){
    for(auto [to,w]:e[u]){
        if(to==father) continue;
        dfs_1(to,u);
        int d=d1[to]+w;
        if(d>=d1[u]) d2[u]=d1[u],d1[u]=d,rec[u]=to;
        else if(d>d2[u]) d2[u]=d;
    }
    ans[u]=d1[u];
}
void dfs_2(int u,int father){
    for(auto [to,w]:e[u]){
        if(to==father) continue;
        if(rec[u]==to) d3[to]=max(d3[to],w+max(d2[u],d3[u]));
        else d3[to]=max(d3[to],w+max(d1[u],d3[u]));
        dfs_2(to,u);
    }
    ans[u]=max(ans[u],d3[u]);
}
ans=*min_element(ans+1,ans+1+n);
```
树的重心
```
vector<int> e[maxn];
int sum[maxn];
int ans=INF;
int n;
void dfs(int u,int father){
	int res=0,s=0;
	for(int to:e[u]){
		if(to==father) continue;
		dfs(to,u);
		s+=sum[to];
		res=max(res,sum[to]);
	}
	sum[u]=s+1;
	ans=min(ans,max(res,n-1-s));
}
```