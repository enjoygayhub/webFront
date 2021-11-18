# webpack

## webpack添加多个loader

1. use中使用数组，包含多个loader，或者对象 use: ['loader3', 'loader2', 'loader1']
2. rules中添加多个规则

`loader`的顺序: 从右到左, 从下到上：且都会执行

通过配置不同的参数改变`loader`的执行顺序，`pre` 前面的， `post`在后面的， `normal`正常

```js
{
    test: /\.js$/,
    use: ['loader1'],
    enforce: "pre"
},
{
    test: /\.js$/,
    use: ['loader2']
},
{
    test: /\.js$/,
    use: ['loader3'],
    enforce: "post"
},
```

```
loader带参数执行的顺序: pre -> normal -> inline -> post
```

引用时可以通过特殊符号禁用loader或选择loader

1. `!`禁用`normal-loader`  

   ```js
   import LeftLogo from '!../../images/svgIcons/chevron-left.svg';
   ```

2. `-!`禁用`pre-loader`和 `normal-loader`

3. `!!` 禁用`pre-loader`、`normal-loader`、`post-loader`,只能行内处理

   ```js
   const req = require.context('!!svg-sprite-loader!!../../images/svgIcons', false, /.svg$/)
   ```

   

