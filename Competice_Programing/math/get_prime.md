# 欧拉筛板子
```
template<int N> struct SHAI{
    vector<int> prim,minp;
    void Run(){  //筛1~N的质数
        minp.assign(N+5,0);
        for(int i=2;i<=N;++i){  
            if(minp[i]==0) prim.pb(i),minp[i]=i;  
            for(int x:prim){
                if(i*x>N) break;
                minp[i*x]=x;
                if(x==minp[i]) break;
            }
        }  
    }
    bool Ask(int _x){  //_x是质数返回true
        return minp[_x]==_x;
    }
    int Minp(int _x){  //返回_x的最小质因数
        return minp[_x];  
    }
};
SHAI<maxn> T;
```
## 埃氏筛
```
const int MAX=1e6+20;  
int prime[MAX];  
bitset<maxn> comp;  //true表合数
int cnt=0;  
void es_prime(int n){  
    for(int i=2;i<=n;++i){  
        if(comp[i]==false){  
            prime[++cnt]=i;  
            for(ll j=i*i;j<=n;j+=i) comp[j]=true;  
        }  
    }  
}  
```
## 线性筛(欧拉筛)
```
const int MAX=1e6+20;  
int prime[MAX]; //true表合数
int minp[MAX]; //每个数的最小质因数
int st[MAX];  
int cnt=0;  
void ola_prime(int n){  
    for(int i=2;i<=n;++i){  
        if(st[i]==false) prime[++cnt]=i,minp[i]=i;  
        for(int j=1;1ll*i*prime[j]<=n;++j){  
            st[i*prime[j]]=true;
            minp[i*prime[j]]=prime[j];  
            if(i%prime[j]==0) break;//prime[j]是i的最小质因子，结束
        }  
    }  
}
```
# 区间筛
```
template<int N>struct Range_Prime{ //N>=sqrt(R) && N>=R-L 
    bitset<N> comp;
    bitset<N> comp_2;
    int minp[N];
    ll Lo,Ro;
    void Init(){
         for(int i=2;i<N;++i){
            if(comp[i]==false){
                for(ll j=1ll*i*i;j<N;j+=i) comp[j]=true;
            }
        }
    }
    void Run(ll L,ll R){  //需要保证N*N>=R且R-L<=N
        Lo=L,Ro=R;
        for(ll i=L;i<=R;++i) comp_2[i-L]=false;
        ll sq=sqrtl(R);
        while(sq*sq<R) ++sq; while(sq*sq>R) --sq;
        for(int i=2;i<=sq;++i){
            if(comp[i]==false){
                for(ll j=max(2ll,(L/i+(L%i!=0)))*i;j<=R;j+=i){
                    if(minp[j-L]==0) minp[j-L]=i;
                    comp_2[j-L]=true;
                }
            }
        }
    }
    bool Ask(ll _k){ //k为质数返回true
        if(_k<=1) return false;
        return comp_2[_k-Lo]==false;
    }
    int Minp(ll _k){
        if(Ask(_k)==true) return _k;
        return minp[_k-Lo];
    } //返回k的最小质因子
};
Range_Prime<maxn> T; 
```