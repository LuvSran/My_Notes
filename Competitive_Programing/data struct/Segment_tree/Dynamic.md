动态开点线段树
```
const int maxn=3*1e5*50;
int root;
namespace L{  
    int sit; //地址
    int ls[maxn],rs[maxn]; //左儿子地址，右儿子地址
    int sum[maxn];
    int tag[maxn]; //1为工作日，2为休息日，0表空
    inline void push_up(int id){
        sum[id]=sum[ls[id]]+sum[rs[id]];
    }
    void push_down(int id,int nl,int nr){
        if(tag[id]==0) return;
        if(ls[id]==0) ls[id]=++sit;
        if(rs[id]==0) rs[id]=++sit;
        int nmid=(nl+nr)>>1;
        int w=(tag[id]==1)?1:0;
        sum[ls[id]]=w*(nmid-nl+1);
        sum[rs[id]]=w*(nr-nmid);
        tag[ls[id]]=tag[id];
        tag[rs[id]]=tag[id];
        tag[id]=0;
    }
    void modify(int& u,int rl,int rr,int nl,int nr,int val){
        if(u==0) u=++sit;
        if(rl<=nl && rr>=nr){
            int w=(val==1)?1:0;
            sum[u]=w*(nr-nl+1);
            tag[u]=val;
            return;
        }
        push_down(u,nl,nr);
        int nmid=(nl+nr)>>1;
        if(rl<=nmid) modify(ls[u],rl,rr,nl,nmid,val);
        if(rr>=nmid+1) modify(rs[u],rl,rr,nmid+1,nr,val);
        push_up(u);
    }
};
```