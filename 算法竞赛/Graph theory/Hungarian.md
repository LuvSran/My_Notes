匈牙利算法，求二分图的最大匹配数， o(nm)
*思路*
遍历集合1所有的点，对于每个点u，尝试与集合2中与u相连的所有点v匹配直到匹配成功或试完。匹配时若v还未匹配(`match[v]==0`)则将v直接匹配给此点u(now)并结束，否则尝试使v所匹配的的点u(v)去匹配u(v)连接的其他点(`xfind(match[v])`)，成功则u(now)匹配v，u(v)匹配u(v)新找到的点。(闺中待嫁，据为己有；名花有主，求他放手)
```
if(match[v]==0 || xfind(match[v])){  
            match[v]=x;  
            return true;  
        }  
```
遍历中对于每个点u的尝试匹配，成功则最大匹配数+1
###### 注意
- 对集合1每个点开始尝试前将集合2每个点设为未访问过
```
  for(int i=1;i<=n1;i++){  
        memset(st,0,sizeof st);  
        if(xfind(i)) ans++;     
        }  
```
*给一个二分图，左集合含n1个点，编号1-n1。右集合含n2个点，编号1-n2。总共m条边。输入n1,n2,m和m条边u,v(表示左集合点u和右集合点v相连)，保证输入的是二分图。输出最大匹配数。*
```
const int MN=1e5+20;  
int h[MN],e[MN],ne[MN],idx;  
int match[520];  
bool st[520];  
void add(int a,int b){  
    e[idx]=b; ne[idx]=h[a]; h[a]=idx; ++idx;  
}  
bool xfind(int x){  
    for(int i=h[x];i!=-1;i=ne[i]){  
        int j=e[i];  
        if(st[j]==true) continue;  
        st[j]=true;  
        if(match[j]==0 || xfind(match[j])){  
            match[j]=x;  
            return true;  
        }  
    }  
    return false;  
}  
inline void solve(){  
    memset(h,-1,sizeof h);  
    int n1,n2,m;  
    cin >> n1 >> n2 >> m;  
    for(int i=1;i<=m;i++){  
        int u,v;  
        cin >> u >> v;  
        add(u,v);  
    }  
    int ans=0;  
    for(int i=1;i<=n1;i++){  
        memset(st,0,sizeof st);  
        if(xfind(i)) ans++;     
        }  
    cout << ans;  
    return;  
}
```