字符串哈希板子
# 无敌双哈
```
mt19937_64 rng(chrono::steady_clock::now().time_since_epoch().count());
int findPrime(ll _x){
    auto isPrime=[&](int _k){
        for(int i=2;i*i<=_k;++i){
            if(_k%i==0) return false;
        }
        return true;
    };
    if(_x<=1) _x=3;
    while(isPrime(_x)==false) ++_x;
    return _x;
}
struct StrHash{
    int _n;
    const array<ll,2> _base={277,1331};
    const array<ll,2> _mod={1122451439,1747937459};
    vector<array<ll,2>> _pre,_p;
    StrHash(){}
    void build(string& _s){
        // _mod[0]=findPrime(rng()%900000000+1000000000);
        // _mod[1]=findPrime(rng()%900000000+1000000000);
        _n=(int)_s.size()-1;
        _pre.resize(_n+5);
        _p.resize(_n+5);
        _p[0][0]=_p[0][1]=1;
        for(int i=1;i<=_n;++i){
            _pre[i][0]=(1ll*_pre[i-1][0]*_base[0]+_s[i])%_mod[0];
            _pre[i][1]=(1ll*_pre[i-1][1]*_base[1]+_s[i])%_mod[1];
            _p[i][0]=1ll*_p[i-1][0]*_base[0]%_mod[0];
            _p[i][1]=1ll*_p[i-1][1]*_base[1]%_mod[1];
        }
    }
    pair<ll,ll> getw(int l,int r){
        ll u=(_pre[r][0]-1ll*_pre[l-1][0]*_p[r-l+1][0]%_mod[0]+_mod[0])%_mod[0];
        ll v=(_pre[r][1]-1ll*_pre[l-1][1]*_p[r-l+1][1]%_mod[1]+_mod[1])%_mod[1];
        return {u,v};
    }
    bool same(int l1,int r1,int l2,int r2){
        return getw(l1,r1)==getw(l2,r2);
    }
};
StrHash Hash;
```
###### O(1)判断某个子串是否是回文串
```
StrHash T1,T2;
int n;    //字符串长度
void init(string& _s){
    string t=" ";
    for(int i=n;i>=1;--i) t+=_s[i];
    T1.build(_s);
    T2.build(t);
}
bool isPal(int l,int r){
    auto idx=[&](int i){
        return n+1-i;
    };
    if(l==r) return true;
    int len=r-l+1;
    return T1.getw(l,l+len/2-1)==T2.getw(idx(r),idx(r-len/2+1));
}
```
# 哈希表/散列表
取模将较大范围内的数据映射到较小范围内
随机化哈希可用`mt19937_64`
```
mt19937_64 rng(chrono::steady_clock::now().time_since_epoch().count());
ll gen(){return rng()&((1ll<<40)-1);} //gen()获取随机数，并控制生成的随机数最多为40位
```

## 字符串哈希
对字符串s(下标1开始)构造has表，`has[i]`表示`s`的`s[1~i]`的子串的哈希值。将`s`视为`base`进制的数，则`has[i]=has[i-1]*base+s[i]`, 则获取`s[l~r]`的哈希值为`has[r]-has[l-1]*p[r-l+1]`;(`p[i]`为`base^i`)
*注意*
- 使用`unsigned long long`，其自然溢出等效于`%(2^64-1)`
## 简单双哈
```
const ll mod=1122451439;
const ull base1=1331;
const ll base2=277;
ull B1[maxn];
ll B2[maxn];
void prework(){
  B1[0]=1; B2[0]=1;
  for(int i=1;i<maxn;++i){
    B1[i]=B1[i-1]*base1;
    B2[i]=B2[i-1]*base2%mod;
  }
}
struct Hash{
  ull H1[maxn];
  ll H2[maxn];
  void Build(string& _s){
    int _n=(int)_s.size()-1;
    for(int i=1;i<=_n;++i){
      H1[i]=H1[i-1]*base1+_s[i];
      H2[i]=H2[i-1]*base2%mod+_s[i];
    }
  }
  ull W_1(int l,int r){
    return H1[r]-H1[l-1]*B1[r-l+1];
  }
  ll W_2(int l,int r){
    return (H2[r]-H2[l-1]*B2[r-l+1]%mod+mod)%mod;
  }
  pair<ull,ll> Get(int l,int r){
    return {W_1(l,r),W_2(l,r)};
  }
};
```
## 单哈
```
ull B[maxn];
const ull base=1331;
void prework(){
	B[0]=1;
	for(int i=1;i<maxn;++i) B[i]=B[i-1]*base;
}
struct Hash{
	ull val[maxn];
	void build(string& s){
		int _n=(int)s.size()-1;
		val[0]=0;
		for(int i=1;i<=_n;++i) val[i]=val[i-1]*base+s[i];
	}
	ull getW(int l,int r){
		return val[r]-val[l-1]*B[r-l+1];
	}
};
```
# 整数哈希
## 拉链法
对h表的每个槽位`k`上拉一条链表，存储哈希值为`k`的元素
*注意*
- 将h所有槽位初始化为`-1`，表示没有链表(元素)
```
const int MN=1e5+20;  
const int mod=100003;  
int h[MN],e[MN],ne[MN],idx;  
void ist(int x){  
    int k=(x%mod+mod)%mod;  
    e[idx]=x; ne[idx]=h[k]; h[k]=idx; ++idx;  
}  
bool qry(int x){  
    int k=(x%mod+mod)%mod;  
    for(int i=h[k];i!=-1;i=ne[i]){  
        if(e[i]==x) return true;  //查询到有此元素
    }  
    return false;  
}  
void solve(){  
    memset(h,-1,sizeof h);  
    int N;  
    cin >> N;  
    while(N--){  
        char c; int a;  
        cin >> c >> a;  
        if(c=='I') ist(a);  
        else if(c=='Q'){  
            if(qry(a)) cout << "Yes\n";  
            else cout << "No\n";  
        }  
    }  
     return;  
}
```




## 开放寻址法(蹲坑法)
建一条has表，看`x`的哈希值`k`上有没有人蹲坑，有人则依次往后找，直到找到没人的坑位
*注意*
- has表长度应该为元素数量3-5倍
- 将has表每个坑位初始化为无穷vd，表示坑位为空
```
const int MN=4*1e5+20;
const int mod=100003,vd=0x3f3f3f3f;
int has[MN];
int find(int x){              //找x的坑位并返回
	int k=(x%mod+mod)%mod;
	while(has[k]!=vd && has[k]!=x){
		++k;
		if(k==MN) k=0;
	}
	return k;     //x已经存在返回其坑位，不存在则返回他应该在的坑位
}
void solve(){
	memset(has,0x3f,sizeof has);
	int N;
	cin >> N;
	while(N--){
		char c; int a;   //c为'I'为插入，'Q'为查询
		cin >> c >> a;
		int pos=find(a);
		if(c=='I') has[pos]=a;
		else if(c=='Q'){
			if(has[pos]==a) cout << "Yes\n";
			else cout << "No\n";
		}
	}
 	return;
}
```
