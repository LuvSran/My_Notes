# 扩展欧几里得（log(min(a,b))）
## 求不定方程
求`ax+by=gcd(a,b)`的一组特解$x_1,y_1$，并返回`gcd(a,b)`.
`ax+by=c` ,若`gcd(x,y) | c`,则一组特解$x_0,y_0$为$x_0$=c/gcd(a,b)\*$x_1$，  $y_0$=c/gcd(a,b)\*$y_1$
```CPP
int exgcd(int _a,int _b,int& x,int& y){  
    if(_b==0){  
        x=1,y=0;  
        return _a;  
    }  
    int x1,y1,g;  
    g=exgcd(_b,_a%_b,x1,y1);  //y=x1-_a/_b*y1，顺序不能错
    x=y1,y=x1-_a/_b*y1;  //进行gcd(a,b)=gcd(b,a%b)过程中，x=y1,y=x1-a/b*y1  
    return g;  
}
```
不定方程通解
x=$x_0+\frac{b}{gcd(a,b)}*k，（x_0,y_0为特解）$
$y=y_0-\frac{a}{gcd(a,b)}*k$
## 求同余方程
求$ax\equiv b(\% m)$，将同余转化为不定即$ax\equiv b(\% m)$可得$$ax+my=b$$
exgcd(a,m,x,y)，若`gcd(a,b) | b`,则同余方程特解$x_0=x*b/gcd(a,b)\%m$
## 求a模m意义下的逆元
要求 x 和 m 互质
`exgcd(a,m,x,y)`得`x`,逆元为`(x%m+m)%m`
