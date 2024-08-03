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
如果请求是非简单请求，浏览器会自动发送预检请求。
成功的预检请求会被浏览器缓存，通常缓存时间为 24 小时或 Access-Control-Max-Age 指定的时间。
在缓存有效期内，相同的非简单请求将不会再次触发预检请求。

3. script标签，在html解析过程中的执行顺序

默认情况下，脚本按顺序同步执行。
async 属性 使得脚本异步加载和执行，执行顺序不确定。
defer 属性 使得脚本异步加载，但在文档解析完成后按顺序执行。
内联脚本 按照在文档中的顺序执行。

4. /\d{1,6}?/  ​匹配000000,结果是什么​

?非贪心匹配原则，结果是0

5. 怎么实现 excludeUndefined，使 excludeUndefined<obj['name']> 的类型是 string 而不是 string | undefined

```ts
type obj = {​
  name?: string;​
}​

type excludeUndefined<T> =  ​​​T extends undefined ? never : T
​```


6. render Check 组件， 点击 button， executed 会不会打印
```jsx
function Check(){
    const [ toggle, setToggle ] = useState(false);
        return <div>
            <Button onClick={()=>{setToggle(!toggle)}} />
        </div>   
    
    function Button({ onClick }){
       console.log('executed')
        return <button onClick={onClick}>toggle</button>
    }
}
    
```

7. 怎么让外层元素，包裹两个元素，不溢出

<div>
  <div>
  xaxsaxasxsaxsaxsaxa
  </div> <div>xbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjkaxbasbxjksabxanbxjasnxjka
  </div>
</div>








