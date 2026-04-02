# 欧拉函数
φ(n)=小于等于 n 且与 n 互质的正整数的个数
## **2. 欧拉函数的性质**

## ​ (1) 积性函数

如果 gcd(a,b)=1，则：

φ(ab)=φ(a)⋅φ(b)

这使得欧拉函数可以分解计算。

## ​(2) 质数的欧拉函数

如果 p 是质数，则：

φ(p)=p−1

因为 1,2,…,p−1 都与 p 互质。

## (3) 质数幂的欧拉函数

如果 p 是质数，k≥1，则：

$$ φ(p^k)=p^k−p^{k−1} $$


## (4) 一般情况的欧拉函数 

如果 n 的质因数分解为：

$$ n=p_1^{k1}​​p_2^{k2}​​…p_m^{km} $$​​

则欧拉函数可以表示为：

$$ φ(n)=n⋅(1−1/p_1)(1−1/p_2​)…(1−1/p_m) $$
欧拉定理
如果 $$ gcd(a,n)=1$$, 则$$a^{φ(n)}≡1( mod n)$$
求单个欧拉函数
```
int phi(int x){
    int res=x;
    for(int i=2;i*i<=x;++i){
        if(x%i==0){
            res=res/i*(i-1);
        }
        while(x%i==0) x/=i;
    }
    if(x>1) res=res/x*(x-1);
    return res;
}
```
线性筛法求欧拉函数
```
vector<int> phi;  
vector<int> minp;  
vector<int> primes;  
void get_phi(int nn){  
    minp.assign(nn+10,0);  
    phi.assign(nn+10,0);  
    primes.clear();  
    phi[1]=1;  
    for(int i=2;i<=nn;++i){  
        if(minp[i]==0){  
            minp[i]=i;  
            phi[i]=i-1;  
            primes.pb(i);  
        }  
        for(int x:primes){  
            if(1ll*x*i>nn) break;  
            minp[x*i]=x;  
            if(minp[i]==x){  
                phi[x*i]=x*phi[i];  //p,i不互质，则phi[i*p]=p*phi[i]
                break;  
            }  
            else phi[p*i]=(p-1)*phi[i];  //p,i互质，则phi[p*i]=(p-1)*phi[i]
        }  
    }  
}
```