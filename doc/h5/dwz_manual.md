# DWZ Mobile 使用技巧

> ## 事件处理

### 绑定事件并传参数

```javascript
$("body").on(
  "touchstart",
  function (event) {
    console.log(event, event.data);
  },
  { test1: "a1", test2: "b1" }
);
```

### 自定义事件并传参数

```javascript
$("body").on("custom.event", function (event) {
  console.log(event, event.data);
});
$("body").trigger("custom.event", { test1: "a1", test2: "b1" });
```

### 绑定多个事件, 解除绑定事件

```javascript
$("body").on("touchstart", testFn, { test: 123 });
$("body").on(
  "touchstart",
  function (event) {
    console.log(event.data);
    $("body").off("touchstart", testFn); // 解除指定的touchstart事件testFn
    // $('body').off('touchstart'); // 解除当前元素全部touchstart事件
  },
  { test: 124 }
);
function testFn(event) {
  console.log(event.data);
}
```

### 判断是否绑定事件 isBind() 方法

```javascript
$(document).on("touchstart touchmove", function (event) {
  console.log(event.type);
});
console.log($(document).isBind("touchstart"));
```

### 绑定事件传参数

```javascript
$("body").on(
  $.event.hasTouch ? "touchstart" : "click",
  function (event) {
    console.log(this.data);
    event.preventDefault();
  },
  { test1: "a", test2: "b" }
);
```

### trigger 传参数

```javascript
$("body").on("custom.event", function (event) {
  console.log(this.data);
  event.preventDefault();
});
$("body").trigger("custom.event", { test1: "a1", test2: "b1" });
```

### 绑定多个事件, 空格分隔的多个事件名称

```javascript
$(document).on("touchstart touchmove", function (event) {
  console.log(event.type);
});
```

> ## 其它技巧

### dwz.eavl() 方法

```javascript
function test1() {
  alert(1);
}
const test2 = dwz.eavl("test1");
test2();
```

### dwz_interceptor 请求拦截器

```html
<a data-href="my.html?dwz_interceptor=checkLogin" target="navView" rel="my"
  >My</a
>
```

### 或者定义 dwz 全局拦截函数

```javascript
$.urlInterceptor = function (url) {
  // Todo
};
```

### dwz_callback 页面加载回调函数

```html
<a href="test.html?dwz_callback=testPageRender" target="navView" rel="test"
  >test</a
>
```

```js
function testPageRender(html) {
  this.html(html).initUI();
}
```

### dwz_helper 页面加载辅助函数

dwz_helper 链接示例

```html
<a href="test.html?dwz_helper=script" target="navView" rel="test">test</a>
```

dwz_helper 页面内容示例

```html
<div
  class="dwz-calendar tb-line"
  minDate="{%y-3}-%M-%d"
  maxDate="%y-%M-{%d+3}"
></div>
<script type="text/javascript">
  function helper(tpl, params) {
    this.find(".dwz-calendar").calendar();
  }
</script>
```

### ajaxTodo 用于收藏、关注等操作

```html
<a class="active" target="ajaxTodo" data-url="{{serverUrl}}">test</a>
```

### unitBox

```js
$box.parentsUnitBox();
$box.parentsUnitBox("myClass");
```

### parentsUntil

```javascript
$box.parentsUntil(function () {
  return $(this).is("selector");
});
```

### parentsByTag

```javascript
$box.parentsByTag("form");
```

> ## JS 模版

[腾讯 art-template](https://github.com/aui/art-template)

- https://aui.github.io/art-template/zh-cn/docs/
- https://aui.github.io/art-template/docs/
- https://blog.csdn.net/pupilxiaoming/article/details/77118855

```javascript
// 基于模板名渲染模板
template(filename, data);

// 将模板源代码编译成函数
template.compile(source, options);

// 将模板源代码编译成函数并立刻执行
template.render(source, data, options);
```

> ## 跨域

> Apache 测试环境跨域

RewriteEngine On
RewriteRule ^/app_proxy/(.\*)$ http://api.xxx.com/$1 [P,L]

/etc/apache2/httpd.conf 放开 proxy 和 mod_rewrite 模块

> 开发环境跨域 chrome 跨域

Allow-Control-Allow-Origin: \*
