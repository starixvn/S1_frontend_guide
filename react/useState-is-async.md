# useState 是非同步的！

### 案例 1 - prevState 應用

```js
const [show, setShow] = React.useState(false)
const handleClick = ()=>{
    show ? setShow(false): setShow(true)
}
return (<div>
        <button onClick={handleClick}>button</button>
    </div>
);
```

想當然爾，上面那段 Code 肯定是壞了

上面那段程式碼，主要是要用state來控制某些元件的顯示與否，然而在實際測試之後，發現在點擊時卻會出現無法順利顯示/關閉的情況。 

原因便在於 `setState` 在 update 時本身是非同步的，導致 update 時留下的微小「**縫隙**」，進而產生切換狀態時造成的無預期 bug。 
而將程式碼修改成以下便恢復正常了：

```js
const [show, setShow] = React.useState(false)
const handleClick = ()=>{
    setShow(prev => !prev)
}
return (<div>
        <button onClick={handleClick}>button</button>
    </div>
);
```

React <a href='https://zh-hant.reactjs.org/docs/hooks-reference.html#usestate'>官方文件</a>中提到 <br/>

>如果新的 state 是用先前的 state 計算出，你可以傳遞一個 function 到 setState。該 function 將接收先前的 state，並回傳一個已更新的值。

### 案例 2 - 緊鄰著 `setState` 執行 function 產生之 bug

我們先看下底下的程式碼，看結果之前可以先試著想一下 console 出來的結果會長怎樣：

```js
const [state , setState] = React.useState(0)

React.useEffect(() => {
    setState(1)
    console.log("1 : " , state)
} , [])

console.log('2 : ' , state)
```

公佈答案：
```js
// 2 : 0
// 1 : 0
// 2 : 1
```

上面只是簡化的示意版，然在專案中出bug的情境主要是： <br/>

> Admin 專案許多頁面在初始化時需要從 queryParams 取 timeZone <br/>
> `onTimeZoneChange` 後再 `handleSearch` 去撈 api 資料

由於 `onTimeZoneChange` 這個 hook 底層是用 `useState` 做撰寫，把情境帶回去上面的程式碼後就可以發現：

我們原先預期 `useEffect` 內的 function 會使用我們 `setState` 後的新值，然而事實並不如此，這個 `effect` 內所做的事情，總是會不如預期。 <br/>

將程式碼調整成底下樣子後便可以解決此問題：

```ts
useEffect(() => {
    const init = async () => {
        if (!isEmpty(queryParams)) {
            const { tz } = queryParams
            // 修改當前 timeZone 
            onTimeZoneChange(Number(tz))
            // call search function 明確傳入指定 timezone
            // 否則會吃到舊的 timezone，因為此時間點 timeZone 尚未更新
            handleSearch({
                currentTimeZoneUtcOffset: Number(tz),
            })
        }}

    init()
}, [])
```