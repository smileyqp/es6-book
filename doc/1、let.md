# let

#### let块级作用域变量声明

- let声明的是代码块中的变量
- for循环计数器时候用let命令

```shell
var a = [];
for (var i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 10
```

```shell
var a = [];
for (let i = 0; i < 10; i++) {
  a[i] = function () {
    console.log(i);
  };
}
a[6](); // 6
```

>  注意：
>
> 对于var声明的而言，全局中就一个变量i；注意在function中的i始终是指向全局的i；
>
> 对于let声明的而言，当前i支队本轮有效

- `for`循环还特别之处：设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域

- #### 变量提升：

  - var存在变量提升，可以在声明之前使用，值为`undefined`
  - let不存在变量提升，在声明之前使用，会报错

- #### 暂时性死区：

  - 块级作用域内进行let变量声明，那么这个变量会绑定这个区域，该变量不再受外部影响，声明该变量值钱的区域不可以使用这个变量，这个区域成为`暂时性死区（TDZ）`
  - 注意：TDZ的出现导致`typeof`不再是一个百分百安全的操作；在TDZ中使用typeof对于这个变量会出现`ReferenceError`；而没有声明的变量只会是`undefined`不会报错

  ```shell
  // 不报错
  var x = x;
  
  // 报错
  let x = x;
  // ReferenceError: x is not defined
  ```

  > 注意：
  >
  > 对于`var x = x`,var声明的没有暂时性死区
  >
  > 对于`let x = x`,let存在暂时性死区，这是在没有声明变量之前就使用了变量进行赋值

```shell
function bar(x = y, y = 2) {
  return [x, y];
}

bar(); // 报错
```

> 注意：
>
> 这里是比较隐蔽的暂时性死区，在没有声明y之前就去使用了y。
>
> # ？ 
>
> 关于这里有个疑问就是，y算是let声明的？为什么也不可以提前使用

#### `let`不允许重复声明

- 不能在同一代码块中重复let声明；不论let声明先后都会报错

```shell
// 报错
function func() {
  let a = 10;
  var a = 1;
}

// 报错
function func() {
  let a = 10;
  let a = 1;
}
```

- 函数内部也不能重复声明参数

```shell
function func(arg) {
  let arg;
}
func() // 报错

function func(arg) {
  {
    let arg;
  }
}
func() // 不报错
```