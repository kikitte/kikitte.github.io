---
layout: post
title:  "Numpy条件表达式的一种实现方法"
date:   2021-03-25 10:18:50 +0800
categories: geographic
---

### Numpy条件表达式的一种实现方法

- 目的

  实现类似numpy.where函数的功能`numpy.``where`(*condition*[, *x*, *y*])

  Return elements chosen from *x* or *y* depending on *condition(*根据条件的真假从x或y中选择一个元素返回, True -> x and False -> y).

- 另一种方法

  (condition) * A + (not condition) * B

  当condition为True时其值为1, not condition其值为0，所以(condition) * A + (not condition) * B等于 A。同理, 当condition为False时(condition) * A + (not condition) * B等于B。

- 应用

  gdal_calc.py中可以直接使用numpy的数组函数(array function)，原有目的使用纯表达式替换where函数。但是目前已经确认gdal_calc可以直接使用where函数。

