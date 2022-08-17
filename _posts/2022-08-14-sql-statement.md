---
layout: post
title:  "sql-statement"
author: "marcelyz"
---

# lc_603. Consecutive Available Seats
自关联
```
SELECT DISTINCT a.seat_id
FROM Cinema a
JOIN Cinema b ON abs(a.seat_id - b.seat_id) = 1
AND a.free = 1
AND b.free = 1
ORDER BY seat_id;
```

# lc_1076. Project Employees II
having
```
SELECT project_id
FROM Project
GROUP BY project_id HAVING count(employee_id) =
  (SELECT count(employee_id)
   FROM Project
   GROUP BY project_id
   ORDER BY count(employee_id) DESC LIMIT 1)
```

# 求股票的波峰和波谷
https://blog.csdn.net/godlovedaniel/article/details/118946573
窗口函数之lag和lead
```
SELECT *
FROM
  (SELECT * ,
          CASE
              WHEN price>lag_price
                   AND price>lead_price THEN "crest"
              WHEN price<lag_price
                   AND price<lead_price THEN "trough"
              ELSE NULL
          END AS res
   FROM
     (SELECT id ,
             price ,
             dt ,
             lag(price,1,price) over(partition BY id
                                     ORDER BY dt) AS lag_price ,
             lead(price,1,price) over(partition BY id
                                      ORDER BY dt) AS lead_price
      FROM stock) t) m
WHERE res IS NOT NULL
```

# 求连续n天登录的用户
https://www.modb.pro/db/60280
窗口函数之lag和lead
```
SELECT b.user_name
  (SELECT user_name,date, lead(date,6) over(partition BY user_name
                                            ORDER BY date DESC) AS date_7
   FROM user_login_table) b
WHERE b.date IS NOT NULL
  AND date_sub(cast(b.date AS date,6)) = cast(b.date_7 AS date)
```

# 求不同性别每日分数累加和
https://blog.csdn.net/qq_21201267/article/details/107452243
```
SELECT gender,
       DAY,
       sum(score_points) over (partition BY gender
                               ORDER BY DAY) total
FROM Scores
ORDER BY gender,
         DAY
```