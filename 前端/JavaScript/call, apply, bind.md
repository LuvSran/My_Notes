# call
```js
function.call(thisArg, arg1, arg2, ...)
```
- 立即执行函数
- 第一个参数是 `this` 的指向
- 后续参数逐个传递

*example:*
```js
function greet(greeting, punctuation) {
  console.log(`${greeting}, ${this.name}${punctuation}`);
}
const person = { name: 'Alice' };
// 使用 call，this 指向 person 对象
greet.call(person, 'Hello', '!');
// 输出: Hello, Alice!
```
# apply
```js
function.apply(thisArg, [argsArray])
```
- 立即执行函数
- 第一个参数是 `this` 的指向
- 第二个参数是*数组*形式的参数列表

*example：*
```js
function greet(greeting, punctuation) {
  console.log(`${greeting}, ${this.name}${punctuation}`);
}
const person = { name: 'Alice' };
// 使用 apply，参数以数组形式传递
greet.apply(person, ['Hello', '!']);
// 输出: Hello, Alice!
```
# bind
```js
function.bind(thisArg, arg1, arg2, ...)
```
- 不立即执行，返回一个新函数
- 第一个参数是 `this` 的指向
- 后续参数会预先绑定到新函数

*example：*
```js
function greet(greeting, punctuation) {
  console.log(`${greeting}, ${this.name}${punctuation}`);

}
const person = { name: 'Alice' };
// 使用 bind，返回一个新函数
const greetAlice = greet.bind(person, 'Hello');
greetAlice('!'); // Hello, Alice!
greetAlice('?'); // Hello, Alice?
```