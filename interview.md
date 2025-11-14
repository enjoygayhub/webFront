# 1 Main

## LBA
1. 如何处理前后端跨域问题,

CORS：
设置 Access-Control-Allow-Origin 响应头，可以是特定域名或通配符 *。
如果需要支持带有自定义头部的请求，还需要设置 Access-Control-Allow-Headers。
如果需要支持 POST、PUT 等非简单请求，还需要设置 Access-Control-Allow-Methods。
代理：
通过服务转发，绕过浏览器的跨域限制

2. post预检请求每次都会发吗

不会
预检请求是 CORS 机制的一部分，用于确保跨域请求符合安全策略。
它们在非简单请求和包含自定义请求头的情况下自动发送。
预检请求帮助确保客户端和服务器之间的通信符合 CORS 规则。
成功的预检请求会被浏览器缓存，通常缓存时间为 24 小时或 Access-Control-Max-Age 指定的时间。
在缓存有效期内，相同的非简单请求将不会再次触发预检请求。

3. script标签，在html解析过程中的执行顺序

默认情况下，脚本按顺序同步执行。
async 属性 使得脚本异步加载和执行，执行顺序不确定。
defer 属性 使得脚本异步加载，但在文档解析完成，在 DOMContentLoaded 事件触发之前执行。后按顺序执行。
内联脚本 按照在文档中的顺序执行。

4. /\d{1,6}?/  ​匹配000000,结果是什么​

?非贪心匹配原则，结果是0

5. 怎么实现 excludeUndefined，使 excludeUndefined<obj['name']> 的类型是 string 而不是 string | undefined

```ts
type obj = {​
  name?: string;​
}​

type excludeUndefined<T> =  ​​​T extends undefined ? never : T
```


6. render Check 组件， 点击 button， executed 会不会打印
```jsx
function Check(){
    const [ toggle, setToggle ] = useState(false);
        return <div>
            <Button onClick={()=>{setToggle(t=>!t)}} />
        </div>   
}    
    function Button({ onClick }){
       console.log('executed')
        return <button onClick={onClick}>toggle</button>
    }
// 优化如下
function App() {
  const [count, setCount] = React.useState(false);
  const click = React.useCallback(() => {
          setCount((c) => !c);
        })
  const MyButton = React.memo(({ click })=>{
        console.log('executed')
        return <button onClick={click}>toggle</button>
  })
  return (
    <div className="App">
      <MyButton onClick={click}>
      </MyButton>
    </div>
  );
}
```



7. 怎么让外层元素，包裹两个元素，不溢出
  
<div>
  <div>
  xaxsaxasxsaxsaxsaxa
  </div> <div>xbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjka
  </div>
</div>

word-wrap: break-word;//允许长单词换行



## 贝壳
setState后发生了什么

### webpack原理，流程
初始化阶段：

初始化参数：从配置文件、 配置对象、Shell 参数中读取，与默认配置结合得出最终的参数
创建编译器对象：用上一步得到的参数创建 Compiler 对象
初始化编译环境：包括注入内置插件、注册各种模块工厂、初始化 RuleSet 集合、加载配置的插件等
开始编译：执行 compiler 对象的 run 方法
确定入口：根据配置中的 entry 找出所有的入口文件，调用 compilition.addEntry 将入口文件转换为 dependence 对象


构建阶段：

编译模块(make)：根据 entry 对应的 dependence 创建 module 对象，调用 loader 将模块转译为标准 JS 内容，调用 JS 解释器将内容转换为 AST 对象，从中找出该模块依赖的模块，再 递归 本步骤直到所有入口依赖的文件都经过了本步骤的处理

完成模块编译：上一步递归处理所有能触达到的模块后，得到了每个模块被翻译后的内容以及它们之间的 依赖关系图


生成阶段：

输出资源(seal)：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会
写入文件系统(emitAssets)：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统


webpack优化
1. 构建用时分析
2. resolve配置
3. externals 配置
4. 缩小范围
5. noParse
6. ignorePlugin
7. 多进程配置
8. 配置缓存AST

sass编译css过程
1. 解析 Less 文件
词法分析: 将 Less 代码拆分成一个个有意义的词法单元（token），如变量名、属性名、函数名等。
语法分析: 根据 Less 语法规则，将词法单元组合成语法树（抽象语法树 AST）。AST 是对 Less 代码结构的一种表示，方便后续的处理。
2. 变量替换
遍历 AST，查找所有变量引用。
根据变量定义，将变量引用替换为对应的值。
处理变量嵌套和递归引用。
3. 混合（Mixin）展开
找到所有混合调用。
根据混合定义，将混合的内容插入到调用处。
处理混合的参数传递和默认值。
4. 嵌套规则展开
将嵌套的规则展开为扁平的规则集。
处理嵌套选择器和继承。
5. 计算表达式
遍历 AST，查找所有的表达式（如颜色计算、单位转换等）。
使用 JavaScript 引擎计算表达式的值。
将计算结果替换回表达式所在的位置。
6. 生成 CSS
根据处理后的 AST，生成符合 CSS 语法的字符串。
处理 CSS 的输出格式，如缩进、注释等。
7. 输出 CSS 文件
将生成的 CSS 字符串写入到指定的文件中。

4. fiber原理
在 React 16 之前，React 的协调过程是不可中断的，如果组件树非常庞大，更新过程可能会导致页面卡顿。而 Fiber 的引入，使得 React 的协调过程变得可中断和可优先级。

Fiber 的主要作用
可中断的渲染: Fiber 将渲染工作拆分成更小的单元，每个单元称为 Fiber。这些 Fiber 可以被中断，让出主线程，从而避免长时间的渲染阻塞。
优先级调度: Fiber 引入了优先级概念，不同的更新可以有不同的优先级。高优先级的更新会被优先处理，保证用户交互的流畅性。
异步渲染: Fiber 使得渲染过程可以异步进行，从而避免主线程阻塞，提升用户体验。
更好的错误边界: Fiber 改善了错误处理机制，使得错误可以被更精准地捕获和隔离，防止整个应用崩溃。
支持渐进式渲染: Fiber 支持渐进式渲染，可以先渲染重要的部分，然后再渲染不太重要的部分，提升首屏加载速度。

Fiber 的工作原理
Fiber 节点: Fiber 节点是 React 的一个内部数据结构，它代表了虚拟 DOM 中的一个元素。
Fiber 树: React 会将虚拟 DOM 转换成 Fiber 树，Fiber 树中的每个节点都包含了该节点的各种信息，如类型、属性、子节点等。
双缓存: Fiber 采用双缓存机制，在内存中维护两棵 Fiber 树，一棵是当前正在使用的树，另一棵是正在构建的新树。
工作循环: React 通过一个工作循环来遍历 Fiber 树，进行 diff 和更新。这个工作循环是可以被中断的，当主线程空闲时，React 会继续执行工作循环。


diff算法
hooks性能优化
1. 使用 useMemo 和 useCallback
2. 有条件地渲染组件
3. 使用 useRef ，避免在每次渲染时创建新的对象
4. 批量更新状态，可以减少不必要的渲染。
5. 使用 React.memo 和 shouldComponentUpdate
6. 使用 useEffect 控制副作用
7. 使用 useContext
8. 代码分割和懒加载 lazy(()=>import())
9. 减少渲染层级

## DD

### 返回值
typeof Array.prototype //object
typeof Function.prototype //function
typeof Object.prototype //object

### 任务顺序
process.nextTick() 最优先执行的，当前轮次的事件循环结束之前被调用。在当前同步代码执行完成后立即执行的。
promise.then()  在所有同步代码和 process.nextTick() 执行完后，但在宏任务之前执行
setImmediate() 红任务，在timeOut之前
setTimeout()

### 异步顺序
```js
function async1(){
  console.log(async1 start!)
  await new Promise(resolve=>{
    console.log("promise!")
  })
  console.log("promise success")
  return "async1 end"
}
console.log("script start!");
async1().then(res=>{
  console.log(res)
})
console.log("script end!");
```

### 状态码
206 Partial Content
200 from cache
304 not modify


### 写findIndex
```js
    Array.prototype.findIndex = function (predicate) {
        'use strict';
        if (this == null) {
            throw new TypeError('Array.prototype.findIndex called on null or undefined');
        }
        if (typeof predicate !== 'function') {
            throw new TypeError('predicate must be a function');
        }
        var list = Object(this);
        var length = list.length >>> 0;
        var thisArg = arguments[1];
        var value;

        for (var i = 0; i < length; i++) {
            value = list[i];
            if (predicate.call(thisArg, value, i, list)) {
                return i;
            }
        }
        return -1;
    };
```

## 其他
React lazy 是怎么实现得

面对大流量项目，前端的容灾措施

监控线上性能指标

倒计数组件如何保持准确

## 前端国际化需要做什么
Intl是处理国际化相关内容i18n的底层API，可选使用。
### 排版布局
阅读顺序和流式布局对应排版的inline轴和block轴

left = inline-start
right = inline-end
top = block-start
bottom = block-end

阅读顺序和流式布局可以不同。dir: rtl 语言的阅读顺序，writing-mode: horizontal-tb 决定了文本的布局方向。

宽高vw和vh也被取代，用 vi(viewport inline) 和 vb(viewport block)替代

### 语言
lang="en"
"en" 代表英语，"es" 代表西班牙语。
希伯来语和阿拉伯语是从右到左阅读的，中文和英语是从左到右阅读的。
 
### 地区
locale="en-US" 语言+地区+文化
"US" 表示美国，"CN" 表示中国. 还有常见的如「zh-CN, en-US, en-GB等」
CN是国家地区码
zh-CN是语言地区码,
### 货币
比如人民币是 ¥ , 美元是 $ , 欧元 € , 英镑 £
用elvish
### 日期和时区

UTC = 「协调世界时（UTC: Coordinated Universal Time）- 由原子钟提供」
DST (Daylight saving time)，日光节约时，夏令时/冬令时等等名称。「它会在每年春天的某一天将时钟向后拨一小时，又在秋天的某一天将时钟向前拨动一个小时。」
周一是哪一天有不同，星期形式不同
日期展示年月日形式不同
日历展示不同
### 数字
英文单复数
数字格式化小数点不同
比较级不同
百分比

### rtl问题
  1 unicode-bidi + direction 强制方向，例如：style="direction: ltr; unicode-bidi: embed;"
  2 用 Unicode 控制字符 LTR，包裹元素ltr，例如：&#x200E;&lt;&#x200E; 
  3 用 <bdi> 标签隔离（适合混合方向文本），例如：
  النطاق السعري: <bdi>50 &lt; 150</bdi> دولار