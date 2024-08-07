#  js-plus

### 隐式转换优先级

Symbol.toPrimitive -> valueOf/toString

1. 调用 obj[Symbol.toPrimitive](hint) 如果这个方法存在，则valueOf()和toString()失效,不存在则进行2，3，
2. 如果 hint 是 "string"
  ○ 尝试 obj.toString() 和 obj.valueOf()，无论哪个存在。
3. 如果 hint 是 "number" 或者 "default"
  ○ 尝试 obj.valueOf() 和 obj.toString()，无论哪个存在。
如果都不返回基础类型则报错；

另外 valueOf返回的是对象本身，因此除非自定义了它的返回值，否则会被忽略。
对象到原始值的转换，是由许多期望以原始值作为值的内建函数和运算符自动调用的。
期望（hint）这里有三种类型：
● "string"（如对于 alert 和其他需要字符串的操作）
● "number"（对于数学运算如一元+，移位运算，大于等于）
● "default"（少数运算符二元+，==）

```js
var obj = {
  [Symbol.toPrimitive](hint) {
    if (hint == "number") {
      return 10;
    }
    if (hint == "string") {
      return "hello";
    }
    return true;
  }
};

```
严等===是比较引用，不会触发隐式转换。
宽松等==， 如果只有一边是对象，会触发隐式转换。 否则，是等同于严等。



### {} 和 [] 的 valueOf 和 toString 的结果，转换为原始值

```js
{} 的 valueOf 结果为 {} ，toString 的结果为 "[object Object]"

[] 的 valueOf 结果为 [] ，toString 的结果为 ""
//于是有
[] +[] //as
[].toString() +  [].toString()
// '' + ''
// ''
[] + {}//as
[].toString() +  {}.toString()
// '' +  '[object Object]'
// '[object Object]'
{} + []// {}在前被解释为代码块，什么也不发生
+ []
// 0
```
### Function.name,Function.length,Function.caller,arguments.callee
+ Function.name 用于获得函数名;
  1 大多数情况包括函数声明式，匿名函数赋值给变量，.name都能取到函数名或变量名
```js
    function foo(){
      return 0;
    }
    foo.name;//foo
    foo=()=>{};
    foo.name;//foo
    "foo"
```
  2 被bind 之后的 函数名 ：name 值就是 "bound " + 函数名
  3 使用new Function创建函数，name值是anonymous
  ```js
  const addFn = new Function("num1", "num2", "return num1 + num2");
  addFn.name;// anonymous
  ```
+ Function.length 用于获取形参个数
  注：排除默认值，剩余参数，仅包含第一个具有默认值之前的参数个数
  bind 之后的length = 函数的length - bind 的参数个数，最小值为0;
  ```js
  function sum(num1, num2 = 1, num3) {
    return num1 + num2 + num3;
  }
    sum.length;//1
    const boundSum1 = sum.bind(null, 1);
    boundSum1.length;//0
  ```
  与arguments.length的区别，后者代表实参的长度。

+ Function.caller 可以获得调用该函数的函数，用于调用栈信息收集；
如果一个函数f是在全局作用域内被调用的,则f.caller为null,相反,如果一个函数是在另外一个函数作用域内被调用的,则f.caller指向调用它的那个函数.
非标准特性，严格模式下无效；

+ arguments.callee，用于递归调用匿名函数自身；
  非标准特性，严格模式下无效；

### 对象

常规属性：字符串作为键，elements

排序属性：数字或者数字字符串作为键 ，properties

内属性：前10个常规属性：直接存储在对象本身；

快属性：线性数据结构存储的属性，查找快，删除慢

慢属性：非线性结构存储的大量属性键

隐藏类：描述了对象的属性布局。map

### 实现完美深拷贝

```js
//自定义递归实现深拷贝
const isComplexDataType = (obj)=>(typeof obj === "object" || typeof obj === 'function')&&(obj!==null);
const deepClone = function (obj,hash=new WeakMap()){
    if(obj.constructor===Date) return new Date(obj)//新建日期对象
    if(obj.constructor===RegExp) return new RegExp(obj)//新建正则对象
    if(hash.has(obj)) return hash.get(obj) //如果发生循环引用，则不再递归
    let allDes = Object.getOwnPropertyDescriptors(obj) //得到所有键描述
    let cloneObj = Object.create(Object.getPrototypeOf(obj),allDes)//使克隆的新对象继承原对象的原型,且有源对象键的所有特性
    hash.set(obj,cloneObj)
    for (let key of Reflect.ownKeys(obj)){
        cloneObj[key] = (isComplexDataType(obj[key]) && typeof obj[key]!=='function')?deepClone(obj[key],hash):obj[key]
    }
    return cloneObj
}
```

### JavaScript 继承

- 原型链继承

  缺点：原型链上的引用对象共享，如果修改会导致全部实例改变

  ```js
  function Parent1(){this.name='parent1';this.play=[1,2,3];}
  function Child1(){this.type='child1';}
  Child1.prototype = new Parent1();//生成一个原型对象，让Child1类的原型指向它
  Child1.prototype.constructor = Child1;//后面解释
  console.log(new Child1()); //Child1 {type: "child1"}
  let s1 = new Child1();
  let s2 = new Child1();
  s1===s2  //false，两个实例不是同一个对象
  s1.play.push(4);   //可以访问play属性，原型链上的，且共享
  console.log(s1.play,s2.play);//(4) [1, 2, 3, 4] (4) [1, 2, 3, 4]
  ```

- 构造函数继承（借助call）。

  ```js
  function Parent1(){this.name='parent1';this.play=[1,2,3];};
  Parent1.prototype.getName = function(){return this.name};
  function Child2(){ 
      Parent1.call(this);
      this.type='child2';
  }
  let child2 = new Child2(); //实例中改变paly数组不会影响其他实例和父类
  //Child2 {name: "parent1", type: "child2"};
  ```

  优点：通过call相当与让子类执行了父类中的属性赋值操作，且子类属性引用属性不再共享；因此原型链上没有父类，不能访问父类原型链上的属性和方法

- 组合继承

  ```js
  function Parent1(){this.name='parent1';this.play=[1,2,3];};
  Parent1.prototype.getName = function(){return this.name};
  function Child3(){ 
      Parent1.call(this);
      this.type='child3';
  } // 以上步骤和上诉构造函数继承相同
  Child3.prototype = new Parent1(); //将原型链接上，此时Child3的原型对象为Parent1的实例，该实例的原型对象上定义了getName，通过2层原型链，可以访问
  
  Child3.prototype.constructor = Child3; //手动挂上构造器，指向自己；
  //解释：Child3类的原型对象中的构造器必须指向自己，此处的原型对象为Parent1的实例，其构造函数指向Parent1，所以需要将其手动指向Child3
  ```

  通过构造函数的方式来实现类型的属性的继承，通过将子类型的原型设置为超类型的实例来实现方法的继承。这种方式解决了上面的两种模式单独使用时的问题，

  以父类的实例来作为子类型的原型，调用了两次父类Parent1的构造函数，造成了额外性能开销，以及原型和子类中有重复的属性。

- 原型式继承

   Object.create(prototype ， ?extraArgs) 实现。

  ```js
  let parent4 = {name:'parent4',
  				friends:[1,2,3],
  				getName : function(){return this.name}};
  let child4 = Object.create(parent4);//{},child4为空，但可以访问parent4中的属性
  child4.__proto__ === parent4 //true
  ```

  缺点与原型链方式相同，共享。

- 寄生式继承

  ```js
  let parent5 = {name:'parent5',
  				friends:[1,2,3，4],
  				getName : function(){return this.name}};
  
  function clone(original){
      let cloneObj = Object.create(original);
      cloneObj.getFriends = function(){return this.friends;};
      return cloneObj;
  }
  let child5 = clone(parent5);//{getFriends：f()}
  child5.__proto__ === parent5 //true
  //对比上面的原型式，多了一个自定义属性。
  ```

  寄生式继承的思路是创建一个用于封装继承过程的函数，通过传入一个对象，然后复制一个对象的副本，然后对象进行扩展，最后返回这个对象。这个扩展的过程就可以理解是一种继承。这种继承的优点就是对一个简单对象实现继承，如果这个对象不是我们的自定义类型时。缺点是没有办法实现函数的复用。

- 寄生式组合继承（最优）

  组合继承的缺点就是使用超类型的实例做为子类型的原型，导致添加了不必要的原型属性。寄生式组合继承的方式是使用超类型的原型的副本来作为子类型的原型，这样就避免了创建不必要的属性。

  ```js
  function clone(parent,child){
      //这里让child的原型直接指向父类原型的副本
      child.prototype = Object.create(parent.prototype);
      child.prototype.constructor =child;
  }
  function Parent6(){this.name = 'parent6';this.play=[1,2,3]};
  Parent6.prototype.getName=function(){return this.name;}
  
  function Child6(){
  Parent6.call(this);  //借用构造函数，子类中添加父类属性
  this.friends = 'child5'; //添加自身独有属性
  }
  clone(Parent6,Child6);  //完成原型链，且跳过了父类，直接链到父类原型的副本
  
  ```

  总结：不使用Object.create 包括原型链继承和构造函数继承，两种组合为组合继承

  ​		使用Object.create 包括原型式继承和寄生式继承

  ​		最终改造为寄生组合继承与ES6中的 extend关键字基本相同


### 实现primisfy

参考[简单实现](https://juejin.cn/post/6844904024928419848)
promisify(fn, reverse)

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

### 柯里化和偏函数
1. 柯里化是将一个多参数转换为单参数的函数，将一个N元函数转换为N个一元函数。
2. 偏函数是固定一部分参数（一个或者多个参数），将 一个N元函数转换成一个N-X函数。用于延迟执行
3. 反柯里化的作用就是扩大适用性，使原来作为特定对象所拥有的功能的函数可以被任意对象所用

```js
// 一个求和的柯里化
const curry = function (fn) {
    let curArgs = [];
    return function () {
        if (arguments.length === 0) {
            return fn.apply(this, curArgs);
        }
        curArgs = curArgs.concat([...arguments]);
        return arguments.callee;
    }
};

function calcSum() {
    return [...arguments].reduce((pre, value, index) => { return pre + value }, 0);
}

const add = curry(calcSum);

console.log("执行添加：", add(2, 3)(5)(8));

console.log("手动调用：", add())

// 一个偏函数实现的
function partial(fn) {
    const args = [].slice.call(arguments, 1);
    return function () {
        const newArgs = args.concat([...arguments]);
        return fn.apply(this, newArgs);
    };
};

const pCalcSum = partial(calcSum, 10);

console.log(pCalcSum(11, 12));
console.log(pCalcSum(15, 20));

// 反科里
function unCurry(fn) {
    return function (context) {
        return fn.apply(context, Array.prototype.slice.call(arguments, 1));
    }
}

// 反柯里化
const toString = unCurry(Object.prototype.toString);
toString({});
toString(() => { });
toString(1);
```

### 执行上下文 调用栈，作用域

+ 执行上下文 为可执行代码块提供了的必要准备工作，比如变量对象的定义、作用域链的扩展、提供调用者的对象引用等信息。
包括this，变量环境（var)，词法环境（let， const），外部环境

+ 调用栈是用于记录我们在程序中的位置。当执行进入一个函数，把它置于栈的顶部。如果从函数中返回则从栈顶部移除函数。
+ 作用域： 一个独立的区域。主要的用途就是隔离变量。
函数的作用域在函数创建时就已经确定了，而不是在函数调用时确定。
1. 全局作用域在任何函数外或者代码块之外的顶层作用域就是全局作用域，而里面的变量成为全局变量
2. 在函数内的作用域就是函数作用域,函数内部的变量，在全局作用域或者块级作用域中，都无法访问。只能在函数内部才可以访问。所以函数内的变量也称为局部变量。

### es6中类的继承 
```js
class B extends A {};

B.__proto__ === A; //B的原型为A;
B.prototype.__proto__ === A.prototype; // 相当于B的实例经过2层原型链路找到A：b.__proto__.__proto__ === A.prototype;
```
使用extends实现继承时：父类中的箭头函数和非函数类的属性不会存在原型链中，子类无法从原型链上访问到

经典题：void (0) === void 0 === undefined === void(0); 

void () ; //Uncaught SyntaxError: Unexpected token ')' ()无法解析

[NaN].indexOf(NaN) === -1；// true NaN不与任何相等，所以indexOf判断不了；

arr.includes(NaN) // true ,ES6的includes做了特殊处理，可以判断

[-0].includes(+0) // true，故意不区分+0,-0，反正都是0

[BigInt(+0)].includes(BigInt(-0))  // false，能区分大数0

### 自定义事件

实现自定义事件方式有三种
1. document.createEvent (废弃)
2. new Event()
3. new CustomEvent()
```js

// bubbles: 表示该事件是否冒泡
// cancelable: 表示该事件能否被取消
var myEvent = new Event("event1", { "bubbles": true, "cancelable": false });
// CustomEvent创建事件,比Event多一个传入参数detail
let myCustomEvent = new CustomEvent("event2", {
    "bubbles": true, "cancelable": false,
    detail: { des: "开始事件描述" },
  });
function listener(ev) {
    console.log("收到事件", ev.type);
}
//在document 上添加监听事件
document.addEventListener("event1", listener);
document.addEventListener("event2", listener);
setTimeout(() => {
    //触发event1 事件
    document.dispatchEvent(myEvent);
    //触发event2事件
    document.dispatchEvent(myCustomEvent);
    ////移除start 监听
    document.removeEventListener("event1", listener);
}, 2000);
// 事件可以在任何元素触发，不仅仅是document
```

### 发布订阅
```js
// 自定义事件+回调版本
window._on = window.addEventListener;
window._off = window.removeEventListener;
window._emit = (type, data) => window.dispatchEvent(new CustomEvent(type, { detail: data }));
window._once = (type, callback) => window.addEventListener(type, callback, { once: true, capture: true });

// 可实例传参版本
        class EventEmitter extends EventTarget {
            on = (type, listener, options) => this.addEventListener(type, function wrap(e) {
                return (listener.__wrap__ = wrap, listener.apply(this, e.detail || []))
            }, options)

            off = (type, listener) => this.removeEventListener(type, listener.__wrap__);

            emit = (type, ...args) => this.dispatchEvent(new CustomEvent(type, { detail: args }));

            once = (type, listener) => this.on(type, listener, { once: true, capture: true });
        }

```

### ES6 Promise 注意事项

异常捕获可以通过 Promise catch 捕获，或在window上监听 unhandledrejection 事件捕获。
在then中以第二参数传入处理reject后，不影响后续then的执行
then中resolve之后 再报错throw new Error，则resolve无效，
then传入非函数，则当前then无效，且穿透。
Promise.all,Promise.race等 有实例reject以后，其他的Promise实例并没有停止

### Error对象

重要的属性：
● name
new Error().name      // Error
new TypeError().name  // TypeError
● message
  错误文本消息
● stack
  错误堆栈信息。

RangeError：当一个值不在其所允许的范围或者集合中。
ReferenceError：一个不存在的变量被引用时发生的错误
SyntaxError：解析语法上不合法的代码的错误。
TypeError：值的类型非预期类型时发生的错误。
URIError：URI处理函数而产生的错误。

捕获错误：
1. try catch
2. window.onerror 捕获全局错误
3. window.addEventListener('error') 全局错误和静态资源加载错误
4. unhandledrejection 未捕获的promise错误