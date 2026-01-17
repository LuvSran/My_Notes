枚举子集的非空子集,o($3^n$)
```cpp
 for(int i=0;i<=lim;++i){
	 for(int s=(i-1)&i;s>0;s=(s-1)&i){
	     int other=i^s;
         if(s>other) continue; //去重,防止重复计算
         //other为i的所有非空子集
	  }
  }
```
求0~n-1的最短哈密顿路径长度
```cpp
#define Bit(_i,_j) ((_i>>_j)&1)
#define Set_0(_i,_j) (_i&=(~(1ll<<_j)))
const int maxn=22;
ll a[maxn][maxn]; //边权邻接矩阵
void solve(){
    int n;
    cin >> n;
    for(int i=1;i<=n;++i) for(int j=1;j<=n;++j) cin >> a[i][j];
    int lim=(1<<n)-1;
    vector<vector<ll>> dp(lim+3,vector<ll>(n+2,INF));
    dp[1][0]=0;
    for(int i=1;i<=lim;++i){
        for(int j=0;j<=n-1;++j){
            if(Bit(i,j)==0) continue;
            int t=i;
            Set_0(t,j);
            for(int k=0;k<=n-1;++k){
                if(Bit(t,k)==0) continue;
                dp[i][j]=min(dp[i][j],dp[t][k]+a[k+1][j+1]);
            }
        }
    }
    KILL(dp[lim][n-1]);
}
```