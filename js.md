# JavaScript

- [JavaScript](#javascript)
  - [js 基本数据类型](#js-基本数据类型)
  
  - [js 遍历对象和遍历数组的方式](#js 遍历对象和遍历数组的方式)
  
  - [JS中的深拷贝与浅拷贝](#JS中的深拷贝与浅拷贝)
  
  - [JavaScript 继承的几种实现方式](#javascript-继承的几种实现方式)
  
  - [js 原型，原型链以及特点](#js-原型原型链以及特点)
  
  - [Object.defineProperty 用法](#objectdefineproperty-用法)
  
  - [async await](#async-await)
  
  - [Event Loop 事件循环](#event-loop-事件循环)
  
  - [new 运算符的过程](#new-运算符的过程)
  
  - [JS 作用域](#js-作用域)
  
  - [ES6 新特性](#es6-新特性)
  
  - [let 和 var 的区别](#let-和-var-的区别)
  
  - [闭包的特性以及优缺点](#闭包的特性以及优缺点)
  
  - [箭头函数与普通函数的区别](#箭头函数与普通函数的区别)
  
  - [ES6 中箭头函数 VS 普通函数的 this 指向](#es6-中箭头函数-vs-普通函数的-this-指向)
  
  - [ES6 class 和 ES5 函数的区别](#es6-class-和-es5-函数的区别)
  
  - [Promise 是做什么的，有哪些API](#promise-是做什么的有哪些api)
  
  - [arguments怎么转化成真数组](#arguments怎么转化成真数组)
  
    

## js 基本数据类型

> js 一共有7种**基本**数据类型，分别是 Undefined、Null、Boolean、Number、String。
> <font color='red'>（注意大写字母开头）</font><font color='red'>（大写字母开头）</font><font color='red'>（大写字母开头）</font>
> ES6 中新增的 Symbol 类型，代表创建后独一无二且不可变的数据类型，主要是为了解决可能出现的全局变量冲突的问题。
>
> ES2020新增BigInt，用于处理任意精度整数的新数字基元n，比如3n。



> **ES5中有6种数据类型**：
> null,undefined,number,string,boolean,object。
>
> + **复杂类型**是指object即广义的对象类型，可由多个简单类型的值的合成，可以看作是一个存放各种值的容器。包括Array,Math，Date,RegExp,Function等
> + **基础类型**指number,string,boolean，null,undefined。
>
> 基础类型和复杂类型的区别：
>
> + 基础类型将内容直接存储在**栈**空间中（大小固定位置连续的存储空间），记录的是该数据类型的值，即直接访问，基础类型赋值是复制（copy）； 
> + 复杂类型将内容存储在堆(与堆排序的堆不是一个概念)中，堆所对应的栈中记录的是**指针**（堆的地址），外部访问时先引出地址，再通过地址去找到值所存放的位置。复杂类型赋值是地址引用。
>
> ```javascript
> //经典考题1地址引用
> let a = {name:'du',age:19};
> let b =a;
> console.log(b.name);//du
> b.name = 'lee';
> console.log(b.name);//lee
> console.log(a.name);//lee a中的属性改变，说明a和b指向同一地址
> ```
>
> ```javascript
> //经典考题2
> let a = {name:'du',age:19};
> function change (obj){
>  obj.age=29;  //obj作为对象参数，引用地址传入，修改age属性影响了原对象
>  obj = {   //赋值操作，相当新建了一个对象让obj指向他
>      name:'lee',
>      age:39
>  }
>  return obj;  //返回新对象
> }
> let b=change(a);
> console.log(a.age);//29，a中已改变
> console.log(b.age);//39，b为新对象
> ```
>
> 

> js是一种解释性：不用编译；动态：不用声明类型，弱类型：类型转换，语言
>
> JavaScript数字全部是浮点数。 根据 IEEE 754标准中的64位二进制(binary64), 也称作双精度规范(double precision)来储存。这些字节按照以下规则分配：
>
> > 0 - 51 位是 分数f（fraction ）
> > 52 - 62 位是 指数（exponent ）
> > 63 位 是 标志位 （sign）,结果二进制形式为是+- 1.f * 2<sup>e</sup>,最大**精确的**整数为2**53

```js
//经典考题
0.1+0.1===0.2 //true
0.1+0.2===0.3 //false
```

原理解析见[本文](https://juejin.cn/post/6991822067234504717)，帮助理解。

## null 和 undefined 的区别

- Undefined 和 Null 都是基本数据类型，这两个基本数据类型分别都只有一个值，就是 undefined 和 null。
- undefined 代表的含义是未定义，null 代表的含义是空对象。一般变量声明了但还没有定义的时候会返回 undefined，null主要用于赋值给一些可能会返回对象的变量，作为初始化。
-  undfined是变量，而null是关键字。在低版本浏览器种可以被赋值而更改，它会影响我们对 undefined 值的判断。可以通过一元运算符void获得安全的 undefined 值，比如说 void 0。
- 使用 typeof 进行判断的时候，null 类型化会返回 “object”，这是一个历史遗留的问题。
- `undefined==null(true)` `undefined===null(false)`
## 判断数据类型的方式

1. typeof  缺陷：可以判断null以外的基础类型，复杂类型除funiton以外均判断为object

   // typeof（null）===‘’object’；typeof (()=>{} )）===‘function’；typeof NaN === “number";

2. instanceof   可以准确判断复杂引用类型，但不能判断基础类型;右边构造函数或者class

   //Object instanceof Object//true   

3. Object.prototype.toString.call() ；可以判断所有类型，返回示例 ''[object Array]"

   注意后面的类型一定是大写字母开头。

   ```javascript
   Object.prototype.toString.call(null);
   //"[object Null]"
   ```

4. .constructor===  //返回构造器，例如(2.1).constructor === Number//true

   注：构造函数可以被复写

   总：使用typeof 先判断是否复杂类型，如果是，再使用toString判断具体。
## instanceof 的作用
> instanceof 运算符用于判断构造函数（放运算符右边）的 prototype 属性是否出现在对象（运算符左边）的原型链中的任何位置。

## 一些值到字符串的显示转换 String()

```js

（1）null 和 undefined 类型 ，null 转换为 "null"，undefined 转换为 "undefined"，
（2）Boolean 类型，true 转换为 "true"，false 转换为 "false"。
（3）Number 类型的值直接转换，那些极小和极大的数字会使用指数形式。
（4）Symbol 类型的值直接转换，但是只允许显式强制类型转换，使用隐式强制类型转换会产生错误。
（5）对普通对象来说，除非自行定义 toString() 方法，否则会调用 toString()（Object.prototype.toString()）来返回内部属性 [[Class]] 的值，如"[object Object]"。如果对象有自己的 toString() 方法，字符串化时就会调用该方法并使用其返回值。
[]转为‘’
```

## 其他值到数字值的显式转换，如Number()

（1）undefined 类型的值转换为 NaN。
（2）null 类型的值转换为 0。
（3）Boolean 类型的值，true 转换为 1，false 转换为 0。
（4）String 类型的值转换如同使用 Number() 函数进行转换，包含非数字值则转换为 NaN，空字符串为 0,"-Infinity"转化为-Infinity
（5）Symbol 类型的值不能转换为数字，会报错。

（6）对象（包括数组）会首先被转换为相应的基本类型值，如果返回的是非数字的基本类型值，则再遵循以上规则将其强制转换为数字。
    <font color='red'>{}转换为NaN。[]转为0.</font>


## 其他值到布尔类型强制类型转换

```js
以下这些布尔强制类型转换是假值：
• undefined
• null
• false
• +0、-0 和 NaN
• ""

假值的布尔强制类型转换结果为 false。从逻辑上说，假值列表以外的都应该是真值。
//Boolen（' '）为true，Boolen（[]）为true
```

## 隐式类型转换

编译器自动完成类型转换的方式就称为隐式转换， JS中其总是期望返回基本类型值
预期的值类型和传入的类型不一致的时候，往往会发生隐式转换。
常见场景：

● 二元+运算符
● 关系运算符 >, <， >=,  <=,  == 
● 逻辑！
● if/while ，三目条件，
● 属性键, for in, let of
● 模板字符串
● || , &&


## 二元操+作符的规则

1. 如果操作数是对象，则对象会转换为原始值

2. 如果其中一个操作数是字符串的话，另一个操作数也会转换成字符串，进行字符串连接

3. 否则，两个操作数都将转换成数字，进行加法操作

   undefined + undefined // NaN
   null + null           // 0
   1 + 'a'               // '1a'


##  NaN

typeof NaN是数字,NaN !== NaN,不可以被删除`delete`，不可以被改写， 也不可以改写配置。

判断NaN的2种方式

Number.isNaN()是严格判断, 必须严格等于`NaN`。`**是不是NaN这个值**`

isNaN()是通过内部的 toNumber 转换结果来判定的,对象本身的valueof方法返回的只可以影响判断结果。`**Number转换的返回值是不是NaN**`

## || 和 && 操作符的返回值

```js
|| 和 && 首先会对第一个操作数执行条件判断，如果其不是布尔值就先进行 ToBoolean 强制类型转换，然后再执行条件判断。

对于 || 来说，如果条件判断结果为 true 就返回第一个操作数的值，如果为 false 就返回第二个操作数的值。

&& 则相反，如果条件判断结果为 true 就返回第二个操作数的值，如果为 false 就返回第一个操作数的值。

|| 和 && 返回它们其中一个操作数的值，而非条件判断的结果
```

## == 操作符的强制类型转换规则

```js
（1）字符串和数字之间的相等比较，将字符串转换为数字之后再进行比较。

（2）其他类型和布尔类型之间的相等比较，先将布尔值转换为数字后，再应用其他规则进行比较。

（3）null 和 undefined 之间的相等比较，结果为真。与本身比较返回真。其他值和它们进行比较都返回假值。

（4）对象和非对象之间的相等比较，对象先调用 ToPrimitive 抽象操作后，再进行比较。

（5）如果一个操作值为 NaN ，则相等比较返回 false（ NaN 本身也不等于 NaN ）。

（6）如果两个操作值都是对象，则比较它们是不是指向同一个对象。如果两个操作数都指向同一个对象，则相等操作符返回 true，否则，返回 false。
[]==![]//true;
[]==[]//false
```

## 如何将字符串转化为数字，例如 '12.3b'

（1）使用 Number() 方法，前提是所包含的字符串不包含不合法字符。

（2）使用 parseInt() 方法，parseInt() 函数可解析一个字符串，并返回一个整数。还可以设置要解析的数字的基数。当基数的值为 0，或没有设置该参数时，parseInt() 会根据 string 来判断数字的基数。[1,2,3].map(parseInt) ===[1,NaN,NaN]

（3）使用 parseFloat() 方法，该函数解析一个字符串参数并返回一个浮点数。

## js 遍历对象和遍历数组的方式

### 遍历对象

- Object.keys()

> 返回一个数组,包括对象自身的(不含继承的)所有可枚举属性(不含Symbol属性).数组中属性名的排列顺序和正常循环遍历该对象时返回的顺序一致 。

```js
let obj = {name: 'lee',sex: 'male',age: 18}
Object.keys(obj).forEach(key => {
    console.log(key, obj[key]);
})
```

- for...in

> 循环遍历对象自身的和继承的可枚举属性(不含Symbol属性).  

- Object.getOwnPropertyNames()

> 返回一个数组,包含对象自身的所有属性(不含Symbol属性,但是包括不可枚举属性).

- Reflect.ownKeys()

> 返回一个数组,包含对象自身的所有属性(包括Symbol属性和不可枚举属性).

### 遍历数组

- forEach()

```js
let arr = [1,2,,3];
arr.forEach((v,k) => {
    console.log(v);
})//跳过空
```

- for...in

```
> 注意for...in遍历的是索引
let arr = [1,2,,3];
for(let ele in arr) {
    console.log(ele);
}
//0 1 3
```

- for...of

```js
let arr = [1,2,,3];
for(let ele of arr) {
    console.log(ele);
}
//1,2,undefined,3
```

## js的对象的常用的方法

```js
Object.assign() //浅复制对象
Object.entries() //返回自身可枚举的[key,value]
Object.keys()，Object.values()
Object.hasOwnProperty(key)//是否有这个属性 true/false
Object.getOwnPropertyNames() //取得对象自身可枚举的属性名
//for in 对对象进行遍历，可以拿到自身以及原型链上的可枚举的属性
Object.freeze()//冻结一个对象，不可修改，不可删除。不可添加新的属性
Object.prototype.toString()// 返回数组[object,object/array/function等]  
//判断是数组还是对象就是用的这个方法
```

## js的字符串的常用的方法

```js
str.concat()//拼接
str.includes()//判断字符串是否包含在另外一个字符串中
str.indexOf()，str.lastIndexOf()
str.split() //按特定的符号分割成字符串数组！
str.toLowerCase() //转换成小写的形式
str.toUpperCase() //转换成大写的形式
str.trim()//去空格
```

## js的数组的常用的方法

```js
var arr = [0,1,2,3,4]

arr.push()，arr.pop()， arr.shift()，arr.unshift()，arr.reverse()，arr.every()，arr.some()
arr.forEach()不会迭代空元素，arr.filter()实现过滤，arr.map()返回数组，arr.reduce()前后
arr.includes()，arr.indexOf()，arr.lastIndexOf() //索引正序，但是从后往前找
arr.findIndex() //找索引，
arr.find()  //找满足条件的元素
arr.join()//默认以逗号隔开
arr.splice(3,1,'o','i')//从索引3开始，删除1个，添加两个字符串。
arr.flat() //数组降维 ，返回新数组
arr.flat(1)
arr.entries() //将数组返回一个对象，包含对象索引的键值对
```

## 数组的 push() 和 pop() 方法的返回值是什么

- `push()`将一个或多个元素添加到数组的末尾，并返回该数组的新长度
- `pop()`方法从数组中删除最后一个元素，并返回该元素的值

# substring substr slice splice的区别。

- substring(star,stop) ,star>stop时会交换两者；相等时返回空串；为负数时会将负数变为0，奇怪点较多。
- substr(star,length),star为负数时，表示倒数位置；length<=0时，返回空串
- slice(star,stop), 可以应用在数组和字符串；star>=stop时返回空串/数组；为负数时代表倒数位置
- splice(start,length,items,只能用于数组，会改变原始数组；satr<0代表倒数，length<0返回空，length过大时截取全部。

## JS中的深拷贝与浅拷贝

浅copy复制，创建一个新对象，接受原对象的值或者地址，改变新对象会引起原对象的变。

深拷贝完全是另一个对象，

```js
Object.assign(newobj,obj)//ES6浅拷贝，遍历所有可枚举属性，然后赋值。把obj的可枚举属性拷贝给newobj，如果属性的值是复杂对象，则会指向同一地址，因此是浅拷贝

let newobj = {...obj} //浅拷贝 同上

Object.create(obj)//不能算是拷贝？该是继承，此方法使用指定的原型对象及其属性去创建一个新的对象

let newObj = JSON.parse(JSON.stringify(oldObj));//不完全的深拷贝
```
## JSON 
  键只能是字符串，必须双引号包裹，值可以是10进制的数，字符串，数组，不能是Date，undefined，函数，
  JSON.parse()：JSON.parse(text[, reviver])，将json串转为对象，可选参数reviver 为一个函数，如果 reviver 返回 undefined，则当前属性会从所属对象中删除，如果返回了其他值，则返回的值会成为当前属性新的属性值。
  JSON.stringify()
    将一个 JavaScript 对象或值转换为 JSON 字符串。  JSON.stringify(value[, replacer [, space]])
    第二个参数 replacer 可选
  如果该参数是一个函数：则在序列化过程中，被序列化的值的每个属性都会经过该函数的转换和处理；
  如果该参数是一个数组：则只有包含在这个数组中的属性名才会被序列化到最终的 JSON 字符串中；
  如果该参数为 null 或者未提供: ，则对象所有的属性都会被序列化。
  第三个参数 space 可选
  指定缩进用的空白字符串，用于美化输出（pretty-print）；

  限制： 对于 undefined， symbol， 函数，这三种在作为对象的值、数组元素、单独的值时会不同。
    ● 作为对象属性时被忽略。
    ● 作为数组元素值时，转为null，（数组的长度不能变）
    ● 作为单独的值， 返回 undefined
  BigInt 会报错，Date类型会便成为字符串，如果有循环引用会报错，map和set类型，什么都不会有JSON.stringify(map)：{}

  如果一个被序列化的对象拥有 toJSON 方法，那么JSON.stringify得到的就是该 toJSON 方法的返回值

## JSON.parse(JSON.stringify(obj)) 实现深拷贝需要注意的问题
  以上


## eval 是做什么的

它的功能是把对应的字符串解析成 JS 代码并运行。
应该避免使用 eval，不安全，非常耗性能（2次，一次解析成 js 语句，一次执行）。

## js垃圾回收机制

基本类型：栈空间，引用类型：堆空间；池：常量

回收方法/：标记清除法和引用计数法

## js 原型，原型链以及特点

```JavaScript
JavaScript 只有一种结构：对象。例子：
function Class(){
    this.name='name';
}
class = new Class();//小写class为Class类的实例；Class为构造函数

每个实例对象（object）都有一个私有属性（称之为 __proto__ ）指向它的构造函数的原型对象（prototype）。

即class.__proto__ === Class.prototype; //true

prototype中一般包含2个属性，一个是constructor，指向Class函数自身；一个是__proto__,指向更高一级的原型对象。可以在prototype中添加新的属性方法，实例上也能访问到。

原型对象也有一个自己的原型对象（__proto__），层层向上直到一个对象的原型对象为 null。根据定义，null 没有原型，并作为这个原型链中的最后一个环节。

ES5 中Object.getPrototypeOf() 方法来获取对象的原型。
当访问一个实例对象的属性时，如果这个对象内部不存在这个属性，那么它就会去它的原型对象里找这个属性，这个原型对象又会有自己的原型，于是就这样一直找下去，也就是原型链的概念。

特点：
JavaScript 对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。
```
## JavaScript 继承的方式

- 原型链继承，分清构造函数，原型，实例：1,每一个构造函数都有一个原型对象;2,原型对象包含一个指向构造函数的指针;3,实例中包含一个原型对象的指针。缺点，在包含有引用类型的数据时，会被所有的实例对象所共享，容易造成修改的混乱。

- 使用构造函数的方式。

- 组合继承。

- 原型式继承。

- 寄生式继承。

- 寄生组合继承。

## Object.defineProperty 用法

```js
Object.defineProperty(obj, prop, descriptor)
```
- obj
  要定义属性的对象。
- prop
  要定义或修改的属性的名称或 Symbol 。
- descriptor
  要定义或修改的属性描述符。参数有：
  - value
  该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。
  默认为 undefined。
  - writable
  当且仅当该属性的 writable 键值为 true 时，属性的值，也就是上面的 value，才能被赋值运算符改变。
  默认为 false。
  - configurable
  当且仅当该属性的 configurable 键值为 true 时，该属性的描述符才能够被改变，同时该属性也能从对应的对象上被删除。
  默认为 false。
  - enumerable
  当且仅当该属性的 enumerable 键值为 true 时，该属性才会出现在对象的枚举属性中。
  - 默认为 false。

## new 运算符的过程

1. 创建一个空对象{}；
2. 构造函数中的 this 指向该空对象
3. 执行构造函数为这个空对象添加属性
4. 判断构造函数有没有返回值，如果返回值是个对象，则返回这个对象；否则返回创建的对象

```js
function Child1(){this.name='child1';}

var p = new Child1();

p//Child1 {name: "child1"} 一个Child1的实例

var p = Child1()  //不使用new

p  //undefined 因为Chilid1函数结果返回值为undefined
name  // child1方法中this指向window，所以window由了一个全局属性name
```

## 变量提升：

+ 函数声明优先与变量声明（函数与变量同名情况下函数有效），函数声明可被覆盖，（声明2次同名函数，后者覆盖前者）
+ 匿名函数不提升，不继承Function原型

## JS 作用域

ES5 只有全局作用域和函数作用域

- 全局作用域：代码在程序的任何地方都能被访问，window 对象的内置属性都存在全局作用域

- 函数作用域：在固定的代码片段才能被访问

- 末定义直接赋值的变量自动声明为拥有全局作用域

  ```js
  var num1 = 1;
  function fun1 (){
      num2 = 2;
  }
  num2;//报错
  fun1();
  num2;//2 需先调用函数后才能完成num2的定义和赋值
  ```


```js
var num1 = 1;
function fun1 (num1){
    console.log(num1);
    num1 = 2;
    console.log(num1);
}
fun1();//undefined,2  未传递参数
num1;//1,

var num1 = 1;
function fun1 (){
    console.log(num1);num1 = 2;console.log(num1);
}
fun1();//1，2  访问全局变量
num1;//2  num1被改变

var num1 = 1;
function fun1 (){
    console.log(num1); var num1 = 2;console.log(num1);
}
fun1();//undefined,2  函数内作用域num1提升到函数作用域顶部，但尚未赋值
num1;//1 全局num1不改变

var num1 = 1;
function fun1 (){
    console.log(num1); let num1 = 2;console.log(num1);
}
fun1();//报错，暂时性死区，let不提升
```

ES6 有块级作用域

```js
for(let j=0;j<2;j++){
    console.log("for let:",j);
}
console.log("let:",j);// Uncaught ReferenceError: j is not defined
```
作用域也可以根据代码层次分层，以便子作用域可以访问父作用域，通常是指沿着链式的作用域链查找，而不能从父作用域引用子作用域中的变量和引用。
```js
var a = 1;

function test() {
    console.log(a);
}

function test1() {
    var a = 2;
    test();
}

test1()  // 1
```
作用域是在函数创建的时候就已经创建,因此这里结果是1而不是2.test执行时从作用域链出来找a=1。

## ES6 新特性

- let const 块级作用域
- 模板字符串
- 解构赋值
- 箭头函数，函数参数默认值
- 扩展运算符（...）
- forEach for...of for...in
- 数组方法map reduce includes
- map和set
- 模块化
- promise proxy
- async
- class

## let 和 var 的区别

- let 是块级作用域，var 是函数作用域
- var 存在变量提升，而 let 没有
- let变量不能覆盖作用域中已定义的变量，const变量定义同时必须赋值
- 如果区块中存在let 或者 const ,这个区块对这些声明的变量，将形成一个封闭的作用域，不允许去父作用域查找。在代码块内，使用 let 命令声明变量之前，该变量都是不可用的。这在语法上，称为“暂时性死区” (TDZ)

## 闭包的特性以及优缺点

本质：当前环境中存在指向父级作用域的引用
闭包有三个特性：

- 函数嵌套函数；
- 内部函数使用外部函数的参数和变量；
- 参数和变量不会被垃圾回收机制回收。

闭包的优点：

- 希望一个变量长期保存内存中；
- 避免全局变量污染；
- 私有成员的存在。

闭包的缺点：

- 常驻内存，增加内存使用量；
- 使用不当造成内存泄漏。

## 箭头函数与普通函数的区别

1. 箭头函数是匿名函数，不能作为构造函数，不能使用new
2. 箭头函数不能绑定arguments，取而代之用...解决

   ```js
   let C = (...c) => {
     console.log(c);
   }
   C(3,82,32,11323);
   // [3, 82, 32, 11323]
   ```
   
3. 箭头函数没有原型属性
4. 箭头函数的this永远指向其上下文的this，没有办改变其指向，
   普通函数的this指向调用它的对象

## ES6 中箭头函数 VS 普通函数的 this 指向


普通函数中 this

1. 查看函数在哪被调用。
2. 点左侧有没有对象？如果有，它就是 “this” 的引用。如果没有，继续第 3 步。
3. 该函数是不是用 “call”、“apply” 或者 “bind” 调用的？如果是，它会显式地指明 “this” 的引用。如果不是，继续第 4 步。
4. 该函数是不是用 “new” 调用的？如果是，“this” 指向的就是 JavaScript 解释器新创建的对象。如果不是，继续第 5 步。
5. 是否在“严格模式”下？如果是，“this” 就是 undefined，如果不是，继续第 6 步。
6. JavaScript 很奇怪，“this” 会指向 “window” 对象。

ES6 箭头函数中 this

1.  默认指向定义它时，所处上下文的对象的 this 指向；
    偶尔没有上下文对象，this 就指向 window


```js
// 例1
function hello() {
  console.log(this) // window
}
hello()

// 例2
function hello() {
  'use strict'
  console.log(this) // undefined
}
hello()

// 例3
const obj = {
  num: 10,
  hello: function () {
    console.log(this) // obj
    setTimeout(function () {
      console.log(this) // window
    })
  },
}
obj.hello()

// 例4
const obj = {
  num: 10,
  hello: function () {
    console.log(this) // obj
    setTimeout(() => {
      console.log(this) // obj
    })
  },
}
obj.hello()

// 例5
const obj = {
  radius: 10,
  diameter() {
    return this.radius * 2//diameter是普通函数，里面的this指向直接调用它的对象obj。
  },
  perimeter: () => 2 * Math.PI * this.radius,//这里上下文没有函数对象，就默认为window。
}
console.log(obj.diameter()) // 20
console.log(obj.perimeter()) // NaN
```

## ES6 class 和 ES5 函数的区别

> 本质上，ES6 的类只是 ES5 的构造函数的一层包装

1. 与ES5不同，ES6 类和模块的内部默认就是严格模式，不存在遍历提升

   es5函数内的函数声明会提升到函数作用域起始处，但函数体本身只会提升到块级作用域起始处。

## 立即执行函数的概念及应用

声明一个函数，并马上调用这个匿名函数就叫做立即执行函数；也可以说立即执行函数是一种语法，让你的函数在定义以后立即执行；

- 不必为函数命名，避免了污染全局变量

- 立即执行函数内部形成了一个单独的作用域，可以封装一些外部无法读取的私有变量

- 只需使用1次的函数。



## Promise 是做什么的，有哪些API

### Promise用法

> Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。
>
> resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

```js
const promise = new Promise(function(resolve, reject) {
  if (1){
    resolve(value);
  } else {
    reject(error);
  }
});
```

### Promise.prototype.then()

> Promise 实例具有 then 方法，也就是说，then 方法是定义在原型对象 Promise.prototype 上的。它的作用是为 Promise 实例添加状态改变时的回调函数。前面说过，then 方法的第一个参数是 resolved 状态的回调函数，第二个参数（可选）是 rejected 状态的回调函数。
>
> **then 方法返回的是一个新的 Promise 实例**（注意，不是原来那个 Promise 实例）。因此可以采用链式写法，即 then 方法后面再调用另一个 then 方法。

```js
getJSON("/posts.json").then(function(json) {
  return json.post;
}).then(function(post) {
  // ...
});
```

### Promise.prototype.catch()

> Promise.prototype.catch()方法是.then(null, rejection)或.then(undefined, rejection)的别名，用于指定发生错误时的回调函数。

```js
getJSON('/posts.json').then(function(posts) {
  // ...
}).catch(function(error) {
  console.log('发生错误！', error);
});
```

> 上面代码中，getJSON()方法返回一个 Promise 对象，如果该对象状态变为resolved，则会调用then()方法指定的回调函数；如果异步操作抛出错误，状态就会变为rejected，就会调用catch()方法指定的回调函数，处理这个错误。另外，then()方法指定的回调函数，如果运行中抛出错误，也会被catch()方法捕获。

### Promise.all()

> Promise.all()方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。

```js
const p = Promise.all([p1, p2, p3]);
```

> 上面代码中，Promise.all()方法接受一个数组作为参数，p1、p2、p3都是 Promise 实例，如果不是，就会先调用下面讲到的Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。
>
> 另外，Promise.all()方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。
>
> p的状态由p1、p2、p3决定，分成两种情况:
>
> （1）只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
>
> （2）只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。

### Promise.race()

> Promise.race()方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。

```js
const p = Promise.race([p1, p2, p3]);
```

> 上面代码中，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。

### Promise.resolve()

> 有时需要将现有对象转为 Promise 对象，Promise.resolve()方法就起到这个作用。
>
> Promise.resolve()等价于下面的写法。

```js
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
```

### Promise.reject()

> Promise.reject(reason)方法也会返回一个新的 Promise 实例，该实例的状态为rejected。

```js
const p = Promise.reject('出错了');
// 等同于
const p = new Promise((resolve, reject) => reject('出错了'))

p.then(null, function (s) {
  console.log(s)
});
// 出错了
```

> 上面代码生成一个 Promise 对象的实例p，状态为rejected，回调函数会立即执行。

## 实现primisfy

参考[简单实现](https://juejin.cn/post/6844904024928419848)

#### promisify(fn, reverse)

- fn <function> 有回调函数作为参数的函数
- reverse <Boolean> 默认False。当fn的回调函数参数在前时(如setTimeout)，设为True。

```js
function promisify(fn, reverse) {
  if ({}.toString.call(fn) !== '[object Function]') throw new TypeError('Only normal function can be promisified');
  return function (...args) {
    return new Promise((resolve, reject) => {
      const callback = function (...args) {
        if ({}.toString.call(args[0]) === '[object Error]') return reject(args[0]);
        resolve(args);
      };
      try {
        if (reverse === true) fn.apply(null, [callback, ...args]);
        else fn.apply(null, [...args, callback]);
      } catch (err) {
        reject(err);
      }
    });
  }
}
```



## Promise不兼容怎么解决

用一些第三方的库来解决兼容性问题：

1. babel-polyfill
2. ES6-Promise
3. bluebird
# callback和Promise的区别是什么？

callback

回调函数本身是我们约定俗成的一种叫法。

优点：比较容易理解；
缺点：1.高耦合，维护困难，回调地狱;2.每个任务只能指定一个回调函数;3.如果几个异步操作之间并没有顺序之分，同样也要等待上一个操作执行结束再进行下一个操作。

Promise

ES6给我们提供了一个原生的构造函数Promise，Promise代表了一个异步操作，可以将异步对象和回调函数脱离开来，通过.then方法在这个异步操作上绑定回调函数，Promise可以让我们通过链式调用的方法去解决回调嵌套的问题，而且由于promise.all这样的方法存在，可以让同时执行多个操作变得简单。

promise对象存在三种状态：
1)Fulfilled:成功状态
2)Rejected：失败状态
3)Pending：既不是成功也不是失败状态，可以理解为进行中状态

Promise的缺点：
1.当处于未完成状态时，无法确定目前处于哪一阶段。
2.如果不设置回调函数，Promise内部的错误不会反映到外部。
3.无法取消Promise，一旦新建它就会立即执行，无法中途取消。

## async await

> async函数返回一个 Promise 对象，可以使用then方法添加回调函数。当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。

- async单独使用的时候，放在函数前面表示这个函数是一个异步函数，如果async函数有返回结果，必须要用.then()方法来承接（也就是返回的值会被自动处理成promise对象）

```js
async function bar() {
  return 'lee'
}
console.log(bar()); // Promise {<resolved>: "lee"}
```

- async await搭配使用的时候，await是等待此函数执行后，再执行下一个，可以把异步函数变成同步来执行，控制函数的执行顺序。await一定要搭配async使用。

> 当await后的函数是返回的promise。

```js
let foo = () => {
  return new Promise(resolve => {
    setTimeout(() => {
      console.log('lee');
      resolve();
    }, 1000);
  })
}
async function bar() {
  await foo();
  console.log('van');
}
bar(); // 隔1秒同时输出 lee van
```

> 当await 后跟的是普通函数（非promise()）

```js
let f1 = () => {
  setTimeout(() => {
    console.log('lee');
  }, 1000)
}

let f2 = () => {
  setTimeout(() => {
    console.log('van');
  }, 1000)
}

async function bar() {
  await f1();
  await f2();
  console.log('yeah');
}
bar(); // yeah 隔1秒同时输出 lee fan
```

## Event Loop 事件循环

> 参考链接：[详解JavaScript中的Event Loop（事件循环）机制](https://zhuanlan.zhihu.com/p/33058983?utm_source=wechat_session&utm_medium=social&utm_oi=859347813597863936)

```js
微任务: promise.then(不是promise，promise里是立即执行)   MutationObserver  process.nextTick(Node.js 环境)
宏任务: script(整体代码)  setTimeout  setInterval   I/O  setImmediate(Node.js 环境)   UI 交互事件
同一次事件循环中:  微任务永远在宏任务之前执行
```

事件循环的过程：
> 首先script脚本整体是一个大的异步任务，先执行script脚本。这个script脚本会包含同步任务和异步任务，同步任务会先在主线程上执行，异步任务（分为宏任务和微任务）会添加到任务队列中，任务队列分为宏任务队列和微任务队列。
>
> 当同步任务执行完毕后，此时的执行栈已经被清空，会去执行异步任务。此时会先从微任务队列中取一个微任务放到执行栈中执行，若有新的微任务或宏任务产生，添加到相应的任务队列中，循环往复，直至微任务队列清空。
>
> 紧接着会从宏任务队列取一个宏任务放到执行栈中执行，此时可能会产生新的微任务，将微任务放到微任务队列中，当这个宏任务执行完后会继续执行微任务队列，如果没有产生就继续执行下一个宏任务。循环往复，直至所有任务执行完毕。

```js
async function async1(){
    console.log('asy1 start');
    await async2();
    console.log("asy1 end");}
async function async2(){console.log("async2");}
async1();
setTimeout(()=>{console.log('timeout')},0);
new Promise(function(resolve){
    console.log("promise1");
    resolve();})
    .then(function(){console.log('promise2')});
console.log('script end');
//结果
//asy1 start
//async2
//promise1
//script end
//asy1 end
//promise2
//timeout
```



## Ajax 基本流程

> 原生 js 代码实现与基于 promise 实现请传送至专栏：[面试高频手撕代码题](./../08.面试高频手撕代码题/面试高频手撕代码题.md)

Ajax 即“Asynchronous Javascript And XML”（异步 JavaScript 和 XML），是指一种创建交互式网页应用的网页开发技术。是一种异步通信的方法，通过直接由 js 脚本向服务器发起 http 通信，然后根据服务器返回的数据，更新网页的相应部分，而不用刷新整个页面的一种方法。

创建一个 ajax 有这样几个步骤:

- 首先是创建一个 XMLHttpRequest 对象。

- 然后在这个对象上使用 open 方法创建一个 http 请求，open 方法所需要的参数是请求的方法、请求的地址、是否异步和用户的认证信息。

- 在发起请求前，我们可以为这个对象添加一些信息和监听函数。比如说我们可以通过 setRequestHeader 方法来为请求添加头信息。我们还可以为这个对象添加一个状态监听函数。一个 XMLHttpRequest 对象一共有 5 个状态，当它的状态变化时会触发 onreadystatechange 事件，我们可以通过设置监听函数，来处理请求成功后的结果。当对象的 readyState 变为 4 的时候，代表服务器返回的数据接收完成，这个时候我们可以通过判断请求的状态，如果状态是 2xx 或者 304 的话则代表返回正常。这个时候我们就可以通过 response 中的数据来对页面进行更新了。

- 当对象的属性和监听函数设置完成后，最后我们调用 sent 方法来向服务器发起请求，可以传入参数作为发送的数据体。

## Ajax 的 readyState 的几种状态分别代表什么

| 状态值 |           含义           |
| :----: | :----------------------: |
|   0    |       请求未初始化       |
|   1    |     服务器连接已建立     |
|   2    |        请求已接收        |
|   3    |        请求处理中        |
|   4    | 请求已完成，且响应已就绪 |

## Ajax 禁用浏览器的缓存功能

> 项目中，一般提交请求都会通过 ajax 来提交，我们都知道 ajax 能提高页面载入的速度主要的原因是通过 ajax 减少了重复数据的载入，也就是说在载入数据的同时将数据缓存到内存中，一旦数据被加载其中，只要我们没有刷新页面，这些数据就会一直被缓存在内存中，当我们提交 的 URL 与历史的 URL 一致时，就不需要提交给服务器，也就是不需要从服务器上面去获取数据，虽然这样降低了服务器的负载提高了用户的体验，但是我们不能获取最新的数据。为了保证我们读取的信息都是最新的，我们就需要禁止他的缓存功能。

解决的方法有：

1. 在 ajax 发送请求前加上 `xhr.setRequestHeader("If-Modified-Since","0")`。
2. 在 ajax 发送请求前加上 `xhr.setRequestHeader("Cache-Control","no-cache")`。
3. 在 URL 后面加上一个随机数： `"fresh=" + Math.random()`;。
4. 在 URL 后面加上时间搓：`"nowtime=" + new Date().getTime()`;。
5. 如果是使用 jQuery，直接这样就可以了`$.ajaxSetup({cache:false})`。这样页面的所有 ajax 都会执行这条语句就是不需要保存缓存记录。

## arguments怎么转化成真数组

- arguments是一个伪数组，是当前函数的内置对象，存储所有的形参，有length属性，但是不能用数组的方法。
- [...arguments] 扩展运算符的方式，拿取剩余参数
- Array.prototype.slice.call(arguments);使用call 一个对象调用另一个函数的方法，slice切割数组并返回一个新的数组
- [].slice.call() 因为[].slice === Array.prototype.slice
- 遍历：arguments有length属性，所以，可以遍历arguments取出每一个元素，并放进新的数组中

#### 递归setTimeout()和setInterval()有何不同

前者可以保证每次调用的间隔不变；后者包括了执行的时间

```js
async function timeTest() {
  await timeoutPromise(3000);
  await timeoutPromise(3000);
  await timeoutPromise(3000);
}
//总共9秒
async function timeTest() {
  const timeoutPromise1 = timeoutPromise(3000);
  const timeoutPromise2 = timeoutPromise(3000);
  const timeoutPromise3 = timeoutPromise(3000);

  await timeoutPromise1;
  await timeoutPromise2;
  await timeoutPromise3;
}//总共3秒
```



