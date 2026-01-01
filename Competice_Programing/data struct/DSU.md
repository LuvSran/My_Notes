 并查集，高效地合并两个集合或者检查两个元素是否在一个集合中
***
# 板子
```cpp
template<int N>struct DSU{
    int p[N],sz[N];
    void Init(int _n){
        for(int i=0;i<=_n+2;++i) p[i]=i,sz[i]=1;
    }
    int find(int x){
        while(p[x]!=x) {
            p[x]=p[p[x]];  
            x=p[x];
        }
        return p[x];
    }
    void Merge(int u,int v){
        int pu=find(u),pv=find(v);
        if(pu==pv) return;
        sz[pv]+=sz[pu];
        p[pu]=pv;
    }
    bool Same(int u,int v){ return find(u)==find(v);}
};
```
*基本原理*：每个集合用一棵树表示。树根的编号就是集合的编号。每个节点存储他的父节点，p\[x]表示x的父节点
1. 如何判断树根：`if(p[x]==x)`
2. 如何求x所在集合的编号:`while(p[x]!=x) x=p[x];`路径压缩优化：第一次找到祖宗（根节点）后，就将寻找路径上的所有节点x的p[x]设为祖宗）
*路径压缩优化*
```cpp
int find(int x){     //返回x的祖宗节点编号+路径压缩
   if(p[x]!=x) p[x]=find(p[x]); 
    return p[x];
}
```
*非递归实现*
 ```cpp
 int find(int x){
    while(p[x]!=x) {
        p[x]=p[p[x]];  // 路径压缩
        x=p[x];
    }
    return p[x];
}
```

3. 如何合并两个集合：将其中一个集合的根节点连接另一个集合的根节点（即将一颗树的根节点视为另一颗树根节点的子结点）:`p[find(a)]=find(b)`
*带权并查集*
## 维护连通块中点的数量`_size[]`
```cpp
int p[MN];  
int _size[MN];
void prepro(int nn){   //初始化
    for(int i=0;i<=nn+5;++i) p[i]=i,_size[i]=1;  
}     
							//合并x,y 
	int px=find(x),py=find(y);
	if(px!=py) _size[px]+=_size[py];
	p[py]=px;
```
### 注意
1. 更新`_size[]时`，是作根size+=额外合并size
2. 若`pa==pb`，即两节点根相同，不能更新`_size[]`
## 维护节点`i`到祖宗节点的距离`dist[i]`
```cpp
int p[maxn];
int dist[maxn];
int find(int x){
    if(p[x]!=x){
        int root=find(p[x]);
        dist[x]+=dist[p[x]];
        p[x]=root;
    }
    return p[x];
}
void merge(int _x,int _y,int _d){  //_x接到_y上,_d为xy边权
    int px=find(_x),py=find(_y);
    if(px==py) return;
    p[px]=py;
    dist[px]=dist[_y]+_d; //适用于树形结构,将x为根的子树接到节点y上,即x为根,px==x
}
void init(int _n){
    for(int i=0;i<=_n;++i){
        p[i]=i;
        dist[i]=0;
    }
}
```
### 注意
更新`d[]`时，将`x`合并到`y`上，则是更新`d[px]`
***
## 交换两个集合内所有元素
```cpp
int p[maxn];
int fx[maxn]; //部落i的集合编号
int xf[maxn]; //集合编号i对应的部落
int be[maxn]; //人i属于哪个集合
int find(int x){
    if(p[x]!=x) p[x]=find(p[x]);
    return p[x];
}
void merge(int x,int y){
    p[find(y)]=find(x);
}
void qk(int n){
    for(int i=1;i<=n;++i){
        p[i]=i;
        fx[i]=i;
        xf[i]=i;
        be[i]=i;
    }
}
void solve(){  
    int n,q;
    cin >> n >> q;
    qk(n+10);
    while(q--){
        int o; //1：将部落a的人移到部落b中。 2：将人a移到部落b中。
	    cin >> o; //3:交换部落a,b的人。 //4:查询人a在哪个部落
        if(o==1){
            int a,b;
            cin >> a >> b;
            merge(fx[a],fx[b]);
        }
        if(o==2){
            int a,b;
            cin >> a >> b;
            be[a]=fx[b];
        }
        if(o==3){
            int a,b;
            cin >> a >> b;
            int ta=fx[a],tb=fx[b];
            xf[ta]=b,xf[tb]=a;
            fx[a]=tb,fx[b]=ta;
        }
        if(o==4){
            int a;
            cin >> a;
            int t=xf[find(be[a])];
            cout << t << '\n';
        }
    }
}
```