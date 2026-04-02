# 拓扑序
```cpp
const int N = 100010;
int n,m,a,b;
vector<int> e[N], tp;
int din[N];

bool toposort(){
  queue<int> q;
  for(int i = 1; i <= n; i++)
    if(din[i]==0) q.push(i);
  while(q.size()){
    int x=q.front(); q.pop();
    tp.push_back(x);
    for(auto y : e[x]){
      if(--din[y]==0) q.push(y);
    }
  }
  return tp.size() == n;
}
int main(){
  cin >> n >> m;
  for(int i=0; i<m; i++){
    cin >> a >> b;
    e[a].push_back(b);
    din[b]++;
  }
  if(!toposort()) puts("-1");
  else for(auto x:tp)printf("%d ",x);
  return 0;
}
```

```
const int N = 100010;
int n,m,a,b;
vector<int> e[N], tp;
int c[N]; //染色数组

bool dfs(int x){
  c[x] = -1;
  for(int y : e[x]){
    if(c[y]<0)return 0; //有环 
    else if(c[y]==0)
      if(dfs(y)==false)return false;
  }
  c[x] = 1;
  tp.push_back(x);
  return true;
}
bool toposort(){
  memset(c, 0, sizeof(c));
  for(int x = 1; x <= n; x++)
    if(!c[x])
      if(!dfs(x))return 0;
  reverse(tp.begin(),tp.end());
  return 1;
}
int main(){
  cin >> n >> m;
  for(int i=0; i<m; i++){
    cin >> a >> b;
    e[a].push_back(b);
  }
  if(!toposort()) puts("-1");
  else 
    for(int x:tp)printf("%d ",x);
  return 0;
}
```