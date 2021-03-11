## ES6
### let
#### 不属于顶层对象window
#### 不允许重复声明

### ES5中数组的遍历方式
#### for循环
#### forEach(): 没有返回值,只是针对每个元素调用func
```js
let arr = ['tom','jack','jane']

arr.forEach(function(item, index, arr) {
  console.log(item);  // tom,jack,jane
  console.log(index); // 0,1,2
  console.log(arr); // ['tom','jack','jane']
})
```
forEach()内不能使用break、continue。

#### map(): 返回新的Array，每个元素为调用func的结果
```js
let arr = ['tom','jack','jane']

let a = arr.map(function(item, index) {
  item+= '-hi'
  return item
})

console.log(a); //["tom-hi", "jack-hi", "jane-hi"]
```

####  filter(): 返回符合func条件的元素数组(筛选)
```js
let arr = ['tom','jack','jane']

let a = arr.filter(function(item) {
  return item==='jack'
})
console.log(a); //["jack"]
```

#### some(): 返回Boolean，判断是否有元素符合func条件
```js
let arr = ['tom','jack','jane']

let result = arr.some(function(item) {
  return item == 'tom'
})

console.log(result);
```

#### every(): 返回Boolean，判断是否每个元素都符合func条件
```js
let arr = ['tom','jack','jane']

let result = arr.every(function(item) {
  return typeof item === 'string'
})

console.log(result);
```

#### reduce(): 接受一个函数作为累加器
```js
// 求和应用
let arr = [1,2,3]

let result = arr.reduce(function(prevValue,currentValue, currentIndex, array){
 return (prevValue + currentValue); 
},0)

console.log(result); //6

//数组去重
let arr = [1,2,3,2,4,1]

let result = arr.reduce(function(prevValue,currentValue){
 prevValue.indexOf(currentValue) == -1 && prevValue.push(currentValue)
 return prevValue 
},[])

console.log(result);
```

