---
date: 2022-03-14
title: React Hooks
categories:
  - react
featured_image: '-'
---
Hook 是 React 16.8 的新增特性。可以让你在不编写 class 的情况下使用 state 以及其他的 React 特性
Hook 使你在无需修改组件结构的情况下复用状态逻辑，解决了在组件之间复用状态逻辑很难的问题。
Hook 将组件中相互关联的部分拆分成更小的函数（比如设置订阅或请求数据），而并非强制按照生命周期划分。

### State Hook
```javascript
import React, { useState } from 'react';

 function Example() {
  // count 的值为 useState 返回的第一个值, setCount是返回的第二个值
  const [count, setCount] = useState(0);

   return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
       Click me
      </button>
    </div>
  );
}
```

### Effect Hook
useEffect 做了什么？ 

通过使用这个 Hook，你可以告诉 React 组件需要在渲染后执行某些操作。React 会保存你传递的函数（我们将它称之为 “effect”），并且在执行 DOM 更新之后调用它。

将 useEffect 放在组件内部让我们可以在 effect 中直接访问 count state 变量（或其他 props）。

useEffect 会在每次渲染后都执行, 每次我们重新渲染，都会生成新的 effect，替换掉之前的。若加入依赖，则根据依赖进行判断是否执行 effect

如果你的 effect 返回一个函数，React 将会在执行清除操作时调用它

```javascript
useEffect(() => {
    document.title = `You clicked ${count} times`;
}, []);
```
如果你传入了一个空数组（[]），effect 内部的 props 和 state 就会一直拥有其初始值

Effect 性能优化：传递数组作为 useEffect 的第二个可选参数。（请确保数组中包含了所有外部作用域中会随时间变化并且在 effect 中使用的变量，否则你的代码会引用到先前渲染中的旧变量）

tips: 在订阅外部数据源情况下，需要进行副作用清除，可防止引起内存泄露！

典型场景可能有三个：
异步获取数据后的赋值。
使用setInterval或者setTimeout的。
添加监听事件addEventListener的。

### Callback Hook
返回一个 memoized 回调函数。

把内联回调函数及依赖项数组作为参数传入 useCallback，它将返回该回调函数的 memoized 版本，该回调函数仅在某个依赖项改变时才会更新。当你把回调函数传递给经过优化的并使用引用相等性去避免非必要渲染
```javascript
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

### Memo Hook
把“创建”函数和依赖项数组作为参数传入 useMemo，它仅会在某个依赖项改变时才重新计算 memoized 值。这种优化有助于避免在每次渲染时都进行高开销的计算。
如果没有提供依赖项数组，useMemo 在每次渲染时都会计算新的值。

```javascript
const value= useMemo(() => {
    const stu = {
      name: '小明',
      age: '12'
    }
    return stu.name
}, [])
```

### Ref Hook
useRef 返回一个可变的 ref 对象，其 .current 属性被初始化为传入的参数（initialValue）。返回的 ref 对象在组件的整个生命周期内持续存在

```javascript
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` 指向已挂载到 DOM 上的文本输入元素
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
```

### 自定义 Hook 🌰
```javascript
import { useState, useEffect } from 'react';

function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}
```

```javascript
function FriendStatus(props) {
  const isOnline = useFriendStatus(props.friend.id);

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}

function FriendListItem(props) {
  const isOnline = useFriendStatus(props.friend.id);

  return (
    <li style={{ color: isOnline ? 'green' : 'black' }}>
      {props.friend.name}
    </li>
  );
}
```
