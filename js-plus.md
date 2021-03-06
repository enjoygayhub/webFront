#  js-plus

## 实现深拷贝

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

## JavaScript 继承

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
  let child5 = Object.create(parent5);//{getFriends：f()}
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

	## 实现new call apply bind

```js
function _new(ctor,...args) {
	if(typeof ctor !== 'function') {
		throw 'ctor must be a function';}
	let obj = new Object();
	obj.__proto__ = Object.create(ctor.prototype);
	let res = ctor.apply(obj,...args);
	let isObject = typeof res === 'object' && typeof res !== null;
	let isFunction = typeof res === 'function';
	return isObject || isFunction ? res : obj;
}；
```

```js
Function.prototype.call = function (context, ..args) {
var context = context | window; 
context.fn = this;
var result = eval('context.fn(...args)'); 
delete context.fn
return result;
}
Function.prototype.apply = function (context, args) {
let context = context | window;
context.fn = this;
let result = eval('context.fn(...args)'); 
delete context.fn
return result;}
```

```js
Function.prototype.bind = function (context,...args) {
if (typeof this !== "function"){
throw new Error("this must be a function");
}
var self = this;
var fbound = function () {
self.apply(this instanceof self ? this : context,
args.concat(Array.prototype.slice.cal(arguments)));
}
if(this. prototype) { 
fbound.prototype = Object.create(this. prototype);
}
return fbound;
}
```

## 实现add(1)(2,3)(4,5,6)

```js
function add(...args){
    let arr =args;
    function fn(...newArgs){
        arr=[...args,...newArgs];
        return fn;
    }
    fn.toString=fn.valueOf=function(){
        return arr.reduce((acc,cur)=> acc+parseInt(cur))
    }
    return fn;
}
```



###  [重排与重绘，提升性能。](http://www.ruanyifeng.com/blog/2015/09/web-page-performance-in-depth.html)

 [defer与async](https://segmentfault.com/a/1190000006778717?utm_source=sf-related)

+ async 无序异步解析，会阻塞

+ defer 异步延迟

[事件代理](https://davidwalsh.name/event-delegate)

[浮点数的存贮表示](https://zhuanlan.zhihu.com/p/75581822)

[cookies,session](https://cnblogs.com/moyand/p/9047978.html)

[跨域错误捕获](https://www.jianshu.com/p/315ffe6797b8)

## 变量提升：

+ 函数声明优先与变量声明，函数声明可被覆盖
+ 匿名函数不提升，不继承Function原型

[三次握手和4次挥手详解](https://www.zhihu.com/search?type=content&q=%E8%AF%B4%E4%B8%80%E8%AF%B4TCP%E7%9A%84%E4%B8%89%E6%AC%A1%E6%8F%A1%E6%89%8B)

[cdn加速](https://zhuanlan.zhihu.com/p/82793949)  ： 最近结点内容分发

[输入url后发生了什么](https://segmentfault.com/a/1190000014872028) 

1. 逐级寻找缓存对应ip地址,dns服务器找

2. 建立tcp连接，请求资源，服务端判断缓存时间，可选择返回304

3. 下载网页，渲染：生成DOM树，遇到js文件会解释执行阻塞渲染过程，图片和css异步加载，css规则树与dom合并成render渲染树，布局绘制。

## [页面性能提升](https://www.jianshu.com/p/d9c20eafa67e)

   1. 减少HTTP资源请求次数,使用可缓存的AJAX：使用静态资源CDN来存储文件：推荐使用异步JavaScript资源;消除阻塞渲染的CSS及JavaScript：
   2. 减少DOM元素数量和深度：减少使用关系型样式表的写法：
   3. 合理利用浏览器缓存：图片懒加载：使用iconfont代替图片图标：
   4. 尽量使用id;合理缓存DOM对象：
   5. 避免各种形式重排重绘：
# [响应式布局](https://juejin.im/post/5caaa230e51d452b672f9703)

- 设置viewport
- 媒体查询
- 字体的适配（字体单位）
- 百分比布局
- 图片的适配（图片的响应式）
- 结合flex，grid，BFC，栅格系统等已经成型的方案
# es6中类的继承
class B extends A {};
B.__proto__ ===A;
B.prototype.__proto__ === A.prototype;

## [数据结构数组与类数组](http://www.360doc.com/content/18/0925/05/3175779_789416619.shtml)

# Css

[浮动元素的特点](https://juejin.cn/post/6844903891155288072)

[外边距溢出问题](https://juejin.cn/post/6844904033509965831) [边距折叠问题](https://juejin.cn/post/6844903497045917710)

## js渲染10w数据优化

[虚拟列表](https://blog.csdn.net/weixin_39932611/article/details/110746868?utm_medium=distribute.pc_relevant.none-task-blog-baidujs_baidulandingword-1&spm=1001.2101.3001.4242)

