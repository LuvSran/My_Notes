# 图的存储
有向图：在两个节点间建一条单向边
无向图：在两个节点建建两条相反方向的边
## 邻接矩阵
`g[i][j]`表示点i和点j间有一条边，值为权值
## 邻接表
对图中每一个节点，建一条链表，链表上的点为此节点能到的节点，走到-1表示空，即链表末尾。在节点a和b之间加一条a指向b的边，就在a链表的头节点前加个b
## 写法1
```cpp
int h[MN],e[2*MN],ne[2*MN],idx=0; 
memset(h,-1,sizeof h);
//h[i]，节点i的链表的头节点的指针；
//e[i],指针i对应的节点
//ne[i],指针对应节点的下一个节点的指针
```
在节点a和b之间加一条a指向b的边(a能到节点b)
```cpp
void add(int a,int b){
	e[idx]=b; ne[idx]=h[a]; h[a]=idx; idx++; 
}
```
无向图
```cpp
for(int i=1;i<=n-1;i++){
		cin >> a >> b;
		add(a,b); add(b,a);  //a,b间建两条方向相反的边
	}
```
## 写法2
```cpp
vector<int> e[MN]; //e[i][]存储e可到达的每个节点
```
添加a指向b的边
```cpp
e[a].emplace_back(b);
```
含边权的图
```cpp
struct Edge{
int to; int wt;
}
vector<Edge> g[MN];
e[a].emplace_back(Edge{b,w});  //添加a->b，边权为w的边
```