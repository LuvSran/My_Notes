**递归**
```js
function flatten(arr){
  let res=[];
  for(let i=0;i<arr.length;++i){
    let val=arr[i];
    if(Array.isArray(val)) res=res.concat(flatten(val));
    else res.push(val);
  }
  return res;
}
```
**栈**
```js
function flatten(arr){
  const res=[];
  const stack=[...arr];
  while(stack.length){
    let cur=stack.pop();
    if(Array.isArray(cur)) stack.push(...cur);
    else res.push(cur);
  }
  res.reverse();
  return res;
}
```
**进阶**
v1
```js
function flatten(arr){
  const res=arr.slice();
  while(res.some((val)=>Array.isArray(val))){
    res=[].concat(...res);
  }
  return res;
}
```
v2
```js
function flatten(arr){
  const res=arr.reduce((acc,cur)=>{
    if(Array.isArray(cur)) return acc.concat(flatten(cur));
    return acc.concat(cur);
  },[]);
  return res;
}
```
