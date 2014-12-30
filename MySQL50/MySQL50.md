## MySQL 50题练习 语句

```
1.FROM 子句对其后面的表 和join后的表执行笛卡尔积,产生虚拟表 
2.ON子句对虚拟表进行过滤，产生虚拟表 
3.JOIN子句 未符合条件的保留表中的数据添加虚拟表2，形成虚拟表3 
4. WHERE子句对虚拟表3进行数据过滤，产生虚拟表  
5 GROUP 子句对虚拟表4分组，产生虚拟表 
6 WITH 子句进行操作，产生虚拟表
7 HAVING子句对数据进行过滤，产生虚拟表 
8 SELECT自选选取要获取的字段 ，产生虚拟表 
9 DISTINCT 去重操作 产生虚拟表 
10 ORDER 排序操作，产生虚拟表 
11 LIMIT 取出制定的数据，产生虚拟表11 ，返回表

```

建表及数据
```
-----------------------------------表结构---------------
--学生表tblStudent（编号StuId、姓名StuName、年龄StuAge、性别StuSex）
--课程表tblCourse（课程编号CourseId、课程名称CourseName、教师编号TeaId）
--成绩表tblScore（学生编号StuId、课程编号CourseId、成绩Score）
--教师表tblTeacher（教师编号TeaId、姓名TeaName）
-------------------------------------------------------
```
###1.查询“某1”课程比“某2”课程成绩高的所有学生的学号；
```

SELECT
	s.StuName,
	m.score,
	C.CouName
FROM
	(/* 查询课程位 1 的课程成绩和学号*/
		SELECT
			`Score`.`CoId`,
			`Score`.`StId`,
			`Score`.`score`
		FROM
			Score
		WHERE
			`Score`.`CouId` = 1
	) m,

	(/* 查询课程位 4 的课程成绩和学号*/
		SELECT
			`Score`.`CoId`,
			`Score`.`StId`,
			`Score`.`score`
		FROM
			Score
		WHERE
			`Score`.`CoId` = 4
	) n,
	Student s,
	Course C
	/* 课程1，4 的 成绩和学号 表建成  */
WHERE
	m.Score > n.Score  /* 对比 成绩 */
AND m.StId = n.StId  /* 学号 id 相等 */
AND s.StuId = m.StId
AND C.CouId = m.CoId


```
###查询平均成绩大于60分的同学的学号和平均成绩；
```
SELECT
	avg(score),
	sc.CoId
FROM
	Score sc
WHERE
	1
GROUP BY /* 根据 学号进行聚合分组 */
	sc.StId
HAVING
	AVG(score) > 60 /* 添加条件 having 替代where */
ORDER BY
	sc.StId;

```