```js
function myNew(fn,...args){
  if(typeof fn !== 'function') return 'fn must be a function';
  const obj = Object.create(fn.prototype);
  const res=fn.apply(obj,args);
  if(res!==null && (typeof res === 'object' || typeof res === 'function')) return res; //typeof null返回'object'
  return obj;
}
```