```
int GCD(int a, int b) {  
	return b == 0 ? a : gcd(b, a % b);  
}  
```
## GCD性质
1. gcd(a,b)=gcd(a,b+ka)
2. $\gcd(k*x,k*y)=k*\gcd(x,y)\rightarrow对于x,y,gcd(x,y)为g.则\frac{x}{g}与\frac{y}{g}互质$
3. $\gcd(x,y)=\gcd(x,y-x)=\gcd(x-y,y)$



