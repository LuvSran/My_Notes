1. 异或运算（^）优先级低于加减法，要注意加括号
2. 判断一个数是否是2的幂：`if(lowbit(a)==a)`
3. 即使运算过程中不断进行取模，也有可能数据溢出，比如`res*=i%mod`可能会溢出，应写`res=res%mod*i%mod`
4. 优化
```cpp
#pragma GCC optimize("O3,unroll-loops")
#pragma GCCtarget("avx2,bmi,bmi2,lzcnt,popcnt")
```
5. 一个数加上奇数会改变奇偶性，加上偶数不会改变奇偶性。1和0xor1会变，xor0不变
6. 获取2的i次幂应该`(1ll<<i)`而非`1<<i`
7. `log(x)`计算的是`e`为底，而非2为底，`log2(x),log10(x)`以2和10为底