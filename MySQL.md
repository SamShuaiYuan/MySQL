#  MySQL

## 1 Terminology

DB -- database. data warehouse that holds organized data

DBMS- database management system,  for example MySQL Oracle SqlServer...

SQL -- structure query language

***

## 2 Data feature in database

+ 将数据放入表中，表再放入库中
+ 一个数据库有多个表，表名唯一
+ 表的特新定义了数据在表中如何存储
+ 表由列组成，我们也称为字段，每一列类似于Java中的属性
+ 表中数据按行存储，每行类似Java的一个对象
+ 数据库好处： 
  - 可以持久化到本地
  - 结构化查询

***

## 3 MySQL installation

[Link]( [https://blog.csdn.net/qq_37350706/article/details/81707862?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase#%E9%85%8D%E7%BD%AE%E5%88%9D%E5%A7%8B%E5%8C%96%E7%9A%84my.ini%E6%96%87%E4%BB%B6%E7%9A%84%E6%96%87%E4%BB%B6](https://blog.csdn.net/qq_37350706/article/details/81707862?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase#配置初始化的my.ini文件的文件))

- Download MySQL online 
- Configure the initialization file  my.ini 
  + Put it under the directory same with license
  + Do not create Data file manually
- Initialize MySQL
  - Run CMD as administrator (C:\Windows\System32...)
  - go to bin catalogue and run: mysqld --initialize -- console, keep the root@localhost password

- Install MySQL service and 
  - Run: mysqld --install (you should see Service successfully installed)

+ Start MySQL service
  - Run: net start mysql (MySQL service is starting...  MySQL service started successfully)

- Stop MySQL service 
  - Run: net stop mysql

***

## 4 MySQL login and logout

+ Login 
  - Method 1: Computer -> MySQL 8.0 command line client (only for root user)
  - Method 2: mysql -h localhost -P 3306 -u root -p  [mysql -h host name -P port number -u username -p password]

+ Logout
  - exit or ctrl +C

***

## 5 MySQL common command

```mysql
show databases;
use test; # test is a database name, then you can do DML
show tables;
show tables from test; # test is a database name
creat table stuinfo(  # stuinfo is a table name
	id int, # column name  type
    name varchar(20)
);
desc stuinfo; -- check the table structure
drop table stuinfo; -- delete table 
insert into stuinfo(id, name) values(1, 'Lelei');
update stuinfo set name = 'Lucy' where id = 1;
delete from stuinfo where id = 1;
select version(); -- check mysql version in mysql client server
/* in cmd enviroment, 
we type:
*/
mysql --version; 
mysql -V;

```

## 6  MySQL learning

#### DQL

##### 'where'

+ Filter by conditional expression
  - conditional operator:   <	>	=	!=	<>	<=	>=
  - logical operator:   &&   ||	!     and     or   not
  - fuzzy query:     like    between and    in     is null    is not null

+ Notice:

  - between and :
    + between and is neater than >= <=
    + boundary included
    + boundaries **can not** be exchanged

  - safe equal <=>  vs  is null
    + is null  can only be used to verify null, readable, highly recommend
    + <=> can be used to verify null and also verify numerical number. ex: salary <=> 1000 unreadable

  - 'is' only used for 'is null' and 'is not null'

+ Interview questions:

  - customer table has three columns: id last_name first_name, what are their differences?

    ```mysql
    select * from customer
    select * from customer where id like '%%' and last_name like '%%' # null
    select * from customer where id like '%%' or first_name like '%%' or last_name like '%%' # same with the first query, can not all be null
    ```

    

+ Supplement material
  - function: isnull()--- return 1/0 

```mysql
select name from customer where salary in (1000, 2000, 3000);
select name from customer where name like '%o%'; # whose name contains o
select name from customer where name like '__r%'; # third char is r
select name from customer where name like '_\_r_l%'; # 2:_ 3:r  5:l
select name from customer where name like '_$_r_l%' escape '$'; # 2:_ 3:r  5:l
select name from customer where name is null;
select salary as "out put" from customer; # double quotes is necessary
select 100;
select 'lili';
select 100*90;
/* wildcard character
% represents any number of characters
_ represents one character
*/
```

#### 'order by'

```mysql
select salary&12(1+ifnull(commission_pet, 0)) as year_salary from employee
order by year_salary asc, length(last_name), first_name desc
```

+ Notice:
  - default setting: asc
  - 'order by' support field, fields, expression, function, alias
  - only limit could be in front of it (second last)

***

***

##### '+' functionality

In java, '+' has two meaning, First, if two operands are numerical numbers, '+' is operator; Second, if one of the two operands is character, then '+' is concatenation operator.

In MySQL, '+' only means operator. If one operand is char type, then first mysql try to convert the char to numerical, if success, then continue to do addition, otherwise convert the char operand to 0. Once one operand is null, then the result is null.

```mysql
select concat(first_name, last_name) as name from customer; # can not use '+'
# if one operand in braces is null, then the result is null
# ifnull('commission_pet' 0) as 'bonus' -- one way to avoid null
```

***

***

#### Functions

Similar with Java, encapsulate a set of logical statements in the body of a method, expose method name.

+  Benefits: 1. hide implementation details 2. Improve reusability

+ Classification: 

  - Classification 1:
    + single row functions:  count length  ifnull           
    + grouping functions:

  - Classification 2:

    + char function （ten functions）

      - length: return bytes 

        ```mysql
        select length('john'); # 4
        select length('袁帅')； # 6 -- utf8 every chinese char takes three bytes
        ```

      + concat 

        ```mysql
        select concat(last_name, '_', first_name) name from employee
        ```

      + upper  

      + lower

      + substr  substring

        ```mysql
        select substr('Jake loves Rose', 6) out_put; 
        /*
        sql index start from 1
        truncate a string from a specified index
        out_put: loves Rose
        */
        select substr('Jake loves Rose', 2, 4) out_put; 
        /*
        truncate a string at a specified length from a specified index
        out_put: ake
        */
        # ex: capitalize the first char in surname and concatenate last name and first name using "_"
        select concat(upper(subsr(last_name, 1,1)), lower(substr(last_name,2)), '_', lower(first_name) from employee;
                      
        select substr(email, 1,intr(email,'@')-1) username from stuinfo;
        ```

      + instr 

        return the index where the substring first occur, return 0 if it is not find.

        ```mysql
        select instr('Jake loves Rose', 'Ros') as out_put; # 12
        ```

      + trim

        get ride of blanks or specified char from head and tail

        ```mysql
        select length(trim('    张 三 丰   ')) as out_put; # 11
        select trim('aa' from 'aaaaa张aaaaaa三丰aaaaa') as out_put; # a张aaaaaa三丰a
        ```

      + lpad  rpad

        left/right pad with specified length by using given char

        ```mysql
        SELECT LPAD('张三丰',2,'*') AS out_put; #   张三
        SELECT RPAD('张三丰',8,'ab') AS out_put; #  张三丰ababa
        ```

      + replace

        ```mysql
        select replace('Jake loves Rose, Rose loves Jake', 'Rose', 'Lucy');
        # Jake loves Lucy, Lucy loves Jake
        ```

    + math function

      round	ceil	floor	truncate	mod	%

      ```mysql
      select round(-1.55) # -2
      select round(1,567, 2) # l.57
      select ceil(1.01) # 2
      select truncate(1.6999, 1); # 1.6
      select mod(10,-3); # 1 mod(a, b)= a-a/b*b 
      select mod(-10,-3); # -1
      ```

    + date and time function 

      now	curdate	curtime	year	month	monthname	day 	hour	minute	second

      str_to_date	date_format

      ```mysql
      select now(); # date + time
      select curdate(); # date  without time
      select curtime(); # time without date
      select year(now()); 
      select year('1998-1-1');
      select year(hiredate) from employee;
      select str_to_date('9-13-2019','%m-%d-%Y');
      select 
      ```

      - Notice:
        + %Y  four digits year
        + %y  two digits year
        + %m months(01,02,...11,12)
        + %c months(1,2...,11,12)
        + %d days(01,02...)
        + %H hours(24 hours view)
        + %h  hours(12 hours view)
        + %i  minutes(00, 01, 02...59)
        + %s seconds(00,01,02...59)

      ```mysql
      select * from employees where hiredate= str_to_date('4-3-1669','%c-%d-$Y');
      select date_format(hiredate, '%m-%d-$Y');
      select datediff(max(hiredate), min(hiredate)) as diff;
      where datediff(dob, '1989-1-1')>0; # born after a specified date
      ```

    + other functions

      ```mysql
      select version();
      select datebase();
      select user();
      ```

    + process control functions:

      if	case	

      - case

        In java

        ```java
        switch expression{
        	case const1: statement; break;
        	case const2: statement; break;
        	default: statement; break;
        }
        ```

        In MySQL

        Usage1 (switch)

        ```mysql
        /*
        'case' used to judge fields or expression
        when const1 then to_be_output/statement1; 
        when const2 then to_be_output/statement2; 
        else to_be_output/statementn; 
        end
        */
        
        select salary, department_id,
        case department_id
        when 30 then salary*1.1
        when 40 then salary*1.2
        when 50 then salary*1.3
        else salary
        end as new_salary from employees;
        ```

        Usage2 (if)

        ```mysql
        /*
        case
        when cond1 then to_be_output/statement1; 
        when cond2 then to_be_output/statement2; 
        else to_be_output/statementn;
        end
        */
        select salary,
        case
        when salary>2000 then 'A'
        when salary>1500 then 'B'
        when salary>1000 then 'C'
        else 'D'
        end 
        as salary_level
        from employees;
        ```

  + aggregate function

    sum	avg	max	min	count

    + Notice 

      + ignore null or not? -- **Yes**

      - what types of parameters are supported

    ​                sum, avg -- numerical

    ​                max, min -- sortable

    ​                count -- any type

  + work with distinct to remove duplicates
  + count(*)  -- statistical function

  ```mysql
  # count the number of rows which do not contain null 
  select count(*) from employees; 
  # same with above, add one extra column, element is 1 or null
  select count(1) from employees; 
  # same with above, add one extra column
  select count('Jake') from employees; 
  /*
  Efficiency comparison: 
  MYISAM engine, count(*) is the most efficiency statement
  INNODB enging, count(*) and count(1) are similar, both better than the last one, since it will judge if the field is null or not
  */
  ```

  + the fields query together with group function are required to be the fields after group by

    

  ```mysql
  select column, group_function(column)
  from table
  where condition # can not be group condition
  group by group_by_expression
  having group_condition
  order by column
  ```

  Pregroup query is preferred compared to aftergroup query

  ​        

#### Multiple table query

+ MySQL 92 grammar

  - Cartesian product

    no connection condition, invalid condition

  - alias

    + Benefits: 

      concise

      distinguish same field in different table

    - Notice:

      in select statement, original table name is not allowed if alias is used in from statement. The reason is that from statement is executed before select. So either use alias in whole command or only use original table name

    ```mysql
    # query multiple tables conncection
    select last_name, department_name, city
    from employee e, department d, location l
    where e.'department_id' = d.'department_id'
    and d.'location_id' = l.'location_id'
    and city like 's%'
    order by length(last_name) desc
    ```

  - equal value connection

    n tables, at least n-1 connection conditions

  - nonequal value connection

    ```mysql
    select salary, grade_leve
    from employee e, job_grades g
    where salary between g.'lowest_sal' and g.'highest_sal'
    and g.'grade_leave'='A'
    
    create table job_grades
    (grade_level varchar(3),
     lowest_sal int,
     highest_sal int);
     insert into job_grades
     values('A', 1000, 2999);
     insert into job_grades
     values('B', 3000, 5999);
     insert into job_grades
     values('C', 6000, 9999);
     insert into job_grades
     values('D', 10000, 14999);
     insert into job_grades
     values('E', 15000, 24999);
     insert into job_grades
     values('F', 25000, 40000);
    ```

  - self connection

    ```mysql
    # query employee name and its superior name
    select e.employee_id, e.last_name, m.employee_id, m.last_name
    from employee e, employee m
    where e.'manager_id'=m.'employee_id'
    ```

  - Some examples

    ```mysql
    # 1 show employee name, department id and department name
    select concat(last_name, '_', first_name) as employee_name department_id, department_name
    from employee e, department d
    where e.'department_id' = d.'department_id'
    
    # 2 query employee's job id in department 90 and the location id of that department
    select job_id location_id
    from employee e, department d
    where e.'department_id' = d.'department_id'
    and e.'department_id' = 90
    
    # 3 query all employee's  last_name, department_name, location_id, city who have commission
    select last_name, department_name, l.location_id, city
    from employee e, department d, location l
    where e.'department_id'=d.'department_id'
    and d.'location_id'=l.'location_id'
    and e.'commision_pet' is not null
    
    # 4 query job_tile department_name and minimum salary for every job every department
    select job_tile, department_name, min(salary) as min_sal
    from employee e, departments d, jobs j
    where e.'department_id'=d.'department_id'
    and e.'job_id' = j.'job_id'
    group by job_title, department_name
    
    # 5 query the country id with the corresponding country has more than 2 departments for every country
    select country_id  count(*) as num_department
    from department d, location l
    where d.'location_id' = l.'location_id'
    group by country_id
    having num_department>2
    
    # the alias for column should be put in single quote or double quotes(recommand)
    ```

    

+ MySQL 99 grammar

  + Grammar:

    ```mysql
    select column_name
    from table1 as t1 [join type]
    join table2 as t2
    on connection_condition
    [where filter_condition]
    [group by groupping]
    [having filter_condition]
    [order by oder_list]
    ```

  + join type

    + inner join
    + outer join
      + left join
      + right join
      + full join

    + cross join  -- Descartes 

      ```mysql
      # inner join  $A \cap B$
      select select_list-[pppppp]
      from A
      right join B
      on A.key = B.key
      [where A.key1 is null]
      
      # full join $ A \cup B$
      select select_list
      from A
      full join B
      on A.key = B.key
      [where A.key1 is null or B.key1 is null]
      ```

    - exercise

      ```mysql
      # query employee information who is in department sal or it
      select e.*, d.department_name
      from department d
      left join employee e
      on d.'department_id'=e.'department_id'
      where d.'department_name' in ('sal', 'it');
      ```

  + subquery

    select 后面：

    ​		仅仅支持标量子查询[一行一列]

    from 后面：

    ​		支持表子查询[多行多列] 将子查询结果充当一张表，必须起别名

    where 或 having 后面：

    ​		标量子查询

    ​		列子查询[一列多行]

    ​		行子查询[一行多列]

    exits 后面： （相关子查询）只关心有没有，返回0-1

    /*

    特点：

    1 子查询放在小括号里

    2 子查询一般放在条件的右侧

    3 标量子查询，一般配着单行操作符使用 < > >= <= <>

    4 列子查询一般配着多行操作符使用 in any/some all

    */

    ```mysql
    # query the employee information who has smallest employee_id and highest salary
    select * 
    from employee
    where (employee_id, salary)=(
    	select min(employee_id), max(salary)
    	from employee
    );
    
    # or 
    select *
    from employee
    where employee_id = (select min(employee_id) from employee)
    and salary = (select max(salary) from employee);
    
    # query employee number of each department
    SELECT d.*, (
        SELECT COUNT(*)
        FROM employees e
    	WHERE e.department_id = d.`department_id`
        )employee_number
    FROM departments d;
    /*
    Above has all department_id, employee_number could be 0
    Below does not have 0 in count(*)
    */
    select department_id, count(*)
    from employees
    where department_id is not null
    group by department_id
    
    # query department_name which has employee with employee_id = 102
    select (
    	select department_name
    	from department d
        inner join employees e
        on d.department_id = e.department_id
    	where e.employee_id = 102) dep_name;
    	
    # query salary level of avg(salary) for each department
    # step 1
    select avg(salary), department_id
    from employees
    group by department_id
    
    # step2 
    select * from job_grades
    
    create table job_grades
    (grade_level varchar(3),
     lowest_sal int,
     highest_sal int);
     insert into job_grades
     values('A', 1000, 2999);
     insert into job_grades
     values('B', 3000, 5999);
     insert into job_grades
     values('C', 6000, 9999);
     insert into job_grades
     values('D', 10000, 14999);
     insert into job_grades
     values('E', 15000, 24999);
     insert into job_grades
     values('F', 25000, 40000);
    # step3
    select ag_dep.*, g.`grade_level`
    from (
    	select avg(salary) ag, department_id
    	from employees
    	group buy department_id) ag_dep
    inner join job_grades g
    on ag_dep.ag between lowest_sal and highest_sal;
    
    # 查询有员工的部门名
    select department_name
    from departments
    where exists(
    	select * 
    	from employee e
    	where d.department_id=e.department_id
    );
    # can be replaced by in
    select department_name
    from departments d
    where d.department_id in (
    	select department_id
    	from employees
    );
    # query boys' information who do not have girlfriend
    select bo.*
    from boys bo
    where bo.id not in (
    	select boyfriend_id
    	from beauty
    );
    
    select bo.*
    from boys bo
    where not exists (
    	select boyfriend_id
    	from beauty b
    	where bo.`id`=boyfriend_id
    );
    
    # query employee_id, last_name and salary in each department with their salary above the average salary in his/hers department
    select employee_id, last_name, salary
    from employees e
    inner join(
    	select avg(salary) ag, department_id
    	from employees
    	group by department_id
    ) ag_dep
    on e.department_id = ag.department_id
    where salary > ag_dep.ag
    
    # query employees' employee_id who works for the department with location_id = 1700
    select employee_id
    from employees
    where department_id in (  # in or =any
    	select distinct department_id
    	from departments
    	where location_id = 1700
    );
    
    # query employees' last_name and salary whoes manager last_name is king
    select last_name, salary
    from employees
    where manager_id in (
    	select employee_id 
        from employees
        where last_name='king'
    );
    
    
    ```

  

  + paging query (must be at the last)

    limit start_index(start with index 0), item_number 

    ex: limit 0, 10		limit 10, 10	limit 20, 10	limit 10, 15

  + Execute order

    from - join - on - where - group by - having - select - order by - limit

  

  + Exercise

```mysql
# query the department information which has lowest avg(salary)
# Method 1
# step 1
select avg(salary)
from employees
group by department_id
# step 2
select min(ag)
from (
	select avg(salary) ag, department_id
	from employees
	group by department_id 
) ag_dep;
# step 3
select avg(salary), department_id
from employees
group by department_id
having avg(salary) = (
    select min(ag)
    from (
        select avg(salary) ag, department_id
        from employees
        group by department_id 
    ) ag_dep
);
step 4 
select d.*
from departments d
where d.department_id = (
    select distinct department_id
    from employees
    group by department_id
    having avg(salary) = (
        select min(ag)
        from (
            select avg(salary) ag, department_id
            from employees
            group by department_id 
        ) ag_dep
    )
);

# Method 2
# step 1
select avg(salary)
from employees
group by department_id
# step 2
select *
from departments
where department_id = (
    select avg(salary), department_id
    from employees
    group by department_id
    order by avg(salary) desc
    limit 1
);


# # query the department information and avg(salary) of that department which has lowest avg(salary)
select d.*, ag
from departments d
join (
    select avg(salary) ag, department_id
    from employees
    group by department_id
    order by avg(salary) desc
    limit 1
) ag_dep
on d.department_id = ag_dep.department_id


# query employee_id, last_name and salary in each department with their salary above the average salary in his/hers department
select employee_id, last_name, salary
from employees e
inner join(
	select avg(salary) ag, department_id
	from employees
	group by department_id
) ag_dep
on e.department_id = ag.department_id
where salary > ag_dep.ag

# query all maneger information
select *
from employees 
where employee_id = any(
    select distinct manager_id
    from employees
);

# query the min salary in the department with min max salary among all departments
# step 1
select deparment_id
from employees
group by department_id 
order by max(salary) asc
limit 1
# step 2
select min(salary), department_id
from employees
where department_id = (
    select deparment_id
    from employees
    group by department_id 
    order by max(salary) asc
    limit 1
); # Q: how to avoid equal min max salary --- using distinct?

# query the manager information who is in charge of the deparment with max avg salary
select last_name, d.department_id, salary
from employee e
inner join departments d on d.manager_id=e.employee_id
where d.department_id in (
    select department_id
    from employees
    group by department_id 
    order by avg(salary) desc
    limit 1 
);

# query the female and male students number in each major
select count(*), sex, majorid
from student
group by sex, majorid
# or 
select majorid,
(select count(*) from student where sex='male' and majorid=s.majorid) male,
(select count(*) from student where sex='female' and majorid=s.majorid) female
from student
group by majorid
```

- ​	

  +  union query

    union several tables, query information are the **same** [column information and order] for the several tables

    ```mysql
    # query chinese male information and foreigner male information
    select id, cname, csex from t_ca where sex='male'
    union
    selct t_id, tNmae, tSex from t_ua where tSex='male'
    ```

    - Notice
      + column numbers are the same for the several tables
      + column type and order are the same
      + union delete duplicates(default), union all keep all duplicates

#### DML 

insert	update	delete

+  insert 

  Grammar1: insert into table_name(column_list) value(value1, value2,...)

  Notice: 

   1. The types of values to be inserted should be  compatible with the table, char should be in a pair of single quotes

   2. You must insert value to the column without allowance of null, for nullable column, there are two ways to set it to be null in insertion

      ```mysql
      insert into beaury(id, name, sex, borndate, phone, photo, boyfriend_id)
      values(13,'Elsa', 'female', '1990-01-01', '8037192000', NULL, 2);
      
      insert into beaury(id, name, sex, borndate, phone, boyfriend_id)
      values(13,'Elsa', 'female', '1990-01-01', '8037192000', 2);
      ```

     3. the order of columns is not fixed in insertion command, but they should match with the           corresponding values in values command.

        ```mysql
        insert into beauty(name, sex, id, phone)
        values('guanyu', 'male', 17, '110');
        ```

        

     4. the column names could be omitted(then you should fill all column values by default), but the corresponding values have to keep the order

        ```mysql
        insert into beauty
        values(18, 'zhangfei', 'male', NULL, '119', NULL, NULL);
        # order should be consistent with the order of the  columns in table; NULL is not allowed to be omitted
        ```

  Grammar2: insert into table_name set column1=value1, column2=value2,...

  + Method1 vs Method2

    1. method1 support multiple rows insertion, but method2 does not

       ```mysql
       insert into beauty(name, sex, id, phone)
       values('guanyu', 'male', 17, '110')
       ,('guanguan', 'female', 18, '120')
       ,('yuyu', 'female', 19, '12315');
       ```

       

    2. method1 support subquery, but method2 does not

       ```mysql
       insert into beauty(id, name, phone)
       select 26, 'feifei', '911';
       ```

+ modify

  + modify one table

    update table_name

    set column_name1=new_val1, column_name2=new_val2,...

    where screening condition

  + modify multiple table

    sql92: 

    update table1 alias1, table2 alias2

    set column_name = val. ...

    where connection condition

    and screening condition

    sql99: 

    update table1 alias1

    inner|left|right  join  table2 alias2

    on connection  condition

    set column_name = val. ...

    where screening condition

    ```mysql
    # replace 张无忌's girlfriends' phone number with 114
    update boys bo
    inner join beauty b 
    on bo.id=b.boyfriend_id
    set b.phone= '114'
    where bo.boyName='张无忌';
    
    # set girl's boyfriend_id = 2 if the girl does not have boyfriend
    update boys bo
    inner join beauty b
    on bo.id=b.boyfriend_id
    set b.boyfriend_id=2
    where bo.id is null
    ```

+ delete

  1. delete from one table

  ​       delete from table_name [where screening condition] [limit k];

  2. delete from multiple tables

     Method1:

     sql92:

     delete alias[the alias of the table you are gonna delete]

     from table1 alias1, talbe2 alias2

     where connection condition

     and screening condition

     sql99:

     delete alias[the alias of the table you are gonna delete]

     from table1 alias1

     inner|left|right  join  talbe2 alias2

     on connection condition

     where screening condition

     Method2:

  ​        truncate table table_name;

  ```mysql
  # delete item with phone tail number 9
  delete from beauty where phone like '%9';
  # delete 张无忌’s girlfriend information
  delete b
  from beauty b
  inner join boys bo 
  on b.boyfriend_id = bo.id
  where bo.boyName= '张无忌'
  
  # delete 黄晓明's information and his girlfriends'
  delete b, bo
  from beauty b
  inner join boys bo
  on b.boyfriend_id=bo.id
  where bo.boyName='黄晓明'
  
  # delete boys' information 
  truncate table boys;
  ```

  ​	delete pk truncate:

   1. delete could have where screening condition after it, whereas truncate could not

   2. truncate is a little bit more efficiency

   3. If insertion after delete, the new data index start with the breakpoint, whereas the insertion after truncate start with increment column n+1 if the table has self increment column

   4. truncate -- no return value; delete -- has return value

   5. truncate -- can not rollback; delete -- can do rollback

      

+ DDL 

  database management: create delete alter

  + database creation

    create database [if not exists] database_name [character set char_set_name];

  + database modification

    rename database database_name to new_database_name;

    alter database database_name character set gbk; #change character repertoire

  + delete

    drop database if exists database_name;

  table management

  + table creation

    create table table_name (

    ​	col_name1 col_type1 [constrain1],

    ​	col_name2 col_type2 [constrain2],

    ​	...

    ​	col_namen col_typen [constrainn]

    )

    ```mysql
    create table [if not exists] book(
    	id int, # index
    	bName varchar(20), # book name
    	author varchar(20), # author name
    	authorId int, # auther id
    	publishDate datetime # publish date
    );
    desc book;
    create table author(
    	id int,
    	au_name, varchar(20),
    	nation varchar(20)
    );
    desc author;
    ```

  + table modification

    + column name: 

      alter table table_name change column column_name new_column_name col_type

      ```mysql
      alter table book change column publishDate pubdate datetime;
      ```

    + column type or constrain

      alter table table_name modify column column_name col_type;

      ```mysql
      alter table book modify column pubdate timestamp;
      ```

    + add new column

      alter table table_name add column col_name col_type;

      ```mysql
      alter table author add column annual double [first|after|last col_name]; 
      # command in [] seldom used
      ```

    + delete column

      alter table table_name drop column col_name;

      ```mysql
      alter table author drop column annual;
      ```

    + table name

      alter table table_name rename to new_table_name;

      ```mysql
      alter table author rename to book_author;
      ```

  + table delete

    drop table table_name;

    ```mysql
    drop table [if exists] book_author;
    show tables;
    ```

    Common method:  --- first delete then create

    drop database if exists old_database_name;

    create database new_database_name;

    drop table if exists old_table_name;

    create table new_table_name();

    

  + table duplicate

    ```mysql
    use test;
    insert into author values
    (1, 'Siga', 'japan'),
    (2, 'feifei', 'china');
    ```

    + just duplicate the structure of the table 

      ```mysql
      create table copy like author
      ```

    + copy the structure and data

      ```mysql
      create table copy2
      select * from author
      ```

      just copy part data

      ```mysql
      create table copy3
      select id, au_name
      from test.author  # can across database, you can avoid database name if under same database
      where nation='china';
      ```

      just copy some fields

      ```mysql
      create table copy4
      select id, au_name
      from author
      where 0;
      # no data in the table copy4, just fields id and au_name
      ```

  

  + Data type

    + int 

      tinyint, 			smallint, 			mediumint, 			int/integer, 			bigint

      1            			2             			3                    			4                   			8           --- bytes

      (-128~127)  （-32768~32767）（-8388608，8388607）...                                 ---- signed

      (0~255)           (0~65535)            (0~1677215)                                                          ---- unsigned

         

      + sign, unsigned

        ```mysql
        create table tab_int(
        	t1 int, # default setting -- signed
        	t2 int unsigned
        );
        ```

      + Feature:

        1. default setting is signed, need attach unsigned after data type to get an unsigned value

        2. will throw out 'out of range' error if the number to be inserted is out  of corresponding range and the critical value will be inserted

        3. integer type has default length if it is not specified, length represents maximum width, 

           left pad the number with 0 using command zerofill (default unsigned)

           ```mysql
           create table tab_int(
           	t1 int(7) zerofill, # default setting -- signed
           	t2 int(7) zerofill
           );
           ```

    + decimal

      ​      floating point										fixed point

      float				double						dec(M,D)		decimal(M,D)	

      ​	 4					8									        	M+2                                  ---- bytes

      ```mysql
      create table tab_float(
      	f1 float(5,2),
      	f2 double(5,2),
      	f3 decimal(5,2)
      );
      insert into tab_float values 
      (123.45, 123.45,123.45),
      (123.456, 123.456, 123.456)， # it will be truncated to two decimal places
      (123.4, 123.4,123.4), # fill second decimal with zero
      (1233.4, 1233.4,1233.4); #fill with critial number(999.99, 999.99, 999.99)
      ```

      + Feature:

        1. M: integer places + decimal places

           D: decimal places

        2. M and D could be omitted, if it is decimal, then the default setting is M=10, D=0; if it is float or double, then M and D are depends on the number to be inserted.

        3. fixed point has higher accuracy, if the number to be inserted need high accuracy, for example, currency, then fixed point is a good choice
        4. Choose easier data type if you can (less memory)

    + char type

      + ​				short string									              long string

        ​	    char					varchar						text 			            blob(large binary)        

      ​		        Feature:

      ​				char ---- char(M) ----M means maximum number of char (could be omitted, default 1)

      ​                          --- fixed length -- memory consuming

      ​				varchar- varchar(M)-M means max number of char (could not be omitted)

      ​                           -- variable length -- save memory

    + binary  varbinary (similar with char vs varchar)

    + Enum  

      ``` mysql
      create table tab_char(
      	t1 enum('a','b','b')
      );
      insert into tab_char values
      ('a'),
      ('b'),
      ('a');
      ```

    + set 

    + datetype

      date 	-- 	4 bytes 	-- 	min 1000-01-01 	-- 	max 9999-12-31

      datetime-   8 bytes     --	 min  1000-01-01 00:00:00	-- 	max 9999-12-31 23:59:59

      timestamp 4 bytes     --	 min  19700101080001		  --     max some time in 2038

      time 	--	 3 bytes	 --	 min -838:59:59					  --	 max  838:59:59

      year	 --	 1 bytes	  -- 	min 1901		 					   -- 	max  2155

     ```mysql
    create table tab_date(
    	ti datetime,
    	t2 timestamp
    );
    insert into tab_date value(now(), now());
    show variables like 'time_zone';
    set time_zone='+9:00'
     ```

    ​	timestamp vs datatime

    	1. timestamp support a narrow time period; datatime support a wider time period
     	2. timestamp depends on time zone, reflect real time; datatime only reflect local time when insert
     	3. timestamp depends on MySQL version and SQLMode

    + sfaas

  + Constraint

    not null		unique		primary key(not null and unique)	foreign key		check		default

    + MySQL does not support check, but you can use check without any effect
    + When add constraint: creation/modification
    + column level constraint vs table level constraint
      1. column level constraint: all six constraints are applicable, but foreign key constraint has no effect; position: no constraint name
      2. table level constraint does not support not null, default, the rest are supported; could have constraint name(no effect on primary key)

    ```mysql
    create table table_name(
    	field1 type1 column_level_constraint,
    	field2 type2,
    	table_level_constraint
    );
    ```

    + Adding constraint when creating table

      + add column level constraint

        support  not null, primary key, default, unique

        ```mysql
        create database students;
        use students;
        create table stuinfo(
            id int primary key,
            stuName varchar(20) not null,
            gender char(1) check(gender='male' or gender='female'),
            seat int unique not null, # could have multiple constraint
            age int default 18,
            magorId int references major(id)
        );
        create table major(
        	id int primary key,
        	majorName varchar(20)
        );
        # lookup constraint
        desc stuinfo;
        show index from stuinfo; # index includes primary key, foreign key, unique
        ```

      + add table level constraint

        Grammar: [constraint constraint_name] constraint_type(field) 

        ```mysql
        drop table if exists stuinfo;
        create table stuinfo(
        	id int,
        	stuname varchar(20),
            gender char(1),
            seat int,
            majorId int,
            constraint pk primary key(id),
            constraint uq unique(seat),
            constraint ck check(gender='male' or gender='female'),
            constraint fk_stuinfo_major foreign key(majorId) references major(id)
        );
        show index from stuinfo;
        ```

      + Common usage:

        ```mysql
        create table if not exists stuinfo(
            id int primary key,
            stuName varchar(20) not null,
            gender char(1) check(gender='male' or gender='female'),
            seat int unique,
            age int default 18,
            magorId int,
            constraint fk_stuinfo_major foreign key(majorId) references major(id)
        );
        ```

      + primary key vs unique

        + primary key:   unique   not nullable	at most one pk	combination allowed(不推荐)

        + unique:            unique   nullable           0 to n                    combination allowed(不推荐)

          ```mysql
          # combination
          primary key(id, seat);
          ```

      + foreign key 
        + only allowed to set foreign key in secondary table
        + the data type of foreign key column in secondary table must consistent with or compatible with the data type of the corresponding column in primary table
        + the corresponding column in primary table must be a key (usually primary key or unique)
        + when inserting data, first insert primary table then secondary table; when deleting data, first secondary table then primary table

    + Adding constraint when modification

      ```mysql
      drop table if exists stuinfo;
      create table stuinfo(
      	id int,
          stuname varchar(20),
          gender char(1),
          seat int,
          age int,
          majorid int
      );
      desc stuinfo;
      
      alter table stuinfo modify column stuname varchar(20) not null;
      alter table stuinfo modify column ag int default 18;
      alter table stuinfo modify column id int primary key; # column level constraint
      alter table stuinfo add primary key(id); # table level constraint
      alter table stuinfo modify column seat int unique; # column level constraint
      alter table stuinfo add unique(seat); # table level constraint
      ```

    + delete constraint when modification

      ```mysql
      # delete not null constraint
      alter table stuinfo modify column stuname varchar(20) null; 
      # delete default constraint
      alter table stuinfo modify column age int;
      # delete primary key
      alter table stuinfo drop primary key;
      # delete unique
      alter table stuinfo drop index seat;
      # delete foreign key
      alter table stuinfo drop foreign key fk_stuinfo_major;
      
      show index from stuinfo;
      ```

    ```mysql
    # add primary key constaint(my_emp_id_pk) to col id in emp2 table
    alter table emp2 modify column id int primary key;
    alter table emp2 add constraint my_emp_id_pk primary key(id);
    # add column dept_id in table emp2, and define it as forein key, its reference is dep
    alter table emp2 add column dept_id int;
    alter table emp2 add constraint fk_emp2_dept2 foreign key(dept_id) references dep;
    ```

    + cascade delete

      when deleting data in primary table

      + cascade delete

        ```mysql
        select * from major;
        insert into major
        values(1,'java'),(2,'h5'),(3,'big data');
        
        select * from stuinfo;
        insert into suinfo
        select 1, 'john1', 'female', null, null, 1 union all
        select 2, 'john2', 'female', null, null, 1 union all
        select 3, 'john3', 'female', null, null, 2 union all
        select 4, 'john4', 'female', null, null, 3 union all
        select 5, 'john5', 'female', null, null, 3 union all
        select 6, 'john6', 'female', null, null, 1 union all
        select 7, 'john7', 'female', null, null, 1 
        
        alter table stuinfo add constraint fk_stu_major foreign key(majorid) references major(id)
        # deletion
        delete from major where id=3; # throw out an error because of foreign key
        # cascade deletion 级联删除
        alter table stuinfo add constraint fk_stu_major foreign key(majorid) references major(id) on delete cascade;
        delete from major where id=3;
        /*
        Corresponding items with majorid=3 are deleted
        */
        # cascade empty级联置空
        alter table stuinfo add constraint fk_stu_major foreign key(majorid) references major(id) on delete set null;
        delete from major where id=3;
        /*
        Corresponding items with majorid=3 are set to majorid=null
        */
        ```

        

    + identity column (auto increment column)

      system provide default sequence value

      ```mysql
      drop table if exists tab_identity;
      create table tab_identity(
      	id int primary key auto_increment,
          name varchar(20)
      );
      truncate talbe tab_identity;
      insert into tab_identity(id, name) values(Null, 'liubei'); # auto number
      insert into tab_identity(name) values('guanyu');
      select * from tab_identity;
      ```

      ```mysql
      set auto_increment_increment = 3;  # step length
      # you can not set auto_increment_offset; initial value
      ```

      + Notice: 

        identity column must be a key (primary key or unique)

        at most one identity column in a table

        the type of identity column must be numerical type

        use set auto_increment_increment = 3 to change step length; manually insert a value to set

        initial index value

      + Set and delete identity column when modification

        ```mysql
        alter table tab_identity modify column id int primary key auto_increment;
        alter table tab_identity modify column id int; # delete auto_increment;
        ```

+ #### TCL

  Transaction control Language

  A transaction is a logical unit of work that contains one or more SQL statements. Transactions are atomic units of work that can be committed or rolled back. 

  + MySQL engines: innodb(default), myisam, memory ...

  ```mysql
  show engines; # check storage engine
  ```

  ​	innodb supports transaction; others do not
  + Transaction properties: ACID  (Atomicity, Consistency, Isolation, Durability) 

    **Atomicity:**  not separable, do all or do nothing

    **Consistency:** An execution convert data from one consistent status to another consistent status

    **Isolation:** Transactions are isolated, do not affected by other transaction

    **Durability:** Once committed, data changed permanently.

  + transaction creation

    + implicit transaction: does not have clear start and end marks. Ex insert update delete

      ```mysql
      delete from table1 where id=1;
      ```

    + explicit transaction: firstly, set autocommit = 0;

      ```mysql
      # step 1: start transaction
      set autocommit=0;
      start transaction; # optional
      # step 2 sql commands (select insert update delete)
      # step 3 end transaction
      commit;# submit transaction
      rollback; # roll back transaction
      savepoint;
      ```

      ```mysql
      drop table if exists account;
      create table account(
      	id int primary key auto_increment,
          username varchar(20),
          balance double
      );
      insert into account(username, balance)
      values('wuji', 1000), ('zhaoming', 1000);
      show variables like 'autocommit'
      show engines;
      
      set autocommit = 0;
      start transaction;
      update account set balance=500 where username='wuji';
      update account set balance=1500 where username='zhaoming';
      commit;
      select * from account;
      
      set autocommit = 0;
      start transaction;
      update account set balance=1000 where username='wuji';
      update account set balance=1000 where username='zhaoming';
      rollback;
      select * from account; # does not change
      
      
      ```

  + Database isolation level
    + 对于同时运行的多个事务, 当这些事务访问数据库中相同的数据时, 如果没有采取必要的隔离机制, 就会导致各种并发问题:
      脏读: 对于两个事务T1, T2, T1 读取了已经被T2 更新但还没有被提交的字段. 之后, 若T2 回滚, T1读取的内容就是临时且无效的. (一个事务读取了其他事务更新的数据)
      不可重复读: 对于两个事务T1, T2, T1 读取了一个字段, 然后T2 更新了该字段. 之后, T1再次读取同一个字段, 值就不同了. （一个事务多次读取，结果不同）
      幻读: 对于两个事务T1, T2, T1 从一个表中读取了一个字段, 然后T2 在该表中插入了一些新的行. 之后, 如果T1 再次读取同一个表, 就会多出几行. （一个事务读取了其他事务还没有提交的数据，只是读到的是其他事务插入的数据）

  + 数据库事务的隔离性: 数据库系统必须具有隔离并发运行各个事务的能力, 使它们不会相互影响, 避免各种并发问题.
    一个事务与其他事务隔离的程度称为隔离级别. 数据库规定了多种事务隔离级别, 不同隔离级别对应不同的干扰程度, 隔离级别越高, 数据一致性就越好, 但并发性越弱.

    + 数据库提供的4 种事务隔离级别:

      + | 隔离级别         | 描述                                                         |
        | ---------------- | ------------------------------------------------------------ |
        | read uncommitted | 允许读到其它未提交事务的数据，也称之为**脏读**，不可重复读和幻读的问题都会出现 |
        | read committed   | 只允许事务读取已经被其他事务提交的变更， 可避免脏读，但是不可避免不可重复读和幻读。Oracle 和 SQL Server 的默认隔离级别 |
        | repeatable read  | 确保事务可以多次从一个字段读取相同的值，在这个事务持续期间，禁止其他事务对该字段进行更新，可以避免脏读和不可重复读，但是不能避免幻读。该隔离级别是 MySQL 默认的隔离级别 |
        | serialization    | 在该隔离级别下事务都是串行顺序执行的，这这个事务持续期间，禁止其他事务对该表执行插入更新删除操作，MySQL 数据库的 InnoDB 引擎会给读操作隐式加一把读共享锁next-key locks，从而避免了脏读、不可重读复读和幻读问题，但性能低下 |

      + Oracle 支持的2 种事务隔离级别：READ COMMITED, SERIALIZABLE。Oracle 默认的事务隔离级别为: READ COMMITED
        Mysql 支持4 种事务隔离级别. Mysql 默认的事务隔离级别为: REPEATABLE READ

    + 操作

      ```mysql
      # in command prompt
      select @@transaction_isolation; # check transaction isolation,default: repeatable read
      set session transaction isolation level read uncommitted;
      select @@transaction_isolation; 
      use test;
      select * from account;
      set names gbk; # solve encoding problem
      select * from account;
      /*
      id            username                balance
      25             wuji                    1000
      28            zhaoming                 1000
      */
      set autocommit=0;
      update account set usernaem='john' where id=25;
      
      # open anathor command prompt
      select @@transaction_isolation; # repeatable read
      set session transaction isolation level read uncommitted;
      select @@transaction_isolation;
      use test;
      set autocommit=0;
      select * from account;
      /*
      id            username                balance
      25             john                    1000
      28            zhaoming                 1000
      */
      
      # back to previous command prompt
      rollback;
      
      # back to second command prompt
      select * from account;
      /*
      id            username                balance
      25             wuji                    1000
      28            zhaoming                 1000
      */
      
      # first command prompt
      set session transaction isolation level read committed;
      set autocommit=0;
      update account set username='john' where id=25;
      # second command prompt
      select * from account;
      /*
      id            username                balance
      25             wuji                    1000
      28            zhaoming                 1000
      */
      # first command prompt
      commit;
      # second command prompt
      select * from account;
      /*
      id            username                balance
      25             john                    1000
      28            zhaoming                 1000
      */
      
      # set global transaction isolation level
      set global transaction isolation level read committed; 
      
      # savepoint usage
      set autocommit=0;
      start transaction;
      delete from account where id=25;
      savepoint a;
      delete from account where id=20;
      rollback to a; # just roll back to a point
      ```

  + View

    MySQL从5.0.1版本开始提供视图功能。一种虚拟存在的表，行和列的数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的，只保存了sql逻辑，不保存查询结果
    应用场景：
    多个地方用到同样的查询结果
    该查询结果使用的sql语句较复杂

    + benefits of view (like packaging)
      + reusing sql commands
      + simplify sql operation, do not have to know query details
      + protect data, improve security

    + view creation

      create view view_name

      ```mysql
      # query the employees' last_name which contains 'a', department_name, job_title
      # creation
      create view myv1
      as
      select last_name, department_name, job_title
      from employee e
      join department d on e.department_id=d.department_id
      join jobs j on j.job_id=e.job_id
      # use
      select * from myv1 where last_name like '%a%'
      # query avg(salary) of each department
      create view myv2
      as 
      select avg(salary) ag, department_id
      from employees
      group by department_id;
      
      select myv2.'ag', g.grade_level
      from myv2
      join job_grades g
      on myv2.'ag' between g.'lowest_sal' and g.'highest_sal'
      
      # query the department info with lowest avg(salary)
      select * from myv2 order by ag limit 1;
      # query the department name and salary for the department with lowest avg(salary)
      create view myv3
      as 
      select * from myv2 order by ag limit 1;
      
      select d.*, m.ag
      from myv3 m
      join departments d
      on m.department_id = d.department_id;
      ```

    + view modification

      + method 1: create or replace view view_name as

      + method 2: alter view view_name as 

        ```mysql
        create or replace view myv3
        as
        select avg(salary), job_id
        from employees
        group by job_id;
        
        alter view myv3
        as 
        select * from employees;
        ```

    + delete view (you need the authorization first)

      drop view view_name1, view_name2,...

    + look over view

      + desc myv3;  
      + show create view myv3;    ----  show the creation process

    + update view

      ```mysql
      create or replace view myv1
      as 
      select last_name, email, salary*12*(1+ifnull(comission_pct,0)) "annual salary"
      from employees;
      
      select * from myv1;
      # insertion
      insert into myv1 values('feifei', 'feifi@qq.com'); # update original table also
      # midification
      update myv1 set last_name='wuji' where last_name='feifei';
      #deletion 
      delete from myv1 where last_name='wuji'
      ```

      + view with the follow features could not been updated

        + sql commands contain: aggregate function, distinct,group by,having,union,union all

        + constant view

          ```mysql
          create or repalce view myv2
          as 
          select 'john' name;
          ```

        + Select has subquery

          ```mysql
          create or replace view myv3
          as 
          select department_id, (select max(salary) from employees) max_sal
          from departments;
          ```

        + join

          ```mysql
          create or replace view myv4
          as
          select last_name, department_id
          from employees e
          join departments d
          on e.department_id = d.department_id;
          
          # update
          select * from myv4;
          update myv4 set last_name='feifei' where last_name='wuji' # doable
          insert myv4 values('zhenzhen', 'xxxx') # error
          ```

        + from a view which is not able to be updated

          ```mysql
          create or replace view myv5
          as 
          select * from myv3;
          # update
          select * from myv5
          update myv5 set max_sal=10000 where department_id=60; # not updatable
          ```

          

        + The subquery in where used some table in from statement

          ```mysql
          create or replace view myv6
          as 
          select last_name, email, salary
          from employees
          where employee_id in (
              select manager_id
              from employees
              where manager_id is not null
          );
          
          select * from myv6;
          update myv6 set salary=10000 where last_name='king' # not updatable
          ```

      + view vs table

      + |       | statement    | memory              | operation             |
        | ----- | ------------ | ------------------- | --------------------- |
        | view  | create view  | only save sql logic | 增删改查,通常不增删改 |
        | table | create table | save data           | 增删改查              |

      + delete vs truncate

        delete can roll back; truncate can not roll back

  + variable

    1. system variable: global variables, session variables
    2. self-defined variable: user variable, local variables

    + system variable

      ```mysql
      /*
      if it is global variable, then keyword global is necessary
      if it is session variable, then add keyword session
      default is session
      */ 
      # look up all variable
      show global variables;
      show session variables;
      # look up all variable with condition
      show global variables like '%char%';
      show session variables like '%char%';
      # look up a specified syetem variale
      select @@global.system_variable_name;
      select @@session.system_variable_name;
      # assign a specified system variable
      set golbal system_variable_name = value1;
      set session system_variable_name = value2;
      
      set @@global.system_variable_name = value1;
      set @@session.system_variable_name = value2;
      ```

      + global variable

        action scope: server assigns initial values to all global variables when to start, it is valid for all connect session, but not for restart.

      + session variable

        action scope: just work for the session at present

    + self-defined variable

      + user variabel

        action scope: just work for current session

        ```mysql
        # declarition and initialization
        set @user_variable_name = value1;
        # or
        set @user_variable_name := value1;
        # or
        select @user_variable_name := value1;
        
        # assignment
        # Method 1
        set @user_variable_name = value1;
        # or
        set @user_variable_name := value1;
        # or
        select @user_variable_name := value1;
        # Method 2
        select field1 into user_variable_name
        from table1;
        # example as follow
        select count(*) into @count
        from employees;
        
        # look up the value of user variables
        select @user_variable_name;
        ```

      + local variable

        action scope: just in begin end where its definition is. Just used in the first statement in begin end.

        ```mysql
        # declarition
        declare var_name type;
        declare var_name type default val;
        # assignment
        # Method 1
        set @var_name = value1;
        # or
        set @var_name := value1;
        # or
        select @var_name := value1;
        # Method 2
        select field1 into var_name
        from table1;
        # use
        select var_name;
        ```

        | ~         | scope       | 定义和使用的位置                 | 语法                 |
        | --------- | ----------- | -------------------------------- | -------------------- |
        | user var  | 当前会话    | 会话中任何位置                   | 必有@， 不用限定类型 |
        | local var | begin end中 | 只能在begin end 中，且为第一句话 | 一般无@，要限定类型  |

  + storage process and function (similar with method in java)

    + benefits: reusing, simplify operation, reduce compile  and serve connection  times (efficiency)

    + storage process: a collection of pre-compiled SQL statements, just like batch statements

      + creation

        ```mysql
        create procedure storage_process_name(prameter list)
        begin
        	storage process block (a collection of legal SQL statements)
        end
        ```

      + Notice:

        1. parameter list contains three parts
           parameter mode  --  parameter name   --   parameter type

           ```mysql
           in stuname varchar(20)
           ```

        2. Parameter mode
           + in: this parameter could be input, in other words, this parameter need incoming value from caller
           + out: this parameter could be output, in other words, this parameter could be used as return value
           + inout: this parameter could be input or output		

        3. if there is only one statement in begin end, then begin end could be omitted. In the storage process block, every SQL statement must end with semicolon. It could be reset by using delimiter at the end of storage process. 

           ```mysql
           delimiter end_mark
           delimiter;
           ```

      + call

        ```mysql
        call storage_process_naem(parameter list)
        ```

        ```mysql
        # empty parameter list
        # ex: insert four rows in table admin 
        select * from admin;
        delimiter $
        create procedure myp1()
        begin
        	insert into admin(username, password) 
        	values('john','000'),('tom','001'),('jerry','002'),('rose','003');
        end 
        
        # call, end with $ sign
        call myp1()$
        
        # create storage process with in parameter
        # ex: create storage process, query boy's info according to girl's info
        create procedure myp2(in beautyNme varchar(20))
        begin
        	select bo.*
        	from boys bo
        	right join beauty b 
        	on bo.id=b.boyfriend_id
        	where b.name=beautyName;
        end $
        
        call myp2('rose') $
        # set names gbk --- if you have error incorrect string value: '\xC1\xF8...'
        
        # ex: create storage process, check if login successful or not
        create procedure myp3(in username varchar(20), in password varchar(20))
        begin
        	declare result varchar(20) default '' # declaration and initializati
        	select count(*) into result # assignment
        	from admin
        	where admin.username = username
        	and admin.password=password;
        	select result; # usage
        end $
        
        call myp3('feifei', '8888') $
        
        
        
        create procedure myp4(in username varchar(20), in password varchar(20))
        begin
        	declare result varchar(20) default '' # declaration and initializati
        	select count(*) into result # assignment
        	from admin
        	where admin.username = username
        	and admin.password=password;
        	select if(result>0,'success', 'fail'); # usage
        end $
        
        call myp4('feifei', '8888') $
        
        
        # create storage process with out parameter
        # ex: create storage process, query boy's name according to girl's info
        create procedure myp5(in beautyName varchar(20), out boyName varchar(20))
        begin
        	select bo.boyName into boyName
        	from boys bo
        	inner join beauty b 
        	on bo.id=b.boyfriend_id
        	where b.name = beautyName;
        end $
        # call
        set @bName$
        call myp5('rose',@bName) $
        select @bName$
        
        # ex: create storage process, query boy's userCP according to girl's info
        create procedure myp6(in beautyName varchar(20), out boyName varchar(20), out userCP int)
        begin
        	select bo.boyName, bo.userCP into boyName, userCP
        	from boys bo
        	inner join beauty b 
        	on bo.id=b.boyfriend_id
        	where b.name = beautyName;
        end $
        call myp6('rose',@bName,@userCP) $
        select @bName, @userCP $
        
        
        # create storage process with inout parameter
        # input a and b, then return doubled a and b
        create procedure myp8(inout a int, inout b int)
        begin
        	set a=a*2;
        	set b=b*2;
        end $
        # call
        set @m=10 $ 
        set @n=20 $
        call myp8(@m, @n) $
        # look up
        select @m, @n $
        
        
        
        # ex: create storage process, input username and password and insert them into table admin 
        create procedure test_p1(in username varchar(20), in loginPwd varchar(20))
        begin
        	insert into admin(admin.username, Password)
        	values(username, loginPwd);
        end $
        call test_p1('admin', '0000') $
        select * from admin$
        
        # create storage process, 
        ```

      + delete storage procedure

        ```mysql
        # once only one procedure, no combinaion
        drop procedure storage_procedure_name; 
        ```

      + look up storage procedure

        ```mysql
        # desc myp2; is wrong
        show create procedure myp2
        ```

      + Exercise

        ```mysql
        # create storage procedure, input a date, convert its format to %y%m%d and return
        create prodedure test_p2(in mydate datetime, out strDate varchar(20))
        begin
        	select date_format(mydate, '%y%m%d') into strDate;
        end $
        call test_p2(now(), @str) $
        select @str $
        
        # create storage procedure, input beautyName, return beauty and boy
        create procedure test_p3(in beautyName varchar(20), out str varchar(50))
        begin
        	select concat(beautyName, ' and ', ifnull(boyName,'null') ) into str
        	from boys bo
        	right join beauty b 
        	on b.boyfriend_id = bo.id
        	where b.name=beautyName;
        end $
        call test_p2('rose', @str) $
        select @str $
        
        # create storage procedure, input item number and start index, return beauty table record
        create procedure test_p3(in startIndex int, in size int)
        begin
        	select * from beauty limit startIndex, size;
        end $
        call test_p3(3,5)$
        ```

    + function

      function has and only has one return, fit to deal data and return a result

      procedure could have 0 return or multiple return, fit to do batch insert and batch update 

      + creation

        ```mysql
        create function func_name(para_list) returns return_type
        begin
        	func_body
        end
        ```

        parameter_list contains two parts: para_name, para_type

        function_body: must have return statement, if not, there will be an error throw out; if return statement is not put at the end of function, there will be no error throw out, but not recommend; if there is only one statement if function_boy, then begin end could be omitted

        delimiter: delimiter can be used as end mark

        ```mysql
        delimiter $;
        ```

      + call

        ```mysql
        select func_name(para_list)
        ```

      + Exercise

        ```mysql
        # no para has return
        create function myf1() return int
        begin
        	declare c int default 0; # define local variable
        	select count(*) into c
        	from employees;
        	return c;
        end $
        select myf1() $
        
        # ex return salary base on employee name
        create function myf2(empName varchar(20)) return double
        begin
        	set @sal=0; # define user variable
        	select salary into @sal
        	from employees
        	where last_name = empName;
        	return @sal	
        end $
        select myf2('k_ing') $ # make sure there is only one out put otherwise error
        
        # return avg(salary) for given department_name
        create function myf3(deptName varchar(20)) return double
        begin
        	declare sal double;
        	select avg(salary) into sal
        	from employees e
        	join department d 
        	on e.department_id = d.department_id
        	where d.department_name=deptName;
        	return sal
        end $
        select myf3('IT') $
        
        ```

      + look up function

        ```mysql
        show create function myf3;
        ```

      + delete function

        ```mysql
        drop function myf3;
        ```

  + process control structure

    sequential structure, branch structure, loop structure

    + branch structure

      + if function (could be any place)

        if  (exp1, exp2, exp3) 

        exp1 is true, then do exp2 else do exp3.

      + case  structure (in or out of begin end)

        scenario 1: similar with switch in java, usually used as value judgement

        ​	case var|exp|field

        ​	when val1 then return_val1 or sentence1;

        ​	when val2 then return_val2 or sentence2;

        ​	...

        ​	else return_val or sentence;

        ​	end case;

        scenario 2: similar with if in java, usually used as interval judgement

        ​	case 

        ​	when cond1 then return_val1 or sentence1;

        ​	when con2 then return_val2 or sentence2;

        ​	...

        ​	else return_val or sentence;

        ​	end case;

        ~~~mysql
        # create procedure, show grade level based on input garde
        create procedure test_case(in score int)
        begin
        	case
        	when score>= 90 and score<= 100 then select 'A';
        	when score>=80 then select 'B';
        	when score>=70 then select 'C';
        	else select 'D';
        	end case;
        end $
        call test_case('89') $
        ~~~

      + if  structure (in begin end)

        if cond1 then sentence1;

        elseif cond2 then sentence2;

        ...

        [else sentence;]

        end if;

        ```mysql
        create function test_if(score int) return char
        begin
        	if score>= 90 and score <=100 then return 'A';
        	elseif score>=80 then return 'B';
        	elseif score>=70 then return 'C';
        	else return 'D';
        	end if;
        end $
        select test_if(89) $
        ```

    + loop structure

      while		loop		repeat

      loop control: 

      ​	iterate -- similar with continue

      ​	leave   -- similar with break

      + while

        ```mysql
        [label:]while while_cond do
        	while_body;
        end while [label];
        ```

      + loop 

        can be used to simulate dead loop

        ````mysql
        [label:] loop
        	loop_body;
        end loop [label];
        ````

      + repeat (execute first, then judge)

        ```mysql
        [label:] repeat
        	repeat_body;
        until end_repeat_cond
        end repeat[label];
        ```

      + Exercise

        ```mysql
        # batch insert into table admin according to given number
        create procedure pro_while1(in insertCount int)
        begin
        	declare i int default 1;
        	while i<=insertCount do
        		insert into admin(username, password) values(concat('rose',i), '666');
        		set i=i+1;
            end while;
        end $
        call pro_while1(10) $
        
        # batch insert into table admin according to given number, break if more than 2o items
        truncate table admin$
        drop procedure test_while1$
        create procedure test_while1(int insertCount int)
        begin
        	declare i int default 1;
        	a: while i <= insertCount do
        		insert into admin(username, password) values(concat('rose',i), '666');
        		if i>=20 then leave a;
        		end if;
        		set i = i+1;
        	end while a;
        end $
        call test_while1(100) $
        select * from admin$
        
        # add iterate statement
        # batch insert into table admin according to given number, just insert even times
        truncate table admin$
        drop procedure test_while1$
        create procedure test_while1(int insertCount int)
        begin
        	declare i int default 0;
        	a: while i <= insertCount do
        		set i = i+1;
        		if mod(i,2) !=0 then iterate a;
        		end if;
        		insert into admin(username, password) values(concat('rose',i), '666');	
        	end while a;
        end $
        call test_while1(100) $
        select * from admin $
        
        
        /*
        table stringcontent
        id autoincrement
        contern varchar(20)
        
        insert given number of random string into the table
        */ 
        drop table if exists stringContent;
        create table stringContent(
        	id int primary key auto_increment,
            content varchar(20)
        );
        delimiter $
        create procedure test_randstr_insert(in insertCount int)
        begin
        	declare i int default 1;
        	declare str varchar(26) default 'abcdefghijklmnopqrstuvwxyz';
        	declare startIndex int default 1;
        	declare len int default 1;
        	while i<=insertIndex do		
        		set len=floor(rand()*(20-startIndex+1)+1);
        		set startIndex=floor(rand()*26+1);
        		insert into stringContent(content) values(substr(str, startIndex,len));
        		set i = i+1；
            end while;
        end $
        call test_randstr_insert(10) $
        select * from stringContent$
        ```

        + Notice
          1. Label is not necessary, it is necessary when using loop control language(leave iterative)
          2. loop -- usually used to implement dead loop; while -- judge then do; repeat -- do then judge(at least do once)

  

  

  

  

  + 

  ```
  
  ```

  

  

  

  

  

