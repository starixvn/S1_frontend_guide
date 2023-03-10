# 正確且安全的更新 state 內的 Arrays/Objects


## 請永遠把 state 當作是 immutable 的！

首先我們先看看[React](https://beta.reactjs.org/learn/updating-arrays-in-state)官方文件怎麼說
> Arrays are mutable in JavaScript, but you should treat them as immutable when you store them in state. Just like with objects, when you want to update an array stored in state, you need to create a new one (or make a copy of an existing one), and then set state to use the new array.

再來我們先看一下簡化版的情境：

```js
import { useState } from 'react';

function App() {
  const [list, setList] = useState([1]);

  const handleButtonClick = () => {
    list.push(2)
    setList(list)
  };

  return (
    <div>
      {list.map(value => <p>{value}</p>)}
      <button onClick={handleButtonClick}>add</button>
    </div>
  )
}
```


解釋一下真實情境，實際上這功能在每次 `click` 之後會去撈取特定 api 資料，拿到資料後再經過一系列的轉換，將符合前端畫面的資料，塞回原本已經存在的 `list`

因為中間做了很多資料結構的轉換，所以在一開始除錯的時候，我們花了大把時間在確認「轉換」的 function 行為是否異常導致元件無法正確更新，最後才發現bug居然藏在最後的 `setState` 中

接著我們回到上面的案例，當事人記得 **「 觸發畫面 re-render 需要 setState 」**，但卻忘記最重要的 immutable，而他在先前 debug 時，表示曾在下面塞過 log，確實 Array 也有被異動：

```js
import { useState } from 'react';

function App() {
  const [list, setList] = useState([1]);

  const handleButtonClick = () => {
    list.push(2)
    console.log(list)  // (2) [1, 2]
    setList(list)
  };

  return (
    <div>
      {list.map(value => <p>{value}</p>)}
      <button onClick={handleButtonClick}>add</button>
    </div>
  )
}
```
> 欸不是！我傳進去的資料確實不一樣啊

## setState 的畫面更新機制

我們可以從 React 的底層邏輯來解釋這樣子的行為，底下附上資料來源，這裡僅提下重點：

**React 在 `setState` 時會先以 `Object.is()` 來檢查新舊 state 有無變更，如果相同的話則不會做更新**

也就是說，當我們直接去 mutate state 時，就算 state 本身的值已經被改變，但在傳入 `setState` 時因為根本上物件的位址相同，所以並不會觸發後續的動作，進而導致畫面沒有任何更新

推薦閱讀：
[React 畫面更新的核心機制（下）：Reconciliation](https://ithelp.ithome.com.tw/articles/10298053)

## 如何正確的更新儲存在 state 內的 Array / Object

回到最初 React 官方文件中提到的：

> 當你要更新 state 中的陣列 (物件) 時，你應該造一個新的 (或複製現有的)，並 setState 新的陣列 (物件)

總之面對陣列(物件)，我們永遠應該用新的去取代舊的：

```js
import { useState } from 'react';

function App() {
  const [list, setList] = useState([1]);

  const handleButtonClick = () => {
    const clone = [...list];
    clone.push(2);
    setList(clone);
  };

  return (
    <div>
      {list.map(value => <p>{value}</p>)}
      <button onClick={handleButtonClick}>add</button>
    </div>
  )
}
```

將程式碼改掉後，行為便恢復正常了，範例只有使用擴展符，要用 `lodash` 的 `cloneDeep` 之類的也都是沒問題的，只要確保是複製一個新的就好。