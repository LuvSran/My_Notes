数位dp
Windy 定义了一种 Windy 数：不含前导零且相邻两个数字之差至少为 2 的正整数被称为Windy 数。
Windy 想知道，在 A 和 B 之间，包括 A 和 B，总共有多少个 Windy 数？
```cpp
ll dp[20][20][2];
int word;
vector<int> num;
int Len;
ll ABS(ll _x){
    return _x>=0?_x:-_x;
}
ll dfs(int pos,int lst,bool lim,bool lead){
    if(pos==0) return 1;
    if(lim==false && dp[pos][lst][lead]!=-1) return dp[pos][lst][lead];
    int up=9;
    if(lim==true) up=num[pos];
    ll res=0;
    for(int i=0;i<=up;++i){
        if(lead==false && ABS(i-lst)<=1) continue;
        bool nlim=(lim&&(i==num[pos]));
        res+=dfs(pos-1,i,nlim,lead && (i==0));
    }
    if(lim==false) dp[pos][lst][lead]=res;
    return res;
}
ll work(ll _x){
    num.assign(1,0);
    while(_x) num.pb(_x%10),_x/=10;
    Len=Size(num)-1;
    for(int i=0;i<=Len+2;++i){
        for(int j=0;j<=11;++j) dp[i][j][0]=dp[i][j][1]=-1;
    }
    return dfs(Len,0,true,true);
}
void solve(){
    ll L,R;
    cin >> L >> R;
    ll ans=work(R)-work(L-1);
    cout << ans << '\n';
}
```
某人又命名了一种取模数，这种数字必须满足各位数字之和 mod N 为 0。
现在大家又要玩游戏了，指定一个整数闭区间 \[a，b]，问这个区间内有多少个取模数。(N<=100)
```cpp
int dp[40][300];
vector<int> num;
int Len;
int N;
int dfs(int pos,int sum,bool lim){
	if(pos==0) return sum%N==0;
	if(lim==false && dp[pos][sum]!=-1) return dp[pos][sum];
	int up=9;
	if(lim==true) up=num[pos];
	int res=0;
	for(int i=0;i<=up;++i){
		bool nlim= lim && (i==num[pos]);
		res+=dfs(pos-1,sum+i,nlim);
	}
	if(lim==false) dp[pos][sum]=res;
	return res;
}
int work(int _x){
	num.assign(1,0);
	while(_x) num.pb(_x%10),_x/=10;
	Len=Size(num)-1;
	for(int i=0;i<=Len;++i){
		for(int j=0;j<=Len*9;++j) dp[i][j]=-1;
	}
	return dfs(Len,0,true);
}
void solve(){
	ll L,R;
	cin >> L >> R;
	ll ans=work(R)-work(L-1);
	cout << ans << '\n';
}
```
如果一个整数符合下面三个条件之一，那么我们就说这个整数和 7 有关：

1. 整数中某一位是 7；
2. 整数的每一位加起来的和是 7 的整数倍；
3. 这个整数是 7 的整数倍。

现在问题来了：吉哥想知道在一定区间内和 7 无关的整数的平方和。
```cpp
const int maxn=2*1e5+20;
const ll mod=1e9+7;
vector<int> num;
int Len;
struct Info{
	ll cnt,sum_1,sum_2;
};
ll pw[100];
Info dp[70][10][10];
Info dfs(int pos,int y1,int y2,bool lim){ //数位和%7，数%7
	if(pos==0) return {(y1!=0) && (y2!=0),0,0}; //err
	if(lim==false && dp[pos][y1][y2].cnt!=-1) return dp[pos][y1][y2];
	int up=9;
	if(lim==true) up=num[pos];
	ll cnt=0,sum_1=0,sum_2=0;
	for(int i=0;i<=up;++i){
		if(i==7) continue;
		ll B=i*pw[pos-1]%mod;
		bool nlim= lim && (i==num[pos]);
		Info t=dfs(pos-1,(y1+i)%7,(y2*10+i)%7,nlim);
		cnt=(cnt+t.cnt)%mod;
		sum_1=(sum_1+t.sum_1%mod+(i128)B*t.cnt%mod)%mod;
		sum_2=(sum_2+((i128)B*B%mod*t.cnt%mod+t.sum_2)%mod+(i128)2*B%mod*t.sum_1%mod)%mod;
	}
	if(lim==false) dp[pos][y1][y2]={cnt,sum_1,sum_2};
	return {cnt,sum_1,sum_2};
}
ll work(ll _x){
	num.assign(1,0);
	while(_x) num.pb(_x%10),_x/=10;
	Len=Size(num)-1;
	for(int i=0;i<=Len;++i){
		for(int j1=0;j1<=8;++j1) for(int j2=0;j2<=8;++j2) dp[i][j1][j2]={-1,0,0}; //err
	}
	return dfs(Len,0,0,true).sum_2;
}
void solve(){
	ll L,R;
	cin >> L >> R;
	ll ans=(work(R)-work(L-1)+mod)%mod;
	KILL(ans);
}
void prework(){
	pw[0]=1;
	for(int i=1;i<=70;++i) pw[i]=pw[i-1]*10ll%mod;
}
```
给定正整数 N,K。 请求出所有小于等于 N 的正整数 x 中，满足以下条件的数的总和，并将结果对 998244353 取模。
- x 的 popcount 值恰好为 K。
popcount 指的是，对于正整数 y，其 popcount 值 popcount(y) 表示 y 在二进制表示中数字 1 出现的次数。例如，popcount(5)=2，popcount(16)=1，popcount(25)=3。
```cpp
const ll mod=998244353;
pii dp[105][80][2]; //dp[i][j]:前i位，有j位1的总和/个数
vector<int> num;
int Len,k;
pii dfs(int pos,int cnt,bool lim){
    if(pos==0){
        if(cnt==0) return {0,1};
        return {0,0};
    }
    if(cnt<0) return {0,0};
    if(dp[pos][cnt][lim]!=make_pair(-1,-1)) return dp[pos][cnt][lim];
    int up=1;
    if(lim==true) up=num[pos];
    pii res={0,0};
    ll B=(1ll<<(pos-1))%mod;
    for(int i=0;i<=up;++i){
        bool nlim=lim && (i==num[pos]);
        pii t=dfs(pos-1,cnt-(i==1),nlim);
        res.sd=(res.sd+t.sd)%mod;
        res.ft=((res.ft+t.ft)%mod+(i==1)*B*t.sd%mod)%mod;
    }
    dp[pos][cnt][lim]=res;
    return res;
}
ll work(ll _N){
    num.assign(1,0);
    while(_N){
        num.pb(_N%2);
        _N/=2;
    }
    Len=Size(num)-1;
    for(int i=0;i<=Len+5;++i) for(int j=0;j<=75;++j) dp[i][j][0]=dp[i][j][1]={-1,-1};
    ll res=dfs(Len,k,true).ft;
    return res%mod;
}
void solve(){    
    ll N;
    cin >> N >> k;
    ll ans=(ll)work(N);
    cout << ans;
}
```