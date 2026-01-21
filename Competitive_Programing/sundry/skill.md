1 . 优化
```cpp
#pragma GCC optimize("O3,unroll-loops")
#pragma GCCtarget("avx2,bmi,bmi2,lzcnt,popcnt")
```
快读快写
```cpp
long long read()  
{  
    long long x=0,f=1;char ch=getchar();  
    while (ch<'0'||ch>'9'){if (ch=='-') f=-1;ch=getchar();}  
    while (ch>='0'&&ch<='9'){x=x*10+ch-48;ch=getchar();}  
    return x*f;  
}  
void print(int x)  
{  
    if(x<0){putchar('-');x=-x;}  
    if(x>9) print(x/10);  
    putchar(x%10+'0');  
}
```
2 .随机化哈希可用`mt19937_64`
```cpp
mt19937_64 rng(chrono::steady_clock::now().time_since_epoch().count());
ll gen(){return rng()&((1ll<<40)-1);} //gen()获取随机数，并控制生成的随机数最多为40位
```
$$s[y_1][y_2][…][y_n] + (-1)^1*(s[x_1-1][y_2][…][y_n]+s[y_1][x_2-1][…][y_n]+…+s[y_1][y_2][…][x_n-1])+(-1)^2*(s[x_1-1][x_2-1][…][yn]+s[x_1-1][y_2][x_3-1][…][y_n]+…+s[y_1][y_2][…][x_{n-1}-1][x_n-1])+…+(-1)^n*(s[x_1-1][x_2-1][…][x_n-1])$$
3.sqrt函数开大数根时可能舍入出错，手写纠正或者二分开根
```cpp
ll isqrt(ll x){  
    ll res=sqrtl(x);
    while(res*res<x) ++res;  
    while(res*res>x) --res;  
    return res;  
}
```
4.判断一个数是否是2的幂
```cpp
if((x&(x-1))==0) //true为2的幂
```
5.对齐打印
```cpp
 for(int i=1;i<=n;++i){
        for(int j=1;j<=m;++j) cout<< left << setw(4) << a[i][j] << " ";
        cout << '\n';
    }
```
6.神秘大优化
```cpp
#pragma GCC diagnostic error "-std=c++11"
#pragma GCC target("avx")
#pragma GCC optimize(1)
#pragma GCC optimize(2)
#pragma GCC optimize(3)
#pragma GCC optimize("Ofast")
#pragma GCC optimize("inline")
#pragma GCC optimize("-fgcse")
#pragma GCC optimize("-fgcse-lm")
#pragma GCC optimize("-fipa-sra")
#pragma GCC optimize("-ftree-pre")
#pragma GCC optimize("-ftree-vrp")
#pragma GCC optimize("-fpeephole2")
#pragma GCC optimize("-ffast-math")
#pragma GCC optimize("-fsched-spec")
#pragma GCC optimize("unroll-loops")
#pragma GCC optimize("-falign-jumps")
#pragma GCC optimize("-falign-loops")
#pragma GCC optimize("-falign-labels")
#pragma GCC optimize("-fdevirtualize")
#pragma GCC optimize("-fcaller-saves")
#pragma GCC optimize("-fcrossjumping")
#pragma GCC optimize("-fthread-jumps")
#pragma GCC optimize("-funroll-loops")
#pragma GCC optimize("-fwhole-program")
#pragma GCC optimize("-freorder-blocks")
#pragma GCC optimize("-fschedule-insns")
#pragma GCC optimize("inline-functions")
#pragma GCC optimize("-ftree-tail-merge")
#pragma GCC optimize("-fschedule-insns2")
#pragma GCC optimize("-fstrict-aliasing")
#pragma GCC optimize("-fstrict-overflow")
#pragma GCC optimize("-falign-functions")
#pragma GCC optimize("-fcse-skip-blocks")
#pragma GCC optimize("-fcse-follow-jumps")
#pragma GCC optimize("-fsched-interblock")
#pragma GCC optimize("-fpartial-inlining")
#pragma GCC optimize("no-stack-protector")
#pragma GCC optimize("-freorder-functions")
#pragma GCC optimize("-findirect-inlining")
#pragma GCC optimize("-fhoist-adjacent-loads")
#pragma GCC optimize("-frerun-cse-after-loop")
#pragma GCC optimize("inline-small-functions")
#pragma GCC optimize("-finline-small-functions")
#pragma GCC optimize("-ftree-switch-conversion")
#pragma GCC optimize("-foptimize-sibling-calls")
#pragma GCC optimize("-fexpensive-optimizations")
#pragma GCC optimize("-funsafe-loop-optimizations")
#pragma GCC optimize("inline-functions-called-once")
#pragma GCC optimize("-fdelete-null-pointer-checks")
```