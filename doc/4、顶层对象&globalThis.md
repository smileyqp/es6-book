# 顶层对象&globalThis

#### 顶层对象

- ES5:
  - 浏览器中：window
  - node中：global
  - ES5中：顶层对象与全局变量等价

- ES6：

  - var、function声明的全局变量依旧是顶层对象属性

  - let、const声明的全局变量不属于顶层对象的属性（全局对象和顶层对象脱钩）

    ```shell
    var a = 1;
    // 如果在 Node 的 REPL 环境，可以写成 global.a
    // 或者采用通用方法，写成 this.a
    window.a // 1
    
    let b = 1;
    window.b // undefined
    ```

  #### globalThis 对象 

  - 各个环境下顶层对象并不统一，所以引入`globalThis`作为任何情况下的顶层对象

    - 浏览器window(node和web worker中没有window)

    - 浏览器和web worker中，self只想顶层对象，node没有self

    - node中的顶层对象是global，但是其他的都不支持

    - 在所有情况下，都取到顶层对象，如下:

      ```shell
      // 方法一
      (typeof window !== 'undefined'
         ? window
         : (typeof process === 'object' &&
            typeof require === 'function' &&
            typeof global === 'object')
           ? global
           : this);
      
      // 方法二
      var getGlobal = function () {
        if (typeof self !== 'undefined') { return self; }
        if (typeof window !== 'undefined') { return window; }
        if (typeof global !== 'undefined') { return global; }
        throw new Error('unable to locate global object');
      };
      ```

      

