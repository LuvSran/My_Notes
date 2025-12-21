# 线性DP
状态之间是简单的线性关系

---------

# 背包DP(一种特殊的线性DP)
## 例题
### 01背包
有 N 件物品和一个容量是 V的背包。每件物品只能使用一次。
第 i件物品的体积是 vi，价值是 wi。
求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。  
输出最大价值。
#### 输入格式
第一行两个整数，N，V，用空格隔开，分别表示物品数量和背包容积。
接下来有 N 行，每行两个整数 vi,wi，用空格隔开，分别表示第 i件物品的体积和价值。
#### 输出格式
输出一个整数，表示最大价值。
#### 题解1(二维)
```
const int N=1050;
int f[N][N];    //f(i)(j)只考虑前i个物品，且背包体积为j时，背包能装的最大价值
int v[N],w[N]; //每个物品的体积，价值
int n,m;
void solve(){
    cin >> n >> m;
    for(int i=1;i<=n;i++) cin >> v[i] >> w[i];
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            f[i][j]=f[i-1][j];
            if(j>=v[i]) f[i][j]=max(f[i-1][j],f[i-1][j-v[i]]+w[i]);
        }
    }
    int ans=f[n][m];
    cout << ans << '\n';
    return;
}
```
#### 题解2(优化一维)
```
const int N=1050;
int f[N];    //f[i]为背包容量为i时，能装的最大价值
int v[N],w[N]; //每个物品的体积，价值
int n,m;
void solve(){
    cin >> n >> m;
    for(int i=1;i<=n;i++) cin >> v[i] >> w[i];
    for(int i=1;i<=n;i++){//考虑加不加第i个物品，即计算只考虑前i个物品情况下的最大价值
      for(int j=m;j>=v[i];j--){//考虑背包容量为j的情况下的最大价值
            f[j]=max(f[j],f[j-v[i]]+w[i]);
        }
    }
    int ans=f[m];
    cout << ans << '\n';
    return;
}
```
***
### 完全背包
有 N 种物品和一个容量是 V的背包，每种物品都有无限件可用。
第 i种物品的体积是 vi，价值是 wi。
求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。  
输出最大价值。
#### 输入格式
第一行两个整数，N，V，用空格隔开，分别表示物品种数和背包容积。
接下来有 N 行，每行两个整数 vi,wi，用空格隔开，分别表示第 i 种物品的体积和价值。
#### 输出格式
输出一个整数，表示最大价值。
#### 题解1(朴素做法，容易TLE)
```
int f[N][N];   
int v[N],w[N]; //每个物品的体积，价值
int n,m;
void solve(){
    cin >> n >> m;
    for(int i=1;i<=n;i++) cin >> v[i] >> w[i];
    for(int i=1;i<=n;i++){      //考虑加不加第i个物品，即只考虑前i个物品情况下的最大价值 
       for(int j=1;j<=m;j++){   //考虑背包容量为j的情况下
           for(int k=0;k*v[i]<=j;k++){  //考虑加k个第i件物品情况下最大价值
               f[i][j]=max(f[i][j],f[i-1][j-k*v[i]]+k*w[i]);
           }
       }
    }
    int ans=f[n][m];
    cout << ans << '\n';
    return;
}
```
```
//优化
for(int i=1;i<=n;i++){      //考虑加不加第i个物品，即只考虑前i个物品情况下的最大价值 
       for(int j=1;j<=m;j++){   //考虑背包容量为j的情况下
          f[i][j]=f[i-1][j];
          if(j>=v[i]) f[i][j]=max(f[i][j],f[i][j-v[i]]+w[i]);
       }
    }
```
优化证明:*f(i,j)=max(f(i-1,j),==f(i-1,j-v)+w, f(i-1,j-2v)+2w, f(i-1,j-3v)+3w......==*
而*f(i,j-v)= ==max(f(i-1,j-v), f(i-1,j-2v)+w, f(i-1,j-3v)+2w)==*
因此*f(i,j)=max(f(i-1,j), f(i,j-v)+w);*
#### 题解2(一维)
```
const int N=1050;
int f[N];   
int v[N],w[N]; //每个物品的体积，价值
int n,m;
void solve(){
    cin >> n >> m;
    for(int i=1;i<=n;i++) cin >> v[i] >> w[i];
    for(int i=1;i<=n;i++){      //考虑加不加第i个物品，即只考虑前i个物品情况下的最大价值 
       for(int j=v[i];j<=m;j++){   //考虑背包容量为j的情况下
           f[j]=max(f[j],f[j-v[i]]+w[i]);
       }
    }
    int ans=f[m];
    cout << ans << '\n';
    return;
}
```
***
### 多重背包
有 N种物品和容量是V 的背包。
第 i种物品最多有 si 件,每件体积是 vi,价值是 wi。
输入N，V，和每件物品的vi,wi,si。
输出背包最大价值。
#### 朴素做法(TLE)
```
const int N=120;
int n,m;
int f[N][N];
int v[N],w[N],s[N];
int main(){
    cin >> n >> m;
    for(int i=1;i<=n;i++) cin >> v[i] >> w[i] >> s[i];
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            for(int k=0;k<=s[i]&&k*v[i]<=j;k++){
                f[i][j]=max(f[i][j],f[i-1][j-k*v[i]]+k*w[i]);
            }
        }
    }
    int ans=f[n][m];
    cout << ans << '\n';
    return 0;
}
```
#### 二进制优化
*二进制优化思想*（N\*V\*logS）
1,2,4,8,....,2^k可组合成1-2^(k+1)-1之间的任何数，因此将每件物品的个数si拆分成1,2,4,……,c,使其能组合成1---si之间的任何数，例如si为300，则拆成1,2,4……128,45(因为300-(128\*2-1)=45),将拆分的每种个数的i物品合在一起组成新物品，则处理完每件物品只能用一次，然后用01背包解法做
```
const int N=30000;
int n,m;
int f[N];
int v[N],w[N];
int main(){
    cin >> n >> m;
    int vi,wi,si;
    int cnt=0;
    for(int i=1;i<=n;i++){
        cin >> vi >> wi >> si;
        int k=1;
        while(k<=si){   //将第i个物品多件捆绑，看作新的物品，每件分别由1,2,4...件i物品组成
            cnt++;
            v[cnt]=k*vi;
            w[cnt]=k*wi;
            si-=k;
            k*=2;
        }
        if(si>0){
        cnt++;
        v[cnt]=si*vi;
        w[cnt]=si*wi;
        }
    }
    n=cnt;
    for(int i=1;i<=n;i++){   //01背包做法
        for(int j=m;j>=v[i];j--){
            f[j]=max(f[j],f[j-v[i]]+w[i]);
        }
    }
    int ans=f[m];
    cout << ans << '\n';
    return 0;
}

```
***
### 分组背包
有若干组物品，每组物品中有若干件，只能选其中一件，输入组数，背包容量，然后输入每组件数k，再输入k个物品的体积，价值。输出背包最大值
#### 题解
```
const int N=120;
int f[N];
int v[N][N],w[N][N],k[N];
int n,m;
int main(){
    cin >> n >> m;
    for(int i=1;i<=n;i++){
        cin >> k[i];
        for(int j=1;j<=k[i];j++) cin >> v[i][j] >> w[i][j];
    }
    
    for(int i=1;i<=n;i++){
          for(int j=m;j>=0;j--)
           for(int z=1;z<=k[i];z++)
            if(j>=v[i][z]) f[j]=max(f[j],f[j-v[i][z]]+w[i][z]);
    }
    int ans=f[m];
    cout << ans << '\n';
    return 0;
}
```
***
# 区间DP
一般情况，区间DP枚举时，第一维通常枚举len，且len=1时通常用来初始化，状态计算从==len=2==开始枚举。第二维枚举循环起点i，左端点==l=i==，右端点==r=i+len-1==
*模板*
```
for(int i=1;i<=n;i++) f[i][i]=初始化value //len为1时的value
for(int len=2;len<=n;len++){
   for(int l=1,r=l+len-1;r<=n;l++,r++){
      for(int k=l;k<=r-1;k++){     //枚举分割点
         f[l][r]=min(f[l][r],f[l][k]+f[k+1][r]+w[i]……); //状态转移
      }
  }
}

