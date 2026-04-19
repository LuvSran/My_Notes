```js
Function.prototype.call=function(thisArg,...args){
  if(thisArg===null || thisArg===undefined) thisArg=window;
  else thisArg=Object(thisArg); //确定上下文，是空则为window，否则包装为对象

  const tag=Symbol('call'); //创建唯一键
  thisArg[tag]=this;  //函数挂到键上(func.call()时，this指func())
  const res=thisArg[tag](...args);
  delete thisArg[tag]; //删除键
  return res;
}
```