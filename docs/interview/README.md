```js
var length = 10;
  function fn() {
  console.log(this.length);
}
var obj = {
  length: 5,
  method: function(fn) {
  fn();
  arguments[0]();
}
}
obj.method(fn, 1)

// 输出 10
// 输出 2
```

```js
function foo() {
  return {
    name: "tom",
    ref: this
  };
}

let user = foo();

console.log(user.ref.name);

// 输出 “”
```