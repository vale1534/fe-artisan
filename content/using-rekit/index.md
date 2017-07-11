---
date: 2016-03-09T20:08:11+01:00
title: 引入 Ant.D 样式
---

按照 Ant.D 官方文档上，要定制样式得引入一大堆的库，包括 **less-vars-to-js** 和 **babel-plugin-import** ，还要进行很多的配置，见：https://ant.design/docs/react/customize-theme 。

这样做的好处是，可以按需引入组件样式，减小最终需下载的样式文件大小。但是缺点很明显，给项目引来不必要的复杂度。

于是研究一下 Ant.D 的样式结构。

Ant.D 使用 less 转译样式，入口文件在 *~antd/dist/antd.less* ：

```less
@import "../lib/style/index.less";
@import "../lib/style/components.less";
```

目录 `~antd/lib/style` 存放了 Ant.D 基础样式文件，结构如下：


```txt
lib/style/
    index.less              - 入口文件
    components.less         - 组件样式汇总文件
    color/                  - 样色相关样式
        [bezierEasing,colorPalette,colors,tinyColor].less
    core/                   - 核心样式
        [base,iconfont,index,motion,normalize].less
        motion/             - 核心动画样式
            [fade,move,other,slide,swing,zoom].less
    mixins/                 - 混合功能样式
        [clearfix,compatibility,iconfont,index,motion,opacity,size].less
    theme/                  - 主题定义样式
        default.less
```

再看 *~antd/lib/style/index.less* 内容：

```less
@import "./themes/default";
@import "./core/index";
```

与 *~antd/lib/style/components.less* 内容：

```less
@import "../affix/style/index.less";
@import "../alert/style/index.less";
@import "../anchor/style/index.less";
@import "../auto-complete/style/index.less";
@import "../avatar/style/index.less";
@import "../back-top/style/index.less";
@import "../badge/style/index.less";
@import "../breadcrumb/style/index.less";
@import "../button/style/index.less";
...
```

Ant.D 样式结构已经很清晰了，很容易在我们的项目中引用 Ant.D 的基础样式，并做定制。新建 *antd-custom.less* 文件定制 Ant.D 样式，内容如下：

```less
// 文件：src/styles/antd-custom.less
// 源自：~antd/lib/style/theme/default.less

@import "~antd/lib/style/color/colors";

// The prefix to use on all css classes from ant.
@ant-prefix             : ant;

// -------- Colors -----------
@primary-color          : @blue-6;
@info-color             : @blue-6;
@success-color          : @green-6;
@error-color            : @red-6;

// ...
// 根据需要覆写
```

在项目的主样式文件中加入 Ant.D 样式：

```less
// 文件：src/styles/index.less

@import "./antd-custom";
@import "~antd/lib/style/core/index";
@import "~antd/lib/style/components";
```

在项目组件中复用 Ant.D 的样式代码，如样色、尺寸变量，直接引用 *antd-custom.less* 即可。更深入点的应用，可以学习 Ant.D 组件是如何使用 *~antd/lib/style* 下的 *motion* 、 *mixins* 等基础功能，将同样代码挪用到自己的项目中。

删除多余的 *reset.less* 和 *global.less* 文件，在 *mixins.less* 文件引用 **Ant.D** 的功能样式：

```less
// 文件：src/styles/mixins.less

@import '~antd/lib/style/mixins/index';
```

这样下来，与 **rekit** 生产的代码融合得很好。缺点也有一个，需要浏览器下载的样式表很大，包括了
所有 Ant.D 组件的样式，不管项目有没用到。如果要进一步优化，可以在项目中覆写 *~antd/lib/style/components* 文件，剔除没用到的组件样式。