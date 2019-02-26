# NEL_Block_API

<aside class="notice">
This error section is stored in a separate file in <code>includes/_errors.md</code>. Slate allows you to optionally separate out your docs into many files...just save them to the <code>includes</code> folder and add them to the top of your <code>index.md</code>'s frontmatter. Files are included in the order listed.
</aside>
The Kittn API uses the following error codes:



### getnodetype

返回对应的Cli Node 的网络类型（主网、测试网）



#### 请求方法

post

#### 请求参数

| -参数名称- | -是否必须- | -类型- | -默认值-    | -说明- |
| ---------- | ---------- | ------ | ----------- | ------ |
| jsonrpc    | true       | string | 2.0         |        |
| method     | true       | string | getnodetype |        |
| params     | true       | array  |             |        |
| id         | true       | number | 1           |        |

#### 响应数据

| 参数名称 | 是否必须 | 数据类型 | 描述 |
| -------- | -------- | -------- | ---- |
| jsonrpc  | true     | string   |      |
| id       | true     | number   |      |
| result   | true     | array    |      |
| nodeType | true     | string   |      |







`GET http://api.nel.group/testnet`