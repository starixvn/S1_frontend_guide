# NextJS 的導轉路由設計 

NextJS 中主要可以透過三種方式做路由導轉 ：

1. `next/link` - 生成 `<Link/>` 標籤 
2. `next/router` - 透過 `useRouter` hook 來 trigger `router.push`
3. `<a>` - 原生 a 標籤


### `router.push()`

通常會綁定在 event handler ( onClick ) 上，或是在 function 處理時做路由導轉。

ex : 處理完 api / 登入後 跳至首頁。

因為是透過 `router.push()` 再不 reload 的情況下直接導轉，並不會在 HTML 頁面實際渲染 a 標籤，這沒有辦法被爬蟲爬到，不利於 SEO。

假如有 SEO 需求，避免使用 `router.push()`

### `<Link>`
    
NextJS 提供的 `<Link>` 會在 HTML 渲染 a 標籤，並且再不 reload 的情況下導轉路由。

同時解決 SEO 問題以及保持 React 的 SPA 特性。

99% 情境應使用 `<Link>` 作為頁面上元件的導轉機制。

ex : 首頁的各種 NavBar button , 頁面上各種連結 

### `<a>`

直接使用原生 a tag ，行為如同原生 a 標籤，會導轉至新頁面並 reload。

SEO 層面上雖與 `Link` 相同都會生成 a tag，但會導致頁面 reload 而失去 SPA 特性，除了少數必須強制 reload 頁面的情境下，應盡量避免使用原生 a tag。