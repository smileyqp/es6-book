## Generator函数

- `function`和函数名之间有一个星号
- 函数内部使用`yield`表达式，定义不同的内部状态
- 只有调用`next`方法才会遍历下一个内部状态
- 每次遇到`yield`函数暂停执行，下一次再从该位置继续向下进行（Generator函数不适用yield表达式的话就变成了一个单纯的暂缓执行函数）

- `yield`表达式只能用在Generator函数里面

- yield表达式如果用在另外一个表达式中一定要加括号

  ```shell
  function* demo(){
  	console.loh('hello'+yield)	//SyntaxError
  	console.log('hello'+yield 123)	 // SyntaxError
  
  	console.log('hello'+(yield))	//OK
  	console.log('hello'+(yield 123))
  }
  ```

- `yield`表达式用作函数参数或者放在肤质变大时的右边可以不加括号

  ```shell
  function* demo(){
  	foo(yield 'a',yield 'b');		//OK
  	let input = yield;		//OK
  }
  ```

  