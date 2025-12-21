# 权值线段树
```cpp
template <int N>struct Segment_tree{  
	#define ls (p<<1)
	#define rs ((p<<1)|1)
    struct{  
        int l,r;  
        int sum;  
    }tr[4*N];  
    void push_up(int p){  
        tr[p].sum=tr[ls].sum+tr[rs].sum;  
    }  
    void build(int p,int lo,int ro){  
        tr[p].l=lo,tr[p].r=ro;  
        tr[p].sum=0;  
        if(lo==ro) return;  
        int mid=(lo+ro)>>1;  
        build(ls,lo,mid);  
        build(rs,mid+1,ro);  
        push_up(p);
    }  
    void Fix(int p,int x,int k){   //数x增加/减少k  
        if(tr[p].l==tr[p].r){  
            tr[p].sum+=k;  
            return;  
        }  
        int ndmid=(tr[p].l+tr[p].r)>>1;  
        if(x<=ndmid) Fix(ls,x,k);  
        else Fix(rs,x,k);  
        push_up(p);  
    }  
    int Ask(int p,int lo,int ro){  //查询范围内有多少个数,可查询x的排名:Ask(1,1,x-1)+1  
        if(lo<=tr[p].l && ro>=tr[p].r) return tr[p].sum;  
        int res=0,ndmid=(tr[p].l+tr[p].r)>>1;  
        if(lo<=ndmid) res+=Ask(ls,lo,ro);  
        if(ro>ndmid) res+=Ask(rs,lo,ro);  
        return res;  
    }  
    int Kth(int p,int k){  //查询第k小的数  
        if(tr[p].l==tr[p].r) return tr[p].l;  
        if(k<=tr[ls].sum) return Kth(ls,k);  
        else return Kth(rs,k-tr[ls].sum);    
    }  
};
```