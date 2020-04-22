---
title: 【数据库原理】实验一·SQL Server数据库及对象的设计
tags:
  -数据库
---

```ddl
create table major
    (major_no CHAR(4) NOT NULL check (major_no!='0000') PRIMARY KEY ,
    GB_major_no CHAR(6) NOT NULL check (GB_major_no!='000000') ,
    major_name VARCHAR(60) NOT NULL,
    en_major_name VARCHAR(250) NOT NULL,
    length_school  TINYINT NOT NULL default 4,
    edu_level char(6) NOT NULL default '本科',
    ddegree CHAR(12) NOT NULL,
    department_no CHAR(2) NOT NULL check (department_no!='00'),
    department VARCHAR(40) NOT NULL);
	
CREATE TABLE student( 
	sno  CHAR(12) NOT NULL CHECK(sno like '[2-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9][0-9]' and substring(sno, 11, 2) not like '00' ) PRIMARY KEY , 
	sname  CHAR(16)  NOT NULL, 
	sex CHAR(2) NOT NULL CHECK(sex IN ('男', '女')) default ' 男', 
	birthday DATE NOT NULL , 
	nationality char(12) NOT NULL DEFAULT '汉族' , 
		native char(16) NOT NULL DEFAULT '东莞市' ,
	political char(10) NOT NULL DEFAULT '共青团员' , 
		district char(12) NOT NULL DEFAULT '松山湖校区' , 
	student_source char(24) NOT NULL DEFAULT '东莞市' ,
	 enter_year DATE NOT NULL, school_year smallint NOT NULL , 
	class char(24) NOT NULL , 
	major_no CHAR(4) NOT NULL, 
	FOREIGN KEY (major_no) REFERENCES Major(major_no)
) 

CREATE TABLE course (
	school_year INT NOT NULL,
	semester TINYINT NOT NULL CHECK (semester IN ( '1', '2', '3', '4', '5', '6', '7', '8' )),
	course_NO CHAR ( 8 ) NOT NULL CHECK ( course_no != '000000' ),
	PRIMARY KEY ( school_year, semester, course_no ),
	course_name VARCHAR ( 40 ) NOT NULL,
	credit NUMERIC ( 3, 1 ) NOT NULL check(credit=1.5 or credit=1 or credit=2.5 or credit=2 or credit=3.5 or credit=3 or credit=4.5 or credit=4 or credit=5 ),
	credit_hourse TINYINT NOT NULL,
	CONSTRAINT credit_chk CHECK ( credit_hourse <= 18 * credit ),
	course_type_1 CHAR ( 16 ) NOT NULL,
	course_type_2 CHAR ( 16 ),
	cement_type CHAR ( 16 ),
	examine_way CHAR ( 6 ) NOT NULL 
)

CREATE TABLE select_course(
    sno char(12)NOT NULL,
    school_year INT NOT NULL,
    semester TINYINT NOT NULL,
    course_no char(8)NOT NULL,
    PRIMARY KEY(sno,school_year,semester,course_no),
    FOREIGN KEY(Sno) REFERENCES student(sno),
    FOREIGN KEY(school_year,semester,course_no)REFERENCES course(school_year,semester,course_no),
    score NUMERIC(6,2)
	)



```