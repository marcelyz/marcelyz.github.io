---
layout: post
title:  "sql问与答"
author: "marcelyz"
---

- **常见窗口函数**  
答：主要用于解决对一组数据进行操作。主要有二类：排序函数(rank/dense_rank/row_number)和聚合函数(max/min/avg/count)。它与group by分组聚合的区别是它不会减少原表中的行数。row_number属于没有重复值的排序(即使两条记录相同也不重复)；dense_rank属于连续排序(两条记录相同，序号相等)；rank属于跳跃排序(两个第二名后面是第四名)

- **row_number().over(Window.partitionBy().orderBy())**  
答：row_number()是排名的窗口函数，Window.partitionBy().orderBy()创建一个窗口，使用orderBy去排序，其中over来指定窗口函数作用的窗口。  

- **lag和lead函数**  
答：lag和lead函数是跟偏移量相关的窗口函数，通过这两个函数可以在一次查询中取出同一字段的前N行(lag)和后N行(lead)的数据，使用over指定范围，里面可以使用partition by指定分组，使用order by指定排序。语法lead(field, num, defaultvalue)；常见题有求股票的波峰和波谷。https://blog.csdn.net/godlovedaniel/article/details/118946573。  

- **having关键字**  
答：通常where语句是在group by之前做数据筛选的，而having语句是对group by之后的结果进行筛选的。