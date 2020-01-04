## Reflect

#### Reflect设计目的

- 将一些明显属于语言内部的方法只不熟到Reflect对象上

- 修改某些Object方法的返回结果，让其变得更加合理

  ```shell
  Object.defineProperty(obj,name,desc)在无法定义属性的时候会抛出错误
  Reflect.defineProperty(obj,name,desc)则会返回false
  ```

- 让Object的操作都变成函数行为

  ```shell
  var obj = {'aaa','fff‘}
  
  //Old
  'aaa' in obj
  
  //new
  obj.has('aaa
  ```

- Reflect和Proxy一一对应，这样Proxy对象可以方便的调用Reflect方法完成默认行为

#### 静态方法

- Reflect对象一共有13个静态方法
  	-  apply
  	-  construct
  	-  get
  	-  set
  	-  defineProperty
  	-  deleteProperty
  	-  has
  	-  ownKeys
  	-  isExtensible
  	-  preventExtensions
  	-  getOwnPropertyDescriptor
  	-  getPropertyOf
  	-  setPropertyOf