# 分解质因数
试除(o(log)~o(n^(1/2)))
```
map<int,int> dvo;
void divide(int x){  
    for(int i=2;i<=x/i;++i){  
        if(x%i==0){  
            int s=0;  
            while(x%i==0){  
                ++s;  
                x/=i;  
            }  
            dvo[i]+=s;;  
        }  
    }  
    if(x>1) dvo[x]+=1;  
}

	for(auto [ds,zs]:dvo){  //获取质因数底数，指数
		cout << ds << ' ' << zs << '\n';
	}
	
```
线性筛预处理
```
#define pb emplace_back
vector<int> minp;
vector<int> prime;
void ola_prime(int n){
	minp.assign(n+10,0);
	prime.clear();
	for(int i=2;i<=n;++i){
		if(minp[i]==0){
			minp[i]=i;
			prime.pb(i);
		}
		for(int p:primes){
			if(1ll*i*p>n) break;
			minp[i*p]=p;
			if(p==minp[i]) break;//p是i的最小质因子，结束
		}
	}
}
map<int,int> div;
void divide(int x){ //logX
	while(minp[x]>1) ++div[minp[x]],x/=minp[x];
}

```