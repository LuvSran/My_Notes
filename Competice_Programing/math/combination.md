# 求组合数
```cpp
const ll mod=1e9+7;
namespace Comb{
    int type=3;
    int N=1e6+20;
    const ll Mod=mod;
    ll Mod2=1331;
    vector<ll> fac;
    vector<ll> invfac;
    ll Inv(ll _a){  
        ll res=1;
        ll _b=Mod-2;
        while(_b){  
            if(_b&1) res=res*_a%Mod;  
            _a=_a*_a%Mod;  
            _b>>=1;    
        }  
        return res%Mod;  
    }
    ll F_getC(int n,int m){
        if(m>n) return 0;
        return fac[n]*invfac[n-m]%Mod*invfac[m]%Mod;
    }
    ll L_getC(ll n,ll m){
        if(m>n) return 0;
        return fac[n]*invfac[n-m]%Mod2*invfac[m]%Mod2;
    }
    ll lucas(int n,int m){
        if(m==0) return 1;
        return (lucas(n/Mod2,m/Mod2)*L_getC(n%Mod2,m%Mod2)+Mod2)%Mod2;
    }
    void set_normal(int n){  //普通,o(nlogP)时间，o(n)空间处理，o(1)查询，P要与n,m互质
        N=n; type=2;
        fac.resize(N+5);
        invfac.resize(N+5);
        fac[0]=invfac[0]=1;
        for(int i=1;i<=N;++i){
            fac[i]=fac[i-1]*i%Mod;
            invfac[i]=invfac[i-1]*Inv(i)%Mod;
        }
    }
    void set_lucas(int P){  //o(logP)查询，P可不与n互质，且适用于n=1e18等大数情况
        N=P; type=3; Mod2=P;
        fac.resize(N+5);
        invfac.resize(N+5);
        fac[0]=invfac[0]=1;
        for(int i=1;i<=N;++i){
            fac[i]=fac[i-1]*i%Mod2;
            invfac[i]=invfac[i-1]*Inv(i)%Mod2;
        }
    }
    ll C(ll n,ll m){         //n下标，m上标
        if(type==2) return F_getC(n,m);
        else return lucas(n,m);
    }
}
```
## 结论：
`if(n&m==m)`,$C^m_n$是奇数，否则是偶数
## 杨辉三角递推,o($n^2$)预处理，o(1)查询
$C^m_n=C^m_{n-1}+C^{m-1}_{n-1}$
```cpp
ll comb[maxn][maxn];  //下标/上标  
void init(int nn,ll p){  
    for(int i=0;i<=nn;++i){  
        for(int j=0;j<=i;++j){  
            if(j==0) comb[i][j]=1;  
            else comb[i][j]=(comb[i-1][j]+comb[i-1][j-1])%p;  
        }  
    }  
}
ll getC(int _n,int _m){
	return comb[_n][_m];  //_m>_n返回0
}
```

## 快速幂,o($n*logP$)预处理,o(1)查询

模数p应为质数，且n,m与p互质。n,m过大或n,m不与p互质，应用lucas
$$C^m_n=\frac{n!}{(n-m)! m!}$$

```cpp
ll fac[maxn],invfac[maxn]; //i的阶乘，i的阶乘的逆元
void init(int _n){
    fac[0]=invfac[0]=1;
    for(int i=1;i<=_n;++i) fac[i]=fac[i-1]*i%mod;
    for(int i=1;i<=_n;++i) invfac[i]=invfac[i-1]*qpow(i,mod-2)%mod;
}
ll getC(int _n,int _m){
    if(_m>_n) return 0;
    return fac[_n]*invfac[_m]%mod*invfac[_n-_m]%mod;
}
```
## 卢卡斯定理,o($n*logP$)预处理,o($logN$)查询
模数P应为质数
$$C^m_n≡C^{m/p}_{n/p}*C^{m\%p}_{n\%p}(mod P)$$
```cpp
void init(int nn,int _p){   //传入nn=p
    fac[0]=invfac[0]=1;  
    for(int i=1;i<=nn;++i){  
        fac[i]=fac[i-1]*i%_p;  
        invfac[i]=invfac[i-1]*qpow(i,_p-2,_p)%_p;  
    }  
}  
ll getC(int _n,int _m,int _p){  
    if(_m>_n) return 0;  
    return fac[_n]*invfac[_n-_m]%_p*invfac[_m]%_p;  
}  
ll lucas(ll _n,ll _m,ll _p){  
    if(_m==0) return 1;  
    return (lucas(_n/_p,_m/_p,_p)*getC(_n%_p,_m%_p,_p)+_p)%_p;  
}
```