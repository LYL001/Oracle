# Oracle
# 实验一：分析SQL执行计划，执行SQL语句的优化指导

## 实验内容：
- 对Oracle12c中的HR人力资源管理系统中的表进行查询与分析。
- 首先运行和分析教材中的样例：本训练任务目的是查询两个部门('IT'和'Sales')的部门总人数和平均工资，以下两个查询的结果是一样的。但效率不相同。
- 设计自己的查询语句，并作相应的分析，查询语句不能太简单。

## 教材中的查询语句

查询1：

```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
from hr.departments d，hr.employees e
where d.department_id = e.department_id
and d.department_name in ('IT'，'Sales')
GROUP BY department_name;
```
运行结果：
![运行结果](https://github.com/LYL001/Oracle/blob/master/test1/1.jpg)



- 查询2：
```SQL
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');
```
运行结果：
![运行结果](https://github.com/LYL001/Oracle/blob/master/test1/2.jpg)

比较：我认为下面一条语句较为优。两条语句都是查询所有部门及以下员工的总数与平均工资，第一条语句直接在匹配外键约束时，进行查询内容过滤，在结果返回之前起作用，后者则是在匹配出外键之后相当于新生成一张表，再从那张表里进行查询内容过滤，在结果返回之后起作用。


- 自定义：
Oracle并没给出优化建议，但在数据量大时，可以使用OFFSET....FETCH分页显示内容

```sql
SELECT d.department_name，count(e.job_id)as "部门总人数"，
avg(e.salary)as "平均工资"
FROM hr.departments d，hr.employees e
WHERE d.department_id = e.department_id
GROUP BY department_name
HAVING d.department_name in ('IT'，'Sales');
OFFSET 5 ROWS FETCH NEXT 5 ROW ONLY;
```

