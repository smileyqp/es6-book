#### const

- const声明一个只读常量，一旦声明值不可以改动；并且一旦声明就必须立即初始化

- `const`与`let`一样只在声明的块级作用域中有效

- `const`存在暂时性死区，只能在声明后使用

- `const`与`let`声明的变量一样不可以重复声明

- 注意`const`实际保证的并不是值不变，而是其指向的内存地址不变，至于其内部数据结构是否改变就不能控制

  ```shell
  const foo = {};
  
  // 为 foo 添加一个属性，可以成功
  foo.prop = 123;
  foo.prop // 123
  
  // 将 foo 指向另一个对象，就会报错
  foo = {}; // TypeError: "foo" is read-only
  ```

  >  提示：常量储存的是一个地址，地址指向一个对象，知识这个地址是不可变的，但是这个对象本身是可以改变的

  ```shell
  const a = [];
  a.push('Hello'); // 可执行
  a.length = 0;    // 可执行
  a = ['Dave'];    // 报错
  ```

  > 提示：常量a是一个数组，这个数组本身是可写的，但是如果将另外一个数组复制给a的话就会报错

- 如果想冻结`const`声明的常量可以用`freeze`方法

  ```shell
  const foo = Object.freeze({});
  
  // 常规模式时，下面一行不起作用；
  // 严格模式时，该行会报错
  foo.prop = 123;
  ```

  - 冻结对象本身之外，冻结对象属性

    ```shell
    var constantize = (obj) => {
      Object.freeze(obj);
      Object.keys(obj).forEach( (key, i) => {
        if ( typeof obj[key] === 'object' ) {
          constantize( obj[key] );
        }
      });
    };
    ```

    