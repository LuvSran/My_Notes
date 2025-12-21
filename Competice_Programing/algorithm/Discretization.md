# 离散化
## 离散化含义
有 有序a[ ]: 1 3 100 2000 50000 .....
分别对应i: 0 1 2     3       4...... 即排完序后，下标是其映射的值
可通过映射的i(下标)，映射到其他数组/容器中对应的另一个值，或进行其他操作 
 * a[ ]中可能含重复元素，应去重
```cpp
	vector<int> alls;//存储待离散化的值
	sort(alls.begin(),alls.end()); //有序化
	alls.erase(unique(alls.begin(),alls.end()) , alls.end() ); //去重
```
* 找x对应离散化的值(i)，二分
```cpp
	int find(x){ //找到第一个大于等于x的位置
	int mid,l=0,r=alls.size()-1;
	while(l<r){
	 mid=l+r >> 1;
	 if(alls[mid]>=x) r=mid;
	 else l=mid+1;
	}
	return r+1; //加不加1具体看题目，从0开始还是从1开始
	}
```
### 例题
假定有一个无限长的数轴，数轴上每个坐标上的数都是 0。首先进行 n次操作，每次操作将某一位置 x上的数加 c。接下来，进行 m次询问，每个询问包含两个整数 l和 r，你需要求出在区间 [l,r]之间的所有数的和。
######  输入格式
第一行包含两个整数 n 和 m。
接下来 n行，每行包含两个整数 x和 c。
再接下来 m 行，每行包含两个整数 l和 r。
   ###### 输出格式
共 m行，每行输出一个询问中所求的区间内数字和。
###### 数据范围
−10^9≤x≤10^9,  
1≤n,m≤10^5,  
−10^9≤l≤r≤10^9,  
−10000≤c≤10000
###### 输入样例：
```cpp
3 3
1 2
3 6
7 5
1 3
4 6
7 8
```
###### 输出样例：
```
8
0
5
```
###### 题解(前缀和+离散化)
```cpp
const int MAX=4*1e5+20;
int a[MAX];
int s[MAX];
vector<int> alls;
vector<pii> opt;
vector<pii> query;
int find(int x){
	int l=1,r=(int)alls.size()-1;
	while(l<r){
		int mid=(l+r+1)>>1;
		if(alls[mid]<=x) l=mid;
		else r=mid-1;
	}
	return l;
}
void solve(){
	int n,m;
	cin >> n >> m;
	alls.pb(-1e17);
	while(n--){
		int x,c;
		cin >> x >> c;
		opt.pb(make_pair(x,c));
		alls.pb(x);
	}
	while(m--){
		int l,r;
		cin >> l >> r;
		alls.pb(l); alls.pb(r);
		query.pb(make_pair(l,r));
	}
	sort(alls.begin(),alls.end());
	alls.erase(unique(alls.begin(),alls.end()),alls.end());
	int nsz=(int)alls.size()-1;
	for(auto [x,c]:opt){
		a[find(x)]+=c;
	}
	for(int i=1;i<=nsz;++i) s[i]=s[i-1]+a[i];
	for(auto [l,r]:query){
		int ans=s[find(r)]-s[find(l)-1];
		cout << ans << '\n';
	}
	return;
}