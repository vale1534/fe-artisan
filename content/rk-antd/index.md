---
date: 2016-03-09T20:08:11+01:00
title: 使用 Ant Design 设计组件
weight: 30
---

本文介绍在 rekit 项目中引入 Ant.D ，完成项目代码： [rk-antd](https://github.com/wenris/rk-antd)

> Ant design 项目在 github 上的 id 是 [ant-design/ant-design](https://github.com/ant-design/ant-design) ， Ant.D 不是其名字或代号，只是我发现用 Ant.D 指代这个项目很方便，就用上了。

## 安装 Ant.D 模块

安装 Ant.D 模块只需要一条命令：

```sh
yarn add antd       # or npm install --saved antd
```

完成后就可以在项目组件中引用 Ant.D 的组件了，比如引入 Button ：

```js
import { Button } from 'antd';
```

Ant.D 实现了丰富的基础组件，参考 [官方文档](https://ant.design/docs/react/introduce) 了解更多。

## 引入 Ant.D 样式

参照 [Ant.D 官方文档](https://ant.design/docs/react/customize-theme) ，要定制样式得引入一大堆的库，包括 **less-vars-to-js** 和 **babel-plugin-import** ，还要进行很多的配置 。配置完成后的好处是，按需引入组件样式，减小发送至浏览器的文件大小。但是在我看来，缺点更明显，给我的项目招致了不少的麻烦和复杂度。

于是研究 Ant.D 的样式文件结构，准备强行引入，在项目中直接生产 Ant.D 样式，还要在项目组件中复用 Ant.D 的基础样式。这其实并不难，甚至比官方文档上的方法更简单。

> 在生产项目时， Webpack 通过 less-loader 解析 *~module_name* 路径，支持在 *\*.less* 文件中引入 *node_modules* 目录下的模块文件。在下文中，我将用 *~antd* 指代 Ant.D 的安装目录，即 *\<PROJECT_DIR\>/node_modules/antd* 。

Ant.D 使用 less 转译样式，入口文件在 *~antd/dist/antd.less* ：

```less
@import "../lib/style/index.less";
@import "../lib/style/components.less";
```

目录 *~antd/lib/style* 存放了 Ant.D 基础样式文件，结构如下：

```txt
~antd/lib/style/
    index.less              // 入口文件

    components.less         // 组件样式汇总文件
    
    color/                  // 样色相关样式
        [bezierEasing,colorPalette,colors,tinyColor].less
    
    core/                   // 核心样式
        [base,iconfont,index,motion,normalize].less
    
        motion/             // 核心动画样式
            [fade,move,other,slide,swing,zoom].less
    
    mixins/                 // 混合功能样式
        [clearfix,compatibility,iconfont,index,motion,opacity,size].less
    
    theme/                  // 主题定义样式
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

可见 Ant.D 样式结构清晰明了，很容易在项目中直接导入。

先在 *styles* 目录下新建 *antd-custom.less* 文件，将 *~antd/lib/style/theme/default* 文件内容直接拷贝至该文件，按需定制如下：

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

然后修改项目的主样式文件 *index.less* ，引入 Ant.D 样式：

```less
// 文件：src/styles/index.less

@import "./antd-custom";
@import "~antd/lib/style/core/index";
@import "~antd/lib/style/components";
```

在项目组件中复用 Ant.D 的样式代码，如样色、尺寸变量，直接引用 *antd-custom.less* 即可。

更深入点的应用，可以学习 Ant.D 组件是如何使用 *~antd/lib/style* 下的 *motion* 、 *mixins* 等基础功能，将同样代码挪用到自己的组件设计中。

最后删除多余的 *reset.less* 和 *global.less* 文件，在 *mixins.less* 文件引用 Ant.D 的 mixins 代码：

```less
// 文件：src/styles/mixins.less

@import '~antd/lib/style/mixins/index';
```

这样下来， Ant.D 与 rekit 融洽相处。缺点也有一个，发送至浏览器的样式代码很大，包括了 Ant.D 所有组件的样式，不管项目有没用到。如果要进一步优化，可以在项目中覆写 *~antd/lib/style/components* 文件，剔除没用到的组件样式。