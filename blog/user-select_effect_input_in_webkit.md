---
title: 小米10内置字体在 React-Native 下被截断
date: 2021-08-11
tags: CSS,input,webkit,无法输入
---

## 现象及复现条件

在移动端的页面中，我们常常会通过设置 `user-select: none` 来禁用用户复制行为，避免由 touch 行为带来的误触，但是该属性在 `webkit` 内核下会导致部分元素出现异常行为，如果 `input` 会无法输入，但是可以聚焦。可以在 Safari 下测试该代码：

```html
<style>
input {
  -webkit-user-select: none; // 这句声明才会导致 webkit 出错
  -khtml-user-select: none;
  -moz-user-select: none;
  -o-user-select: none;
  user-select: none;
}
</style>

<input />
```

## 解决方案

目前 （2021-08-11）官方还未修复该问题，可以通过避开为 `input`、`textarea` 设置 `-webkit-user-select: none` 来规避这个问题。


## 参考

- webkit bug地址：[https://bugs.webkit.org/show_bug.cgi?id=82692](https://bugs.webkit.org/show_bug.cgi?id=82692)