#### Map

- Map的键值不限于字符串(值对值进行存储)。Object的键值对，只接受字符串作为键名

  ```shell
  //Object例子
  const data = {};
  const ele = [1,2,3];
  
  data[ele] = 111;
  console.log(data);		//{1,2,3:111}
  ```

  > 注意：将ele的数组转化成了字符串`1,2,3`，然后用`1,2,3`字符串作为key的

  ```shell
  //Map例子
  const map = new Map();
  const o = {smileyqp:'testit'}
  
  map.set(o,'content')
  map.get(o)	//content
  
  map.has(o)	//true
  map.delete(o)	//true
  map.has(o)	//false
  ```

- 任何具有`literator`借口并且成员是一个双元素数组的结构都可以用来生成新的Map

  - Array:Map可以接受作为一组`键值对的数组`作为函数初始化

  ```shell
  const map = new Map([
  	['name':'smileyqp'],
  	['age':20]
  ]);
  
  map.size		//2
  map.has('name')		//true
  map.has('age')		//true
  map.get('name')		//smileyqp
  map.get('age')		//20
  ```

  - Set：

    ```shell
    const set = new Set([
    	['name':'smileyqp'],
    	['age':20]
    ]);
    //set是两个值分别为['name':'smileyqp']  ['age':20]的
    const map = new Map(set);
    
    map.size		//2
    map.has('name')		//true
    map.has('age')		//true
    map.get('name')		//smileyqp
    map.get('age')		//20
    ```

  - Map:

    ```shell
    const map1 = new Map([
    	['name':'smileyqp'],
    	['age':20]
    ]);
    const map = new Map(map1);
    
    map.size		//2
    map.has('name')		//true
    map.has('age')		//true
    map.get('name')		//smileyqp
    map.get('age')		//20
    ```

- Map中多次赋值，后面会覆盖前面

  ```shell
  const map = new Map();
  map.set(111,'ok')
  map.set(111,'hello world')
  
  console.log(map.get(111))//hello world
  ```

- Map中，键值跟内存地址绑定，并非跟值绑定；只有同一引用才视作同一个键

  ```shell
  const map = new Map();
  map.set([5],'hello');
  map.get([5]);	//undefined	
  ```

  > 注意：set和get方法表面是同一个键，但是其实际上是不同的数组实例，内存地址不一样，因此Map将其视为两个不一样的，所以无法取到值

- Map中简单类型的键值（数字、字符串、布尔值）只要严格相等，那么认为是同一个

  ```shell
  let map = new Map();
  
  map.set(-0,233)
  map.get(0)	//233
  
  map.set(true,111)
  map.set('true',222)
  map.get(true);		//111
  
  map.set(undefined,123);
  map.set(null,111);
  map.get(undefined);		//123
  
  map.set(NaN,1);
  map.get(NaN);	//1
  ```

#### Map的实例属性和操作方法

- size
- set(key,value)
- get(key)
- has(key)
- delete(key)
- clear()    清除所有成员没有返回值



#### 遍历方法

- 三个遍历生成函数和一个遍历方法
  - keys
  - values
  - entries遍历一个一个[key，value]形式
  - forEach

```shell
for(let item of map.keys()){}

for(let item of map.values()){}

for(let item of map.entries()){}
for(let [key,value] of map.entries()){}
for(let [key,value] of map){}	//等同于使用map.entries()

```

###### Map默认遍历接口是entries；Set默认遍历接口是values

#### Map转化成数组以及遍历过滤

- 数组转化

```shell
const map = new Map([
	[1,'111'],
	[2,'222'],
	[3,'333']
]);
[...map.keys()]
[...map.values()]
[...map.entries()]	//[[1,'111'],[2,'222'],[3,'333']]
[...map]	//[[1,'111'],[2,'222'],[3,'333']]
```

- 借助数组遍历过滤

```shell
const map = new Map([
	[1,'111'],
	[2,'222'],
	[3,'333']
]);
const map1 = new Map(
[...map].filter((k,v)=>{k<=2});		//产生{1=>'111',2=>'222'}结构的Map
)
const map2 = new Map(
[...map].map((k,v)=>[k*2,v+'smile'])	//产生{2=>'111smile',4=>'222smile',6=>'333smile'}
)
```

- forEach方法

  ```shell
  map.forEach((value,key,map)=>{},reporter)	//reporter参数是用于绑定this
  ```

  

#### Map数据结构转化

- Map转化成Array（拓展操作符...）
- Array转化成Map(传入数组进入Map构造函数)
- Map转化成对象

```shell
funcrion strMaptoObj(strMap){
	let obj = Object.create(null);
	for(let [key,val] of strMap){
		obk[key] = val;
	}
}
```

> 注意：如果Map中的键为String那么会直接转化；如果部位String那么会转化成String再进行添加

- Objeact转化成Map

```shell
 function objTostrMap(obj){
 	let strMap = new Map();
 	for(let k of Object.keys(obj){
 		strMap.set(k,obj[k])
 	}
 }
```

- Map转化成JSON

  - 转化成对象JSON：Map中的键都为String

  ```shell
  //转化成Object再JSON.stringify()将Object转化成JSON
  fuunction mapToJson(strMap){
  	return JSON.stringify(strMaptoObj(strMap))
  }
  ```

  - 转化成数组JSON:Map的键包含非String的值

  ```shell
  function mapToArrJson(strMap){
  	return JSON.stringify([...strMap])
  }
  ```

  

- JSON转化成Map

  - 常见键名为字符串的JSON转化成Map（转化成Object；转化成Map）

  ```shell
  function JsonToMap(jsonstr){
  	return objTostrMap(JSON.parse(jsonstr))
  }
  ```

  - 整个JSON就是一个数组，并且数组本身有事一个有两个成员的数组；可以使用Map转化成为JSON的逆操作

  ```shell
  function josnarrayToMap(jsonstr){
  	return new Map(JSON.parse(jsonstr))
  }
  ```

  











































































