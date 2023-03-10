<a name="zOWpz"></a>

# Scoped CSS

实际开发中，CSS 文件中的选择器是全局生效的，不同文件中的同名选择器，根据 build 后生成文件中的先后顺序，后面的样式会将前面的覆盖进而造成全局污染。<br />为了解决这个问题，[vue loader](https://vue-loader.vuejs.org/zh/guide/scoped-css.html#%E6%B7%B7%E7%94%A8%E6%9C%AC%E5%9C%B0%E5%92%8C%E5%85%A8%E5%B1%80%E6%A0%B7%E5%BC%8F) 对 `<style>` 增加了 `scoped`属性，Vue 中对样式的 `scoped` 处理基本原理很简单，就是对每个类名所在的 dom 元素按照一定规则进行转换，增加 `data-v-xx` 属性，并让编译后的样式表类名与 `data-x` 选择器合作来保证它的唯一性。
<a name="i8Dr3"></a>

## 只有父组件

如果在浏览器里查看这个示例的 dom 结构，你会发现实际渲染出来是这样的：

```vue
<template>
  <div class="app">
    <h1>我是父组件</h1>
  </div>
</template>

<style scoped>
.app {
  color: #2c3e50;
  margin-top: 60px;
  text-align: center;
}
</style>
```

```vue
<template>
  <div data-v-7ba5bd90 class="app">
    <h1 data-v-7ba5bd90>我是父组件</h1>
  </div>
</template>

<style>
.app[data-v-7ba5bd90] {
  color: #2c3e50;
  margin-top: 60px;
  text-align: center;
}
</style>
```

| ![image.png](https://cdn.nlark.com/yuque/0/2023/png/2399662/1678179912688-2f0c76f2-b75a-482a-a603-3ebeec6ba3fa.png#averageHue=%23fcfaf5&clientId=u0a90a542-25a6-4&from=paste&height=398&id=u976a2f54&name=image.png&originHeight=800&originWidth=782&originalType=binary&ratio=2&rotation=0&showTitle=true&size=237643&status=done&style=none&taskId=u7e911fe5-1943-4578-b4aa-4e227e68c3a&title=%E5%8F%AA%E6%9C%89%E7%88%B6%E7%BB%84%E4%BB%B6%20scoped%20%E8%BD%AC%E5%8C%96%E5%90%8E%20dom&width=389 "只有父组件 scoped 转化后 dom") |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

类名被自动添加了一个 hash 值的选择器，这保证了它的唯一性。
<a name="PnBAN"></a>

## 父组件 & 子组件

<a name="rvXBs"></a>

### 子组件的根元素

使用 `scoped` 后，父组件的样式将不会渗透到子组件中。不过一个子组件的根节点会同时受其父组件的 scoped CSS 和子组件的 scoped CSS 的影响。这样设计是为了让父组件可以从布局的角度出发，调整其子组件根元素的样式。

```vue
<template>
  <div id="app" class="app">
    <h1>我是父组件</h1>
    <HelloWorld />
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld";

export default {
  name: "App",
  components: {
    HelloWorld,
  },
};
</script>

<style scoped>
.app {
  color: #2c3e50;
  margin-top: 60px;
  text-align: center;
}

/* 未找到子组件内同类名元素 */
.text-red {
  color: chartreuse;
}
</style>
```

```vue
<template>
  <div class="hello">
    <h2>我是子组件默认色</h2>
    <h2 class="text-red">我是子组件红色</h2>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
};
</script>

<style>
.hello {
  background: #e3e3e3;
  padding: 10px 0;
}

.text-red {
  color: red;
}
</style>
```

| ![image.png](https://cdn.nlark.com/yuque/0/2023/png/2399662/1678241007243-7788527e-26a3-4ccd-848c-3988be533a3a.png#averageHue=%23f2f2f1&clientId=uad8f22e3-584e-4&from=paste&height=393&id=u643cfdfa&name=image.png&originHeight=786&originWidth=1533&originalType=binary&ratio=2&rotation=0&showTitle=true&size=366796&status=done&style=none&taskId=uce32ce09-f333-4d96-93a5-4d1daf851b2&title=%E5%AD%90%E7%BB%84%E4%BB%B6%E6%A0%B9%E8%8A%82%E7%82%B9%E5%B8%A6%20data-v-xxx%20%E5%B1%9E%E6%80%A7&width=766.5 "子组件根节点带 data-v-xxx 属性") |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

<a name="mHBLb"></a>

## 深度作用选择器

我们经常在开发中希望在当前页面修改 UI 组件库样式，如果需要在父组件中修改子组件样式，你可以使用 `>>>` 操作符来影响子组件：

```vue
<style scoped>
>>> .text-red {
  color: chartreuse;
}
</style>
```

上述代码将会编译成：

```vue
<style scoped>
[data-v-7ba5bd90] .text-red {
  color: chartreuse;
}
</style>
```

| ![image.png](https://cdn.nlark.com/yuque/0/2023/png/2399662/1678243556917-310bdd26-87ec-4fe4-a6a5-a1722d3e2ae6.png#averageHue=%23eff3ed&clientId=uad8f22e3-584e-4&from=paste&height=398&id=ue05a3e02&name=image.png&originHeight=796&originWidth=771&originalType=binary&ratio=2&rotation=0&showTitle=true&size=284241&status=done&style=none&taskId=u072a8de6-94a3-4cf1-be2d-c661ff508b3&title=%3E%3E%3E%20%E7%BC%96%E8%AF%91%E5%90%8E%E6%A0%B7%E5%BC%8F&width=385.5 ">>> 编译后样式") |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

有些像 Sass 之类的预处理器无法正确解析 `>>>`。这种情况下你可以使用 `/deep/` 或 `::v-deep` 操作符取而代之——两者都是 `>>>` 的别名，同样可以正常工作。
<a name="zFqtO"></a>

# 总结

- `scoped` 属性会给元素增加一个 hash 值属性，保证 hash 值选择器的唯一性。
- `>>>` 编译后可以给 css 选择器增加与元素相同的 hash 值选择器。
  <a name="RF1L0"></a>

# 参考

- [Vue Loader](https://vue-loader.vuejs.org/zh/guide/scoped-css.html#%E6%B7%B7%E7%94%A8%E6%9C%AC%E5%9C%B0%E5%92%8C%E5%85%A8%E5%B1%80%E6%A0%B7%E5%BC%8F)
- [Ant Design Pro of Vue](https://pro.antdv.com/docs/style)
- demo 地址：[codesandbox](https://codesandbox.io/s/nifty-torvalds-fkgoqq?file=/src/App.vue)
