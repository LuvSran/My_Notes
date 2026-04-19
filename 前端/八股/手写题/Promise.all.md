```js
function promiseAll(arr) {
  const _return = new Promise((resolve, reject) => {
    if (!arr.length) resolve([]);
    let res = [],
      count = 0;
    arr.forEach((val, idx) => {
      Promise.resolve(val).then((x) => {
        res[idx] = x;
        if (++count == arr.length) resolve(res);
      }, reject);
    });
  });
  return _return;
}
```