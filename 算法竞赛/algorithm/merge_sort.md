*归并排序模板*
```cpp
const int N=1e5+10;
int a[N];
int tmp[N];
void merge_sort(int a[],int l,int r){
	if(l>=r) return;
	int mid=(l+r)/2,i=l,j=mid+1;
	merge_sort(a[],l,mid);
	merge_sort(a[],mid+1,r);
	int k=0;
	while(i<=mid && j<=r){
		if(a[i]<=a[mid]) tmp[k++]=a[i++];
		else tmp[k++]=a[j++];
	  }
	while(i<=mid) tmp[k++]=a[i++];
	while(j<=r)   tmp[k++]=a[j++];
	for(int i=l,j=0;i<=r;i++) a[i]=tmp[j];
return;
}
```
*链表归并排序*
```JavaScript
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
let DC=(x,y)=>(x/y)|0; //获取x/y下取整
function merge_sort(head,n){
    if(n<=1) return head;
    let mid=null;
    let p=head;
    for(let i=1;i<=n/2-1;++i) p=p.next;
    let head2=p.next;
    p.next=null;
    let h1=merge_sort(head,DC(n,2));
    let h2=merge_sort(head2,n-DC(n,2));
    let dummy=new ListNode();
    if(h1.val<h2.val) dummy.next=h1,h1=h1.next;
    else dummy.next=h2,h2=h2.next;
    p=dummy.next;
    while(h1 && h2){
        if(h1.val<h2.val) p.next=h1,h1=h1.next;
        else p.next=h2,h2=h2.next;
        p=p.next;
    }
    if(h1!=null) p.next=h1;
    if(h2!=null) p.next=h2;
    return dummy.next;
}
var sortList = function(head) {
    let n=0;
    let p=head;
    while(p) ++n,p=p.next;
    return merge_sort(head,n);
};
```