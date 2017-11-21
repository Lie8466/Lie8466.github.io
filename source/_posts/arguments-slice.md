---
title: 浅析"对arguments对象使用Array.prototype.slice()可以将其转化为数组"
date: 2017-11-21 11:34:05
tags: JS
---

## 背景
《Javascript高级程序设计(第3版)》的250页有一句话叫“对arguments对象使用Array.prototype.slice()可以将其转化为数组”，为什么这么说？

## arguments
Js中的每一个函数(箭头函数除外)自动获得两个变量this和arguments。因此随便定义一个非箭头函数，可以打印出它的auguments;

```
> function add (a, b) { return arguments;}
> var arg = add (1, 2);
> arg  // 打印arg
```
打印结果
![](https://user-gold-cdn.xitu.io/2017/11/21/15fdc92725301f34?w=796&h=348&f=png&s=52299)

arg并不是一个数组，但是可以通过arg[0],arg[1]及arg.length来获取参数的一些属性。可以通过Array.prototype.slice()来将其转化为一个数组

![](https://user-gold-cdn.xitu.io/2017/11/21/15fdc993c8e020d5?w=882&h=578&f=png&s=85993)

上图中可以看出以下两点：

1.Array.prototype.slice()返回一个新数组
2.Array.prototype.slice()并不会影响其参数

## Array.prototype.slice()

Array.prototype.slice是怎么实现返回一个新数组的呢？网上也有一些通过看源码来解析其原理的文章，例如 http://www.cnblogs.com/henryli/p/3700945.html ，但是作为一个前端这个理解起来有一定的困难，我的建议是查看loadash对slice的实现来理解一下其原理。
文档:http://lodash.think2011.net/slice
源码：https://github.com/lodash/lodash/blob/master/slice.js

_.slice(array, [start=0], [end=array.length])
创建一个裁剪后的数组，从 start 到 end 的位置，但不包括 end 本身的位置。

```
/**
 * Creates a slice of `array` from `start` up to, but not including, `end`.
 *
 * **Note:** This method is used instead of
 * [`Array#slice`](https://mdn.io/Array/slice) to ensure dense arrays are
 * returned.
 *
 * @since 3.0.0
 * @category Array
 * @param {Array} array The array to slice.
 * @param {number} [start=0] The start position.
 * @param {number} [end=array.length] The end position.
 * @returns {Array} Returns the slice of `array`.
 */
function slice(array, start, end) {
  let length = array == null ? 0 : array.length
  if (!length) {
    return []
  }
  start = start == null ? 0 : start
  end = end === undefined ? length : end

  if (start < 0) {
    start = -start > length ? 0 : (length + start)
  }
  end = end > length ? length : end
  if (end < 0) {
    end += length
  }
  length = start > end ? 0 : ((end - start) >>> 0)
  start >>>= 0

  let index = -1
  const result = new Array(length)
  while (++index < length) {
    result[index] = array[index + start]
  }
  return result
}
```
因此当我们使用Array.prototype.slice.call(arg, 0)时，实际上返回了一个新的数组result,该数组的长度等于arg.length，其元素包含从0到arg.length的所有元素，即arg[0],arg[1]
