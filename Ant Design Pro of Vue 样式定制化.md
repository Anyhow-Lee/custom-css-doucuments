# 业务背景
开发中绕不开修改UI组件样式，本篇以 [vue-balck-dashboard-pro](https://www.creative-tim.com/product/vue-black-dashboard-pro?_ga=2.27510777.470527024.1678074211-190161301.1677479562)** 菜单栏**为例记录一下如何在 AntD Pro Vue 中一步步完成样式修改。
# 实践
## 准备

- 对照demo [vue-black-dashboard-pro](https://www.creative-tim.com/product/vue-black-dashboard-pro?_ga=2.27510777.470527024.1678074211-190161301.1677479562)
- 使用框架 [ant-design-pro-vue](https://pro.antdv.com/docs/getting-started)
## 第一步：分析不同
修改样式之前，首先要知道需要修改哪些地方。对比可以看出此次需要修改的主要有：

- 头部导航与左侧菜单栏布局
- 左侧菜单栏背景色、展开宽度、圆角
- 左侧菜单栏图标及字体大小、菜单选中状态、子菜单展开、间距
| ![image.png](https://cdn.nlark.com/yuque/0/2023/png/2399662/1678095569851-35dc36e9-c299-4546-93f4-0a784499ca06.png#averageHue=%2326283a&clientId=u7bac6461-d802-4&from=paste&height=468&id=u36f67b52&name=image.png&originHeight=936&originWidth=1902&originalType=binary&ratio=2&rotation=0&showTitle=true&size=351230&status=done&style=none&taskId=u9dd422ef-5740-4340-ace6-4e0074872bb&title=vue-black-dashboard-pro&width=951 "vue-black-dashboard-pro")
![image.png](https://cdn.nlark.com/yuque/0/2023/png/2399662/1678095862732-14afe830-a462-4b5a-b9b1-6d63e86380b7.png#averageHue=%23f0f8ed&clientId=u7bac6461-d802-4&from=paste&height=466&id=u5fa719df&name=image.png&originHeight=932&originWidth=1901&originalType=binary&ratio=2&rotation=0&showTitle=true&size=619985&status=done&style=none&taskId=u977bc948-fb34-4b37-a46d-adf51765fcd&title=ant%20design%20pro%20of%20vue&width=950.5 "ant design pro of vue") |
| --- |

## 第二步：全局改动变量
在 `vue.config.js` 使用 [modifyVars](http://lesscss.org/usage/#using-less-in-the-browser-modify-variables) 的方式来进行覆盖变量，这里可以对任何 antd 已经定义好的 [变量](https://github.com/vueComponent/ant-design-vue/blob/master/components/style/themes/default.less) 进行全局修改。在文档中搜索你需要关注的组件来获取可修改变量名。

| ![image.png](https://cdn.nlark.com/yuque/0/2023/png/2399662/1678344457266-3b88d01f-59bd-4570-8705-0f51ccd79448.png#averageHue=%23fdfbfa&clientId=u662097eb-822c-4&from=paste&height=674&id=u857236dc&name=image.png&originHeight=1348&originWidth=1230&originalType=binary&ratio=2&rotation=0&showTitle=true&size=302527&status=done&style=none&taskId=ucb203b63-7c25-4021-bad6-cb74a3f080a&title=default.less&width=615 "default.less") |
| --- |

```javascript
  css: {
    loaderOptions: {
      less: {
        modifyVars: {
          // less vars，customize ant design theme
          // 'primary-color': '#F5222D',
          // 'link-color': '#F5222D',
          'border-radius-base': '5px',
          'layout-body-background': '#1e1e2f',
          'layout-header-background': '#1e1e2f',
          'layout-trigger-background': '#1e1e2f',
          'component-background': '#27293d',
          'menu-item-active-bg': 'transparent',
          'menu-icon-size': '20px',
          'menu-item-font-size': '.62475rem'
        },
        // DO NOT REMOVE THIS LINE
        javascriptEnabled: true
      }
    }
  },
```
| ![image.png](https://cdn.nlark.com/yuque/0/2023/png/2399662/1678375793444-00759214-60ad-4608-a807-6644baed2f6f.png#averageHue=%23252739&clientId=u662097eb-822c-4&from=paste&height=828&id=u73833fc3&name=image.png&originHeight=1656&originWidth=2534&originalType=binary&ratio=2&rotation=0&showTitle=true&size=498724&status=done&style=none&taskId=u48d6a5aa-cdf4-4c04-abcf-1efd1caabb4&title=%E5%AE%8C%E6%88%90%E7%AC%AC%E4%BA%8C%E6%AD%A5&width=1267 "完成第二步") |
| --- |

修改完变量也许还不能看出明显修改，是因为 antd 暴露的变量是有限的，还有部分样式修改无法通过变量修改，只能通过样式覆盖。
## 第三步：样式覆盖
对于自定义样式，可以在 `src/global.less` 中修改，也可以自定义文件在 `main.js` 中引入。本次自定义 `custom.less` 来覆盖菜单样式，这样可以和原样式区分开来。
样式覆盖操作也不难，只需要细心、耐心一点。在对照demo中审查元素，找出需要修改的属性，接着在 antd 元素审查中找出需要修改的类名，copy下来进行样式重写。此处仅展示部分代码，完整代码见文末仓库地址。
```less
// 修改位置布局
.ant-layout-sider {
  position: fixed;
  top: 0x;
  min-height: calc(100vh - 90px);
  background: linear-gradient(0deg, #389466, #42b883);
  margin-left: 20px;
  border-radius: 5px;
  margin-top: 82px;
  box-shadow: 0 0 45px 0 rgb(0 0 0 / 60%);

  &::before {
    content: '';
    position: absolute;
    border-left: 5px solid transparent;
    border-right: 5px solid transparent;
    border-bottom: 5px solid #2da979;
    top: -5px;
    left: 40px;
    transform: translate(-50%);
  }
}

// logo修改
.ant-pro-sider-menu-logo {
  background: transparent;
  padding: 0.5rem 0.7rem 1rem;
  line-height: 30px;
  height: auto;

  img {
    width: 34px;
    max-width: 34px;
    text-align: center;
    margin-left: 12px;
    margin-right: 12px;
  }

  h1 {
    font-weight: 400;
    font-size: 0.875rem;
    padding: 0.5rem 0;
    margin: 0;
    text-transform: uppercase;
  }

  &::after {
    content: '';
    position: absolute;
    bottom: 0;
    right: 15px;
    height: 1px;
    width: calc(100% - 30px);
    background: hsla(0, 0%, 100%, 0.5);
  }
}
```
| ![image.png](https://cdn.nlark.com/yuque/0/2023/png/2399662/1678378449633-95af065b-b15e-448e-8787-574cc6d09856.png#averageHue=%2326283a&clientId=u662097eb-822c-4&from=paste&height=828&id=u49a75e9b&name=image.png&originHeight=1656&originWidth=2528&originalType=binary&ratio=2&rotation=0&showTitle=true&size=423707&status=done&style=none&taskId=u4543effb-54dd-4448-ada5-a06be73b87f&title=%E5%AE%8C%E6%88%90%E7%AC%AC%E4%B8%89%E6%AD%A5&width=1264 "完成第三步") |
| --- |

到这一步基本样子已经有了，还有些细节如菜单栏宽度、菜单展开收起等没有完成。
## 第四步：组件内部修改
除了全局样式，我们还需要关注**组件**内部样式 / **页面**内部样式。菜单栏宽度可以通过css覆盖，但阅读 `Menu` 组件代码后，其实组件提供了 变量 / api 可以传参修改，那么就需要到组件内部进行过修改。

| ![image.png](https://cdn.nlark.com/yuque/0/2023/png/2399662/1678379729556-7b0f4248-8172-4f8f-8119-55b696516e8c.png#averageHue=%232f3339&clientId=u662097eb-822c-4&from=paste&height=585&id=u90e8ad60&name=image.png&originHeight=1170&originWidth=1348&originalType=binary&ratio=2&rotation=0&showTitle=true&size=338112&status=done&style=none&taskId=uc380d589-8aef-431c-9f4f-57b5192249f&title=SideMenu.js&width=674 "SideMenu.js") |
| --- |

此外还需动态改变右侧内容区域宽度：
```vue
<template>
  <pro-layout
    :menus="menus"
    :collapsed="collapsed"
    :mediaQuery="query"
    :isMobile="isMobile"
    :handleMediaQuery="handleMediaQuery"
    :handleCollapse="handleCollapse"
    :i18nRender="i18nRender"
    siderWidth="230"
    v-bind="settings"
  >
  </pro-layout>
</template>

export default {
    methods{
      handleCollapse(val) {
        this.collapsed = val
        // 动态增加padding-left值
        const ele = document.getElementsByClassName('ant-layout-content')[0]
        if (val) {
          ele.style.paddingLeft = '110px'
        } else {
          ele.style.paddingLeft = '260px'
        }
      },
  }
}
```
| ![菜单折叠gif.gif](https://cdn.nlark.com/yuque/0/2023/gif/2399662/1678414297791-32786472-4f4e-4461-bcab-c9deab27f85a.gif#averageHue=%2324293d&clientId=u12abb67b-27e2-4&from=ui&id=uc97a393a&name=%E8%8F%9C%E5%8D%95%E6%8A%98%E5%8F%A0gif.gif&originHeight=283&originWidth=600&originalType=binary&ratio=2&rotation=0&showTitle=true&size=3286473&status=done&style=none&taskId=u19147668-a7f8-46c6-a64b-bbaddcee9c6&title=%E8%8F%9C%E5%8D%95%E6%8A%98%E5%8F%A0 "菜单折叠") |
| --- |

同样的步骤，可以把顶部导航栏补上，再来看看效果
## 补充

- 演示代码仓库：
- [vue中scoped属性 & /deep/](https://www.yuque.com/nihaohaohaohao/kb/emaec6ccidl5199i?view=doc_embed)
- [浅克隆推远程仓库解决办法](https://www.yuque.com/nihaohaohaohao/kb/mdm5uopo2kwrpgs1?view=doc_embed)
