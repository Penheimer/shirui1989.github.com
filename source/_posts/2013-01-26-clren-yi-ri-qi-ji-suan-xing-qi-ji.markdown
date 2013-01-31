---
layout: post
title: "cl任意日期计算星期几"
date: 2013-01-26 15:20
comments: true
categories: [lisp, 计算机]
keywords: lisp, 任意两天相差天数, 任意日期对应星期几
---
<!--more-->
Common Lisp实现输入日期计算当天是星期几，并可以计算从某年一月一日起算当天是当年第几天和任意两个日期之间差异。		
`(day-count-in-year month day year)`计算同年该日期是第几天。		
`(day-count-all m-initial d-initial y-initial m-final d-final y-final)`计算任意两天之间差多少天。		
`(week-days-indicator month day year)`计算当天是星期几。		
包含了1752年9月的特殊(`cal 9 1752`看看)，并且考虑到了1800年前后整百年份闰年定义方法的不同（1800年以后整百年份是整除400方为闰年，以前只要是整除4即是)。		
P.S.最后`(dairy year)`是苦逼屌丝周年日记。XD
``` python ds.lisp
(defun month-length (month year) ;;月长，1800年以前的闰年算法不一样……shit
  (case month
    ((1 3 5 7 8 10 12) 31)
    (9 (if (= year 1752) 19 30))
    (2 (if (> 1800 year) (progn (if (= (mod year 4) 0) 29 28)) (progn (if (= (mod year 100) 0) (if (= (mod year 400) 0) 29 28) (if (= (mod year 4) 0) 29 28)))))
    ((4 6 11) 30)))
(defun year-length (year) ;;年长
  (case year
    (1752 355)
    (otherwise (if (> 1800 year) (progn (if (= (mod year 4) 0) 366 365)) (progn (if (= (mod year 100) 0) (if (= (mod year 400) 0) 366 365) (if (= (mod year 4) 0) 366 365)))))))
(defun day-count-in-year (month day year) ;;同一年内从1月1起算日数
  (case year
    (1752 (if (= month 1) day
        (if (= month 9)
		  (if (<= day 2) (+ day (loop for mon from 1 to (- month 1) summing (month-length mon year))) (- (+ day (loop for mon from 1 to (- month 1) summing (month-length mon year))) 11))
		  (+ day (loop for mon from 1 to (- month 1) summing (month-length mon year))))))
    (otherwise (if (= month 1) day (+ day (loop for mon from 1 to (- month 1) summing (month-length mon year)))))))
(defun day-count-all (month-initial day-initial year-initial month-final day-final year-final) ;;任意两个日期相差日数
  (if (= year-initial year-final)
      (- (day-count-in-year month-final day-final year-final) (day-count-in-year month-initial day-initial year-initial))
      (-
	(+ (day-count-in-year month-final day-final year-final) (loop for year from year-initial to (- year-final 1) summing (year-length year)))
	(day-count-in-year month-initial day-initial year-initial))))
(defun week-days-after-1752-9 (daynum) ;;以1752年为基准计算星期
  (case (mod daynum 7) (0 "星期四") (1 "星期五") (2 "星期六") (3 "星期天") (4 "星期一") (5 "星期二") (6 "星期三")))
(defun week-days-before-1752-9 (daynum) ;;以1752年为标准计算星期
  (case (mod daynum 7) (6 "星期四") (5 "星期五") (4 "星期六") (3 "星期天") (2 "星期一") (1 "星期二") (0 "星期三")))
(defun week-days-indicator (month day year) ;;输出任意月/日/年对应星期几
  (if (string> (format nil "~4,'0d~2,'0d~2,'0d~%" year month day) "17520902")
      (week-days-after-1752-9 (day-count-all 9 14 1752 month day year))
      (week-days-before-1752-9 (day-count-all month day year 9 2 1752))))
(defvar *what2do* nil)
(defun things () ;;连续喜当爹问题让我搞定了，现在是概率问题和我期望设计的”周一到周五工作，周六周日撸管子有点冲突……怎么都不完美……“
  (case (random 4)
    (0
     (push "搬砖、撸管子、意淫女神" *what2do*)
     (push "完事儿又被女神甩掉了" *what2do*)
     (push "只好陪女神去医院" *what2do*)
     (push "悲剧，原来是喜当爹了" *what2do*)
     (push "女神主动找我聊QQ，春天终于来了" *what2do*)
     (push "搬砖、撸管子、意淫女神" *what2do*)
     (push "搬砖、撸管子、意淫女神" *what2do*))
    (1 (push "搬砖、撸管子、意淫女神" *what2do*)
       (push "搬砖、撸管子、意淫女神" *what2do*)
       (push "搬砖、撸管子、意淫女神" *what2do*)
       (push "给我发了好人卡的女神又去找高富帅了" *what2do*)
       (push "原来是因为女神分手了心情不好" *what2do*)
       (push "女神终于接受我的表白" *what2do*)
       (push "搬砖、撸管子、意淫女神" *what2do*))
    (2 (push "搬砖、撸管子、意淫女神" *what2do*)
       (push "搬砖、撸管子、意淫女神" *what2do*)
       (push "和女神聊QQ，被\"吃饭呵呵去洗澡\"" *what2do*)
       (push "搬砖、撸管子、意淫女神" *what2do*)
       (push "搬砖、撸管子、意淫女神" *what2do*)
       (push "搬砖、撸管子、意淫女神" *what2do*)
       (push "搬砖、撸管子、意淫女神" *what2do*))
    (3 (push "搬砖、撸管子、意淫女神" *what2do*)
       (push "搬砖、撸管子、意淫女神" *what2do*)
       (push "搬砖、撸管子、意淫女神" *what2do*)
       (push "搬砖、撸管子、意淫女神" *what2do*)
       (push "和女神聊QQ，被\"吃饭呵呵去洗澡\"" *what2do*)
       (push "搬砖、撸管子、意淫女神" *what2do*)
       (push "搬砖、撸管子、意淫女神" *what2do*))))
(defun write-db () (setf *what2do* nil) (loop repeat 53 do (things)))
(defun dairy (year)
  (write-db)
  (loop for mon from 1 to 12 do
       (loop
	  for day from 1 to (month-length mon year)
	  for item in *what2do* do
	    (format t "~2,'0d~a~2,'0d~a:~a~%" mon "月" day (week-days-indicator mon day year) item))))
```
