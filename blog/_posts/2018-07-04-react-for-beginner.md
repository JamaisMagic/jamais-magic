---
title: "React初体验"
description: "React初体验"
excerpt_separator: "<!--more-->"
last_modified_at: 
comments: true
categories:
  -
tags:
  - React
  - create react app
---

之前做项目一直习惯用Vue，而且用得也比较顺手，所以想尝试一下别的框架，把目标瞄准React。

学习一个框架，自然是看官方文档啦。<site><a target="_blank" href="https://reactjs.org/">React</a></site>

看完最基础那部分，基本上可以开始动手写了，这些不多说了。

但是能动手写不一定就能写得顺手。所以我找了一个脚手架工具，看这个东西貌似是Facebook官方出品的而且star数多，就打算用它。<site><a target="_blank" href="https://github.com/facebook/create-react-app">create-react-app</a></site>

后来用的时候发现，这货不支持css预处理，对于好久没有直接写css的人来说应该很不习惯，所以我Google了下看看有什么办法支持，发现网上的一些办法大概就是把这个项目的webpack配置eject出来，然后手动修改配置，添加自己想加的配置。例如<site><a target="_blank" href="https://medium.com/front-end-hacking/how-to-add-sass-or-scss-to-create-react-app-c303dae4b5bc">How to add SASS/SCSS to a create-react-app Project</a></site>
我尝试了一下，eject出来的配置文件是只读的，然后需要修改权限配置可读写，然后觉得自己改配置也不是很handy，所以继续Google搜，最终发现了这个东西。
<site><a target="_blank" href="https://medium.com/@kitze/configure-create-react-app-without-ejecting-d8450e96196a">Configure create-react-app without ejecting</a></site>

<site><a target="_blank" href="https://github.com/kitze/custom-react-scripts">custom-react-scripts</a></site>
这个作者把react-script fork过来自己改，添加了css预处理器和css module的支持。

然后我就用这个东西，基本上在构建方面已经很handy了。不过还有一样，就是貌似没有http请求的proxy，然后我搜了一下，发现可以在package.json文件中配置。不过目前我还不需要用到。下次用的时候试试。<site><a target="_blank" href="https://coursework.vschool.io/setting-up-a-full-stack-react-application/">Setting up a Full Stack React Application</a></site>

开发的过程中常用的一些包也顺便列一下吧。
* i18n相关： <site><a target="_blank" href="https://github.com/yahoo/react-intl">react-intl</a></site> <site><a target="_blank" href="https://www.codeandweb.com/babeledit/tutorials/how-to-translate-your-react-app-with-react-intl">How to translate your React app with react-intl</a></site>

* React Router相关： <site><a target="_blank" href="https://github.com/ReactTraining/react-router">react-router</a></site> <site><a target="_blank" href="https://medium.com/@pshrmn/a-simple-react-router-v4-tutorial-7f23ff27adf">A Simple React Router v4 Tutorial</a></site>

* toast相关： <site><a target="_blank" href="https://github.com/fkhadra/react-toastify#demo">react-toastify</a></site>

大概就是这些吧，至于其他Redux之类的还没用到，主要是我用了16.4版本的Context，感觉还算够用，所以要下次再看看Redux相关的东西咯。
16.4版本的Context的api重新设计了一遍，而且文档已经去掉了不建议用的提示，估计这个api应该可以放心用的了。

假如页面需要code splitting，可以参考这里。这篇文章最好看完整一点，要看到最后。<site><a target="_blank" href="https://serverless-stack.com/chapters/code-splitting-in-create-react-app.html">Code Splitting in Create React App</a></site>

一个足够handy的工具链差不多就是这样了。

最后需要注意的一点是，build的时候如果页面不是在根目录，需要在package.json文件的homepage属性修改地址喔。

和Vue做个简单的对比。

从构建工具来说，vue-cli明显比create-react-app好用好多，许多方面都考虑进去，除了双方都做好了配置的es6+语法转es5之外，vue-cli把三种主流的css与处理器都做了配置，支持一个scope属性的css module。路由代码分割vue-cli也比较简答，对多页面构建的支持还没搞过不好对比。

从框架的使用来说，Vue模板和react jsx孰优孰劣我觉得看每个人自己想法或者习惯，没有什么好说的，我觉得都OK。只是jsx比较容易在html里夹杂太多逻辑，例如渲染个列表需要自己写循环Array.prototype.map啊之类的。

组件之间的通信方面，别的不说，只说父子组件通信，双方都是子组建可以根据父组件传入的props的变化渲染页面，但是假如子组件需要通知父组件做一些事情，就有点不同了，Vue可以通过事件实现，而React则需要把函数通过props传递给自组建，由子组件调用，感觉这个耦合会比较大。

React有些组件的重新渲染需要在props里面加key={xxx}才行，这个不知道什么原因。

在事件绑定的函数的this，React需要自己手动做绑定，而Vue已经做好了绑定。

React组件修改state不能直接给对象赋值，而是要调用this.setState()方法，而这个方法是merge式工作的，即是只改写了的属性，不写的属性是不会修改的。

引用外部组件，React import进来后可以直接在jsx里用，而Vue还需要在components对象里声明一下，这个不知道为什么。

关于表单的绑定，例如input text，React需要手动在onChange事件里修改state，而Vue的v-model已经自动做了这些事情了。

总的来说确实是Vue使用比较简单，很多东西都已经考虑过了，React还是有更多需要“手动”干的事情。
