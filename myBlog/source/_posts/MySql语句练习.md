---
title: Mysql语句练习
date: 2018-05-3 11:30:00
author: 空灵
img: /medias/featureimages/3.jpg
top: true
cover: true
coverImg: /medias/featureimages/8.jpg
password: 8d969eef6ecad3c29a3a629280e686cf0c3f5d5a86aff3ca12020c923adc6c92
toc: false
mathjax: false
summary: Mysql语句练习
categories: Mysql
tags:
  - Mysql
---
#                         mysql语句练习

😠此表情标注为重要练习

### 创建表及数据

```mysql
CREATE TABLE STUDENT  
(SNO VARCHAR(3) NOT NULL,   
SNAME VARCHAR(4) NOT NULL,  
SSEX VARCHAR(2) NOT NULL,   
SBIRTHDAY DATE,  
SCLASS VARCHAR(5))  
  
CREATE TABLE TEACHER   
(TNO VARCHAR(3) NOT NULL,   
TNAME VARCHAR(4) NOT NULL, TSEX VARCHAR(2) NOT NULL,   
TBIRTHDAY DATETIME NOT NULL, PROF VARCHAR(6),   
DEPART VARCHAR(10) NOT NULL)  
  
CREATE TABLE COURSE  
(CNO VARCHAR(5) NOT NULL,   
CNAME VARCHAR(10) NOT NULL,   
TNO VARCHAR(10) NOT NULL)  
  
CREATE TABLE SCORE   
(SNO VARCHAR(3) NOT NULL,   
CNO VARCHAR(5) NOT NULL,   
DEGREE NUMERIC(10, 1) NOT NULL)   
  
ALTER TABLE student  
ADD CONSTRAINT primary key (sno);  
ALTER TABLE course  
ADD constraint primary key (cno);  
ALTER TABLE score  
ADD constraint primary key (sno, cno);  
ALTER TABLE teacher  
ADD constraint primary key (tno);  
ALTER TABLE course  
ADD constraint foreign key (tno) references teacher (tno);  
ALTER TABLE score  
ADD constraint foreign key (cno) references course (cno);  
  
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,SCLASS) VALUES (108 ,'曾华'   
,'男' ,'1977-09-01',95033);  
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,SCLASS) VALUES (105 ,'匡明'   
,'男' ,'1975-10-02',95031);  
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,SCLASS) VALUES (107 ,'王丽'   
,'女' ,'1976-01-23',95033);  
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,SCLASS) VALUES (101 ,'李军'   
,'男' ,'1976-02-20',95033);  
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,SCLASS) VALUES (109 ,'王芳'   
,'女' ,'1975-02-10',95031);  
INSERT INTO STUDENT (SNO,SNAME,SSEX,SBIRTHDAY,SCLASS) VALUES (103 ,'陆君'   
,'男' ,'1974-06-03',95031);  
  
INSERT INTO TEACHER(TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART)   
VALUES (804,'李诚','男','1958-12-02','副教授','计算机系');  
INSERT INTO TEACHER(TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART)   
VALUES (856,'张旭','男','1969-03-12','讲师','电子工程系');  
INSERT INTO TEACHER(TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART)  
VALUES (825,'王萍','女','1972-05-05','助教','计算机系');  
INSERT INTO TEACHER(TNO,TNAME,TSEX,TBIRTHDAY,PROF,DEPART)   
VALUES (831,'刘冰','女','1977-08-14','助教','电子工程系');  
  
INSERT INTO COURSE(CNO,CNAME,TNO) VALUES ('3-105','计算机导论',825);  
INSERT INTO COURSE(CNO,CNAME,TNO) VALUES ('3-245','操作系统',804);  
INSERT INTO COURSE(CNO,CNAME,TNO) VALUES ('6-166','数据电路',856);  
INSERT INTO COURSE(CNO,CNAME,TNO) VALUES ('9-888','高等数学',831);  
  
INSERT INTO SCORE(SNO,CNO,DEGREE) VALUES (103,'3-245',86);  
INSERT INTO SCORE(SNO,CNO,DEGREE) VALUES (105,'3-245',75);  
INSERT INTO SCORE(SNO,CNO,DEGREE) VALUES (109,'3-245',68);  
INSERT INTO SCORE(SNO,CNO,DEGREE) VALUES (103,'3-105',92);  
INSERT INTO SCORE(SNO,CNO,DEGREE) VALUES (105,'3-105',88);  
INSERT INTO SCORE(SNO,CNO,DEGREE) VALUES (109,'3-105',76);  
INSERT INTO SCORE(SNO,CNO,DEGREE) VALUES (101,'3-105',64);  
INSERT INTO SCORE(SNO,CNO,DEGREE) VALUES (107,'3-105',91);  
INSERT INTO SCORE(SNO,CNO,DEGREE) VALUES (108,'3-105',78);  
INSERT INTO SCORE(SNO,CNO,DEGREE) VALUES (101,'6-166',85);  
INSERT INTO SCORE(SNO,CNO,DEGREE) VALUES (107,'6-166',79);  
INSERT INTO SCORE(SNO,CNO,DEGREE) VALUES (108,'6-166',81);
```



#### 表格数据

![img](https://itswcg.com/images/mysql1.png)

![img](https://itswcg.com/images/mysql2.png)

![img](https://itswcg.com/images/mysql3.png)



### 题目

1、查询Student表中的所有记录的Sname、Ssex和Class列。

回答:  SELECT SNAME, SSEX, SCLASS FROM STUDENT;

2、查询教师所有的单位即不重复的Depart列。

回答: SELECT  DISTINCT DEPART FROM TEACHER;

3、查询Student表的所有记录。

回答: SELECT * FROM STUDENT;

4、查询Score表中成绩在60到80之间的所有记录。

回答:  SELECT * FROM SCORE WHERE DEGREE BETWEEN 60 AND 80;

5、查询Score表中成绩为85，86或88的记录。

回答:  SELECT * FROM  SCORE WHERE DEGREE IN(85,86,88);

6、 查询Student表中“95031”班或性别为“女”的同学记录。

回答:  SELECT * FROM STUDENT WHERE SCLASS='95031' OR SSEX='女';

7、 以Class降序查询Student表的所有记录。 

回答： SELECT * FROM STUDENT ORDER BY SCLASS DESC;

8、 以Cno升序、Degree降序查询Score表的所有记录。 

回答： SELECT * FROM SCORE ORDER BY Cno ASC,Degree desc;

9、 查询“95031”班的学生人数。

回答:   SELECT COUNT(*) FROM STUDENT WHERE SCLASS=’95031’;

10、查询Score表中的最高分的学生学号和课程号。 

回答： SELECT SNO, CNO FROM SCORE ORDER BY DEGREE DESC LIMIT 1; SELECT SNO,CNO,DEGREE  FROM SCORE WHERE DEGREE=(SELECT MAX(DEGREE) FROM SCORE); 😠😠😠

11、查询‘3-105’号课程的平均分。 

回答:   SELECT AVG(DEGREE) FROM SCORE WHERE CNO='3-105';

12、查询Score表中至少有5名学生选修的并以3开头的课程的平均分数。 

回答:   SELECT CNO, AVG(Degree ) from SCORE where CNO like '3%' group by CNO having COUNT(*)>=5;😠😠😠

 13、查询最低分大于70，最高分小于90的Sno列。 

回答:  SELECT SNO FROM SCORE GROUP BY SNO HAVING MIN(DEGREE) > 70 AND MAX(DEGREE)< 90; 😠😠😠'

14、查询所有学生的Sname、Cno和Degree列。 

回答:  SELECT A.SNAME, B.CNO, B.DEGREE FROM STUDENT A JOIN SCORE B ON A.SNO=B.SNO;

15、查询所有学生的Sno、Cname和Degree列。

回答： SELECT A.SNO, B.CNAME, C.DEGREE FROM STUDENT A JOIN (COURSE B, SCORE C) ON A.SNO=C.SNO AND B.CNO=C.CNO;

16、查询所有学生的Sname、Cname和Degree列。 

回答:  SELECT A.SNAME, B.CNAME, C.DEGREE FROM STUDENT A JOIN (COURSE B, SCORE C) ON A.SNO=C.SNO AND B.CNO=C.CNO;

17、查询“95033”班所选课程的平均分。

回答：SELECT AVG(A.DEGREE) FROM SCORE A JOIN STUDENT B ON A.SNO=B.SNO WHERE B.SCLASS='95033' GROUP BY A.CNO; 😠😠😠

 18、假设使用如下命令建立了一个grade表： create table grade(low number(3,0),upp number(3),rank char(1)); insert into grade values(90,100,’A’); insert into grade values(80,89,’B’); insert into grade values(70,79,’C’); insert into grade values(60,69,’D’); insert into grade values(0,59,’E’); commit; 

现查询所有同学的Sno、Cno和rank列。 

19、查询选修“3-105”课程的成绩高于“109”号同学成绩的所有同学的记录。 

回答:  SELECT A.* FROM SCORE A WHERE A.CNO='3-105' AND A.DEGREE>(SELECT DEGREE FROM SCORE B WHERE B.SNO=109 AND B.CNO='3-105'); 

20、查询score中选学一门以上课程的同学中分数为非最高分成绩的记录。 

回答：SELECT * FROM SCORE WHERE DEGREE < (SELECT MAX(DEGREE) FROM SCORE) GROUP BY SNO HAVING COUNT(CNO) > 1; 

21、查询成绩高于学号为“109”、课程号为“3-105”的成绩的所有记录 。

回答:  SELECT * FROM SCORE WHERE CNO = '3-105' AND DEGREE > (SELECT DEGREE FROM SCORE WHERE SNO = 109 AND CNO ='3-105');

22、查询和学号为108的同学同年出生的所有学生的Sno、Sname和Sbirthday列。

回答：SELECT A.SNO, A.SNAME, A.SBIRTHDAY FROM STUDENT A WHERE YEAR(A.SBIRTHDAY)=(SELECT YEAR(SBIRTHDAY) FROM STUDENT B WHERE B.SNO=108); 备注 表名可以省略 A为别名

23、查询“张旭“教师任课的学生成绩。

回答：SELECT A.SNO, A.DEGREE FROM SCORE A JOIN (TEACHER B, COURSE C) ON A.CNO = C.CNO AND B.TNO=C.TNO WHERE B.TNAME='张旭';

24、查询选修某课程的同学人数多于5人的教师姓名。

回答：SELECT B.TNAME FROM SCORE A JOIN (TEACHER B, COURSE C) ON A.CNO=C.CNO AND B.TNO=C.TNO GROUP BY A.CNO HAVING COUNT(A.CNO) >5; 😠😠😠

25、查询95033班和95031班全体学生的记录。

回答:  SELECT * FROM STUDENT WHERE SCLASS IN (95033, 95031);

26、查询存在有85分以上成绩的课程Cno.

回答：SELECT CNO FROM SCORE GROUP BY CNO HAVING MAX(DEGREE) > 85;

27、查询出“计算机系“教师所教课程的成绩表。

回答:   select sno,Cno ,Degree from Score where Cno in (select Cno from Course where Tno in (select tno from Teacher where Depart='计算机系'))

28、查询“计算机系”与“电子工程系“不同职称的教师的Tname和Prof。

回答:  SELECT TNAME, PROF FROM TEACHER WHERE DEPART = '计算机系' AND PROF NOT IN (SELECT PROF FROM TEACHER WHERE DEPART='电子工程系');

29、查询选修编号为“3-105“课程且成绩至少高于选修编号为“3-245”的同学的Cno、Sno和Degree,并按Degree从高到低次序排序。

回答： 第一种方式 select Cno,Sno,Degree from Score a where (select Degree from Score b where Cno='3-105' and b.Sno=a.Sno)>=(select Degree from Score c where Cno='3-245' and c.Sno=a.Sno) order by Degree desc;   第二种方式  select * from Score where Cno='3-105' and Degree >any(select Degree from Score where Cno='3-245')

30、查询选修编号为“3-105”且成绩高于选修编号为“3-245”课程的同学的Cno、Sno和Degree.

回答：第一种方式 select Cno,Sno,Degree from Score a where (select Degree from Score b where Cno='3-105' and b.Sno=a.Sno)>(select Degree from Score c where Cno='3-245' and c.Sno=a.Sno);

第二种方式 select cno,sno,degree from score where cno='3-105' and degree>all(select degree from score where cno='3-245') order by degree desc

 31、查询所有教师和同学的name、sex和birthday.

回答：  SELECT SNAME AS NAME, SSEX AS SEX, SBIRTHDAY AS BIRTHDAY FROM STUDENT UNION SELECT TNAME AS NAME, TSEX AS SEX, TBIRTHDAY AS BIRTHDAY FROM TEACHER;

32、查询所有“女”教师和“女”同学的name、sex和birthday.

回答：SELECT SNAME AS NAME, SSEX AS SEX, SBIRTHDAY AS BIRTHDAY FROM STUDENT WHERE SSEX='女' UNION SELECT TNAME AS NAME,TSEX AS SEX, TBIRTHDAY AS BIRTHDAY FROM TEACHER WHERE TSEX='女';

33、查询成绩比该课程平均成绩低的同学的成绩表。

回答： SELECT A.* FROM SCORE A WHERE DEGREE < (SELECT AVG(DEGREE) FROM SCORE B WHERE A.CNO=B.CNO);

34、查询所有任课教师的Tname和Depart.

回答：第一种方式：SELECT TNAME, DEPART FROM TEACHER A JOIN COURSE B ON A.TNO=B.TNO;

第二种方式:   SELECT TNAME, DEPART FROM TEACHER A WHERE EXISTS (SELECT * FROM COURSE B WHERE A.TNO=B.TNO);

第三种方式：SELECT TNAME,DEPART FROM TEACHER WHERE TNO IN (SELECT TNO FROM COURSE);

35、 查询所有未讲课的教师的Tname和Depart. 

回答：第一种方式：SELECT TNAME, DEPART FROM TEACHER A WHERE NOT EXISTS (SELECT * FROM COURSE B WHERE A.TNO=B.TNO);

第二种方式： SELECT TNAME,DEPART FROM TEACHER WHERE TNO NOT IN (SELECT TNO FROM COURSE);

36、查询至少有2名男生的班号。

回答： SELECT SCLASS FROM STUDENT WHERE SSEX='男' GROUP BY SCLASS HAVING COUNT(SSEX)>1;

37、 查询Student表中不姓“王”的同学记录。

回答： SELECT * FROM STUDENT WHERE SNAME NOT LIKE '王%';

38、查询Student表中每个学生的姓名和年龄。

回答：SELECT SNAME,(YEAR(NOW())-YEAR(SBIRTHDAY)) AS AGE FROM STUDENT;

39 、查询Student表中最大和最小的Sbirthday日期值。

回答： SELECT SBIRTHDAY AS MAX FROM STUDENT WHERE SBIRTHDAY = (SELECT MIN(SBIRTHDAY) FROM STUDENT) UNION SELECT SBIRTHDAY AS MIN FROM STUDENT WHERE SBIRTHDAY = (SELECT MAX(SBIRTHDAY) FROM STUDENT);

40、以班号和年龄从大到小的顺序查询Student表中的全部记录。

回答：SELECT * FROM STUDENT ORDER BY SCLASS DESC, SBIRTHDAY ASC;

41、查询“男”教师及其所上的课程。

回答：  SELECT A.TNAME , B.cname FROM TEACHER A JOIN COURSE B ON A.TNO = B.TNO WHERE A.TSEX='男';

42、查询最高分同学的Sno、Cno和Degree列。

回答:    SELECT * FROM SCORE WHERE DEGREE = (SELECT MAX(DEGREE) FROM SCORE);

43、查询和“李军”同性别的所有同学的Sname.

回答:  SELECT SNAME FROM STUDENT WHERE SSEX =(SELECT SSEX FROM STUDENT WHERE SNAME ='李军');

44、查询和“李军”同性别并同班的同学Sname.

回答： SELECT SNAME FROM STUDENT WHERE SSEX=(SELECT SSEX FROM STUDENT WHERE SNAME='李军') AND SCLASS=(SELECT SCLASS FROM STUDENT WHERE SNAME='李军');

45、查询所有选修“计算机导论”课程的“男”同学的成绩表

回答：SELECT C.* FROM STUDENT A JOIN (COURSE B, SCORE C) ON A.SNO=C.SNO AND B.CNO=C.CNO WHERE B.CNAME='计算机导论' AND A.SSEX='男'

