求单源最短路，适用于所有边权都是正数，朴素 `o(n^2)`，适用于稠密图。堆优化`o(mlogn)`，适用于稀疏图。（m:边数，n:点数）
*思路*
设集合T为已经确定最短路的点的集合，集合S为未确定最短路的点的集合。对于图中所有的点，初始化与起点的距离为无穷，并在输入初始化距离前初始化与其他点间距离为无穷，加入到集合S中。然后设置起点与起点的距离为0，每次选取与起点最近的点t，此时该点t最短路已经确定，加入到T中。然后以该点为中转站对S中所有点s进行迭代。`dist[s]=min(dis[s],dist[t]+w[t][s])`。然后不断迭代直到所有点加入到T中或目标点加入到T中（确定了最短路）
#### 朴素版
```
const int INF=0x3f3f3f3f;
const int MN=550;
int g[MN][MN];  //g[a][b],点a到b的距离
bool st[MN];    //点i是否被加入到T中（是否确定了最短路）
int dst[MN];   //距离
inline void solve(){
	memset(dst,0x3f,sizeof dst);
	memset(g,0x3f,sizeof g);
	int n,m;
	cin >> n >> m;
	for(int i=1;i<=m;i++){
		int a,b,c;
		cin >> a >> b >> c;
		g[a][b]=min(g[a][b],c);
	}
	dst[1]=0;
	for(int i=1;i<=n;i++){
		//int ndst=INF;
		int t=-1;
		for(int j=1;j<=n;j++){
			if(st[j]==false&&(t==-1||dst[j]<dst[t])) t=j;
		}
		st[t]=true;
		for(int s=1;s<=n;s++){
			if(st[s]==false) 
			dst[s]=min(dst[s],dst[t]+g[t][s]);
		}
	}
	if(dst[n]>=INF) cout << -1 << '\n';
	else cout << dst[n];
	return;
}
```
## 堆优化版(稀疏图)：使用优先队列，注意堆中会有重复的已经在T中的元素，要continue
```
typedef pair<int,int> pii;
const int INF=0x3f3f3f3f;
const int MN=1*1e4+20;
int h[MN],e[MN],ne[MN],w[MN],idx;
int dist[MN];
bool st[MN];
void add(int a,int b,int c){
	e[idx]=b; ne[idx]=h[a]; h[a]=idx; w[idx]=c; idx++;
}
inline void solve(){
	memset(h,-1,sizeof h);
	memset(w,0x3f,sizeof w);
	memset(dist,0x3f,sizeof dist);
	int n,m;
	cin >> n >> m;
	for(int i=1;i<=m;i++){
		int a,b,c;
		cin >> a >> b >> c;
		add(a,b,c);
	}
	priority_queue<pii,vector<pii>,greater<pii>> q; //距离,编号
	q.push({0,1});
	//st[1]=true;
	dist[1]=0;
	while(!q.empty()){
		auto [d,id]=pq.top();
		q.pop();
		if(st[id]==true) continue;
		st[id]=true;    //标记已经确定最短路
		for(int i=h[id];i!=-1;i=ne[i]){
			int j=e[i];
			if(d+w[i]<dist[j]){
				dist[j]=d+w[i];
				q.push({dist[j],j});
			}
		}	
	}
	if(dist[n]>=INF) cout << -1;
	else cout << dist[n];
	return;
}
```
## 求经过某点的最小环
```
// 边权不为1
void dijkstra(int sta){
    memset(dist,0x3f,sizeof dist);
    vector<bool> st(n+10,false);
    dist[sta]=0;
    priority_queue<pii,vector<pii>,greater<pii>> pq; //小
    pq.push({0,sta});
    bool ok=true;
    while(!pq.empty()){
        auto [dst,id]=pq.top();
        pq.pop();
        if(st[id]) continue;
        st[id]=true;
        for(auto [to,wt]:e[id]){
            if(dst+wt<dist[to]){
                dist[to]=dst+wt;
                pq.push({dist[to],to});
            }
        }
        if(ok){ ok=false; dist[sta]=INF; st[1]=false;}
    }
}       //dist[sta]就是经过sta点的最小环

//边权为1,多源广搜
    for(int i=1;i<=n;++i) dist[i]=INF;  
    queue<int> q;  
    for(int to:e[1]){  
        q.push(to);  
        dist[to]=1;  
    }  
    while(!q.empty()){  
        int u=q.front();  
        q.pop();  
        for(int to:e[u]){  
                if(dist[to]<=dist[u]+1) continue;  
                dist[to]=dist[u]+1;  
                q.push(to);  
        }  
    }  
    if(dist[1]>=INF) cout << -1;  
    else cout << dist[1];
```