# Ant-Design Form 

***備註：我們使用的版本是 `4.23.1`，底下皆以這版本為主。***

## 目錄
- Basic
  - [元件結構](#元件結構)
  - [表單元件綁定](#表單元件與-form-綁定)
  - [FormItem 綁定 Switch 或 CheckBox](#formitem-綁定-switch-與-checkbox-時)
  - [加入驗證](#加入驗證)
  - [表單初始值](#表單初始值)
- [FormInstance](#forminstance)
  - [設定表單欄位指定值](#設定表單欄位指定值-setfieldsvalue)
  - [初始化來自於 api 的資料](#初始化來自於-api-的資料)
  - [在子元件使用FormInstance](#在子元件中使用-forminstance)
  - [重置表單](#重置表單-formresetfields)
  - [提取表單資料](#提取表單資料-getfieldsvaluegetfieldvalue)
  - [監聽表單資料變化](#監聽表單資料變化-formusewatch)
---

## 基本用法：
***接下來範例會以著重在資料處理為主，程式碼中跟排版介面相關的內容會全部簡化***
### 元件結構

```tsx
// Form 表單
<Form>
    // 透過 Form.Item 將 Input 元件與 Form 綁定
    <Form.Item>
        <Input/>
    </Form.Item>
</Form>
```

### 表單元件與 Form 綁定

```tsx
// label 表示顯示的標籤，name 則是表單物件的 key
<Form.Item label="Username" name="username">
    <Input />
</Form.Item>
```

透過前兩個範例，可以很快理解，Antd Form 元件的使用方式，只要是被包在 `<Form>` 元件內的 `<Form.Item>` 均會被加到表單中，透過 `<Form.Item>` 的 `name` ，我們可以很輕鬆的定義它在表單物件內的 key 。

### Form.Item 綁定 Switch 與 Checkbox 時：

```tsx
<Form.Item name="remember" valuePropName="checked">
    <Checkbox>Remember me</Checkbox>
</Form.Item>
```

絕大多數的表單元件其 value 都會直接被綁定，但有少數如 Checkbox , Switch 等需要透過綁定 `valuePropName="checked"` ，才可以正常運作。

### 加入驗證

```tsx
<Form.Item
      label="Username"
      name="username"
      rules={[{ required: true, message: 'Please input your username!' }]}
>
    <Input />
</Form.Item>
```

直接在 `Form.Item` 加入 rules，便會在 submit 時做驗證，上面範例加入 `required` 必填驗證。

[rules 可支援的參數](https://ant.design/components/form#rule)

### 表單初始值

```tsx
<Form initialValues={{ remember: true }}>
    // ...
</Form>
```

透過 `<Form>` 提供的 `initialValues` 可以給予表單特定欄位初始值或避免 `undefined` 資料發生。

### submit

```tsx
const onFinish = (values: any) => {
  console.log('Success:', values);
};

const onFinishFailed = (errorInfo: any) => {
  console.log('Failed:', errorInfo);
};

<Form
    initialValues={{ remember: true }}
    onFinish={onFinish}
    onFinishFailed={onFinishFailed}
>
    // ...
    <Form.Item >
        <Button type="primary" htmlType="submit">
            Submit
        </Button>
    </Form.Item>
</Form>
```

建立一個 `htmlType="submit"` 的 button 後便可以執行 submit ，可以從 `Form` 的 `onFinish` 與 `onFinishFailed` 取得相對應的資料做處理。

假如有 rule 沒通過時，便會跑到 `onFinishFailed` 去，十分銀杏！

最後我們把上面所有片段組合起來，完整官方範例 [CodeSandBox](https://codesandbox.io/s/b543dj?file=/demo.tsx)

```tsx
const onFinish = (values: any) => {
  console.log('Success:', values);
};

const onFinishFailed = (errorInfo: any) => {
  console.log('Failed:', errorInfo);
};

const App: React.FC = () => (
  <Form
    initialValues={{ remember: true }}
    onFinish={onFinish}
    onFinishFailed={onFinishFailed}
  >
    // userName Input
    <Form.Item
      label="Username"
      name="username"
      rules={[{ required: true, message: 'Please input your username!' }]}
    >
      <Input />
    </Form.Item>
    // remember me checkbox
    <Form.Item name="remember" valuePropName="checked">
      <Checkbox>Remember me</Checkbox>
    </Form.Item>
    // submit
    <Form.Item >
      <Button type="primary" htmlType="submit">
        Submit
      </Button>
    </Form.Item>
  </Form>
);
```

以上，我們便完成了一個最基本帶**必填驗證**、**初始值**以及 **submit** 的表單！

但，真實情況往往沒那麼單純，我們的表單不一定每次都要 submit、我們可能需要在不透過操作元件的情況下異動表單內的資料...等的操作。

而這時候我們便需要透過 Antd 提供的 `FormInstance` 來解決問題，這也是我們在實務上使用 Form 時 99% 選用的方式。

--- 

## FormInstance

關於 `FormInstance`，我們翻閱官方文件時可以看到，`<Form>` 元件內的 `form` property 便是提供我們傳入 `FormInstance` 的地方。底下是關於 `form` property 的描述：

> Form control instance created by Form.useForm(). Automatically created when not provided

所以其實在我們前面的範例中，就算我們沒有特別建一個 `FormInstance` 的情況下，也會自動在表單中建立，那麼他究竟有什麼用呢？

網路上對於 `FormInstance` 的完整介紹非常少，以下是簡單的解釋：

> FormInstance 是封裝了表單操作的對象，提供我們直接操作儲存資料的地方，而非一定要透過各種受控元件才可以對資料進行操作。

### Usage

```tsx
import { Form } from 'antd';

interface IForm {
    userName : string
    remember : boolean
}

function App () {
   // ts 底下， formInstance 可以定義 interface，在做後續操作時會驗證 type
   // type Form.useForm = (): [FormInstance]
   // 建立 FormInstance
  const [form] = Form.useForm<IForm>();

  return (
    <Form form={form}>
      {/* ... */}
    </Form>
  );
};
```

### 設定表單欄位指定值 - `setFieldsValue()`

```tsx
function App () {
  const [form] = Form.useForm();

  return (
    <Form form={form}>
        <Form.Item name="input1">
            <Input onChange={(e) => form.setFieldsValue({input2: e.target.value})} />
        </Form.Item>
        <Form.Item name="input2">
            <Input />
        </Form.Item>
    </Form>
  );
};
```

從上面範例可以看到，我們透過綁定在 `input1` onChange 上的 function ，可以達到 **在 input1 輸入時，input2 會同步更新**。

### 初始化來自於 api 的資料

`setFieldsValue` 除了可以在使用者不操作元件的情況下去設置各個欄位的值，我們在實務上，很容易遇到：**需要在 Form 初始化從 api 取得的資料。**

這時若使用先前範例中的 `initialValues` 的話，是完全不會 work 的，因為在 api 取得資料前，Form 就已經初始化完成了。

這時候可以透過 `useEffect` 搭配 `setFieldsValue` 在 api loading 完成後去設置資料。

```tsx
import { useEffect } from  "react";
import { Form, Input } from "antd";

function App() {
  const [form] = Form.useForm();
  useEffect(() => {
    // 模擬從 api 取資料
    setTimeout(() => {
      form.setFieldsValue({
        name: "Wen",
        age: 18,
      });
    }, 1000);
  }, []);
  return (
    <Form form={form}>
      <Form.Item name="name">
        <Input />
      </Form.Item>
      <Form.Item name="age">
        <Input />
      </Form.Item>
    </Form>
  );
}
```

### 在子元件中使用 FormInstance

在 `4.20.0` 版之前，想要在子元件中使用 `FormInstance` 的話，我們必須透過傳遞 `FormInstance` 至子元件內才有辦法做使用，而現在可以透過 `Form.useFormInstance` 在子元件直接使用。

```tsx
import { Form, Button } from "antd";

const Sub = () => {
  const form = Form.useFormInstance();

  return <Button onClick={() => form.setFieldsValue({})} />;
};

export default () => {
  const [form] = Form.useForm();

  return (
    <Form form={form}>
      <Sub />
    </Form>
  );
};
```

### 重置表單 - `form.resetFields()`

提供我們重置表單的功能，可以用在提交表單後、或是透過獨立重置 Button 來達到重置效果

```tsx
import React from "react";
import { Form, Input, Button } from "antd";

function App() {
    const [form] = Form.useForm();
    const handleReset = () => {
        // 用 form.resetFields() 來重置表單
        form.resetFields();
    };

    return (
        <Form form={form}>
            <Form.Item name="name">
                <Input />
            </Form.Item>
            <Form.Item name="age">
                <Input />
            </Form.Item>
            <Button onClick={handleReset}>重置</Button>
        </Form>
    );
}
```

也可以指重置特定欄位：

```tsx
import React from "react";
import { Form, Input, Button } from "antd";

function App() {
    const [form] = Form.useForm();
    const handleReset = () => {
        // 用 form.resetFields() 來重置表單
        form.resetFields(['name']);
    };

    return (
        <Form form={form}>
            <Form.Item name="name">
                <Input />
            </Form.Item>
            <Form.Item name="age">
                <Input />
            </Form.Item>
            <Button onClick={handleReset}>重置姓名</Button>
        </Form>
    );
}
```

> Tips : form 實際上會 reset 至 `initialValues`

### 提取表單資料 - `getFieldsValue()`、`getFieldValue()`

```tsx
// 取多個 field ，可以傳入 NamePath[] 來回傳特定資料
const { userName } = form.getFieldsValue()

// 取特定 field
const userName = form.getFieldValue("userName")
```

`getFieldsValue` 和 `getFieldValue` 就如同字面上意思，一個是取多欄位、一個是取單一。

`getFieldsValue()` 在沒有傳入任何值時，預設會取所有 **mounted** 表單欄位。

上面有特別強調 `mounted`，原因在於，有些時候我們會透過 Form 來存取特定表單資料，像是：`id` 之類的，而那些資料並不需要顯示在畫面上給 user 更改。或是我們的表單有搭配 `Tab` 等元件使用時，在沒做特定處理的情況下，沒有選中的 `Tab` 預設都是 `unmounted` 的。

這時若要取值時，`getFieldsValue()` 是拿不到沒有 mounted 的資料的，可以加上 `getFieldsValue(true)` 來強制取得所有資料，又或是 `getFieldValue("id")` 取得指定欄位資料。

小整理：

1. `getFieldsValue()` 只會取到 `mounted` 資料，要同時取得不在畫面上的需使用 `getFieldsValue(true)`。
2. `getFieldsValue()` 只要有給予指定 `namePath`，不管有沒有 `mounted` 都拿得出來。

### 監聽表單資料變化 - Form.useWatch()

```tsx
import { useEffect } from "react";
import { Form, Input } from "antd";

const Demo = () => {
  const [form] = Form.useForm();
  const userName = Form.useWatch('username', form);

  useEffect(() => {
      console.log(userName)
      // doSomeSideEffect
  } , [userName])
  return (
    <Form form={form}>
      <Form.Item name="username">
        <Input/>
      </Form.Item>
    </Form>
  );
};
```

`Form.useWatch("要監聽的欄位" , FormInstance)` 監聽的值**會觸發畫面的 re-render**，可以用來監聽特定欄位的即時資料，以便即時更新畫面。

又或是可以搭配 `useEffect` 來根據資料異動觸發副作用，但這會發生一點問題：

- useEffect 會在 mounted 後默認執行一次，可能需要在裡面加入更多邏輯判斷來確保運行符合我們期望。

- 若是有明確 `onChange` 變化的時候，可以透過 `onChange` 來執行副作用，避免非預期地觸發。

**總結來說，在可以用 `onChange` 處理的情況下，還是建議多使用 `onChange` 來處理。**

少數非得用 `useWatch` 的情境：

- 需要監聽多個欄位資料做相對應邏輯處理

- 直接透過 `setFieldsValue` 異動欄位資料時 (不會觸發 `onChange`)

- 根據資料變化做條件渲染