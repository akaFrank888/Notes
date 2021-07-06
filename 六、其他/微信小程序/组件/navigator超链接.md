# navigator超链接



主要讲了nacigator标签中的 url和 open-type 属性

- url是跳转的链接（只能是小程序内的链接，不能是如百度的链接）
- open-type是打开方式，其值可以为：
  - 默认，即可返回
  - redirect，即不可返回
  - reLaunch，即不可返回且可携带参数



代码：

```wxml
<view>
  <navigator url="/pages/logs/logs">跳转到log页面可返回</navigator>
  <navigator url="/pages/logs/logs" open-type="redirect">跳转到log页面并不返回</navigator>
</view>
```

