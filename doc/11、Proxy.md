#### Proxy

- 可以理解成在目标对象之前设置一层拦截

```shell
var proxy = new Proxy(target,handler);//target是要拦截的对象；handler表示要拦截的行为
```



- 如果handler没有设置任何拦截，就等同于直接通向原对象（即：访问proxy等同于访问要拦截的对象）

```shell
var target= {}；
var handler= {}；
var proxy= new Proxy(target,handler);

proxy.a = '111';
console.log(target.a);		//'111'
```

- 可以将Proxy对象设置到object.proxy属性，从而可以在object上直接调用

```shell
var obj = {proxy:new Proxy(target,handler)}
```

- Proxy实例也可以作为其他对象的原型对象

```shell
var proxy = new Proxy(target,{
	get:function(target,propKey){
		return 35;
	}
}
let obj = Object.create(proxy);
console.log(obj.time)		//35
```

> 注意：proxy是obj的原型；obj上并没有time，那么会到原型上去读取这个值，呆滞被拦截，get返回35

- Proxy拦截的操作一共十三种

  - set

    

  - get(target，propKey，receiver)：拦截属性的读取操作

    - target:目标对象，propKey属性名，receiver指proxy实例

  - has

  - deleteProperty

  - ownKeys

  - getOwnPropertyDescription

  - defineProperty

  - preventExtensioons

  - getPropertyOf

  - isExtensible

  - setPropertyOf

  - apply

  - construct

































