## JS

### **数据的检测方式有哪些？**

1. typeof 【除了null的基本类型 + function】
2. instanceof 【引用类型】【从子类到父类直到object】【顺着原型链】
3. Array.isArray() 【数组】
4. Object.prototype.toString() 【任意类型】

**在 JavaScript 中，可以使用以下几种方式来判断数据的类型：**

1. `typeof` ：直接在计算机底层基于数据类型的值（二进制）进行检测 返回的是**字符串**  前缀 特点

   ```js
   typeof 42; // 返回 "number"
   typeof NAN; // 返回 "number"
   typeof "Hello"; // 返回 "string"
   typeof true; // 返回 "boolean"
   typeof undefined; // 返回 "undefined"
   typeof null; // 返回 "object"（注意这是一个历史遗留问题 bug，实际上 null 是一个基本数据类型 空指针对象）// 对象存储在计算机中，都是以000开始的二进制存储，null也是，所以检测出来的结果是对象
   typeof function() {}; // 返回 "function"
   typeof [] // 'object'
   typeof {} // 'object'
   typeof 普通对象/数组对象/正则对象/日期对象  "object"
   ```

2. `instanceof` :  检测当前实例是否属于这个类的

   底层机制：只要当前类出现在实例的原型链上，结果都是`true`  

   由于我们可以肆意的修改原型的指向，所以检测出来的结果是不准的

   不能检测基本数据类型

   ```js
   console.log({} instanceof Object); // 返回 true
   console.log([] instanceof Array); // 返回 true
   console.log([] instanceof RegExp); // 返回 false
   console.log([] instanceof Object); // 返回 true !!!!!
   console.log(1 instanceof Number); // 返回 false
   
   function Fn() {
       this.x = 100;
   }
   Fn.prototype = Object.create(Array.prototype);
   let f = new Fn;
   console.log(f, f instanceof Array);
   ```

   ```js
   function Instance_of(left, right) {
       if (typeof left !== 'object' || left === null) return false;
       let proto = Object.getPrototypeOf(left);
       while (true) {
           if (proto === null) return false;
           if (proto === right.prototype) return true;
           proto = Object.getPrototype0f(proto);
       }
   }
   
   // 实例.__proto__ === 类.prototype
   
   let arr = [];
   console.log(instance_of(arr, Array));
   console.log(instance_of(arr, RegExp));
   console.log(instance_of(arr, Object));
   ```

3. `constructor`

   用起来看似比`instance of`还好用一些（基本类型支持的）

   `constructor`可以随便改，所以也不准

   ```js
   let arr = [];
   console.log(arr.constructor === Array); // true
   console.log(arr.constructor === RegExp); // false
   console.log(arr.constructor === Object); // false
   
   let n = 1;
   console.log(n.constructor === Number); // true 
   
   Number.prototype.constructor = 'AA';
   let n = 1;
   console.log(n.constructor === Number); // false 
   ```

4. `Object.prototype.toString.call()` ：`"[object Xxx]"`

   标准检测数据类型的办法：`Object.prototype.toStrin`g不是转换为字符串，是返回当前实例所属类的信息

   标准检测的办法 "`[object Number/String/Boolean/Null/Undefined/Symbol/Object/Array/RegExp/Date/Function]`"

   ```js
   Object.prototype.toString.call(42); // 返回 "[object Number]"
   Object.prototype.toString.call("Hello"); // 返回 "[object String]"
   Object.prototype.toString.call(true); // 返回 "[object Boolean]"
   Object.prototype.toString.call(undefined); // 返回 "[object Undefined]"
   Object.prototype.toString.call(null); // 返回 "[object Null]"
   Object.prototype.toString.call(function() {}); // 返回 "[object Function]"
   Object.prototype.toString.call({}); // 返回 "[object Object]"
   Object.prototype.toString.call([]); // 返回 "[object Array]"
   Object.prototype.toString.call(/123/g)    //"[object RegExp]"
   Object.prototype.toString.call(new Date()) //"[object Date]"
   Object.prototype.toString.call(document)  //"[object HTMLDocument]"
   Object.prototype.toString.call(window)   //"[object Window]"
   ```

5. `Array.isArray()` ：

   ```js
   Array.isArray([]); // 返回 true
   Array.isArray({}); // 返回 false
   ```

6. `通用的API`

   ```js
   function getType(obj) {
     const type = typeof obj;
     if (typeof obj !== "object") {
       return type;
     } else if (obj === null) {
       return "null";
     } else {
       const typeName = Object.prototype.toString.call(obj);
       return typeName.slice(8, -1).toLowerCase(); // 去除 "[object" 和 "]" 部分
     }
   }
   ```

-----

####   `typeof`与`instanceof`区别


- `typeof`会返回一个**变量的基本类型**，`instanceof`返回的是一个**布尔值**
- `instanceof` 可以准确地判断**复杂引用数据类型**，但是不能正确判**断基础数据类型**
- 而`typeof` 也存在弊端，它虽然可以判断基础数据类型（`null` 除外），但是引用数据类型中，除了`function` 类型以外，其他的也**无法判断**

如果需要通用检测数据类型，可以采用`Object.prototype.toString`，调用该方法，统一返回格式`“[object Xxx]”`的字符串

-----

### JS数组的十三个遍历方法/JS中三类循环对比及性能分析?

1. **for和while循环**

    基于var声明的时候，FOR和WHILE性能差不多「不确定循环次数的情况下使用WHILE」 

    基于let声明的时候，FOR循环性能更好「原理：没有创造全局不释放的变量」

2. **for of循环的底层机制**

   迭代器iterator规范「具备next方法，每次执行返回一个对象，具备 value/done 属性」

   让对象具备可迭代性并且使用for of循环

   ```js
   // 迭代映射对象
   const map = new Map([["name", "John"], ["age", 30]]); 
   for (const [key, value] of map) { 
       console.log(key, value); 
   }
   // name John
   // age 30
   ```

   > iterator 迭代器
   >
   > 部分数据结构实现了迭代器规范
   >
   > 1. Symbol.iterator
   > 2. 数组/部分类数组/Set/Map...「对象没有实现」
   >
   >  for of循环的原理是按照迭代器规范遍历的 拥有迭代器规范的才可以使用 不能遍历对象
   >
   > ```js
   > arr = [10, 20, 30];
   > arr[Symbol.iterator] = function () {
   >     let self = this,
   >         index = 0;
   >     return {
   >         // 必须具备next方法，执行一次next方法，拿到结构中的某一项的值
   >         // done:false value:每一次获取的值
   >         next() {
   >             if (index > self.length - 1) {
   >                 return {
   >                     done: true,
   >                     value: undefined
   >                 };
   >             }
   >             return {
   >                 done: false,
   >                 value: self[index++]
   >             };
   >         }
   >     };
   > };
   > 
   > let itor=arr[Symbol.iterator]();
   > itor.next()
   > 
   > // 类数组对象「默认不具备迭代器规范」
   > let obj = {
   >     0: 200,
   >     1: 300,
   >     2: 400,
   >     length: 3
   > };
   > obj[Symbol.iterator] = Array.prototype[Symbol.iterator];
   > for (let val of obj) {
   >     console.log(val);
   > } 
   > ```

3. **for in 循环的BUG及解决方案**

   迭代所有可枚举属性「私有&公有」，按照原型链一级级查找很耗性能（有些document.body原型链很长）

   问题很多：不能迭代Symbol属性、迭代顺序会以数字属性优先、公有可枚举的{一般是自定义属性}属性也会进行迭代 性能很差

   > for in性能很差:迭代当前对象中所有可枚举的属性的「私有属性大部分是可枚举的，公有属性{出现在所属类的原型上的}也有部分是可枚举的」 查找机制上一定会搞到原型链上去
   >
   > ```js
   > Object.prototype.fn = function fn() { };
   > let obj = {
   >     name: 'zhufeng',
   >     age: 12,
   >     [Symbol('AA')]: 100,
   >     0: 200,
   >     1: 300
   > };
   > ```
   >
   > ```js
   > //会遍历所有的属性 私有属性和公有属性 而且以Number优先
   > for (let key in obj) {
   >     console.log(key); // 0 1 name age fn
   > }
   > 
   > //把公有属性去掉
   > for (let key in obj) {
   >     if (!obj.hasOwnProperty(key)) break; //0 1 name age 
   >     console.log(key);
   > }
   > ```
   >
   > ```js
   > let keys = Object.keys(obj); //获得私有属性
   > //判断浏览器是否兼容
   > if (typeof Symbol !== "undefined") 
   >     keys = keys.concat(Object.getOwnPropertySymbols(obj));
   > //遍历所有的属性
   > keys.forEach(key => {
   >     console.log('属性名：', key);
   >     console.log('属性值：', obj[key]);
   > });
   > ```

4. **forEach**

    `forEach`性能会差一些 函数式编程（更多关注的是结果 无法管控过程 方便 性能上有所消耗） 命令式编程（看重过程 过程和管控很灵活）

    - 参数: forEach(callbackFn[, thisArg])，接受一个回调函数作为参数，回调函数可以接受三个参数：`element`正在处理的当前元素。、`index`当前元素的索引、`array`被遍历的数组。
    - 返回值: `undefined`
    - 是否改变原数组: `可以改变原数组`
    - **使用时机: `用于对数组中的每个元素执行指定的操作`**

    特性:

    - 除非抛出异常，否则没有办法停止或中断，不能使用break 会报错
    - 不可以直接修改元素，但是可以修改元素的属性

    ```js
    const numbers = [1, 2, 3, 4, 5];
    
    // 除非抛出异常，否则没有办法停止或中断。不能使用break。返回值为undefined
    const value = numbers.forEach((item, index, arr) => {
        console.log(item, index, arr)å
        // if(item === 2) break; // 报错 SyntaxError: Illegal break statement
        return item // 无效
    })
    
    // 不可以直接修改元素，但是可以修改元素的属性
    numbers.forEach((item, index, arr) => {
        item = item * 2; // 这里只是修改了形参item的值，并不会修改原数组
        arr[index] = item * 2; // 通过修改属性修改原数组值
    })
    ```

5. **some()** 

    不改变原数组 对数组每一项都运行传入的测试函数，如果至少有1个元素返回 true ，则这个方法返回 true

    ```js
    let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
    let someResult = numbers.some((item, index, array) => item > 2);
    console.log(someResult) // true
    ```

6. **every()**

    不改变原数组 对数组每一项都运行传入的测试函数，如果所有元素都返回 true ，则这个方法返回 true

    ```js
    let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
    let everyResult = numbers.every((item, index, array) => item > 2);
    console.log(everyResult) // false
    ```

7. **filter()**

    对数组每一项都运行传入的函数，函数返回 `true` 的项会组成数组之后返回

    ```js
    let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
    let filterResult = numbers.filter((item, index, array) => item > 2);
    console.log(filterResult); // 3,4,5,4,3
    ```

8. **reduce**

    - 参数: 接收两个参数

    - - 参数一：是一个函数，函数有四个参数，分别是：上一次的值，当前值，当前值的索引，数组
      - 参数二：可选的基底值，可以设置一个数字参与求和运算

    - 返回值: `最终累积结果：reduce方法返回最终的累积结果，可以是任意数据类型，如数字、字符串、对象或数组。`

    - 是否改变原数组: `不改变原数组`

    - **使用时机: 用于各种集合处理和聚合的情况,可以根据需求定义自定义的合并操作**

    **应用1: 求和**

    ```js
    const numbers = [1, 2, 3, 4, 5]; 
    const sum = numbers.reduce((acc, curr) => acc + curr, 0); 
    console.log(sum); // 15
    ```

    **应用2: 数组扁平化**

    ```js
    const nestedArray = [[1, 2], [3, 4], [5, 6]]; 
    const flattenedArray = nestedArray.reduce((acc, curr) => acc.concat(curr), []); 
    console.log(flattenedArray); // [1, 2, 3, 4, 5, 6]
    ```

    **应用3: 数组元素的最大值和最小值**

    ```js
    const numbers = [10, 5, 8, 3, 12]; 
    const maxNumber = numbers.reduce((max, curr) => Math.max(max, curr), Number.MIN_SAFE_INTEGER); 
    console.log(maxNumber); // 12 
    
    const minNumber = numbers.reduce((min, curr) => Math.min(min, curr), Number.MAX_SAFE_INTEGER); 
    console.log(minNumber); // 3
    ```

    **应用4:求平均值**

    ```js
    const scores = [85, 90, 76, 92, 88]; 
    const averageScore = scores.reduce((sum, curr) => sum + curr, 0) / scores.length; 
    console.log(averageScore); // 86.2
    ```

9. **map()**

    对数组每一项都运行传入的函数，返回由每次函数调用的结果构成的数组

    ```js
    let numbers = [1, 2, 3, 4, 5, 4, 3, 2, 1];
    let mapResult = numbers.map((item, index, array) => item * 2);
    console.log(mapResult) // 2,4,6,8,10,8,6,4,2
    ```

10. **find()、findIndex()**

    `find()`用于找出第一个符合条件的数组成员

    参数是一个回调函数，接受三个参数依次为当前的值、当前的位置和原数组

    ```js
    [1, 5, 10, 15].find(function(value, index, arr) {
      return value > 9;
    }) // 10
    ```
    `findIndex`返回第一个符合条件的数组成员的位置，如果所有成员都不符合条件，则返回-1

    ```js
    [1, 5, 10, 15].findIndex(function(value, index, arr) {
      return value > 9;
    }) // 2
    ```

    这两个方法都可以接受第二个参数，用来绑定回调函数的`this`对象。

    ```js
    function f(v){
      return v > this.age;
    }
    let person = {name: 'John', age: 20};
    [10, 12, 26, 15].find(f, person);    // 26
    ```

11.  **findLast findLastIndex**

     > `findLast` 方法用于在数组中从后往前查找满足条件的元素，并返回该元素。和 `find` 方法类似，但是从数组的末尾开始搜索，找到满足条件的最后一个元素

     应用场景：

     - 查找数组中的最后一个满足特定条件的元素。
     - 需要从数组的末尾开始搜索并获取满足条件的元素。

     ```js
     const numbers = [1, 2, 3, 4, 5];
     const lastEvenNumber = numbers.findLast(num => num % 2 === 0); 
     console.log(lastEvenNumber); // 4
     ```

     > `findLastIndex` 方法用于在数组中从后往前查找满足条件的元素，并返回该元素的索引。和 `findIndex` 方法类似，但是从数组的末尾开始搜索，找到满足条件的最后一个元素的索引。

     应用场景：

     - 查找数组中最后一个满足特定条件的元素的索引。
     - 需要从数组的末尾开始搜索并获取满足条件的元素的索引。

     注意： **找到的最后一个满足条件的索引不是从数组末尾开始的，而是从数组第一个元素开始的索引**

     ```js
     const numbers = [1, 2, 3, 4, 5];
     const lastEvenNumberIndex = numbers.findLastIndex(num => num === 4);
     console.log(lastEvenNumberIndex);  // 3
     ```

------

### 类型转换机制

[YK菌](https://blog.csdn.net/weixin_44972008/article/details/111460647)

[面试官](https://vue3js.cn/interview/JavaScript/type_conversion.html#%E4%B8%80%E3%80%81%E6%A6%82%E8%BF%B0)

-----

### this

>  **谈谈你对this的了解及应用场景?**
>
>  * this的五种情况分析 this执行主体，谁把它执行的「和在哪创建&在哪执行都没有必然的关系」
>
>    1. 函数执行，看方法前面是否有“点”，没有“点”，this是window「严格模式下是undefined」，有“点”，“点”前面是谁this就是谁
>
>    2. 给当前元素的某个事件行为绑定方法，当事件行为触发，方法中的this是当前元素本身「排除attachEvent」
>
>    3.  构造函数体中的this是当前类的实例
>
>    4. 箭头函数中没有执行主体，所用到的this都是其所处上下文中的this
>
>    5.  可以基于Function.prototype上的call/apply/bind去改变this指向

[文档](https://segmentfault.com/a/1190000011194676)

[面试官](https://vue3js.cn/interview/JavaScript/this.html#%E4%B8%80%E3%80%81%E5%AE%9A%E4%B9%89)

[YK菌](https://blog.csdn.net/weixin_44972008/article/details/118607326)

**定义：**`this` 关键字是函数运行时自动生成的一个内部对象，只能在函数内部使用，总指向调用它的对象

this指向什么，完全取决于 什么地方以什么方式调用，而不是 创建时。

**绑定规则：**

- 默认绑定：**这种直接使用而不带任何修饰的函数调用** ，就 **默认且只能** 应用 默认绑定。

- 隐式绑定：当函引用有上下文**对象**（函数是否被某个对象拥有或者包含时），隐式绑定规则会把函数调用中的this绑定到这个上下文对象上

  > 对象属性引用链中只有上一层或者说最后一层在调用位置中起作用。

- 显示绑定：apply()、call()、bind()

- new绑定：

  用new 做到函数的`构造调用`后，js帮我们做了什么工作呢:

  1. 创建一个新对象。
  2. 把这个新对象的`__proto__`属性指向 **原函数**的`prototype`属性。(即继承原函数的原型)
  3. **将这个新对象绑定到 此函数的this上** 。
  4. 返回新对象，如果这个函数没有返回其他**对象**。

**优先级：**new绑定优先级 > 显示绑定优先级 > 隐式绑定优先级 > 默认绑定优先级

**箭头函数：**箭头函数不使用this的四种标准规则，而是根据外层（函数或者全局）作用域【词法作用域】来决定this【继承外层函数调用的this绑定】

**总结：**

​		① 由new调用？绑定到新创建的对象。
​		② 由call或者apply（或者bind）调用？绑定到指定的对象。
​		③ 由上下文对象调用？绑定到那个上下文对象。
​		④ 默认：在严格模式下绑定到undefined，否则绑定到全局对象。

-----

### call apply bind

**相同点：**

改变函数运行时的`this`指向（如果如果没有这个参数或参数为`undefined`或`null`，则默认指向全局`window`）

**区别：**

- 传参，`apply`是数组，`call`是参数列表，且`apply`和`call`是一次性传入参数，而`bind`可以分为多次传入
- `apply`、`call` 则是立即执行，`bind`是返回绑定this之后的函数

**主要应用场景：**

`call` 经常做继承.

> func函数基于__proto__找到Function.prototype.call，把call方法执行
>
> 在call方法内部「call执行的时候」  call(context->obj,...params->[10,20])
>
> + 把func中的this改为obj
>
> + 并且把params接收的值当做实参传递给func函数
>
> + 并且让func函数立即执行
>
> + call和apply只有语法上的区别
>
> func.call(obj, 10, 20);
>
> func.apply(obj, [10, 20]);

```js
// 原理：就是利用 “点”定THIS机制，context.xxx=self “obj.xxx=func” => obj.xxx()
// this/self->func  context->obj  params->[10,20]
Function.prototype.call = function call(context, ...params) {
    let self = this,
        key = Symbol('KEY'),
        result;
    context == null ? context = window : null;
    !/^(object|function)$/i.test(typeof context) ? context = Object(context) : null;
    context[key] = self;
    result = context[key](...params);
    delete context[key];
    return result;
};
```

`apply` 经常跟数组有关系. 比如借助于数学对象实现数组最大值最小值
`bind` 不调用函数,但是还想改变this指向. 比如改变定时器内部的`this`指向

```js
Function.prototype.bind = function bind(context, ...params) {
    // this/self->func  context->obj  params->[10,20]
    let self = this;

    return function proxy(...args) {
        // 把func执行并且改变this即可  args->是执行proxy的时候可能传递的值
        self.apply(context, params.concat(args));
    };
};
```

> func函数基于__proto__找到Function.prototype.bind，把bind方法执行
>
> 在bind方法内部
>
> 和call/apply的区别：并没有把func立即执行
>
> 把传递进来的obj/10/20等信息存储起来「闭包」
>
> 执行bind返回一个新的函数 例如:proxy，把proxy绑定给元素的事件，当事件触发执行的是返回的proxy，在proxy内部，再去把func执行，把this和值都改变为之前存储的那些内容
>
> document.body.addEventListener('click', func.bind(obj, 10, 20));
>
> document.body.addEventListener('click', proxy)

```js
Function.prototype.myBind = function (context) {
    // 判断调用对象是否为函数
    if (typeof this !== "function") {
        throw new TypeError("Error");
    }

    // 获取参数
    const args = [...arguments].slice(1),
          fn = this;

    return function Fn() {

        // 根据调用方式，传入不同绑定值
        return fn.apply(this instanceof Fn ? new fn(...arguments) : context, args.concat(...arguments)); 
    }
}
```

> `call:`
>
> ```js
> fun.call(thisArg, arg1, arg2, ...)
> ```
>
> ```js
> function Father(uname, age, sex) {
>   this.uname = uname
>   this.age = age
>   this.sex = sex
> }
> 
> function Son(uname, age, sex){
>   Father.call(this, uname, age, sex)
> }
> 
> let son = new Son('YK菌', 18, '男')
> console.log(son) // Son {uname: "YK菌", age: 18, sex: "男"}
> ```
>
> `apply:`
>
> ```js
> fun.apply(thisArg, [argsArray])
> ```
>
> ```js
> var arrayLike = {
>   0: 'OB',
>   1: 'Koro1',
>   length: 2
> }
> Array.prototype.push.call(arrayLike, '添加元素1', '添加元素2');
> console.log(arrayLike) // {"0":"OB","1":"Koro1","2":"添加元素1","3":"添加元素2","length":4}
> ```
>
> ```js
> const arr = [15, 6, 12, 13, 16];
> const max = Math.max.apply(Math, arr); // 16
> const min = Math.min.apply(Math, arr); // 6
> ```
>
> `bind:`
>
> ```js
> fun.bind(thisArg, arg1, arg2, ...)
> ```
>
> ```js
> let btn = document.querySelector('button')
> btn.onclick = function() {
>   this.disabled = true
>   setTimeout(function(){
>     this.disabled = false;
>   }.bind(this), 3000)
> }
> ```

-----

### new

用new 做到函数的`构造调用`后，js帮我们做了什么工作呢:

1. 创建一个新对象`obj`。
2. 把这个新对象的`__proto__`属性指向 **原函数**的`prototype`属性。(即继承原函数的原型)
3. **将这个新对象绑定到 此函数的this上** 。
4. 返回新对象，如果这个函数没有返回其他**对象**。

```js
function newInstance(Fn, ...args) {
  // 1. 创建新对象
  // 创建空的object实例对象，作为Fn的实例对象
  const obj = {};
  // 修改新对象的原型对象
  // 将Fn的prototype（显式原型）属性赋值给obj的__proto__（隐式原型）属性
  obj.__proto__ = Fn.prototype;
  // 2. 修改函数内部this指向新对象，并执行
  const result = Fn.call(obj, ...args);
  // 3. 返回新对象
  // return obj
  // 与new保持一直，如果构造函数有返回值，返回值是对象a就返回对象a，否则返回实例对象
  return result instanceof Object ? result : obj;
}
```

-----

### 闭包

**定义：**内层函数+引用的外层函数的变量

**什么时候用到return：**外部如果想要使用闭包的变量，此时需要return

**场景：**

- 创建私有变量
- 延长变量的生命周期

```js
for (var i = 0; i < 5; i++) {
    setTimeout(function() {
        console.log(new Date, i);
    }, 1000);
}

console.log(new Date, i);
```

```js
for (var i = 0; i < 5; i++) {
    (function(j) {  // j = i
        setTimeout(function() {
            console.log(new Date, j);
        }, 1000);
    })(i);
}

console.log(new Date, i);
```

```js
for (var i = 0; i < 5; i++) {
    setTimeout(function(j) {
        console.log(new Date, j);
    }, 1000, i);
}

console.log(new Date, i);
```

```js
const tasks = []; // 这里存放异步操作的 Promise
const output = (i) => new Promise((resolve) => {
    setTimeout(() => {
        console.log(new Date, i);
        resolve();
    }, 1000 * i);
});

// 生成全部的异步操作
for (var i = 0; i < 5; i++) {
    tasks.push(output(i));
}

// 异步操作完成之后，输出最后的 i
Promise.all(tasks).then(() => {
    setTimeout(() => {
        console.log(new Date, i);
    }, 1000);
});
```

```js
//可以使用 setInterval 方法来实现每隔 1 秒打印一个数字的功能。下面是一个示例代码：
let count = 1;
const intervalId = setInterval(() => {
  console.log(count);
  count++;

  // 终止条件，例如打印到 10 后停止
  if (count > 10) {
    clearInterval(intervalId);
  }
}, 1000);
```

----

### 作用域

- 全局作用域
- 函数作用域
- 块级作用域

----

### JavaScript原型，原型链 ? 有什么特点？

**原型：每个函数**都有prototype属性，称之为原型，也称为原型对象

- 原型可以放一些属性和方法，共享给实例对象使用
- 原型可以做继承

**原型链：每个对象**都有\_proto\_属性，这个属性指向**创建它的构造函数的原型对象**，原型对象也是对象，也有\_proto\_属性，指向原型对象的原型对象，这样一层一层形成的链式结构称为原型链，最顶层找不到则返回null

-----

### Class

-----

![](https://img-blog.csdnimg.cn/20210520204137302.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk3MjAwOA==,size_16,color_FFFFFF,t_70)

