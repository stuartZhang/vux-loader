# vux-loader-stzhang

A webpack loader for processing .vue file before vue-loader

## Forking Reasons

相对于【官方版本】，此分支解决了“`npm`模块缓存失效·导致·每个`*.vue`文件被重复装载两次的`bug`”。无论是·静态模块`import from`·还是·动态模块`return import(***);`·，这个`bug`都是`100%`复现。下面是我给`vux-loader`原作者提交`PR`申请的描述。

> bug fix: Each "*.vue" file is loaded twice in runtime. The root reason is that the same ".vue" file is referenced twice with different ref paths.
>
> - The 1st ref path: ./node_modules/babel-loader/lib/index.js!./node_modules/vux-loader/src/script-loader.js **!./node_modules/vux-loader/src/script-loader.js**!./node_modules/vue-loader/lib/selector.js?type=script&index=0!./src/components/VuxDemo.vue
> - The 2nd ref path: ./node_modules/babel-loader/lib/index.js!./node_modules/vux-loader/src/script-loader.js!./node_modules/vue-loader/lib/selector.js?type=script&index=0!./src/components/VuxDemo.vue
>
> As you see, there are two ref paths for the identical "*.vue" file (i.e. ./src/components/VuxDemo.vue). What's worse, there are two duplicate fragments "**!./node_modules/vux-loader/src/script-loader.js**" in the first one, which is erroneous and brings about the module-cache failure. In my view, the 2nd ref path is correct.
>
> My change fixes it.

## My Own `vux-loader` plugin `vux-loader-stzhang`

考虑到我的`PR`被【官方团队】`code review`、采纳/合并、再发布新版本`vux-loader`的时间周期会比较长。所以，包含了该`bug fix`的`vux-loader-stzhang`独立插件已经被发布到[npmjs.org](https://www.npmjs.com/)。

## Installation

`npm i -D vux-loader-stzhang`

## Usage

```javascript
module.exports = require('vux-loader-stzhang').merge(webpackConfig, {
    options: {},
    plugins: [{
        name: 'vux-ui'
    }, {
        name: 'less-theme',
        path: 'src/assets/vux/theme/variables.less'
    }, {
        name: 'progress-bar'
    }, {
        name: 'duplicate-style'
    }]
});
```