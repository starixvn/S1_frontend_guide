# 合併運算符(??)與 OR (||)運算子

### Logical OR (||)
再 ES2020 之前僅有 `&&` 與 `||` 可以做使用，而 `||` 在使用時會判對第一個(左側)值是否為「Falsy」若為 falsy 則回傳右側值：

```js
// 使用 ||
const value = a || b

// 原始寫法，為方便示意，其實不用 Boolean(a)
const value = Boolean( a ) ? a : b

// if...else 寫法
let value
if ( a ) {
   value = a
} else {
   value = b
}
```

### Nullish coalescing operator (??)
`??` 和 `||` 的主要差異則在於，他僅會判斷值是否為「Nullish」，僅有 `undefined` 和 `null` 時才會使用右側值

```js
// 使用 ??
const value = a ?? b

// 假如換成三元式
const value = (a !== undefined && a !== null) ? a : b

```

### 實戰

**我們先上情境 1**

> 當我們有一個大量的用戶資料 Table 要顯示字串資料，需求端希望用戶沒有填資訊的時候，不要顯示空白內容，給予預設值 N/A

此時我們接到 api 資料時便可以這樣寫：

```js
// 假裝 data 是 api 接到的資料
const displayValue = data.name || "N/A"
```

為何要這樣用呢？  因為這個 case 我們明顯不希望他顯示空白，然而某些時候用戶沒有輸入資料時，我們在儲存時可能會存 `""`

所以在這裡我們便可以透過 `||` 來避免 `""` 的出現

**情境2**

> 當我們今天換成要顯示一個數據的 Table , 裡面都是數字，用戶一樣希望沒有設定的內容顯示 N/A

此時我們可以：

```js
// 假裝 data 是 api 接到的資料
const displayValue = data.total ?? "N/A"
```

這裡因為我們可以很明確知道 `0` 可能也是要呈現的資料，透過 `??` 就可以讓 0 可以正確的呈現出來，而不會變成 N/A

總結來說：
>在使用上字串(string)的處理，我大多會用 `||` 來避免空字串的出現，而數字(number)我會傾向使用 `??` 來避免 0 無法正確顯示。
