# vux-loader-stzhang

A webpack loader for processing .vue file before vue-loader

## Forking Reasons

相对于【官方版本】，此分支解决了“`*.vue`文件被装载两次的`bug`”。无论是·静态模块`import from`·还是·动态模块`return import(***);`·，这个`bug`都是`100%`复现。下面是我给`vux-loader`原作者提交`PR`申请的描述。

> bug fix: Each "*.vue" file is loaded twice in runtime. The root reason is that the same ".vue" file is referenced twice with different ref path.
>
> - The 1st ref path: ./node_modules/babel-loader/lib/index.js!./node_modules/vux-loader/src/script-loader.js!./node_modules/vux-loader/src/script-loader.js!./node_modules/vue-loader/lib/selector.js?type=script&index=0!./src/components/VuxDemo.vue
> - The 2nd ref path: ./node_modules/babel-loader/lib/index.js!./node_modules/vux-loader/src/script-loader.js!./node_modules/vue-loader/lib/selector.js?type=script&index=0!./src/components/VuxDemo.vue
>
> As you see, there are two ref paths for the identical "*.vue" file (e.g. ./src/components/VuxDemo.vue). What's worse, there are two fragments "!./node_modules/vux-loader/src/script-loader.js" in the first one, which is erroneous and brings about the module-cache failure.
>
> My change fixes it.
