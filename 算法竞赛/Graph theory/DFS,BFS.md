# 树的dfs
*基础模板*
```cpp
// 从节点u开始进行深度优先搜索
void dfs(int u) {
    vis[u] = true;  // 给u打上已访问的标记
    for (int i = h[u]; i!=-1; i = ne[i]) {
        int v = e[i];
        if (vis[v]==false){
			dfs(v);
			//具体逻辑
		}
    }
    return;
}

```
*例题：树的重心*
输入n表示节点数，然后输入n-1行数据a,b，表示a,b之间连一条边。找到树的重心并输出删除重心后连通块点数的最大值
树的重心：重心是指树中的一个结点，如果将这个点删除后，剩余各个连通块中点数的最大值最小，那么这个节点被称为树的重心。
```cpp
const int MN=1e5+20;  
const int inf=0x3f3f3f3f;  
typedef pair<int,int> pii;  
int h[MN],e[2*MN],ne[2*MN],idx=0;  
bool vis[MN];  
int ans=inf;  
int n;  
void add(int a,int b){  
    e[idx]=b; ne[idx]=h[a]; h[a]=idx; idx++;  
}  
int dfs(int u){  
    vis[u]=true;  
    int sum=1,res=0;  
    for(int i=h[u];i!=-1;i=ne[i]){  
        int j=e[i];  
        if(vis[j]==false){  
            int jvlu=dfs(j);  //以节点j为根的子树的节点数
            sum+=jvlu;  //以节点u为根的子树的节点数
            res=max(res,jvlu);   //u点下方所有连通块的最大连通块
        }  
    }  
    res=max(res,n-sum);  //u下方/u上方
    ans=min(ans,res);  
    return sum;  
}  
inline void solve(){  
    memset(h,-1,sizeof h);  
    cin >> n;  
    for(int i=1;i<=n-1;i++){  
        int a,b;  
        cin >> a >> b;  
        add(a,b); add(b,a);  
    }  
    dfs(1);  
    cout << ans;  
    return;  
}  
int main(){  
    FAST;  
    solve();  
    return 0;  
}
```
# 树/图的bfs
*基础模板*
```cpp
// 从节点u开始进行广度优先搜索
void bfs(int u) {
    queue<int> q;
    vis[u] = true;
    q.push(u);
    while (!q.empty()) {
        int t = q.front();
        q.pop();
        for (int i = h[t]; i!=-1; i = ne[i]) {
            int v = e[i];
            if (vis[v]=false) {
	            vis[v] = true;
	            q.push(v);
	            //具体逻辑
            }
          }
      }
      return;
  }
