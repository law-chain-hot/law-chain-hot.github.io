---
title: How to set markdown table column width
date: 2021-03-16 22:56:20
description: 
  在编辑《影书单》页面的时候，自动计算出来的 Table 宽度不符合我的审美🤷‍♀️。  
  所以我找了如何设置 Table 的列的宽度，发现下面这种方法最好用，一步解决烦恼。
toc: 
tags: 
category: technology
---

>在编辑《影书单》页面的时候，自动计算出来的 Table 宽度不符合我的审美🤷‍♀️。  
>所以我找了如何设置 Table 的列的宽度，发现下面这种方法最好用，一步解决烦恼。

<!--more-->

# 代码示例
```js
// table.md

<style>
    table th:first-of-type {
        width: 10%;
    }
    table th:nth-of-type(2) {
        width: 30%;
    }
    table th:nth-of-type(3) {
        width: 50%;
    }
</style>



| a               | b               | c               |
| --------------- | --------------- | --------------- |
| 列宽 = 10% 行宽   | 列宽 = 30% 行宽 | 列宽 = 60% 行宽 |
```

# 参考
- [Markdown 技巧：如何改变表格宽度（列宽）？- 知乎](https://stackoverflow.com/questions/19183180/how-to-save-an-image-to-localstorage-and-display-it-on-the-next-page)
