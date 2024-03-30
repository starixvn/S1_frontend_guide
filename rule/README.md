# Ailab Frontend 命名規範

## 資料夾命名
- 使用小駝峰命名法 (camelCase)
- 範例：`components`, `pages`, `utils`

## Component 元件命名
- 使用大駝峰命名法 (PascalCase)
- 範例：`Header`, `LoginForm`, `Button`

## Component file命名
- 以元件名稱命名，使用小駝峰命名 (camelCase)
- 範例：`Header.tsx`, `LoginForm.tsx`, `Button.tsx`

## Pages 路徑命名
- 使用破折號命名 (kebab-case)
- 範例：`pages/login.tsx`, `pages/register.tsx`, `pages/change-password.tsx`

## function 命名
- 使用小駝峰命名法 (camelCase)
- 範例：`calculateTotalBalance`, `formatUserName`

## i18n displaykey 命名
- 使用 `Label_XXXX_XXXX` 格式。
- 範例：`Label_Home_Welcome`, `Label_General_MoreInfo`
- 第一個 `XXXX` 代表使用的區域 (頁面或功能)
- 第二個 `XXXX` 是實際的 display key