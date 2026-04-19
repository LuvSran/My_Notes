```js
function parseUrl(url){
  const [path,queryStr='']=url.split('?');
  const query={};
  queryStr?.split('&').forEach((val)=>{
    const [k,v='']=val.split('=');
    if(query[k]) query[k]=[].concat(query[k],decodeURIComponent(v));
    else query[k]=decodeURIComponent(v);
  })
  return {path, query};
}
```