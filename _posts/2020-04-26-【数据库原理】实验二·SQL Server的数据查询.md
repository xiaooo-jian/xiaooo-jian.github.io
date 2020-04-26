---
title: 【数据库原理】实验二·SQL Server的数据查询
tags:
  - 数据库
  - 实验
---


## 实验目的：

 1. 掌握基本的查询、嵌套子查询及连接查询
 2. 掌握各种统计函数的使用。
 3. 掌握视图的定义及通过视图的数据查询操作。

## 实验要求：

 1. 熟练掌握基本的查询、嵌套子查询及连接查询
 2. 熟练掌握各种统计函数的使用。
 3. 熟练掌握视图的定义及通过视图的数据查询操作。
 4. 体会各种查询的异同及相互之间的转换，体会各种查询的执行过程，为综合应用打下良好的基础。


## 实验内容：


	基于自己所创建的数据库设计完成以下各类查询操作的SQL语句。
	
	
###  单表查询：

⑴ 查询全体学生的学号、姓名、性别、年龄，并在查询结果中将列名显示为：学号、姓名、性别、年龄。

``` sql
SELECT sno 学号,sname 姓名,sex 性别,cast((20200420-birthday)/10000 AS decimal) 年龄 
FROM student
```

⑵ 查询选修了课程号为134662的学生学号和成绩，并按成绩降序排列。
SELECT sno 学号,score 成绩

``` oxygene
FROM select_course
where course_no='134662'
ORDER BY score DESC 
```

⑶ 查询select_course表中综合成绩在60到80之间的所有记录。（要求：条件的表示至少用两种方式）

``` sqf
SELECT *
FROM select_course
where score BETWEEN 60 and 80
SELECT *
FROM select_course
WHERE not (score>=80 or score<=60)
```

 
⑷ 查询所有姓王且籍贯不是“东莞市”但生源地是“东莞市”的学生详细信息。

``` sql
SELECT *
FROM student
WHERE sname like '王%' AND student_source!='东莞市'  
```

 
⑸ 查询选修了课程但没有参加考试的学生学号和相应的课程号。

``` sql
SELECT sno,course_no
FROM select_course
WHERE score = NULL
```

⑹ 查询每一门课程选修的学生人数、最高成绩、最低成绩和平均成绩，并在查询结果中将列名显示为：选修人数、最高成绩、最低成绩、平均成绩。

``` sqf
SELECT count(sno) 选修人数,max(score) 最高成绩, min(score) 最低成绩, avg(score) 平均成绩
FROM select_course
GROUP BY course_no
```

 
⑺ 查询平均成绩在70分以上(含70分) 的学生学号、选课门数、最高成绩、最低成绩和平均成绩，并在查询结果中将列名显示为：学号、选课门数、最高成绩、最低成绩、平均成绩。
SELECT sno 学号,count(course_no) 选修门数,max(score) 最高成绩, min(score) 最低成绩, 

``` sqf
avg(score) 平均成绩
FROM select_course
GROUP BY sno
HAVING AVG(score)>=70
```

 
⑻ 平均成绩在80分以上(含80分) 且没有不及格的学生学号、选课门数、最高成绩、最低成绩和平均成绩，并在查询结果中将列名显示为：学号、选课门数、最高成绩、最低成绩、平均成绩。
SELECT sno 学号,count(course_no) 选修门数,max(score) 最高成绩, min(score) 最低成绩, 

``` sqf
avg(score) 平均成绩
FROM select_course
WHERE  score>=60
GROUP BY sno
HAVING AVG(score)>=80
```

 
### 复合条件及连接查询： 
⑴ 查询选修了课程代码是045239且成绩80分(含80分)以上的学生的学号、姓名、成绩。

``` sqf
SELECT student.sno 学号,sname 姓名,score 成绩
FROM student, select_course
WHERE course_no='045239' AND student.sno= select_course.sno AND score>=80
```

 
⑵ 查询选修了课程代码是134647所有同学的学号、姓名、性别、出生日期和年级，并在查询结果中将列名显示为：学号、姓名、性别、出生日期、年级。

``` sqf
SELECT student.sno,sname,score
FROM student, select_course
WHERE course_no='134647' AND student.sno= select_course.sno
```

 
⑶ 查询选修了课程代码是134647且成绩不及格的学生学号、姓名、性别、班级、综合成绩。

``` sqf
SELECT student.sno,sname ,score , birthday , student.school_year,score
FROM student, select_course
WHERE course_no='134647' AND student.sno= select_course.sno AND score<60
```

 
⑷ 查询选修了课程代码是045239且成绩在80分(含80分)以上的学生的学号、姓名、性别、课程名、年龄、成绩。

``` sql
SELECT student.sno,sname ,sex, course_name,cast((20200420-birthday)/10000 AS decimal) ,score
FROM student, select_course,course
WHERE select_course.course_no='045239' AND student.sno= select_course.sno AND score>=80 AND course.course_no=select_course.course_no
```

 
⑸ 查询选修了高等数学所有同学的学号、姓名、性别、院系名称、专业、出生日期和年级，并在查询结果中将列名显示为：学号、姓名、性别、系名、专业、出生日期、年级。（要求：至少用两种方式实现）

``` stylus
SELECT student.sno 学号,sname 姓名,major.department 系名, major.major_name 专业, birthday 出生日期, student.school_year 年级
FROM student, select_course, major,course
WHERE course.course_name LIKE '高等数学%' AND select_course.course_no=course.course_no AND student.sno= select_course.sno AND student.major_no=major.major_no
```

``` sql
SELECT student.sno 学号,sname 姓名,major.department 系名, major.major_name 专业, birthday 出生日期, student.school_year 年级
FROM student LEFT OUTER JOIN select_course ON (student.sno= select_course.sno) LEFT OUTER JOIN major ON (student.major_no=major.major_no ) LEFT OUTER JOIN course ON(select_course.course_no=course.course_no)
WHERE course.course_name LIKE '高等数学%'
```

 
⑹ 查询没有任何同学选修的课程代码、课程名称、学时数、学分。

``` sql
SELECT course.course_no,course_name,credit_hourse,credit
FROM course,select_course
GROUP BY course_no
HAVING count(sno)!=0
```

 
⑺ 查询至少选修了两门以上课程的学生的学号、姓名和所在班级。

``` sql
SELECT DISTINCT student.sno,sname,class
FROM student,select_course
WHERE student.sno = select_course.sno
GROUP BY select_course.sno
HAVING count(course_no)>=2
```

 
⑻ 查询所有课程都及格的学生的学号，姓名，最高成绩，最低成绩，平均成绩。

``` sqf
SELECT student.sno,sname,max(score),min(score),avg(score)
FROM student,select_course
WHERE student.sno = select_course.sno
GROUP BY select_course.sno
HAVING min(score)>=60
 
```

### 嵌套查询：（以下部分必须用嵌套查询来完成）
⑴ 查询选修了课程代码是指定编号的所有同学的学号、姓名、性别、出生日期、级别和班级。（提示：指定课程代码通过局部变量实现）

``` sql
SELECT sno,sname,sex,birthday,school_year,class
FROM student
WHERE sno in 
( SELECT sno
	from select_course 
		where course_no='044374')
```

 
⑵ 查询“刘晨”班上同学的学号、姓名、性别、出生日期、籍贯、民族。

``` sql
SELECT sno,sname,sex,birthday,school_year,native,nationality
FROM student
WHERE class in 
( SELECT class
	from student 
		where sname='刘晨')
```

 
⑶ 查询软件工程专业选修了指定课程且成绩在80分及以上的学号、姓名、性别、级别、年龄、成绩。（提示：指定课程代码通过局部变量实现）

``` sql
SELECT S.sno,sname,sex,S.school_year,cast((20200420-birthday)/10000 AS decimal),score
FROM student S , select_course SC
WHERE SC.sno in 
	(SELECT sno
	from select_course 
	where course_no='044374' and score>80)
and major_no in 
	(SELECT major_no
	FROM major
	WHERE major_name='软件工程')
	GROUP BY s.sno 
```


### 视图的定义及其操作
⑴ 定义一个查询学生的学号、姓名、系名、专业、学年、学期、最高成绩、最低成绩、平均成绩的视图student_score。

``` stylus
CREATE VIEW student_score(sno,sname,department,major_name,school_year,semester,max,min,avg)
AS
SELECT s.sno,sname,m.department,major_name,s.school_year,c.semester,MAX(score),MIN(score),AVG(score)
FROM student s,major m,course c,select_course sc
WHERE s.major_no=m.major_no
AND s.sno=sc.sno
AND sc.course_no=c.course_no
GROUP BY s.sno,sc.school_year,sc.semester
```

 
⑵、定义一个查询选修课程的课程号、课程名称、学年、学期、学分、最高成绩、最低成绩、平均成绩的视图course_score。

``` sql
SELECT sno,sname,department,major_name
FROM student_score2 
WHERE max>=90 AND min>=60
```

 
⑶、通过视图student_score查询最高成绩在90分以上(含90分)且没有不及格成绩的学生学号、姓名、系名、专业。

``` stylus
SELECT
	course.course_no AS course_no,
	course.course_name AS course_name,
	course.school_year AS school_year,
	course.semester AS semester,
	course.credit AS credit,
	max( select_course.score ) AS max_score,
	min( select_course.score ) AS min_score,
	avg( select_course.score ) AS avg_score 
FROM
	(
		course
		JOIN select_course ON (((
					select_course.school_year = course.school_year 
					) 
				AND ( select_course.semester = course.semester ) 
			AND ( select_course.course_no = course.course_no )))) 
GROUP BY
	course.course_no,
	course.course_name,
	course.school_year,
	course.semester,
	course.credit
```

 
⑷、通过视图course_score和其它的表，查询没有不及格成绩的课程号、课程名称、学分及选修这些课程的学生的学号、姓名、系名、专业。并且改变查询结果中显示的列名分别为：学号、姓名、系名、专业、课程号、课程名、学分。

``` n1ql
SELECT
courese_score.course_no AS `课程号`,
courese_score.course_name AS `课程名`,
courese_score.credit AS `学分`,
student.sno AS `学号`,
student.sname AS `姓名`,
major.department AS `系别`,
major.major_name AS `专业`
FROM
courese_score ,
student
INNER JOIN major ON student.major_no = major.major_no
WHERE
courese_score.min_score>=60
```

 
