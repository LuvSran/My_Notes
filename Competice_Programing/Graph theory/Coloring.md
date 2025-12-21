染色法判断二分图，o(n+m)
 `二分图`：能够把图中全部的点分到 两个集合 中，保证两个集合内部没有 任何边 ，图中的边只存在于两个集合之间。一个图是二分图当且仅当它不含奇数环(边数是奇数的环)时。
 *思路*
 dfs将图中所有点染色为1号色或2号色，相邻连接的点染成不同的颜色，矛盾则染色失败
  *输入n点m边的无向图，判断是否是二分图*
```
const int MN=1e5+20;
int h[MN],e[2*MN],ne[2*MN],idx;
int color[MN];
bool flag=true;
void add(int a,int b){
	e[idx]=b; ne[idx]=h[a]; h[a]=idx; idx++;
}
void col_dfs(int u,int c){
	if(color[u]!=0 || flag==false) return;
	color[u]=c;
	for(int i=h[u];i!=-1;i=ne[i]){
		int j=e[i];
		if(color[j]==0) col_dfs(j,3-c);  //相邻连接的点染另一个颜色
		else if(color[j]==c){           //染色失败
			flag=false;
			return;
		}
	}
	return;
}
inline void solve(){
	memset(h,-1,sizeof h);
	int n,m;
	cin >> n >> m;
	for(int i=1;i<=m;i++){
		int a,b;
		cin >> a >> b;
		add(a,b); add(b,a);
	}
	for(int i=1;i<=n;i++){
		if(flag==false) break;
		col_dfs(i,1);
	}
	if(flag) cout << "Yes";
	else cout << "No";
	return;
}
```