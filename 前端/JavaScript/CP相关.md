```JavaScript
const Add=(mp,x,k)=> mp.set(x,(mp.get(x) || 0)+k); //对Map: mp[x]+=k;
const Ap=(s)=>s.charCodeAt(0); //对字母: 获取s的ASKII码
const floor=(x,y)=>(x/y)|0; //获取x/y下取整
const lg=(x)=>Math.floor(Math.log2(x)); //相当于c++的__lg(x)
```
*ACM模式*
```JavaScript
'use strict';
const fs = require('fs');
const cin = fs.readFileSync(0, 'utf8').trim(); //cin为读取到的
```
*数组*
```JavaScript
const dp = Array.from({ length: n }, () => Array.from({ length: m }, () => new Int32Array(k))); //相当于c++: int dp[n][m][k];
```
*二叉树指针转邻接表建树*
```javascript
const maxn=1000+20;
let e=Array.from({length:maxn},()=>[]); //邻接表，根为点1
let val=new Int32Array(maxn); //点权
let tot=0; //总点数
const build=(root)=>{ //传根指针
    for(let i=0;i<=tot;++i) e[i].length=0;
    tot=0;
    let run=(p,father_id)=>{
        let now=++tot;
        val[now]=p.val;
        e[now].push(father_id);
        e[father_id].push(now);
        if(p.left!=null) run(p.left,now);
        if(p.right!=null) run(p.right,now);
    };
    run(root,0);
};
```
*双端队列*
```javascript
class deque {
    constructor(n = 5*1e5 + 20) { this.dq = new Int32Array(n); this.c = n >> 1; this.hh = this.c; this.tt = this.c - 1; }
    push_back = (x) => this.dq[++this.tt] = x;
    push_front = (x) => this.dq[--this.hh] = x;
    pop_back = () => this.dq[this.tt--];
    pop_front = () => this.dq[this.hh++];
    back = () => this.dq[this.tt];
    front = () => this.dq[this.hh];
    size = () => this.tt - this.hh + 1;
    empty = () => this.hh > this.tt;
    clear = () => { this.hh = this.c; this.tt = this.c - 1; };
}
```