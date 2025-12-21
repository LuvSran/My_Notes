# 链表
```
template<int N>struct List{
    int tot=1; //指针编号，虚头指针为0,虚尾指针为1
    struct nd{
        int pr,nx;
        ll w;
    }v[N];
    void Init(){ //非多测也要Init()
        for(int i=1;i<=tot;++i) v[i].pr=v[i].nx=v[i].w=0;
        v[0].nx=1;
        tot=1;
    }
    i32 HD(){return v[0].nx;} //真头指针
    i32 TL(){return v[1].pr;} //真尾指针
    void Add(int ix,int x){ //在ix指针之后插入x
        int u=++tot;
        v[u].w=x;
        v[u].nx=v[ix].nx;
        v[u].pr=ix;
        v[v[ix].nx].pr=u;
        v[ix].nx=u;
    }
    void Add_front(int x){ Add(0,x); }
    void Add_back(int x){Add(v[1].pr,x);}
    void Del(int l,int r){ //删除指针l,r之间的节点(不含l,r)
        v[l].nx=r;
        v[r].pr=l;
    }
    ll Ask_sum(int l,int r){ //求指针l,r之间的节点权值和(不含l,r)
        int p=v[l].nx;
        ll res=0;
        while(p!=r){
            res+=v[p].w;
            p=v[p].nx;
        }
        return res;
    }
};
```