---
title: 小米10内置字体在 React-Native 下被截断
date: 2021-08-05
tags: miui@11,react-native,内容截断
---

## 现象及复现条件

在 miui 11 的设备中，一个普通的自适应内容宽度 Text 元素在实际展示中**可能**出现尾部字符被截断的情况，多出现在内容为 **数值**或者**英文**的情况。

| 条件         | 值     |
| ------------ | ------ |
| miui         | 10     |
| react-native | 0.59.0 |

## 背景知识

React-Native 中的 `Text` 组件，如果没有显示的声明 `fontFamily` 样式属性，则会使用系统预设的字体，而通常情况下，我们考虑到让我们的应用在不同的终端下使用各自最优的显示方式，我们通常不会显示的去声明这个值。另外一个不声明 `fontFamily` 的原因就是 React-Native 中的 `fontFamily` 不支持 CSS 的声明多种字体的，我们没法声明一个同时在 iOS 和 android 中同时存在的字体。

## 解决方案

通过 `Platform.select` 设置安卓下全局 Text 组件的 `fontFamily` 值为 `""`，ios 下不设置 `fontFamily` 值。

ios 中不允许设置 `fontFamily` 为不存在的字体，所以 ios 下不要设置 `fontFamily` 的值。

android 中允许将 `fontFamily` 的值设置为任何值，当`fontFamily` 设置的字体不在设备中时，理论上是会执行降级操作的，使用默认的字体，所以仍能保证应用在各设备中仍优先使用系统预设字体，应用不会出现大的改动。但是此时在 miui 11 中，被截断的现象消失了（_原因不详！猜测应该是降级规则有问题？_）。
