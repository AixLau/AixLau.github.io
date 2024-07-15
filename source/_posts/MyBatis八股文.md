---
title: MyBatis应用与总结
date: 2024-06-26 17:53:37
tags:
  - Java
  - MyBatis
  - 总结
categories: Java
cover: https://alist.aixcc.top/d/OneDrive/img/202407151140097.webp
---

# MyBatis常见面试题总结

### #{} 和 ${} 的区别是什么？

- `${}`是 Properties 文件中的变量占位符，它可以用于标签属性值和 slq 内部，属于原样文本替换，可以替换任意内容。
- `#{}`是 sql 的参数占位符，Mybatis 会将 sql 中`#{}`替换为？号，在 sql 执行前会使用PreparedStatement 的参数设置方法，按序给 sql 的 ？号占位符设置参数值。使用`#{}`可以有效防止 sql 注入。

