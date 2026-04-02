Bellman-ford的队列优化，可用于求负环，o(m)，最坏o(nm),(可能被卡)
*思路*
#####   求最短路
对于Bellman-fold的更新操作`dist[b]=min(dist[b],backup[a]+w)`，发现`dist[b]`可能更新减小的前提是`backup[a]`比起原来减小了，则可得出，可创建一个队列，将更新过的点push到队列中，每次迭代使用队列中的点去更新即可
(并使用`bool st[ ]`来判断某个点是否在队列中，在则不用push，以此优化)
##### 求负环
使用`cnt[i]`记录到点`i`经过的边数，由抽屉原理得，若`cnt[i]`大于等于总点数`cnt[i]>=n`，则必定某个点经过了至少两次，则存在负环
初始化时将所有`dist[]`初始化为无穷。将要求是否含负环的路径出发的点push进队列(并将此点`dist[]`设为0)，然后进行求最短路的操作，在更新`cnt[]`时，`cnt[now]=cnt[last]+1`;
### 注意
- 一般情况下spfa考虑了所有输入的边并取得最优的值，所以不用保留最小边权，可用邻接表

*求1到n最短路,点 n,边 m,有向图*
```
const int MN=1e5+10;
int h[MN],e[MN],ne[MN],w[MN],idx;
int dist[MN];
bool st[MN];
void add(int a,int b,int c){
	e[idx]=b; ne[idx]=h[a]; h[a]=idx; w[idx]=c; idx++;
}
inline void solve(){
	memset(dist,0x3f,sizeof dist);
	memset(h,-1,sizeof h);
	dist[1]=0;
	int n,m;
	cin >> n >> m;
	for(int i=1;i<=m;i++){
		int a,b,c;
		cin >> a >> b >> c;
		add(a,b,c);
	}
	queue<int> q;
	q.push(1);
	st[1]=true;
	while(!q.empty()){
		int t=q.front();
		q.pop();
		st[t]=false;
		for(int i=h[t];i!=-1;i=ne[i]){
			int j=e[i];
			if(dist[j]>dist[t]+w[i]){
				dist[j]=dist[t]+w[i];
				if(st[j]==false){
					q.push(j);
					st[j]=true;
				}
			}
		}
	}
	if(dist[n]>=INF/2) cout << "impossible";
	else cout << dist[n];
	return;
}
```
*点n，边m，有向图，判断是否存在负环，输出No或Yes*
```
const int INF=0x3f3f3f3f;
const int MN=1e5+20;
int h[MN],e[MN],ne[MN],w[MN],idx;
int dist[MN],cnt[MN];
bool st[MN];
void add(int a,int b,int c){
	e[idx]=b; ne[idx]=h[a]; h[a]=idx; w[idx]=c; idx++;
}
inline void solve(){
	memset(dist,0x3f,sizeof dist);      //其实不用初始化为无穷
	memset(h,-1,sizeof h);
	dist[1]=0;
	int n,m;
	cin >> n >> m;
	for(int i=1;i<=m;i++){
		int a,b,c;
		cin >> a >> b >> c;
		add(a,b,c);
	}
	queue<int> q;
	for(int i=1;i<=n;i++){
		q.push(i);
		dist[i]=0;  //其实可以前面不初始化为无穷，这步就可以省
		st[i]=true;    
	}
	while(!q.empty()){
		int t=q.front();
		q.pop();
		st[t]=false;
		for(int i=h[t];i!=-1;i=ne[i]){
			int j=e[i];
			if(dist[j]>dist[t]+w[i]){
				dist[j]=dist[t]+w[i];
				cnt[j]=cnt[t]+1;             //更新经过的边数
				if(cnt[j]>=n){             //抽屉原理得负环
					cout << "Yes";
					return;
				}
				if(st[j]==false){
					q.push(j);
					st[j]=true;
				}
			}
		}
	}
    cout << "No";
	return;
}
```