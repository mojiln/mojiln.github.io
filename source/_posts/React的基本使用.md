---
title: React的基本使用
date: 2017-07-19 20:52:02
categories:
  - React
tags:
  - React
description:	
  - react基础
---

## react 是什么

- React 是一个 JS 库，用来构建用户界面（写 HTML，构建 web 应用）
- 从 MVC 的角度来看，相当于 视图层 V（View） 的内容。

## react 的特点

- 声明式： 我们只需要描述页面长什么样子就可以了，React 负责更新页面
- 基于组件（组件化）
- 学习一次，随处使用（Web 、 安卓/ios、vr ...）

## React 的基本使用

- 安装：npm i react react-dom

- 引入 react 和 react-dom 两个 js 文件（注意：引入顺序，react 在前，react-dom 在后）

- 创建 React 元素

  - `const h1 = React.createElement('h1', null, '子节点')`

- 渲染创建好的 React 元素，到页面中

  - `ReactDOM.render(h1, document.getElementById('root'))`

## React.createElement() 方法的说明

- 作用：创建 react 元素的

```js
// 创建React元素
// 第一个参数：表示要创建什么元素，就是 HTML 标签名称
// 第二个参数：表示元素自身属性，如果没有就传 null
//  如果要指定元素自身的属性，就传递一个对象（{}）
//  1 class ==> className
//  2 for ==> htmlFor
// 第三个及其以后的参数：表示元素的子节点（文本、元素节点）
//  如果是文本节点，就直接传递 字符串。
//  如果是元素节点，就继续调用 React.createElement() 方法，创建新的React元素节点
//
// const h1 = React.createElement('div', null, 'Hello React')
const h1 = React.createElement(
  'h1',
  {
    id: 'title',
    className: 'cls',
    htmlFor: 'd'
  },
  'Hello React',
  'test 文本节点',
  React.createElement('span', null, '这是一个span')
)
```

## React 脚手架初始化项目的步骤

- 命令：`npx create-react-app 项目名称`
  - 比如：`npx create-react-app my-app`
- npx 命令：简化使用脚手架初始化项目的流程
  - 不使用 npx：1 先全局安装脚手架的包 2 使用脚手架包提供的命令来初始化项目
  - 使用 npx：不需要再全局安装脚手架的包，直接就可以初始化项目
- 如何启动项目？进入项目根目录然后，执行以下命令
  - yarn start
  - npm start

## 在脚手架中使用 react

- 导入
  - `import React from 'react'`
  - `import ReactDOM from 'react-dom'`

## JSX

- 为什么要学习 JSX ？
  - 因为 createElement 形式，太繁琐，不直观，书写效率不高，所以，我们不想用这种方式。
  - JSX 特点：不反锁，直观，书写效率高
- JSX 是什么？ JavaScript XML（HTML），也就是在 JS 中书写 HTMl 格式的代码

## JSX 的基本使用

- 导入 react 和 react-dom

- 使用 JSX 语法创建 React 元素

  - JSX 就跟写 HTML 一样

- 渲染创建好的 React 元素

## JSX 语法的注意点

- JSX 元素的属性名推荐使用：驼峰命名法

- class ===> className

- 如果元素没有子节点，可以使用 单标签 方式来结束

  - 比如：`<div />`

- 推荐使用 () 来包裹 JSX，从而避免 JS 中自动插入分号机制

## 在 JSX 中使用 JS 表达式（数据）

- 语法：使用 {} ，就可以在 JSX 中使用 JS 中的数据了
  - `<div>Hello {name + '666'}</div>`
- 原则：可以在 {} 中使用任何的 JS 表达式。
- 注意：不能在 {} 中，使用 语句！
  - 比如： if/for/switch ...
- 注意：不能在 {} 中使用对象，除了 style 属性以外！！！
- JSX 自身也是一个 JS 表达式，所以，可以在 {} 中继续使用 JSX ！！！

## React 的条件渲染

- 使用 if/esle 来实现

```js
const loadData = () => {
  if (isLoading) {
    return <div>loading...</div>
  }

  return <div>加载完成后的列表结构</div>
}

const h1 = <div>{loadData()}</div>
```

- 使用三元表达式

```js
const loadData = () => {
  return isLoading ? <div>loading...</div> : <div>加载完成后的列表结构</div>
}
```

- 逻辑运算符 &&

```js
const loadData = () => {
  return isLoading && <div>loading...</div>
}
```

## React 中的列表渲染

- 使用数组的 map 方法来进行列表渲染
- 需要给被遍历生成的元素添加 key 属性，key 应该是唯一的。尽量避免使用 index 作为索引号。
- 剩下的就是 JS 中 map 方法的使用了。

```js
<ul>
  {songs.map(item => (
    <li key={item.id}>{item.name}</li>
  ))}
</ul>
```

## React 中给 JSX 添加样式

- 行内样式（style） 不推荐

```js
const h1 = (
  <h1 style={{ color: 'red', fontSize: 30, backgroundColor: 'hotpink' }}>
    我变大了
  </h1>
)
```

- className 类名 --- 推荐！！！

```js
const h1 = <h1 className="pink">我变大了</h1>
```

## 案例

```js
;[
  { user: '张三', content: '哈哈，沙发' },
  { user: '张三2', content: '哈哈，板凳' },
  { user: '张三3', content: '哈哈，凉席' },
  { user: '张三4', content: '哈哈，砖头' },
  { user: '张三5', content: '哈哈，楼下山炮' }
]
```
