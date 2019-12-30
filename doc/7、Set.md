#### Set

- 类似于数组，但成员唯一

- (Array)可以接受数组作为参数用于初始化

  ```shell
  //数组去重
  [...new Set(array)]
  Array.from(new Set(array))
  ```

  ```shell
  //字符串去重
  [...new Set('aabbccdd')].join('')
  ```

- (Add)Set中add值的时候不会发生类型转换

  - 但是Set内部任务NaN是同一个值；精确运算中NaN并不等于自身

  - 例如：5和‘5’是两个值

    ```shell
    let set = new Set();
    let a = NaN;
    let b = NaN;
    set.add(a);
    set.add(b);
    console.log(set);	//{NaN}
    ```

- (Object)Set中两个对象总是不相等的

  ```shell
  let set = new Set();
  set.add({});
  console.log(set.size);	//1
  set.add({});
  console.log(set.size);	//2
  ```

#### Set实例的属性和方法

- size	成员数目
- add()   添加成员；返回本身
- delete()    删除本身；返回布尔值
- has()    查询是否包含；返回布尔值
- clear()    清除所有成员；没有返回值

#### Set遍历操作

- keys()		返回键名
- values()        返回值(由于Set没有键名；因此它keys和values所得的值完全一样)
- entries()         返回键值对
- forEach()           使用回调函数遍历每个成员

```shell
let set = new Set(["aaa","bbb","ccc"]);
for(let item of set.keys()){
console.log(item)	//aaa bbb ccc
}
for(let item of set.values()){
console.log(item)	//aaa bbb ccc
}
for(let item of set.entries()){
console.log(item)	//["aaa","aaa"] ["bbb","bbb"] ["ccc","ccc"]
}
```

- Set结构的实例默认可遍历，默认的遍历生成器的函数就是values方法，因此可以用for of直接遍历;也可以直接用`forEach`

  ```shell
  let set = new Set(["aaa","bbb","ccc"]);
  for(let item of set){
  	console.log(item)	//aaa bbb ccc
  }
  ```

  ```shell
  let set = new Set(["aaa","bbb","ccc"]);
  set.forEach((value,key)=>{
  console.log(key+':'+value)//aaa:aaa bbb:bbb ccc:ccc
  })
  ```


#### Set实现并集、交集、差集

```shell
let a = new Set([1,2,3,4]);
let b = new Set([3,4,5,6]);

//并集
let union = new Set([...a,...b]);

//交集
let intersect = new Set([...a].fliter(x => b.has(x)));

//差集
let difference = new Set([...a].fliter(x => !b.has(x)))
```

#### 遍历操作中同步改变Set中的值

- set映射出新的结构

```shell
let set = new Set([1,2,3]);
set = new Set([...set].map(item => item*2));
console.log(set)//2,4,6
```

- 利用Array.from

  ```sehll
  let set = new Set([1,2,3]);
  set = new Set(Array.from(set,val => val*2));
  console.log(set);	//2,4,6
  ```

  