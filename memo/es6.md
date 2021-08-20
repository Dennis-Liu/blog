## 多行字符串
```javascript
console.log(`多行  
字符串  
测试`);
```

## 模板字符串
```javascript
var name = '小明';
var age = 20;
var message = `你好, ${name}, 你今年${age}岁了!`;
```

## 声明类型
|  | var | let | const |
|:--:| :---: | :---: | :--- |
|作用域|全局|局部|局部|
|变量声明|`var a` //正确| `let a` //正确| `const a` //报错，必须有初始值，如`const a = 3`|
|是否可修改|可修改|可修改|若为基本数据类型则不可修改，若为引用类型，可修改值，不可修改应用地址|
|是否可重复声明|可以|报错|报错|
|变量提升|是|否|否|

## 变量提升
```javascript
console.log(a);  //undefined  
var a = 123; 
```
因为变量a的声明被提到了作用域顶端。上面代码编译后应该是下面这个样子
```javascript
var a;
console.log(a)
a = 123
//所以输出内容为 undeifend
```

## 函数提升
具名函数的声明有两种方式：1. 函数声明式 2. 函数字面量式
```javascript
//函数声明式
function bar () {}
//变量形式声明； 
var foo = function () {}
```
函数 变量形式声明 和普通变量一样 提升的 只是一个没有值的变量。

函数声明式的提升现象和变量提升略有不同，函数声明式会提升到作用域最前边，并且将声明内容一起提升到最上边。
```javascript
bar()

var bar = function() {
  console.log(1);
}
// 报错：TypeError: bar is not a function
```
```javascript
bar()
function bar() {
  console.log(1);
}
//输出结果1
```

## filter
```js
<script>
    var arr=[50,86,12,10,0,38,12];
    var arr1=[];
    arr1=  arr.filter(function(value){
        return value>10;
    })
    console.log(arr1);

</script>
```  
>输出[50,86,12,38,12]  
