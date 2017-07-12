---
date: 2016-03-09T20:08:11+01:00
title: 索引
weight: 20
---

我在学习前端，觉得最受用的技能是， **英语阅读** 。本站的文章虽然都是中文的，但一些重要的概念和措辞都源自于英文，给大家提个醒。

一个简单的应用，从工具到架构，动不动就依赖成百上千个 NPM 包，要折腾起来时间永远不够用，但不折腾不行，除非放弃前端。重复造的轮子太多，选择性折腾尤为重要，这是我学习前端最大的感悟。

最初我选择的是 Angular ，使用 Angular CLI 工具，感觉很顺手。但不足的是，缺乏可用的 UI 组件。一直关注 material2 组件库，直到目前连基础的树形容器都没有，又找不到替换的库，放弃了。后面又从入门到放弃用了一段时间的 Vue ，解释不清为什么。

一直来，我对 React 的 JSX 发明反感，思维方式跟不上。后来发现很多漂亮的组件架构都是 React 麾下的，特别是 [ant-design](https://ant.design/) ，让我眼前一亮，就按照文档试着用 [dva-cli](https://github.com/dvajs/dva-cli) 做个几个 hello world ，感觉 JSX 没有想像的那么糟。接着尝试渲染一些常用的组件，按钮、表格、提示框等，效果挺喜欢的，对 React 黑转粉了。

用 [dva](https://github.com/dvajs/dva) 套装的时候，发现了几个 bug ，在 Github 上提交了很久，还做了 PR ，但维护人员迟迟没有响应。后面学习到 router ，喜欢 react-router@4 的功能特性，但 dva 还停留在 react-router@2 。觉得还是 create-react-app 可靠，就转了过去，功能简单了一些，但学习目标更明确了。

后来在 medium.com 读到关于 [rekit@2.0](https://medium.com/@nate_wang/rekit-2-0-next-generation-react-development-ce8bbba51e94) 的文章，觉得这正是我需要的工具。对比 create-react-app reject 后的项目结构，我发现 rekit 生产的项目组织架构真的很清晰，使用 rekit cli 和 rekit-portal 可以很方便地增删项目代码，自动化程度高，可伸缩性强，媲美我之前用过的 Angular CLI 工具。

选定 rekit 这条线，不再纠结选用哪个工具与功能库，也不用处心积虑地想一段代码放在哪个文件更好。对于初学者，最大裨益是解决你无处下手的痛点。

[使用 Ant Design 设计组件](/rk-antd)
