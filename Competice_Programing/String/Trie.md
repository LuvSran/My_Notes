Trie树（字典树）
用于在一个集合中高效地插入和查找字符串
***模板（插入和查询）***
```
int32_t bk[300];
void prework(){
	for(int i='0',j=0;i<='9';++i,++j) bk[i]=j;
	for(int i='a',j=10;i<='z';++i,++j) bk[i]=j;
	for(int i='A',j=36;i<='Z';++i,++j) bk[i]=j;
}
template<int N>struct Trie{ //传字符串s一定要s=" "+s;
    int32_t tr[N][65],cnt[N];
    int32_t fa[N]; char ch[N];
    int32_t ix=0;
    void Clear(){
        for(int i=0;i<=ix;++i){
            cnt[i]=fa[i]=ch[i]=0;
            for(int j=0;j<=65;++j) tr[i][j]=0;
        }
        ix=0;
    }
    void Insert(string& s){ //插入字符串s
        int n=(int)s.size()-1;
        int u=0;
        for(int i=1;i<=n;++i){
            int v=bk[s[i]];
            if(tr[u][v]==0) tr[u][v]=++ix,fa[ix]=u,ch[ix]=s[i];
            u=tr[u][v];
            //++cnt[u]; //注意前缀子串是否统计
        }
        cnt[u]+=1; //前缀子串统计时此句要删
    }
    int Add(string& s,int u){ //在节点u之后接上字符串s,并返回节点编号
        int n=(int)s.size()-1;
        for(int i=1;i<=n;++i){
            int v=bk[s[i]];
            if(tr[u][v]==0) tr[u][v]=++ix,fa[ix]=u,ch[ix]=s[i];
            u=tr[u][v];
        }
        cnt[u]+=1;
        return u;
    }
    int Ask(string& s){  //问字符串s插入次数
        int n=(int)s.size()-1;
        int u=0;
        for(int i=1;i<=n;++i){
            int v=bk[s[i]];
            if(tr[u][v]==0) return 0;
            u=tr[u][v];
        }
        return cnt[u];
    }
    string Recover(int u){ //节点u恢复出原字符串
        string res="";
        while(u!=0){
            res+=ch[u];
            u=fa[u];
        }
        reverse(res.begin(),res.end());
        return res;
    }
};
Trie<1000020> T;
```
## 01 Trie
```
template<int N,int bitN>struct _01Trie{ //总数量 / 数字bit位数
    int son[N*bitN][2];  
    ll val[N*bitN]; //指针i对应的数字
    int cnt[N*bitN];   //指针i对应的节点数量
    int pt=0;
    void Insert(ll x){    //插入xu
        int u=0;
        for(int i=bitN;i>=0;--i){
            int t=(x>>i)&1;
            if(son[u][t]==0) son[u][t]=++pt;
            u=son[u][t];
            ++cnt[u];
        }
        val[u]=x;
    }
    void Del(ll x){     //删除x
        int u=0;
        for(int i=bitN;i>=0;--i){
            int t=(x>>i)&1;
            u=son[u][t];
            --cnt[u];
        }
    }
    ll Ask(ll x){     //求一个数y,返回max(x^y)
        int u=0;
        for(int i=bitN;i>=0;--i){
            int t=(x>>i)&1;
            if(cnt[son[u][t^1]]>=1) u=son[u][t^1];
            else u=son[u][t];
        }
        return x^val[u];
    }
    void Clear(){
        for(int i=0;i<=pt;++i){
            son[i][0]=son[i][1]=0;
            cnt[i]=val[i]=0;
        }
        pt=0;
    }
};
```