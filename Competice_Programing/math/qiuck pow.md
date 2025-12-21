# 快速幂，o(logn)
*模板*
```cpp
const ll mod=998244353;
ll qpow(ll _a,ll _b){ //快速幂求a^b%p  
    ll res=1;  
    while(_b){  
        if(_b&1) res=res*_a%mod;  
        _a=_a*_a%mod;  
        _b>>=1;     
    }  
    return res%mod;  
}
```
# 矩阵快速幂
```cpp
const ll mod=1e9+7;
struct Matrix{
    vector<vector<ll>> mat;
    int n;
    Matrix(int n,ll _val=0):n(n),mat(n+1, vector<ll>(n+1, _val)) {} //n*n矩阵，矩阵每一位初始值
    void Set(vector<array<ll,3>> vc){ //{x,y,z}将mat[x][y]=z
        for(auto& [x,y,z]:vc) mat[x][y]=z;
    }
    Matrix operator*(const Matrix& other)const{
        Matrix res(n,0);
        for (int i=1;i<=n;++i){
            for (int j=1;j<=n;++j){
                for (int k=1;k<=n;++k) {
                    res.mat[i][j]=(res.mat[i][j]+mat[i][k]*other.mat[k][j]%mod)%mod; //矩阵乘法转移式
                }
            }
        }
        return res;
    }
    Matrix Mpow(ll k) { //求矩阵的k次方
	    if(k==1) return *this;
        Matrix res=*this;
        Matrix base=*this;
        --k;
        while(k){
            if(k&1) res=res*base;
            base=base*base;
            k>>=1;
        }
        return res;
    }
};
```
 `i<=3`时`f[i]=1`，否则`f[i]=f[i-3]+f[i-1]`，求`f[n]`
```
#define KILL(x) return cout<<(x)<<'\n',void()
void solve(){
    int n;
    cin >> n;
    if(n<=3) KILL(1);
    Matrix base(3,0);
    base.Set({{2,1,1},{3,2,1},{1,3,1},{3,3,1}});
    Matrix ans(3,0);
    ans.Set({{1,1,1},{1,2,1},{1,3,1}});
    ans=ans*base.Mpow(n-3);
    KILL(ans.mat[1][3]);
}
```