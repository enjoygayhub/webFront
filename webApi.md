##  IntersectionObserver

创建一个新的`IntersectionObserver`对象，当其监听到目标元素的可见部分穿过了一个或多个**阈(thresholds)**时，会执行指定的回调函数。

```js
let observer = new IntersectionObserver((entries, observer) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('animate')
      observer.unobserve(entry.target)
    }
  })
})

document.querySelectorAll('mark').forEach(mark => {
  observer.observe(mark)
})
```
## navigator.getBattery()

获取设备电量信息

## AbortController 
取消异步信号
async function fetchWithTimeout(url, options = {}) {
  const { timeoutMS = 3000 } = options;

  return await fetch(url, {
    ...options,
    signal: AbortSignal.timeout(timeoutMS),
  });
}
