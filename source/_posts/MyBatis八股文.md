---
title: MyBatis八股文
date: 2024-06-26 17:53:37
tags:
  - Java
  - MyBatis
  - 八股文
categories: Java
cover: https://cloud.lushiwu.top/f/pvh9/%E3%80%90%E5%B9%B2%E7%89%A9%E5%A6%B9%E5%B0%8F%E5%9F%8B%E3%80%912024-06-26%2017_55_33.png
---

# MyBatis常见面试题总结

### #{} 和 ${} 的区别是什么？

- `${}`是 Properties 文件中的变量占位符，它可以用于标签属性值和 slq 内部，属于原样文本替换，可以替换任意内容。
- `#{}`是 sql 的参数占位符，Mybatis 会将 sql 中`#{}`替换为？号，在 sql 执行前会使用PreparedStatement 的参数设置方法，按序给 sql 的？号占位符设置参数值。使用`#{}`可以有效防止 sql 注入。
