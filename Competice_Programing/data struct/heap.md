堆，一棵完全二叉树，以小根堆为例，每个节点都小于等于两个子节点，所有元素中最小的在根节点；节点序号从上往下，从左往右从1开始递增（根节点为1）
![[f55babb2eaf8ad7b0b59b429865e04e.jpg]]
则可用一维数组模拟堆：第k个元素的子节点（左右儿子）为2k，2k+1，其中heap[1]是根节点
`down(k)`将节点k向下沉
`up(k)`将节点k上浮
*手写堆*
1. 插入一个数x:`heap[++size]=x; up(size);`
2. 求集合(堆)中最小值`heap[1];`
3. 删除最小值（堆顶）`heap[1]=heap[size]; --size; down(1);`
4. 删除一个元素`heap[k]=heap[size]; --size; down(k); up(k);`
5. 修改一个元素为x `heap[k]=x; down(k); up(k);`
6. ***注意*** size可能会命名冲突，应写hsize或_size;
 ==down(k)==
```cpp
void down(int u){
int t=u;
if(2*u<=size && heap[2*u]<heap[t] ) t=2*u;
if(2*u+1<=size && heap[2*u+1]<heap[t] ) t=2*u+1;
if(u!=t){
	swap(heap[u],heap[t]);
	down(t);
	}
}
```
==up(k)==
```cpp
void up(int u){
	while(u/2>0 && heap[u/2]>heap[u]){
		swap(heap[u/2],heap[u]);
		u/=2;
	}
}
```
*建堆*
先将n个元素输入到heap[]中，然后从n/2开始down(n/2~1),即从倒数第二层开始down,时间复杂度o(n)
*对第k个插入堆的元素进行操作*
需要建映射关系数组`hp[]`和`ph[]`，`ph[k]`表示第k个插入的元素在堆中下标是多少，hp[i]表示堆中下标是i的元素对应ph[]的下标是多少
需要构造==h_swap()==函数
```cpp
void h_swap(int a,int b){      //交换第a和第b个插入的元素
swap( ph[hp[a]] , ph[hp[b]] );
swap(hp[a],hp[b]);
swap(h[a],h[b]);
}
```
相应地，`down(k)`和`up[k]`中的swap要换成`h_swap`,并在对节点k进行操作时先`k=ph[k]`（将节点序号k换成第k个插入的数的节点序号）
*模板*
```cpp
const int MN=1e5+50;
int heap[MN];
int hp[MN];  //节点i对应的ph下标
int ph[MN]; //ph[k]，第k次插入的数对应的节点序号
int hsize,m;
void h_swap(int a,int b){
    swap(ph[hp[a]],ph[hp[b]]);
    swap(hp[a],hp[b]);
    swap(heap[a],heap[b]);
}
void down(int k){
    int t=k;
    if(2*k<=hsize && heap[2*k]<heap[t]) t=2*k;
    if(2*k+1<=hsize && heap[2*k+1]<heap[t]) t=2*k+1;
    if(t!=k){
        h_swap(t,k);
        down(t);
    }
}
void up(int k){
    while(k/2>0 && heap[k/2]>heap[k]){
        h_swap(k,k/2);
        k/=2;
    }
}
int main(){
    fast;
    int n;
    cin >> n;                        //n条命令
    while(n--){
        string s;
        cin >> s;
        if(s=="I"){                  //插入一个数
            int x;
            cin >> x;
            ++hsize,++m;
            heap[hsize]=x;
            ph[m]=hsize;
            hp[hsize]=m;
            up(hsize);
        }
        else if(s=="PM") cout << heap[1] << '\n';  //查询最小值（堆顶）
        else if(s=="DM"){                        //删除最小值
            h_swap(1,hsize);
            --hsize;
            down(1);
        }
        else if(s=="D"){                        //删除第k个插入的数
            int k; 
            cin >> k;
            k=ph[k];
            h_swap(k,hsize);
            --hsize;
            up(k);
            down(k);
        }
        else if(s=="C"){                        //将第k个插入的数替换为x
            int k,x;
            cin >> k >> x;
            k=ph[k];
            heap[k]=x;
            up(k);
            down(k);
        }
        
    }
    return 0;
}
```