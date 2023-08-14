# JS

### 1. 变量的声明与类型

#### 1.1 var let const 区别

> ##### [YK菌](https://blog.csdn.net/weixin_44972008/article/details/112340914)
>
> **变量提升：**将**变量**和**函数的声明**提升到当前作用域的顶部，但是**变量的赋值操作**并不会被提升。这意味着可以在声明之前使用变量，但它的值会是 `undefined`。
>
> **暂时性死区（TDZ）：**在块级作用域中，使用 `let` 或 `const` 声明的变量在声明之前不能被访问或赋值的特性
>
> > ```js
> > for (var i = 0; i < 5; ++i) {
> >  	setTimeout(() => console.log(i), 0)
> > }
> > // 你可能以为会输出0、1、2、3、4
> > // 实际上会输出5、5、5、5、5
> > ```
> >
> > 在循环中使用 `setTimeout` 创建的多个定时器会**在循环结束**后才开始执行回调函数。
> >
> > 由于 JavaScript 是**单线程**的，这些定时器的回调函数会在循环结束后才被触发。
> >
> > 为了解决这个问题，可以使用**闭包或者立即执行函数表达式（IIFE）来创建一个新的作用域，以保存每次循环中的变量 `i` 的值。**下面是使用闭包的示例：
> >
> > ```js
> > for (var i = 0; i < 5; ++i) {
> >   	(function (num) {
> >         	setTimeout(() => console.log(num), 0);
> >   	})(i);
> > }
> > 
> > ```
> >
> > > **JavaScript 的事件循环（Event Loop）**是一种**处理异步操作**的机制，它负责协调和处理事件、回调函数和其他异步任务的执行顺序。事件循环是 JavaScript 单线程模型中的核心机制。
> > >
> > > 事件循环的运行过程如下：
> > >
> > > 1. 执行同步任务：JavaScript 从上到下依次执行同步任务，直到遇到异步任务或者任务队列中有待处理的任务。
> > > 2. 处理异步任务：当遇到异步任务时，会将该任务挂起，并将其回调函数添加到任务队列中，然后继续执行后续的同步任务。
> > > 3. 等待执行：当所有同步任务执行完毕，JavaScript 引擎会检查任务队列中是否有待处理的任务。
> > > 4. 事件循环：如果任务队列中有待处理的任务，JavaScript 引擎会将队列中的第一个任务取出，并执行其回调函数。执行完成后，进入下一个循环，继续检查任务队列。
> > >
> > > 这个过程会不断重复，因此被称为事件循环。
> > >
> > > 任务队列（Task Queue）分为宏任务队列（macro task queue）和微任务队列（micro task queue）两种。宏任务包括整体代码块、setTimeout、setInterval 等，而微任务包括 Promise、MutationObserver 和 process.nextTick（Node.js 环境下）等。
> > >
> > > 事件循环的顺序是先处理微任务队列中的任务，然后再处理宏任务队列中的任务。每次处理一个队列中的所有任务，直到队列为空，然后再执行下一个队列中的任务。
> > >
> > > 这种事件循环机制确保了 JavaScript 中的异步任务能够以非阻塞的方式执行，并且可以按照特定的顺序进行处理。通过合理地使用异步任务和任务队列，可以避免阻塞主线程，提高应用程序的性能和响应能力。
> >
> > ```js
> > for (let i = 0; i < 5; ++i) {
> >  	setTimeout(() => console.log(i), 0)
> > }
> > // 会输出0、1、2、3、4
> > 
> > ```
> >

> ![let-var-const](E:\Front\八股文\My八股文\八股图\let-var-const.png)
>
> 1. var是ES5语法，let、const是ES6的语法
>
> 2. var有变量提升
>
> 3. var、let是变量，可修改；const是常量，不可修改
>
> 4. let、const 块级作用域；var函数作用域
>
>    > **条件式声明**是指在特定条件下声明和初始化变量，例如在条件分支中根据不同情况创建变量。然而，`let` 和 `const` 声明的变量具有块级作用域，它们的作用域仅限于声明它们的代码块内部。如果在条件语句内部使用 `let` 或 `const` 声明变量，那么这些变量的作用域将限定在该条件语句的代码块中，超出该代码块后将无法访问。

-----

#### 1.2 数据类型分类

> [Yk菌](https://blog.csdn.net/weixin_44972008/article/details/111460647)
>
> **基本数据类型**
>
> 1. `undefined`：表示未定义或未赋值的值。
> 2. `null`：表示空值或没有值的对象。
> 3. `boolean`：表示逻辑上的 `true` 或 `false`。
> 4. `number`：表示数值，包括整数和浮点数。
> 5. `bigint`：表示任意精度的整数。
> 6. `string`：表示字符串，由字符组成的文本。
> 7. `symbol`：表示唯一的、不可变的值，通常用作对象属性的键。
>
> **复杂数据类型**
>
> 1. `Object`
> 2. `Array`
> 3. `Function`

#### **1.3 数据类型分类区别？**

> 值类型 存在**栈**内存中，变量拿到的就是它的值
>引用类型 存在**堆**内存中，变量拿到的只是它的一个引用，是它的地址

-----

### **数据的检测方式有哪些？**

1. typeof 【除了null的基本类型 + function】
2. instanceof 【引用类型】【从子类到父类直到object】【顺着原型链】
3. Array.isArray() 【数组】
4. Object.prototype.toString() 【任意类型】

在 JavaScript 中，可以使用以下几种方式来判断数据的类型：

1. `typeof` ：

   ```js
   typeof 42; // 返回 "number"
   typeof "Hello"; // 返回 "string"
   typeof true; // 返回 "boolean"
   typeof undefined; // 返回 "undefined"
   typeof null; // 返回 "object"（注意这是一个历史遗留问题，实际上 null 是一个基本数据类型 空指针对象）
   typeof function() {}; // 返回 "function"
   typeof [] // 'object'
   typeof {} // 'object'
   ```

2. `instanceof` :

   ```js
   const obj = {};
   const arr = [];
   console.log(obj instanceof Object); // 返回 true
   console.log(arr instanceof Array); // 返回 true
   ```

3. `Array.isArray()` ：

   ```js
   Array.isArray([]); // 返回 true
   Array.isArray({}); // 返回 false
   ```

4. `Object.prototype.toString.call()` ：`[object Xxx]`

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

5. 通用的API

	```js
	function getType(obj) {
	  const type = typeof obj;
	  if (type !== "object") {
	    return type;
	  } else if (obj === null) {
	    return "null";
	  } else {
	    const typeName = Object.prototype.toString.call(obj);
	    return typeName.slice(8, -1); // 去除 "[object" 和 "]" 部分
	  }
	}
	```

-----

####   `typeof`与`instanceof`区别


- `typeof`会返回一个**变量的基本类型**，`instanceof`返回的是一个**布尔值**
- `instanceof` 可以准确地判断**复杂引用数据类型**，但是不能正确判**断基础数据类型**
- 而`typeof` 也存在弊端，它虽然可以判断基础数据类型（`null` 除外），但是引用数据类型中，除了`function` 类型以外，其他的也**无法判断**

如果需要通用检测数据类型，可以采用`Object.prototype.toString`，调用该方法，统一返回格式`“[object Xxx]”`的字符串



----

####  1.6 `null` 和`undefined` 区别

>
> 1. `Undefined` 和 `Null` 都是基本数据类型，这两个基本数据类型分别都只有一个值，就是 `undefined` 和 `null`
>
> 2. `undefined` 表示一个变量被声明了但没有赋予任何值。
>
> 3. `null` 表示一个变量被明确地赋予了空值,空对象。它是一个表示空对象指针的特殊关键字
>
> 4. 一般变量声明了但还没有定义的时候会返回 `undefined`，`null` 主要用于赋值给一些可能会返回对象的变量，作为初始化
>
> 5. `typeof null = object`  `typeof undefined = undefined`
>
> 6. `undefined` 是一个原始值，而 `null` 是一个表示空对象指针的特殊值。这意味着 `undefined` 不是对象，没有任何属性或方法。而 `null` 被认为是一个空对象，因此可以在其上面使用属性和方法。例如：
>
>    ```js
>    console.log(undefined.toString()); // 报错，undefined 没有 toString() 方法
>    console.log(null.toString()); // 输出 "null"
>    ```

#### 1.7 如何获取安全的 `undefined` 值？

>
> 在 JavaScript 中，`undefined` 是一个全局变量，它表示一个未定义的值。要获取安全的 `undefined` 值，可以使用以下方法：
>
> 1. **使用 void 运算符：**`void` 运算符接受一个表达式作为操作数，并返回 `undefined`。它的语法是 `void(expression)`。例如：
>
>    ```js
>    let value = void 0;
>    console.log(value); // 输出 undefined
>    ```
>
> 2. **使用函数参数未传递的方式：**如果一个函数参数没有传递对应的实参，它的值将被默认为 `undefined`。你可以定义一个函数，不传递任何参数来获取 `undefined` 值。例如：
>
>    ```js
>    function getUndefined() {
>      return arguments[0];
>    }
>    
>    let value = getUndefined();
>    console.log(value); // 输出 undefined
>    ```
>
> 3. **使用解构赋值的方式：**通过解构赋值可以从一个数组或对象中获取对应的值，如果未定义的属性或元素，将返回 `undefined`。例如：
>
>    ```js
>    let arr = [];
>    let [value] = arr;
>    console.log(value); // 输出 undefined
>                                           
>    let obj = {};
>    let { prop } = obj;
>    console.log(prop); // 输出 undefined
>    ```
>

#### 1.8`JavaScript` 中的类型转换机制

>
>[Yk菌](https://blog.csdn.net/weixin_44972008/article/details/111460647)
>
>[面试官](https://vue3js.cn/interview/JavaScript/type_conversion.html)
>
>[Yk菌](https://blog.csdn.net/weixin_44972008/article/details/111460647)

#### 1.9 `intanceof` 操作符的实现原理及实现

>
> ```js
> function myInstanceof(left, right) {
>     // 这里先用typeof来判断基础数据类型，如果是，直接返回false
>     if(typeof left !== 'object' || left === null) return false;
>     // getProtypeOf是Object对象自带的API，能够拿到参数的原型对象
>     let proto = Object.getPrototypeOf(left);
>     while(true) {                  
>         if(proto === null) return false;
>         if(proto === right.prototype) return true;//找到相同原型对象，返回true
>         proto = Object.getPrototype0f(proto);
>     }
> }
> ```

####  1.10 实现一个全局通用的数据类型判断方法

>
> ```js
> function getType(obj){
>   	let type  = typeof obj;
>   	if (type !== "object") {    // 先进行typeof判断，如果是基础数据类型，直接返回
>     	return type;
>   	}
>   // 对于typeof返回结果是object的，再进行如下的判断，正则返回结果
>   return Object.prototype.toString.call(obj).replace(/^\[object (\S+)\]$/, '$1'); 
> }
> ```


>`NaN` 是 JavaScript 中的一个特殊值，表示非数字（Not-a-Number）。它是一个全局对象属性，其**类型是 `number`。**
> 
>当进行不合理或无法表示为数字的数值操作时，结果会被标记为 `NaN`。例如：
> 
>```js
> javascriptCopy codeconsole.log(0 / 0); // 输出 NaN
> console.log(Math.sqrt(-1)); // 输出 NaN
> console.log(parseInt("abc")); // 输出 NaN
> ```
> 
>`NaN` 的特点是它与任何其他值都不相等，包括它自己。这意味着使用 `NaN` 进行任何比较都会得到 `false` 的结果。例如：
> 
>```js
> javascriptCopy codeconsole.log(NaN === NaN); // 输出 false
> console.log(NaN == NaN); // 输出 false
> console.log(NaN > 0); // 输出 false
> console.log(NaN < 0); // 输出 false
> ```
> 
>可以使用全局函数 `isNaN()` 来检测一个值是否为 `NaN`。该函数接受一个参数，并返回一个布尔值，指示参数是否为 `NaN` 或无法转换为数字。例如：
> 
>```js
> javascriptCopy codeconsole.log(isNaN(NaN)); // 输出 true
> console.log(isNaN(42)); // 输出 false
> console.log(isNaN("abc")); // 输出 true
> console.log(isNaN("42")); // 输出 false
> ```
> 
>需要注意的是，`isNaN()` 函数会**尝试将参数转换为数字**，因此对于非字符串类型的参数，会在进行隐式类型转换后再进行判断。**如果需要严格检查一个值是否为** `NaN`，可以使用 `Number.isNaN()` 方法。该方法只会返回 `true` 当且仅当参数的值为 `NaN` 时。例如：
> 
>```js
> javascriptCopy codeconsole.log(Number.isNaN(NaN)); // 输出 true
> console.log(Number.isNaN(42)); // 输出 false
> console.log(Number.isNaN("abc")); // 输出 false
> console.log(Number.isNaN("42")); // 输出 false
> ```
> 
>总之，`NaN` 是 JavaScript 中表示非数字的特殊值，可用于检测无效的或无法表示为数字的操作结果。

#### 1.11`===` 与 `==`

> `==`
>
> 会做类型转换，再进行值的比较
>
> - 两个都为简单类型，字符串和布尔值都会转换成数值，再比较
> - 简单类型与引用类型比较，对象转化成其原始类型的值，再比较
> - 两个都为引用类型，则比较它们是否指向同一个对象
> - null 和 undefined 相等
> - 存在 NaN 则返回 false

> `===`
>
> 只有两个操作数在不转换的前提下相等才返回 `true`。即类型相同，值也需相同
>
> ```js
> let result1 = ("55" === 55); // false，不相等，因为数据类型不同
> let result2 = (55 === 55); // true，相等，因为数据类型相同值也相同
> ```
>
> `undefined` 和 `null` 与自身严格相等
>
> ```js
> let result1 = (null === null)  //true
> let result2 = (undefined === undefined)  //true
> ```

>  **区别**
> 相等操作符（`==`）会做类型转换，再进行值的比较，全等运算符(`===`)不会做类型转换
>
> ```js
> let result1 = ("55" === 55); // false，不相等，因为数据类型不同
> let result2 = (55 === 55); // true，相等，因为数据类型相同值也相同
> ```
>
> null` 和 `undefined` 比较，相等操作符（==）为`true`，全等为`false
>
> ```js
> let result1 = (null == undefined ); // true
> let result2 = (null  === undefined); // false
> ```

> **其他**
>
> 相等运算符隐藏的类型转换，会带来一些违反直觉的结果
>
> ```js
> '' == '0' // false
> 0 == '' // true
> 0 == '0' // true
> 
> false == 'false' // false
> false == '0' // true
> 
> false == undefined // false
> false == null // false
> null == undefined // true
> 
> ' \t\r\n' == 0 // true
> ```
>
> 但在比较`null`的情况的时候，我们一般使用相等操作符`==`
>
> ```js
> const obj = {};
> 
> if(obj.x == null){
>   console.log("1");  //执行
> }
> ```
>
> 等同于下面写法
>
> ```js
> if(obj.x === null || obj.x === undefined) {
>     ...
> }
> ```
>
> 使用相等操作符（==）的写法明显更加简洁了
>
> 所以，除了在比较对象属性为`null`或者`undefined`的情况下，我们可以使用相等操作符（==），其他情况建议一律使用全等操作符（===）

#### 1.12 truly变量与falsely变量

> `truly`变量：`!!a === true` 的变量
> `falsely`变量：`!!b === false` 的变量
>
> 以下是`falsey`变量，除了这六种情况，其余都是`truely`变量
>
> ```js
> !!0 === false
> !!NaN === false
> !!'' === false
> !!null === false
> !!undefined === false
> !!false === false
> ```

#### 1.13 语句与表达式

> **表达式：**一个表达式会产生一个值，可以放在任何一个需要值的地方
>
> ```javascript
> a
> a+b
> demo(1)
> x===y? 'a': 'b'
> 1234
> ```
>
> **语句**
>
> ```js
> if(){}
> for(){}
> ```

-----

### 2.数组字符串相关

#### 2.1 深拷贝浅拷贝的区别？

> [面试官](https://vue3js.cn/interview/JavaScript/copy.html#%E4%B8%80%E3%80%81%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B%E5%AD%98%E5%82%A8)
>
> 浅拷贝是拷贝一层，深层次的引用类型则共享内存地址

#### 2.2 手写深拷贝

> ```js
> function deepClone(obj){
> 	if (typeof obj !== 'object' || obj === null){
> 		return obj
> 	}
> 	let result = Array.isArray(obj) ? []: {}
> 	for (let key in obj) {
> 		if(obj.hasOwnProperty(key)) {
> 			result[key] = deepClone(obj[key])
> 		}
> 	}
> 	return result
> }
> 
> ```

#### 2.3 手写深度比较

> ```js
> // 判断是否是对象或数组
> function isObject(obj) {
>   return typeof obj === object && obj !== null;
> }
> 
> // 深度比较
> function isEqual(obj1, obj2) {
>   if (!isObject(obj1) || !isObject(obj2)) {
>     // 值类型，直接判断【一般不会传函数，不考虑函数】
>     return obj1 === obj2;
>   }
>   if (obj1 === obj2) {
>     return true;
>   }
>   // 两个都是对象或数组，而且不相等
>   // 1. 先判断键的个数是否相等，不相等一定返回false
>   const obj1Keys = Object.keys(obj1);
>   const obj2Keys = Objext.keys(obj2);
>   if (obj1Keys.length !== obj2Keys.length) {
>     return false;
>   }
>   // 2. 以obj1为基准，和obj2依次递归比较
>   for (let key in obj1) {
>     // 递归比较
>     const res = isEqual(obj1[key], obj2[key]);
>     if (!res) {
>       return false;
>     }
>   }
>   // 3. 全相等
>   return true;
> }
> ```

#### 2.4 数组的API有哪些是纯函数

> **纯函数：**①不改变原数组(没有副作用) ②返回一个新数组
> concat、map、filter、slice
>
> **非纯函数：**push、pop、shift、unshift、forEach、some、every、reduce

#### 2.5 `split()`和`join()`的区别

> `split()` 是字符串的方法
> `join()` 是数组的方法
>
> ```javascript
> '1-2-3'.split('-') // [1,2,3]
> [1,2,3].join('-') // 1-2-3
> ```

#### 2.6 数组`slice`与`splice`区别及 `concat`数组强制打平

> [YK菌](https://blog.csdn.net/weixin_44972008/article/details/113093122)

#### 2.7手写字符串 trim 

> ```js
> function trimString(str) {
>   // 使用正则表达式替换开头和结尾的空格
>   return str.replace(/^\s+|\s+$/g, '');
> }
> 
> // 示例用法
> let str = '  Hello, World!  ';
> let trimmedStr = trimString(str);
> console.log(trimmedStr); // 输出 "Hello, World!"
> ```

-----

### 3.函数相关

#### 3.1 函数声明与函数表达式

> 函数声明式
>
> ```javascript
> function fn(a, b) {
>   return a + b;
> }
> ```
>
> 函数表达式
>
> ```javascript
> let fun = function(a, b){
>   return a + b;
> }
> ```

#### 3.2 什么是JSON

> - JSON是一种**数据格式**，本质是一段**字符串**
> - JSON格式与JS对象结构一致，对JS语言更友好
> - `window.JSON`是一个全局对象，常用的两个方法 `JSON.stringify` 和`JSON.parse`

-----

### 4.原型与原型链

#### 4.3 JavaScript原型，原型链 ? 有什么特点？

> 

#### 4.4 class的原型本质

> 

#### 4.5 new Object() 与 Object.create()的区别

> `new Object()` 和 `Object.create()` 是在 JavaScript 中创建对象的两种不同方式，它们有以下区别：
>
> 1. `new Object()`：这是使用构造函数创建对象的方式之一。通过 `new Object()`，可以创建一个空的对象，并可以通过添加属性和方法来自定义对象的行为。例如：
>
>    ```js
>    const obj = new Object();
>    obj.name = 'John';
>    obj.age = 25;
>    ```
>
> 2. `Object.create()`：这是使用**原型继承创建对象**的方式之一。`Object.create()` 方法接收一个参数，用于**指定新对象的原型**。它会创建一个新对象，并将**指定的原型对象设置为新对象的原型**。例如：
>
>    ```js
>    const personProto = {
>      greet: function() {
>        console.log('Hello!');
>      }
>    };
>                            
>    const person = Object.create(personProto);
>    person.name = 'John';
>    person.age = 25;
>    ```
>
> 区别总结如下：
>
> - `new Object()` 创建的对象是一个**空对象**，然后可以通过**点号或方括号语法添加属性和方法**。
> - `Object.create()` 创建的**对象继承自指定的原型对象**，**可以直接使用原型对象中定义的属性和方法，同时也可以添加自己的属性和方法**。
> - `new Object()` 使用的是**构造函数模式**，而 `Object.create()` 使用的是**原型继承模式。**
> - `new Object()` 创建的对象的原型是 `Object.prototype`，而 `Object.create()` 创建的对象的原型是通**过参数指定的对象。**

#### 4.6 用class语法写一个简单的jQuery

> 

-----

### 5.作用域与闭包

> 一个函数和对其周围状态（lexical environment，词法环境）的引用捆绑在一起（或者说函数被引用包围），这样的组合就是**闭包**（closure）
>
> **闭包**让你可以在一个内层函数中访问到其外层函数的作用域

#### 5.1 说说你对作用域链的理解

> - 全局作用域
> - 函数作用域
> - 块级作用域
>
> **词法作用域**
>
> 词法作用域，又叫静态作用域，**变量被创建**时就确定好了，而**非执行阶段**确定的。也就是说我们写好代码时它的作用域就确定了，`JavaScript` 遵循的就是词法作用域
>
> **作用域链**
>
> 当在`Javascript`中使用一个变量的时候，首先`Javascript`引擎会尝试在当前作用域下去寻找该变量，如果没找到，再到它的上层作用域寻找，以此类推直到找到该变量或是已经到了全局作用域
>
> 如果在全局作用域里仍然找不到该变量，它就会在全局范围内隐式声明该变量(非严格模式下)或是直接报错

#### 5.2 this不同场景下如何取值

> this查找采用的是**动态作用域**
> 【this的指向，取决于在哪里执行，而不是在哪里定义】
>
> [YK菌](https://vue3js.cn/interview/JavaScript/this.html#%E4%B8%80%E3%80%81%E5%AE%9A%E4%B9%89)

#### 5.4 bind、call、apply 区别

> `call`、`apply`、`bind`作用是改变函数执行时的上下文，简而言之就是改变函数运行时的`this`指向
>
> > **apply**
> >
> > **定义：**
> >
> > `apply`接受两个参数，第一个参数是`this`的指向，第二个参数是函数接受的参数，以数组的形式传入
> >
> > 改变`this`指向后原函数会立即执行，且此方法只是临时改变`this`指向一次
> >
> > ```js
> > fun.apply(thisArg, [argsArray])
> > ```
> >
> > **作用**：
> >
> > 调用函数 2. 改变函数this指向，但是他的参数必须是数组（伪数组）
> >
> > **举例子：**
> >
> > ```js
> > function fn(...args){
> >     console.log(this,args);
> > }
> > let obj = {
> >     myname:"张三"
> > }
> > 
> > fn.apply(obj,[1,2]); // this会变成传入的obj，传入的参数必须是一个数组；
> > fn(1,2) // this指向window
> > ```
> >
> > 当第一个参数为`null`、`undefined`的时候，默认指向`window`(在浏览器中)
> >
> > ```js
> > fn.apply(null,[1,2]); // this指向window
> > fn.apply(undefined,[1,2]); // this指向window
> > ```
> >
> > **应用：**
> > 可以利用apply 借助数学内置对象求最大值
> >
> > ```js
> > let arr = [1, 34, 556, 44, 23]
> > Math.max.apply(null, arr); // null 表示不需要改变this指向
> > Math.max.apply(Math, arr); // 不改变的话最好就指回去
> > ```
>
> >  **call**
> >
> > **定义：**
> >
> > `call`方法的第一个参数也是`this`的指向，后面传入的是一个参数列表
> >
> > 跟`apply`一样，改变`this`指向后原函数会立即执行，且此方法只是临时改变`this`指向一次
> >
> > ```js
> > fun.call(thisArg, arg1, arg2, ...)
> > ```
> >
> > **作用：**
> >
> > 调用函数 2. 改变函数this指向
> >
> > **举例子：**
> >
> > ```js
> > function fn(...args){
> >     console.log(this,args);
> > }
> > let obj = {
> >     myname:"张三"
> > }
> > 
> > fn.call(obj,1,2); // this会变成传入的obj，传入的参数必须是一个数组；
> > fn(1,2) // this指向window
> > ```
> >
> > 同样的，当第一个参数为`null`、`undefined`的时候，默认指向`window`(在浏览器中)
> >
> > ```js
> > fn.call(null,[1,2]); // this指向window
> > fn.call(undefined,[1,2]); // this指向window
> > ```
> >
> > **应用：**
> >
> > 实现继承
> >
> > ```js
> > function Father(uname, age, sex) {
> >   this.uname = uname
> >   this.age = age
> >   this.sex = sex
> > }
> > 
> > function Son(uname, age, sex){
> >   Father.call(this, uname, age, sex)
> > }
> > 
> > let son = new Son('YK菌', 18, '男')
> > console.log(son) // Son {uname: "YK菌", age: 18, sex: "男"}
> > ```
>
> >  **bind**
> >
> > **定义：**
> >
> > bind方法和call很相似，第一参数也是`this`的指向，后面传入的也是一个参数列表(但是这个参数列表可以分多次传入)
> >
> > 改变`this`指向后不会立即执行，而是返回一个永久改变`this`指向的函数
> >
> > ```js
> > fun.bind(thisArg, arg1, arg2, ...)
> > ```
> >
> > **作用：**
> >
> > 不会调用函数，但是可以改变函数内部this的指向
> > 返回 由指定的 `this` 值和初始化参数改造的原函数拷贝
> >
> > **举例子：**
> >
> > ```js
> > function fn(...args){
> >     console.log(this,args);
> > }
> > let obj = {
> >     myname:"张三"
> > }
> > 
> > const bindFn = fn.bind(obj); // this 也会变成传入的obj ，bind不是立即执行需要执行一次
> > bindFn(1,2) // this指向obj
> > fn(1,2) // this指向window
> > ```
> >
> > **应用：**
> >
> > 改变定时器内部的`this`指向
> >
> > ```js
> > let btn = document.querySelector('button')
> > btn.onclick = function() {
> >   this.disabled = true
> >   setTimeout(function(){
> >     this.disabled = false;
> >   }.bind(this), 3000)
> > }
> > ```

> 从上面可以看到，`apply`、`call`、`bind`三者的**区别**在于：
>
> ​		**相同点：**
>
> - 三者都可以改变函数的`this`对象指向
>
> - 三者第一个参数都是`this`要指向的对象，如果如果没有这个参数或参数为`undefined`或`null`，则默认指向全局`window`
>
>   **区别：**
>
> - 三者都可以传参，但是`apply`是数组，而`call`是参数列表，且`apply`和`call`是一次性传入参数，而`bind`可以分为多次传入
>
> - `bind`是返回绑定this之后的函数，`apply`、`call` 则是立即执行

> **主要应用场景**
>
> `call` 经常做继承.
> `apply` 经常跟数组有关系. 比如借助于数学对象实现数组最大值最小值
> `bind` 不调用函数,但是还想改变this指向. 比如改变定时器内部的`this`指向

#### 5.5 手写bind

> 实现`bind`的步骤，我们可以分解成为三部分：
>
> - 修改`this`指向
> - 动态传递参数
>
> ```js
> // 方式一：只在bind中传递函数参数
> fn.bind(obj,1,2)()
> 
> // 方式二：在bind中传递函数参数，也在返回函数中传递参数
> fn.bind(obj,1)(2)
> ```
>
> 兼容`new`关键字
>
> 整体实现代码如下：
>
> ```js
> Function.prototype.myBind = function (context) {
>     // 判断调用对象是否为函数
>     if (typeof this !== "function") {
>         throw new TypeError("Error");
>     }
> 
>     // 获取参数
>     const args = [...arguments].slice(1),
>           fn = this;
> 
>     return function Fn() {
> 
>         // 根据调用方式，传入不同绑定值
>         return fn.apply(this instanceof Fn ? new fn(...arguments) : context, args.concat(...arguments)); 
>     }
> }
> ```

#### 5.6 闭包

> **是什么**
>
> [面试官](https://vue3js.cn/interview/JavaScript/closure.html#%E4%B8%80%E3%80%81%E6%98%AF%E4%BB%80%E4%B9%88)

-----

### 6.  ES6新特性

#### 6.1  说说var、let、const之间的区别

#### 6.2 ES6的数组增加了哪些新的拓展

> [面试官](https://vue3js.cn/interview/es6/array.html#%E4%B8%80%E3%80%81%E6%89%A9%E5%B1%95%E8%BF%90%E7%AE%97%E7%AC%A6%E7%9A%84%E5%BA%94%E7%94%A8)

#### 6.3 ES6的对象增加了哪些新的拓展

#### 6.4 ES6的函数增加了哪些新的拓展
