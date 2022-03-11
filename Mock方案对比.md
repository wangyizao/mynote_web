# Mock方案对比

### 1. 代码侵入 (直接在代码中写死 Mock 数据，或者请求本地的 JSON 文件)

优点：无

缺点：

1. 和其他方案比 Mock 效果不好
2. 与真实 Server 环境的切换非常麻烦，一切需要侵入代码切换环境的行为都是不好的

### 2. 请求拦截

代表：[Mock.js](http://mockjs.com/)

示例：

```
Mock.mock(/\\\\/api\\\\/visitor\\\\/list/, 'get', {
  code: 2000,
  msg: 'ok',
  'data|10': [
    {
      'id|+1': 6,
      'name': '@csentence(5)',
      'tag': '@integer(6, 9)-@integer(10, 14)岁 @cword("零有", 1)基础',
      'lesson_image': "<https://images.pexels.com/3737094/pexels-photo-3737094.jpeg>",
      'lesson_package': 'L1基础指令课',
      'done': '@integer(10000, 99999)',
    }
  ]
})
```

优点：

1. 与前端代码分离
2. 可生成随机数据

缺点：

1. 数据都是动态生成的假数据，无法真实模拟增删改查的情况
2. 只支持 ajax，不支持 fetch

(想要了解 ajax 和 fetch 区别的同学来点[我](https://zhuanlan.zhihu.com/p/24594294))

### 3. 接口管理工具

代表：[rap](https://github.com/thx/RAP), [swagger](https://swagger.io/), [moco](https://github.com/dreamhead/moco), [yapi](https://github.com/YMFE/yapi)

优点：

1. 配置功能强大，接口管理与 Mock 一体，后端修改接口 Mock 也跟着更改，可靠

缺点：

1. 配置复杂，依赖后端，可能会出现后端不愿意出手，或者等配置完了，接口也开发出来了的情况。
2. 一般会作为大团队的基础建设而存在， 没有这个条件的话慎重考虑

### 4. 本地 node 服务器

代表：[json-server](https://github.com/typicode/json-server)

优点：

1. 配置简单，json-server 甚至可以 0 代码 30 秒启动一个 REST API Server
2. 自定义程度高，一切尽在掌控中
3. 增删改查真实模拟

缺点：

1. 与接口管理工具相比，无法随着后端 API 的修改而自动修改

## 本课程 Mock 计划

从本章开始，使用 json-server Mock 2 章，

在这 2 章里让大家尽可能多的接触到不同的(GET, POST, DELETE, PATCH)Mock 场景，

剩下的章节里使用真实的接口

## REST API

一句话总结：URI 代表 资源/对象，METHOD 代表行为

```
GET /tickets // 列表
GET /tickets/12 // 详情
POST /tickets  // 增加
PUT /tickets/12 // 替换
PATCH /tickets/12 // 修改
DELETE /tickets/12 // 删除
```

点 [我](https://segmentfault.com/q/1010000005685904) 了解 `patch vs put`

# 为什么react列表要加key

## 为什么列表要加 key 属性，以及为什么用 index 是不好的

遍历对象的每一个属性深度对比是非常浪费性能的

React 使用列表的`key`来进行对比，如果不指定，就默认为 index 下标

那么，为什么 不指定 key/用 index 下标 是不好的呢？

假设现在有这样一段代码：

```
const users = [{ username: "bob" }, { username: "sue" }];

users.map((u, i) => <div key={i}>{u.username}</div>);
```

它会渲染出这个 DOM 树：

```
<div key="1">bob</div>
<div key="2">sue</div>
```

然后用户做了某个操作，`users` 被 `unshift` 另一个对象，变成：

```
const users = [
  { username: "new-guy" },
  { username: "bob" },
  { username: "sue" },
];
```

DOM 树就会变成这样，注意`key`的变化：

```
<div key="1">new-guy</div>
<div key="2">bob</div>
<div key="3">sue</div>
```

DOM 树的前后对比是这样的：

```
<div key="1">bob</div>   |  <div key="1">new-guy</div>
<div key="2">sue</div>   |  <div key="2">bob</div>
                         |  <div key="3">sue</div>
```

我们人类看得出来前后的变化只是在开头加了一个`new-guy`而已

但是由于 React 使用 key 值来识别变化，所以 React 认为的变化是：

1. bob -> new-guy
2. sue -> bob
3. 添加 sue

非常消耗性能 😭

但是如果我们一开始就给它指定一个合适的 key，比如用 name：

```
users.map((u, i) => <div key={u.username}>{u.username}</div>);
```

React 认为的变化就变成：

```
                         |  <div key="1">new-guy</div>
<div key="1">bob</div>   |  <div key="2">bob</div>
<div key="2">sue</div>   |  <div key="3">sue</div>
```

这样只需要做一个`unshift`操作，性能节省 😃

# TypeScript 基本知识梳理

## TypeScript vs JavaScript

TypeScript 是 "强类型" 版的 JavaScript，当我们在代码中定义变量(包括普通变量、函数、组件、hook等)的时候，TypeScript 允许我们在定义的同时指定其类型，这样使用者在使用不当的时候就会被及时报错提醒

```jsx
interface SearchPanelProps {
  users: User[],
  param: {
    name: string;
    personId: string;
  },
  setParam: (param: SearchPanelProps['param']) => void;
}

export const SearchPanel = ({users, param, setParam}: SearchPanelProps) => {}
```

经常用 TypeScript 的感受：比起原来的 JavaScript，TypeScript 带来了完全不一样的开发体验，bug 大大减少了，编辑器提示快了，代码更易读了， 开发速度快了(看似多写代码，其实由于前面几点节省了大量开发时间)，上手了就回不去了

## TypeScript 的类型

在本节中我们使用到了8种类型： number, string, boolean, 函数, array, any, void, object

这一节我们接触到了平常使用中会接触到的大部分的类型，下面我们挨个梳理一遍：

### 1. number

数字类型，包含小数、其他进制的数字：

```jsx
let decimal: number = 6;
let hex: number = 0xf00d;
let binary: number = 0b1010;
let octal: number = 0o744;
let big: bigint = 100n;
```

### 2. string

字符串

```jsx
let color: string = "blue";
```

### 3. array

在 TS 中，array 一般指**所有元素类型相同**的值的集合，比如：

```jsx
let list: Array<number> = [1, 2, 3];

// or

interface User {
  name: string
}
const john = {name: 'john'}
const jack = {name: 'jack'}
let personList = [john, jack] // 这里 john 和 jack 都是 User 类型的
```

和这种混合类型的 "数组"：

```jsx
let l = ['jack', 10]
```

在 TS 中不是 数组/array，它们叫作 tuple，下面会提到

### 4. boolean

布尔值

```jsx
let isDone: boolean = false;
```

### 5. 函数

两种方法

1. 在我们熟悉的 "JS函数" 上直接声明参数和返回值：

```jsx
/**
 * 这是我们上节课写的代码，大家可能发现了
 * 我在这里做了一些修改，在箭头前边加上了 :boolean
 * 但是在我们上节课的代码中是没有这个:boolean 的，
 * 之所以不需要加是因为 类型推断，这个我们在下面会讲
 * @param value
 */
const isFalsy = (value: any): boolean => { 
  return value === 0 ? true : !!value; 
}; 
```

1. 直接声明你想要的函数类型：

```jsx
/**
 * 上节课写的 useMount 和 isFalsy
 */
export const useMount = (fn: () => void) => {
  useEffect(() => {
    fn();
  }, []);
};

const isFalsy: (value: any) => boolean = (value) => {
  return value === 0 ? true : !!value;
};
```

### 6. any

any 表示这个值可以是任何值，被定义为 any 就意味着不做任何类型检查

```jsx
let looselyTyped: any = 4;
// looselyTyped 的值明明是个4，哪里来的ifItExists方法呢？
// 由于声明为any，我们没法在静态检查阶段发现这个错误
looselyTyped.ifItExists();
```

初学 TS 的同学经常会为了让TS不再报错就用了很多any，这样做会失去TS的保护。同学们应该尽量避免使用any

### 7. void

绝大部分情况下，只会用在这一个地方：表示函数不返回任何值或者返回undefined (因为函数不返回任何值的时候 === 返回 undefined)

```jsx
/**
 * 上节课写的 useMount
 */
export const useMount = (fn: () => void) => {
  useEffect(() => {
    fn();
  }, []);
};
```

### 8. object

除了 number, string, boolean, bigint, symbol, null, or undefined，其他都是 object

下面是我们还没有接触到的 TS 类型

### 9. tuple

其实这个大家已经见过了，这是没有给大家指出来

这就是一个典型的 tuple

```jsx
const [users, setUsers] = useState([])
```

tuple 是 "数量固定，类型可以各异" 版的数组

在 React 中有可能使用 tuple 的地方就是 custom hook 的返回值，注意 isHappy → tomIsHappy 以及其他名字的变化，这里使用tuple的好处就显现出来了：便于使用者重命名

```jsx
const useHappy = () => {
   //....
   return [isHappy, makeHappy, makeUnHappy]
}

const SomeComponent = () => {
  const [tomIsHappy, makeTomHappy, makeTomUnHappy] = useHappy(false)
  // ...
}
```

### 10. enum

```jsx
enum Color {
  Red,
  Green,
  Blue,
}
let c: Color = Color.Green;
```

### 11. null 和 undefined

null 和 undefined 在 TypeScript 中既是一个值，也是一个类型：

```jsx
let u: undefined = undefined;
let n: null = null;
```

### 12. unknown

unknown 表示这个值可以是任何值

❓❓❓❓❓❓

这句话怎么这么熟悉，刚才是不是用来形容 any 的？

unknown 的用法：在你想用 any 的时候，用 unknown 来代替，简单来说，unknown是一个"严格"版的 any

```jsx
const isFalsy = (value: unknown) => { 
 // 大家不用考虑这段console有啥意义，把它打在你的代码里对应的位置，观察编辑器会不会报错；
 // 再思考它应不应该报错
  console.log(value.mayNotExist)
  return value === 0 ? true : !!value; 
}; 
```

### 13. never

```jsx
// 这个 func返回的就是never类型，用到比较少，在类型操作等场景会用到
const func = () => {
  throw new Error()
}
```

### interface

interface 不是一种类型，应该被翻译成 接口，或者说使用上面介绍的类型，创建一个我们自己的类型

```jsx
interface User {
  id: number;
}
const u: User = {id: 1}
```

## 啥时候需要声明类型

理论上来说在我们声明任何变量的时候都需要声明类型(包括普通变量、函数、组件、hook等等)，声明 函数、组件、hook 等需要声明参数 和 返回值的类型。

但是在很多情况下，TS可以帮我们自动推断，我们就不用声明了，比如：

```jsx
// 这里虽然没有显式声明，但是ts自动推断这是个number
let a = 1

// 自动推断返回值为number
function add(a: number, b: number) {
  return a + b;
}

// 自动推断返回值为boolean
const isFalsy = (value: unknown) => { 
  return value === 0 ? true : !!value; 
}; 
```

## .d.ts

JS 文件 + .d.ts 文件   === ts 文件

.d.ts 文件可以让 JS 文件继续维持自己JS文件的身份，而拥有TS的类型保护

一般我们写业务代码不会用到，但是点击类型跳转一般会跳转到 .d.ts文件

# CSS - CSS-in-JS

CSS-in-JS 不是指某一个具体的库，是指组织CSS代码的一种方式，代表库有 styled-component 和 emotion

## 传统CSS的缺陷

### 1. 缺乏模块组织

传统的JS和CSS都没有模块的概念，后来在JS界陆续有了 CommonJS 和 ECMAScript Module，CSS-in-JS可以用模块化的方式组织CSS，依托于JS的模块化方案，比如：

```jsx
// button1.ts
import styled from '@emotion/styled'

export const Button = styled.button`
  color: turquoise;
`
// button2.ts
import styled from '@emotion/styled'

export const Button = styled.button`
  font-size: 16px;
`
```

### 2. 缺乏作用域

传统的CSS只有一个全局作用域，比如说一个class可以匹配全局的任意元素。随着项目成长，CSS会变得越来越难以组织，最终导致失控。CSS-in-JS可以通过生成独特的选择符，来实现作用域的效果

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0f62fac7-c078-439a-964f-17bbfa478c0d/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0f62fac7-c078-439a-964f-17bbfa478c0d/Untitled.png)

```jsx
const css = styleBlock => {
  const className = someHash(styleBlock);
  const styleEl = document.createElement('style');
  styleEl.textContent = `
    .${className} {
      ${styleBlock}
    }
  `;
  document.head.appendChild(styleEl);
  return className;
};
const className = css(`
  color: red;
  padding: 20px;
`); // 'c23j4'
```

### 3. 隐式依赖，让样式难以追踪

比如这个CSS样式：

```css
.target .name h1 {
  color: red
}

body #container h1 {
  color: green
}
<!doctype html>
<html lang="en">
<body>
  <div id='container'>
   <div class='target'>
     <div class='name'>
       <h1>我是啥颜色？</h1>
     </div>
   </div>
  </div>
</body>
</html>
```

那么这个h1元素最终显式为什么颜色？加入你想要追踪这个影响这个h1的样式，怎么追踪？

而CSS-in-JS的方案就简单直接、易于追踪

```html
export const Title = styled.h1`
  color: green;
`
<Title>
  我是啥颜色？
</Title>
```

### 4. 没有变量

传统的CSS规则里没有变量，但是在 CSS-in-JS 中可以方便地控制变量

```css
const Container = styled.div(props => ({
  display: 'flex',
  flexDirection: props.column && 'column'
}))
```

### 5. CSS选择器与HTML元素耦合

```css
.target .name h1 {
  color: red
}

body #container h1 {
  color: green
}
<!doctype html>
<html lang="en">
<body>
  <div id='container'>
   <div class='target'>
     <div class='name'>
       <h1>我是啥颜色？</h1>
     </div>
   </div>
  </div>
</body>
</html>
```

如果你想把 `h1` 改成`h2`，必须要同时改动 CSS 和 HTML。而在CSS-in-JS中，HTML和CSS是结合在一起的，易于修改

## Emotion 介绍

Emotion 是目前最受欢迎的 CSS-in-JS 库之一，它还对 React 作了很好的适应，可以方便地创建 styled component，也支持写行内样式：

```css
/** @jsx jsx */
import { jsx } from '@emotion/react'

render(
  <div
    css={{
      backgroundColor: 'hotpink',
      '&:hover': {
        color: 'lightgreen'
      }
    }}
  >
    This has a hotpink background.
  </div>
)
```

这种写法比起React自带的style的写法功能更强大，比如可以处理级联、伪类等style处理的不了的情况

```css
<span style={{ color: "red" }}>{keyword}</span>
```

# Hook

实现的原理：闭包

```jsx
import { useState, useEffect } from "react";

const useWindowsWidth = () => {
  const [isScreenSmall, setIsScreenSmall] = useState(false);

  let checkScreenSize = () => {
    setIsScreenSmall(window.innerWidth < 600);
  };
  useEffect(() => {
    checkScreenSize();
    window.addEventListener("resize", checkScreenSize);

    return () => window.removeEventListener("resize", checkScreenSize);
  }, []);

  return isScreenSmall;
};

export default useWindowsWidth;
import React from 'react'
import useWindowWidth from './useWindowWidth.js'

const MyComponent = () => {
  const onSmallScreen = useWindowWidth();

  return (
    // Return some elements
  )
}
```

优点：

1. 提取逻辑出来非常容易
2. 非常易于组合
3. 可读性非常强
4. 没有名字冲突问题

缺点：

1. Hook有自身的用法限制: 只能在组件顶层使用，只能在组件中使用
2. 由于原理为闭包，所以极少数情况下会出现难以理解的问题

8-1和8-2已解释了闭包的问题

# 无限循环, useCallback 和 useMemo

# 原理

似乎大家对这两个hook的疑问很多，尤其是使用它们的时机，所以在这里给大家再总结一下

先看一下这段代码:

```jsx
export default function App() {
  const value = { name: 1 };

  React.useEffect(() => {
    alert("render");
  }, [value]);

  return (
    <div className="App">
      <h1>Hello CodeSandbox</h1>
      <h2>Edit to see some magic happen!</h2>
    </div>
  );
}
```

首先大家需要明白的是，每次App组件渲染，value变量都会被定义一次

那么，请问上面这段代码中，App渲染几次，value被定义几次， alert 会被弹出几次？

答案是：

1. 第一次初始化以后，没有任何事情能引起它的再次渲染(因为没有父组件、没有状态/props改变)，所以只会渲染一次
2. 因为只渲染一次，value也只会被定义一次
3. 而useEffect的执行时机，是在[组件渲染后](https://zh-hans.reactjs.org/docs/hooks-reference.html#timing-of-effects)，由于只渲染一次，所以useEffect只执行一次，所以alert只弹出一次

再看这段代码：

```jsx
import "./styles.css";
import React from "react";

export default function App() {
  const [count, setCount] = React.useState(0); // 加了这一行
  const value = { name: 1 };

  React.useEffect(() => {
    setCount(Math.random()); // 加了这一行
    ("render");
  }, [value]);
  return (
    <div className="App">
      <h1>Hello CodeSandbox</h1>
      <h2>Edit to see some magic happen!</h2>
    </div>
  );
}
```

这段代码比上一段多了两句，我已经在注释中标出来了，请问现在 App渲染几次，value被定义几次， alert 会被弹出几次呢？

答案是：无限循环，全都无限次

这里循环的原因是：组件渲染 → useEffect执行 → setCount触发循环 → 组件渲染 → useEffect执行 → setCount触发循环...

**绝大多数同学遇到的无限循环的情况，都是这段代码的缩影**

而 React 官方给我们提供的方法，就是 useMemo：

```jsx
import "./styles.css";
import React from "react";

export default function App() {
  const [count, setCount] = React.useState(0); 
  const value = React.useMemo(() => {
    return { name: 1 };
  }, []);

  React.useEffect(() => {
    setCount(Math.random()); 
    alert("render");
  }, [value]);
  return (
    <div className="App">
      <h1>Hello CodeSandbox</h1>
      <h2>Edit to see some magic happen!</h2>
    </div>
  );
}
```

useMemo 的意思就是：不要每次渲染都重新定义，而是我让你重新定义的时候再重新定义(第二个参数，依赖列表)。大家看到这里的依赖列表是空的，是因为useMemo里的回调函数确实没用到啥变量，如果有变量的话大家的IDE就会提醒加上依赖了。

这就是使用useMemo的原理，useMemo适用于所有类型的值，加入这个值恰好是函数，那么用useCallback也可以。也就是说，useCallback是一种特殊的useMemo。

在这里再粗暴地给大家总结一下日常使用的场景：

如果你定义了一个变量，满足下面的条件就最好用useMemo和useCallback给包裹住：

1. 它不是状态，也就是说，不是用useState定义的(redux中的状态实际上也是用useState定义的)
2. 它不是基本类型
3. 它会被放在useEffect的依赖列表里 || 自定义hook的返回值

说一下第3条，中间的两个竖线是 或，也就是两者满足其一第3条就成立。自定义hook的返回值也成立是因为，你不知道自定义hook的返回值将会被用在哪里，它可能会被用在依赖也可能不会，所以干脆都加上；而像上面那个在组件中定义的value，你就可以见机行事了

我们上面例子中的value变量就是一个经典的满足这三个条件的例子，只要遇到这个场景就使用useMemo和useCallback，就不会有无限循环的问题。

当然，更简单粗暴的是，在**定义(不是使用！)**所有"非状态"的变量的时候都用useMemo和useCallback包裹中，也不会有无限循环的问题。但是没必要这么做，代码也不好看。

大家想要做实验的话可以在这里做：https://codesandbox.io/s/immutable-paper-b70ws?file=/src/App.js:0-446

# 课程中的一个失误

课程中有个hook叫作 useMount，大家在使用的时候很可能遇到过这个提示：

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dc525816-858e-49cd-bf65-2217bc18e6d7/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dc525816-858e-49cd-bf65-2217bc18e6d7/Untitled.png)

而在课程问答中我告诉大家用 eslint-disable-next-line 把这个错误压制住就可以，但是我仔细想了一下其实这种做法不符合最佳的规范。如果大家回去读刚才说的那三条规则，会发现：

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/35c910c3-1b26-4b4a-a5ab-08a60755288a/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/35c910c3-1b26-4b4a-a5ab-08a60755288a/Untitled.png)

useMount的这个回调函数完全符合那三个规则，非状态、非基本、要做依赖(按照eslint的提示，在useMount中是必须要放在依赖里的)，所以**定义**它的时候应该用useCallback：

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d786466d-4261-4b8a-af4c-7773addbe7a8/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d786466d-4261-4b8a-af4c-7773addbe7a8/Untitled.png)

# 思考

上面那个eslint规则，就是使用到的变量都要放在依赖里那个，简称 react-hooks/exhaustive-deps，有啥用。为什么eslint坚持要我们这么做？

这个问题就比较复杂了，大家写多代码就能渐渐感觉到了。如果你还不知道原因的话，记住就行：永远遵守这个规则是最佳实践。

如果因为遵守这个规则你的代码出了问题，说明没有用好useMemo和useCallback，需要再仔细读上面所写的

**大家好，我是 Nolan，很高兴这次有机会和大家分享。**

我**在慕课网上开了一套React Hook + TS的实战课程，发现大家对其中的Hook内容非常喜欢。React Hook是写React非常重要的工具，想要写出合理、好看的React代码，掌握Hook是必须要做的一件事情。今天把其中一些知识点给大家分享一下，在分享的最后我会把这些内容总结成一篇文章发给大家方便大家阅读**

我会带大家从这个东西为什么出现，解决了什么问题的角度来分析。这样才能让大家真正带入情景

# hook

## 1. Hook 用来解决什么问题

一句话，Hook 是用来让我们更好地复用 React 状态逻辑代码的。注意这里说的不是模板代码，模板代码可以用组件来复用；而单纯的状态逻辑代码没法用组件复用

有的同学可能会说，普通的函数不就可以实现逻辑代码复用吗？答案是：普通的函数可以复用逻辑代码，但是没法复用带状态的逻辑代码。

什么是React的状态？

举个例子：

```jsx
const Comp = () => {
  const [id, setId] = useState(0)
  const [assets, setAssets] = useState()

    useEffect(() => {
      fetch(`https://google.com?id=${id}`).then(async response => {
       const data = await response.json();
        if (response.ok) {
          setAssets(data)
        } else {
          return Promise.reject(data);
        }
        })
      }, [])

  return <div>{assets.map(a => a.name)}</div>
}
```

这里面的 id，assets就是状态，它的特征是它是由特定的API(useState)定义的，而且它改变的时候组件会做出相应的反应(比如重新render)

```jsx
const sum = (a, b) => a + b
```

这个普通的函数就没有状态，sum的返回值无论怎么变，都不会让任何组件重新render

React团队是非常注重React 状态代码复用性的，从React被创造出来，他们就一直在优化代码复用的解决方案，大概经历了：Mixin → HOC → Render Props，一直到现在的 Hook

所以 Hook 并不是一拍脑门横空出世的产物，不理解这段思路也是无法完全理解 Hook的

下面我会发很多代码截图，为了让大家跟上节奏，大家只需要结合我讲的话题大概浏览这些代码截图，不需要关注太多细节

### 1. Mixin

Mixin 是最早的 React 代码复用方案

```jsx
var SubscriptionMixin = {
  getInitialState: function() {
    return {
      comments: DataSource.getComments()
    };
  },

  componentDidMount: function() {
    DataSource.addChangeListener(this.handleChange);
  },

  componentWillUnmount: function() {
    DataSource.removeChangeListener(this.handleChange);
  },

  handleChange: function() {
    this.setState({
      comments: DataSource.getComments()
    });
  }
};

var CommentList = React.createClass({
  mixins: [SubscriptionMixin],

  render: function() {
    // Reading comments from state managed by mixin.
    var comments = this.state.comments;
    return (
      <div>
        {comments.map(function(comment) {
          return <Comment comment={comment} key={comment.id} />
        })}
      </div>
    )
  }
});
```

它的好处是简单粗暴，符合直觉，也确实起到了重用代码的作用；但是坏处也很明显，隐式依赖，名字冲突，不支持 class component，难以维护，总之，现在已经被完全淘汰了

给大家看一下官方判决书：https://reactjs.org/blog/2016/07/13/mixins-considered-harmful.html

## 2. HOC (higher-order component)

2015年，React团队判处Mixin死刑以后，推荐大家使用HOC模式，HOC是采用了设计模式里的装饰器模式

```jsx
function withWindowWidth(BaseComponent) {
  class DerivedClass extends React.Component {
    state = {
      windowWidth: window.innerWidth,
    }

    onResize = () => {
      this.setState({
        windowWidth: window.innerWidth,
      })
    }

    componentDidMount() {
      window.addEventListener('resize', this.onResize)
    }

    componentWillUnmount() {
      window.removeEventListener('resize', this.onResize);
    }

    render() {
      return <BaseComponent {...this.props} {...this.state}/>
    }
  }
  return DerivedClass;
}

const MyComponent = (props) => {
  return <div>Window width is: {props.windowWidth}</div>
};
```

经典的 容器组件与展示组件分离 (separation of container presidential) 就是从这里开始的

```jsx
// components/AddTodo.js

import React from 'react'
import { connect } from 'react-redux'
import { addTodo } from '../redux/actions'

class AddTodo extends React.Component {
  // ...

  handleAddTodo = () => {
    // dispatches actions to add todo
    this.props.addTodo(this.state.input)

    // sets state back to empty string
    this.setState({ input: '' })
  }

  render() {
    return (
      <div>
        <input
          onChange={(e) => this.updateInput(e.target.value)}
          value={this.state.input}
        />
        <button className="add-todo" onClick={this.handleAddTodo}>
          Add Todo
        </button>
      </div>
    )
  }
}

export default connect(null, { addTodo })(AddTodo)
```

https://www.jianshu.com/p/ddbbbb16f8b6

经典的 容器组件与展示组件分离 (separation of container presidential) 就是从这里开始的

一个很经典的HOC使用案例是react redux 中的 connect 方法，AddTodo组件像一只无辜的小白兔，它的addTodo方法是connect方法给它注入进去的

1. 可以在任何组件包括 Class Component 中工作
2. 它所倡导的 容器组件与展示组件分离 原则做到了：关注点分离

**缺点：**

1. 不直观，难以阅读
2. 名字冲突
3. 组件层层层层层层嵌套

## 3. Render Props

2017年，render props流行起来

```jsx
class WindowWidth extends React.Component {
  propTypes = {
    children: PropTypes.func.isRequired
  }

  state = {
    windowWidth: window.innerWidth,
  }

  onResize = () => {
    this.setState({
      windowWidth: window.innerWidth,
    })
  }

  componentDidMount() {
    window.addEventListener('resize', this.onResize)
  }

  componentWillUnmount() {
    window.removeEventListener('resize', this.onResize);
  }

  render() {
    return this.props.children(this.state.windowWidth);
  }
}

const MyComponent = () => {
  return (
    <WindowWidth>
      {width => <div>Window width is: {width}</div>}
    </WindowWidth>
  )
}
```

2017年，render props流行起来，它的缺点是，难以阅读，难以理解，下面是一个使用案例

## 4. Hook

大家看到上面的两种方法，它们最终的目的是什么呢？就是为了向组件注入 windowWidth 这个状态，为了这一个目的它们用了复杂又不直观的方法，有没有办法直观呢？那就是我们的 Hook 了，

还是上面相同的需求，我用Hook再实现一遍

```jsx
import { useState, useEffect } from "react";

const useWindowsWidth = () => {
  const [isScreenSmall, setIsScreenSmall] = useState(false);

  let checkScreenSize = () => {
    setIsScreenSmall(window.innerWidth < 600);
  };
  useEffect(() => {
    checkScreenSize();
    window.addEventListener("resize", checkScreenSize);

    return () => window.removeEventListener("resize", checkScreenSize);
  }, []);

  return isScreenSmall;
};

export default useWindowsWidth;
import React from 'react'
import useWindowWidth from './useWindowWidth.js'

const MyComponent = () => {
  const onSmallScreen = useWindowWidth();

  return (
    // Return some elements
  )
}
```

Hook相比其他方案的优点：

1. 提取逻辑出来非常容易
2. 非常易于组合
3. 可读性非常强
4. 没有名字冲突问题

Hook分两种，React自带Hook和自定义Hook，自定义Hook是有自带Hook组合而成的，所以我们先讲一下自带Hook

# 2. React 自带 Hook 详解

## 1. useState

useState 是最基础的一个Hook，为什么这么说呢，因为它是状态生产器。它产生的状态和普通变量有什么区别的？

```jsx
const [count, setCount] = useState(initialCount);
---------
const count = 1
const setCount = (value) => count = value
```

这两个有什么区别呢？区别就在于第一个useState产生的是状态，状态改变的时候组件会重新渲染，它是响应式的；而第二个，就是一个普通变量，它改变什么都不会发生，听起来是不是有点可怜呢

## 2. useEffect

有了useState产生的状态，我们就可以写一些简单的组件了，比如

```jsx
const Count = () => {
  const [count, setCount] = useState(0)
  const add = setCount(count + 1)
  return <button onClick={add}>add</button>
}
```

这样一个简单的计数组件

但是，这终归是自娱自乐，我们写的代码，要和这个组件外面的世界产生联系，我们的状态，要和外面的世界同步，才能产生工业的价值。我们将发生在外面的事情统称为副作用

比如说你想将count和服务器的代码同步，你想将count和手机的震动同步，这时候就需要用到useEffect了。要摒弃以前的生命周期的概念，useEffect的唯一作用就是同步副作用。

## 3. useContext

React 的组件化让我们可以将不同的业务代码分割开，但是也带来了一个问题，那就是组件间共享状态是非常不方便的。比如，你有个很多组件都会用到的状态，app 主题状态，如何让一个组件随时可以获取到这个状态呢？大家可能听过状态提升，它缓解但是并没有解决这个问题。而context就是为了解决这个问题，大家可以把它理解成是React自带的Redux，实际上Redux就是用context实现的

## 4. useReducer

大家知道useState是主要的状态生产器，这个useReducer就是另一个没那么常用的状态生产器。它适合状态逻辑很复杂的时候，或者下一个state值依赖于上一个state值，比如

```jsx
const initialState = {count: 0};

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {count: state.count + 1};
    case 'decrement':
      return {count: state.count - 1};
    default:
      throw new Error();
  }
}

function Counter() {
  const [state, dispatch] = useReducer(reducer, initialState);
  return (
    <>
      Count: {state.count}
      <button onClick={() => dispatch({type: 'decrement'})}>-</button>
      <button onClick={() => dispatch({type: 'increment'})}>+</button>
    </>
  );
}
```

这种情况用useState当然也可以，但是用useReducer就显得代码干净漂亮

## 5. useCallback/useMemo

这里React官网文档讲的非常不清楚。

给大家出一个判断题：父组件刷新，所有的子组件都会跟着刷新，这句话对吗？

这句话是对的，父组件刷新，所有的子组件都会刷新，这样听起来很耗性能，但是对于绝大多数组件来说，性能都是没有问题的，应为React真的很快。

但是对于耗性能的组件来说，这样就有很大的问题了，耗性能的组件不希望被经常刷新，所以我们可以用 React.memo包裹住它们，这样只有在它们的props变化的时候它们才会刷新。

这样又有一个问题，比如：

```jsx
const TestComp = () => {
  const value = {name: 'Jack'}
  return <MemoExpensiveList value={value}/>
}
```

大家看MemoExpensiveList是被React.memo给处理过的，它的props变化它才会刷新。但是在上面这个案例里，TestComp一刷新MemoExpensiveList就会刷新，这是为什么呢？原因就是，onClick在每次TestComp刷新时都会生成一个新的实例，{name: 'Jack'} ≠= {name: 'Jack'}

这就是 useMemo派上用场的时候了，我们可以用useMemo包裹住：

```jsx
const value = useMemo(() => {}, [])
```

这样它只会生成一个实例，也就不会骚扰到MemoExpensiveList了

而useCallback就是一个特殊版本的useMemo，专门来处理函数的

## 6. useRef

上面详细给大家讲了状态的概念，有时候我们希望创建一种类型的值，它不是状态，但是又可以在不同的render之间以同一个实例的形式存在。它有点类似于在class component里的 [this.xxx](http://this.xxx/)

```jsx
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

# 3. 自定义 Hook

自定义Hook是目前最好的重用React逻辑的方法，它和普通的函数很像很像，自定义Hook的特殊之处在于，它是有状态的，它返回的也是状态。所以在什么时候我们应该用到自定义Hook？那就是，我们想要抽象出处理状态的逻辑的时候

给大家举一个例子

```jsx
const Comp = () => {
  const [arr, setArr] = useState([1, 2])
  return <button onClick={() => setArr([...arr, value])}>add</button>
}
```

如果你发现你的app里有好几处这种数组处理，你可以

```jsx
export const useArray = <T>(initialArray: T[]) => {
  const [value, setValue] = useState(initialArray);
  return {
    value,
    setValue,
    add: (item: T) => setValue([...value, item]),
    clear: () => setValue([]),
    removeIndex: (index: number) => {
      const copy = [...value];
      copy.splice(index, 1);
      setValue(copy);
    },
  };
};
```

用这样一个自定义的Hook，不仅返回了状态，也返回了处理这个状态的方法

这个例子也展示了，自定义Hook可以以状态为核心，并将它和与它相关的东西封装在一起。这也符合我们编程的seperation of concert，也就是关注点分离的原则。关注点分离是大家写代码时一定要注意的事情，也就是说无关的代码不要放在一起，不然关注点混在一起，会让维护难度大大加大。

大家明天去看一下自己的代码，很可能会发现，有一些面条代码其实是可以用hook抽象出来的，给大家再举个例子

```jsx
const Comp = () => {
  const [id, setId] = useState(0)
  const [assets, setAssets] = useState()

    useEffect(() => {
      fetch(`https://google.com?id=${id}`).then(async response => {
       const data = await response.json();
        if (response.ok) {
          setAssets(data)
        } else {
          return Promise.reject(data);
        }
        })
      }, [])

  return <div>{assets.map(a => a.name)}</div>
}
```

这里的fetch的内容和这个组件关系大吗？不大，因为这个组件其实不怎么在乎fetch的细节，它只在乎拿到result.data，那么我们就可以用hook来抽象

```jsx
// util.ts
const useAssets = (id) => {
    const [assets, setAssets] = useState()

    useEffect(() => {
      fetch(`https://google.com?id=${id}`).then(async response => {
       const data = await response.json();
        if (response.ok) {
          setAssets(data)
        } else {
          return Promise.reject(data);
        }
        })
      }, [])
    return assets
}

// comp.tsx
const Comp = () => {
  const [id, setId] = useState(0)
  const assets = useAssets(id)

  return <div>{assets.map(a => a.name)}</div>
}
```

大家看，这样就实现了逻辑的分离

# API连接失败

1. 确保.env 和 .env.development 放在根文件夹，而不是放在src中
2. 确保.env 和 .env.development 中的变量名为 `REACT_APP_API_URL`, 务必按照视频中的来，不要自己取名字
3. 如果是json-server连接不成功，检查 json-server 启动了没有
4. 检查代码中的url是否正确，比如 `projects` 写成了 `project`

