---
title: "Sleep Sort的思考\U0001F914(SetTimeout的应用)"
date: 2021-03-15 15:10:33
description: 
    用 JS 实现 sort 的方法很多。其中有一个非常有趣的sort类型，sleep sort，原理是基于 SetTimeout 的计时功能，达到延时输入的目的。
    但是这个方法有一个问题，问题的根源是由于 SetTimeout 和 Task Queue 的机制。
toc:
tags:
category:
---

<img src="Sleep-sort.png" width="80%" />

上图便是知名的 `Sleep Sort`。

问题的根源是
- SetTimeout starts counting after executed
- **`SetTimeout()` 被执行时，便开始计时**，而一旦**计时结束**，SetTimeout的内容才会被放入 Task Queue （作为宏任务 marcotask）

所以一旦 `arr` 长度过长，就出现下面下面情况。

<img src="Sleep-sort-2.png" width="130%" alt="例子"/>

