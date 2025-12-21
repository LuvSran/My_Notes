```
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
ll Inv(ll _x){return qpow(_x%mod,mod-2);}
```
# 求模意义下的逆元
*a存在模m的逆元充要条件：a与m互质*
# 快速幂
要求 m 为质数
$a\%m$的逆元为$a^{m-2}$
快速幂求`int inverse=qpow(a,m-2,m)`
## 扩展欧几里得
要求 x 和 mod 互质
`exgcd(a,m,x,y)`得`x`,逆元为`(x%m+m)%m`
# 线性求逆元
```
const ll mod=998244353;
int inv[maxn];
void init(){
    inv[1]=1;
    for(int i=2;i<maxn;++i) inv[i]=(mod-mod/i)*inv[mod%i]%mod;
}
```