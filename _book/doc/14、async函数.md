## async函数

- Generator函数的语法糖
- async函数必须等到内部所有的await执行完才会执行then方法制定的回调函数

- async对于generator的改进方面：

  - 内置执行器（async不用调用next方法执行）
  - 更好的语义（async表示内部有异步操作；await表示紧随后面的表达式需要等待结果）
  - 适用更加广泛
    - yield后面只能是thunk函数或者Promise对象
    - async函数的await后面可以使Promise对象和原始类型的值（数值、字符串、布尔值，但是这时候会自动转换成曾立即resolved的Promise对象）

  - 返回值
    - yield返回值是Iterator对象
    - async函数返回值是Promise对象

- async函数可以看做欧式多个异步函数包装成的一个Promise对象，await命令就是内部then命令的语法糖

#### 基本用法

`async`函数返回一个Promise对象，可以适用`then`方法添加回调函数，当执行的时候一旦遇到await就会先返回等到异步操作完成之后快接着执行函数后面的语句

```shell
function timeout(ms){
	return new Promise((resolve)=>{
		setTimeout(resolve,ms
	})
}

async function asyncPrint(value,ms){
	await timeout(ms);
	console.log(value)
}
asyncPrint('hello world',500)
```

> 注意：上面代码在50ms后输出hello world

由于async的返回是Promise对象，可以作为await的命令函数，那么上面代码也可以写成

```shell
async function timeout(ms){
	await new Promise((resolve)=>{
		setTimeout(resolve,ms)
	})
}
async function asyncPrint(value,ms){
	await timeout(ms);
	console.log(value)
}
asyncPrint('hello world',500)
```

#### async函数多种使用形式

- 函数声明

  ```shell
  async function foo(){}
  ```

- 函数表达式

  ```shell
  const foo = async function(){}
  ```

- 对象的方法

  ```shell
  let obj = {async foo(){}};
  obj.foo().then(...)
  ```

- Class的方法

  ```shell
  class Storage{
  	constructor(){
  		this.cachePromise = caches.open('avatars');
  	}
  	async getAvatar(name){
  		const cache = await this.cachePromise;
  		return cache.match(`/avatars/${name}.jpg`)
  	}
  }
  const storage = new Storage();
  storage.getAvartar('smileyqp').then(...)
  ```

- 箭头函数

  ```shell
  const foo = async() => {}
  ```

  ```
  async function f(){
  	return 'hello world'
  }
  ```

### await

- await命令后面是一个Promise对象，返回该对象的结果，如果不是Promise对象，就直接返回对应的值

```shell
async function f(){
	return await 123;	//等同于 return 123;
}
f().then((v)=>{	console.log(v)})
//123
```

- await实现休眠效果

  ```shell
  function sleep(interval){
  	return new Promise(resolve => {
  		setTimeout(resolve,interval)
  	})
  }
  async function testInterval(){
  	for(let i =1;i <= 5;i++){
  		console.log(i);
  		await sleep(1000)
  	}
  }
  testInterval();
  ```

  

#### 错误处理

如果await后面的异步出错，那么等同于async返回的Promise对象被reject

- 利用`try   catch`
- 利用`catch`方法的回调

#### 多个异步操作，如果不存在继发关系，最好让他们同时触发

```
let foo = await getFoo();
let bar = await getBar();
```

> 注意：上面代码中getFoo和getBar是两个独立的操作，被写成相继关系，这样比较耗时，因为只有当上面的完成之后才会执行下面的getBar；我们完全可以让他们同时出发

```shell
//写法一
let [foo,bar] = await Promise.all([getFoo(),getBar()]);

//写法二：
let fooPromise = getFoo();
let barPromise = getBar();
let foo = await fooPromise;
let bat = await barPromise;
```

#### await只能用在async方法中

```shell
async function func(db){
	let docs = [{},{},{}];
	
	docs.forEach(function(doc){
		await db.post(doc);			//await用在普通方法中报错
	})
}
```

> 注意：将上面的里面的function前面加上async改成异步也会报错；因为那样的话几个db.post是并发执行，即同步执行并不是继发执行的；正确写法是改成for循环

```shell
async function func(db){
	let docs = [{},{},{}];
	
	dor(let doc of docs){
		await db.post(doc)
	}
}
```

> 如果确实希望几个请求并发执行，可以使用`Promise.all`方法

```shell
//写法一
async function func(db){
	let docs = [{},{},{}];
	let promises = docs.map((doc)=>{db.post(doc)})
	let results = await Promise.all(promises)
	console.log(results)
}
//写法二
async function func(db){
	let docs = [{},{},{}];
	let promises = docs.map((doc)=>{db.post(doc)})
	
	let result = [];
	for(let promise of promises){
		results.push(await promise)
	}
	console.log(results)
}
```

#### async函数的实现原理就是Generator函数和自动执行器包装在一个函数里

```shell
async function fn(){

}

//等同于;spawn是一个资方执行器
function fn(args){
	return spawn(function* (){
		...
	})
}
```

