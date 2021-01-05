### JavaScript之数据类型判断

在 ECMAScript 规范中，共定义了 7 种数据类型，分为<span style=color:#ff502c>**基本类型**</span>和<span style=color:#ff502c>**引用类型**</span>两大类，如下所示：

> **基本类型**：String、Number、Boolean、Symbol、Undefined、Null 
>
> **引用类型** ：Object

**基本类型：**存储在<span style=color:#ff502c>栈</span>中

**引用类型：**存储在<span style=color:#ff502c>堆</span>中

引用类型除了包括Object外，还包括Array、Function、Date等等。



#### 1、typeof

***

**typeof** 是一个操作符，其右侧跟一个一元表达式，并返回这个表达式的数据类型。返回的结果用该类型的字符串(全小写字母)形式表示，包括以下 <span style=color:#ff502c>7</span> 种：**number**、**boolean**、**symbol**、**string**、**object**、**undefined**、**function** 等。

```javascript
let num = 18;
let str = "yhd";
let show = true;
let sym = Symbol();
let data = undefined;
let info = null;

console.log(typeof num); // number
console.log(typeof str); // string
console.log(typeof show); // boolean
console.log(typeof sym); // symbol
console.log(typeof data); // undefined
console.log(typeof info); // object


let fun = function add() {};
let arr = ["yhd", "gsr"];
let obj = {name: "yhd", age: "23"};
let date = new Date();


console.log(typeof fun); // function
console.log(typeof arr); // object
console.log(typeof obj); // object
console.log(typeof date); // object

```

**typeof判断总结：**

> * 对于基本类型，除<span style=color:#ff502c> **null**</span> 以外，均可以返回正确的结果。
> * 对于引用类型，除<span style=color:#ff502c>**function**</span> 以外，均返回 <span style=color:#ff502c>**object**</span> 类型。
> * 对于 <span style=color:#ff502c>**null**</span> ，返回 <span style=color:#ff502c>**object**</span> 类型。
> * 对于<span style=color:#ff502c> **function**</span>，返回 <span style=color:#ff502c> **function**</span> 类型。

***



#### 2、instanceof

***

**instanceof** 可以正确判断对象的数据类型，用来判断 <span style=color:#ff502c> **A**</span> 是否为 <span style=color:#ff502c> **B**</span> 的实例，表达式为：<span style=color:#ff502c> **A instanceof B**</span>，如果<span style=color:#ff502c> **A**</span> 是<span style=color:#ff502c> **B**</span> 的实例，则返回 <span style=color:#ff502c> **true**</span>，否则返回<span style=color:#ff502c> **false**</span>。因为内部机制是通过判断对象的原型链中是不是能找到类型的 <span style=color:#ff502c> **prototype**</span>。

```javascript
let fun = function add() {};
let arr = ["yhd", "gsr"];
let obj = {name: "yhd", age: "23"};
let date = new Date();

console.log(fun instanceof Function); // true
console.log(arr instanceof Array); // true
console.log(obj instanceof Object); // true
console.log(date instanceof Date); // true

console.log(arr instanceof Object) // true
console.log(fun instanceof Object); // true
console.log(date instanceof Object); // true

```

​		<span style=color:#ff502c> **instanceof**</span> 能够判断出<span style=color:#ff502c> **arr**</span>是<span style=color:#ff502c> **Array**</span>的实例，但他也认为<span style=color:#ff502c> **arr**</span> 也是<span style=color:#ff502c> **Object**</span> 的实例，为什么呢？

**原因**：

​		从 <span style=color:#ff502c>**instanceof**</span> 中能够判断出 <span style=color:#ff502c>**arr.proto**</span>  指向 <span style=color:#ff502c>**Array.prototype**</span>，而 <span style=color:#ff502c>**Array.prototype.proto**</span> 又指向了<span style=color:#ff502c>**Object.prototype**</span>，最终 <span style=color:#ff502c>**Object.prototype.proto**</span> 指向了<span style=color:#ff502c>**null**</span>，标志着原型链的结束。因此，<span style=color:#ff502c>**arr、Array、Object**</span> 就在内部形成了一条原型链。

在原型链上，<span style=color:#ff502c>**arr**</span> 的 <span style=color:#ff502c>**proto**</span>  直接指向<span style=color:#ff502c>**Array.prototype**</span>，间接指向 <span style=color:#ff502c>**Object.prototype**</span>。function、date同理。



**instanceof总结：**

>  <span style=color:red>**instanceof 只能用来判断两个对象是否属于实例关系， 而不能判断一个对象实例具体属于哪种类型。**</span>

***



#### 3、constructor

***

当一个函数被定义时，JS引擎会为函数添加 prototype 原型，然后再在 prototype上添加一个  <span style=color:red>**constructor**</span> 属性，并让其指向他的构造函数

```javascript
let num = 18;
let str = "yhd";
let show = true;
let sym = Symbol();
let data = undefined;
let info = null;

console.log(num.constructor); // ƒ Number() { [native code] }
console.log(str.constructor); // ƒ String() { [native code] }
console.log(show.constructor); // ƒ Boolean() { [native code] }
console.log(sym.constructor); // ƒ Symbol() { [native code] }
console.log(data.constructor); // Cannot read property 'constructor' of undefined
console.log(info.constructor); // Cannot read property 'constructor' of null

let fun = function add() {};
let arr = ["yhd", "gsr"];
let obj = {name: "yhd", age: "23"};
let date = new Date();

console.log(fun.constructor); // ƒ Function() { [native code] }
console.log(arr.constructor); // ƒ Array() { [native code] }
console.log(obj.constructor); // ƒ Object() { [native code] }
console.log(date.constructor); // ƒ Date() { [native code] }
```



**constructor总结：**

> * <span style=color:red>**null**</span> 和 <span style=color:red>**undefined**</span> 是无效的对象，因此是不会有 constructor 存在的，这两种类型的数据需要通过其他方式来判断。
> * 函数的 constructor 是不稳定的，这个主要体现在自定义对象上，当开发者重写 prototype 后，原有的 constructor 引用会丢失，constructor 会默认为 Object

***



#### 4、toString

***

<span style=color:red>**toString()**</span> 是 <span style=color:red>**Object**</span> 的原型方法，调用该方法，默认返回当前对象的 <span style=color:red>**[[Class]]**</span> 。这是一个内部属性，其格式为 <span style=color:red>**[object xxx]**</span> ，其中 <span style=color:red>**xxx**</span> 就是对象的类型。

对于 <span style=color:red>**Object**</span> 对象，直接调用 <span style=color:red>**toString()**</span> 就能返回 <span style=color:red>**[object Object]**</span> 。而对于其他对象，则需要通过 <span style=color:red>**call / apply**</span> 来调用才能返回正确的类型信息。

```javascript
Object.prototype.toString.call(1);    // [object Number]
Object.prototype.toString.call('');   // [object String]
Object.prototype.toString.call(true); // [object Boolean]
Object.prototype.toString.call(Symbol()); //[object Symbol]
Object.prototype.toString.call(undefined); // [object Undefined]
Object.prototype.toString.call(null); // [object Null]

Object.prototype.toString(Object); // [object Object]
Object.prototype.toString.call(new Function()) ; // [object Function]
Object.prototype.toString.call([]) ; // [object Array]
Object.prototype.toString.call(new Date()) ; // [object Date]

Object.prototype.toString.call(new RegExp()) ; // [object RegExp]
Object.prototype.toString.call(new Error()) ; // [object Error]

Object.prototype.toString.call(NaN) ; // [object Number]
```



**toString()总结：**

> 最好的判断方法，无论什么类型都可以精准判断