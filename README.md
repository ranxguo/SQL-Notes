# SQL--基础

# 数据库的基本概念（Database）简称DB：用于储存和管理数据仓库

  常见的数据库软件：

    Oracle
    MySQL
    Microsoft SQL Server
    PostgreSQL
    MongoDB
    DB2 (IMB)
    Cassandra
    Microsoft Access
    Redis
    SQLite (嵌入式的小型数据库，应用于手机)

  MySQL:

    登陆：
      mysql -uroot -p(password)
      mysql -h127.0.0.1 -uroot -p(password)
      mysql --host=ip --user=root --password=password
    退出：
      exit
      quit


# SQL(Structured Query Language)

1. 什么是SQL？

    Structured Query Language：结构化查询语言。
    其实就是定义来操作所有关系型（relational）数据库的规则。每一种数据库操作的方式存在不一样的地方，简称“方言”。

2. SQL的通用语法：

  1. SQL语句可以单行或多行书写，以分号（；）结尾。
  2. 可以使用空格和缩进来增强语句的可读性。
  3. MySQL数据库的SQL语句不区分大小写，关键字建议使用大写。
  4. 三种注释：
    * 单行注释：-- 注释内容 或 # 注释内容（MySQL特有）
    * 多行注释： /* 注释*/

3. SQL分类：

  1. DDL（Data Definition Language）数据定义语言
  2. DML（Data Manipulation Language）数据操纵语言
  3. DQL（Data Query Language）数据查询语言
  4. DCL（Data Control Language）数据控制语言


## DDL: 操作数据

  1. 操作数据库：CRUD
    1. C（Create）：创建
      * create database 数据库名; （创建数据库）
      * create database if not exists 数据库名; (创建数据，判断是否存在，再创建)
      * create database 数据库名 character set 字符集名字；（gbk, utf8） （创建数据库并指定字符集名）
      * create database if not exists 数据库名 character set gbk; （创建数据，判断是否存在，再创建并指定字符集名）
    2. R（Retrieve）：查询
      * show databases; （查询所有数据库的名称）
      * show create database 数据库名;（查看指定数据库的字符集；查看指定数据库的创建语句）
    3. U（Update）：修改
      * alter database 数据库名 character set 字符集名字；（gbk, utf8）
    4. D（Delete）：删除
      * drop database 数据库名；删除数据库。
      * drop database if exists 数据库名；判断数据库是否存在，再删除。
    5. 使用数据库
      * select database(); 查询当前正在使用的数据库。
      * use 数据库名; 使用数据库。

  2. 操作表：
    1. C（Create）：创建
      1. 语法：
        * create table 表名(列名1 数据类型1，列名2 数据类型2，...，列名n 数据类型n)；

        常见的数据类型：

          1. int: age int
          2. double: score double(5, 2)
          3. date: yyyy-MM-dd
          4. datetime: yyyy-MM-dd HH:mm:ss
          5. timestamp: yyyy-MM-dd HH:mm:ss (如果将来没有给timestamp类型赋值，或者赋值为null，则默认使用当前的系统时间，来自动赋值)
          6. varchar：name varchar(20) (姓名最大20个字符)

          创建表：

              create table student(
                id int,
                name varchar(32),
                age int,
                score double(4, 1),
                birthdat date,
                insert_time timestamp
              );

          复制表：

              create table student02 like student; (复制student表)

    2. R（Retrieve）：查询
      * show tables; 查询指定数据库所有的表名
      * desc 表名; 查询表的结构
      * show create table 表名
    3. U（Update）：修改
      * alter table 表名 rename to 新的表名; (修改表名)
      * alter table 表名 character set 字符集名; （修改表字符集）
      * alter table 表名 add 列名 数据类型; （添加新列）
      * alter table 表名 change 列名 修改后新列名 新数据类型; （修改列和数据类型）
      * alter table 表名 modify 列名 新数据类型; （修改列的数据类型）
      * alter table 表名 drop 列名; (删除列)
    4. D（Delete）：删除
      * drop table 表名;
      * drop table if exists 表名；

## DML: 增删改表中的数据

  1. 添加数据
    * 语法：
      insert into 表名（列名1， 列名2，...，列名n）values(数据类型1，数据类型2，...，数据类型n);
    * 注意：
      1. 列名和值要一一对应
      2. 如果表名后不定义列名，则默认给所有的列添加值
      3. 除了数字类型，其他类型都需要用引号引起来（“”/ ‘’）
  2. 删除数据
    * 语法：
      * delete from 表名 （可写）where 条件;
      * truncate table 表名；（删除表，然后再创建一个一摸一样的空表）
    * 注意
        * 如果不加条件，则会删除所有表中的记录。
        * 如果要删除所有记录
          1. 不建议使用delete from 表名；
          2. 建议使用truncate table 表名；
  3. 修改数据
    * 语法：
      * update 表名 set 列名1 = 值2， 列名2 = 值2... （可写）where 条件
    * 注意：
        * 如果不加任何条件，则会将表中所有记录全部修改


## DQL：查询表中的记录
  * select * from 表名;
  1. 语法：


    select
      字段列表
    from
      表名列表
    where
      条件列表
    group by
      分组列表
    having
      分组之后的条件
    order by
      排序
    limit
      分页限定

  2. 基础查询
    * 多个字段查询：select 字段名1， 字段名2，...from 表名；
      * SELECT name，age from 表名; //只查询姓名和年龄
      * SELECT address from 表名; //只查询地址
    * 去除重复：distinct
      * SELECT distinct address from 表名; //只查询地址
    * 计算列：一般可以使用四则运算计算一些列的值，IFNULL（表达式1，表达式2）：null参与运算结果都为null。表达式1: 那个字段需要判断是否为null  表达式2: 如果该字段为null后的替换值。
      * SELECT name, math, english, math + english from 表名； 计算math与english的和。
      * SELECT name, math, english, math + ifnull(english, 0) from 表名；如果english为null则替换为0。
    * 起别名：as
      * SELECT name, math, english, math + ifnull(english, 0) as 中分 from 表名；起别名，as可以省略。

    3. 条件查询
      * where 子句后跟条件
      * 运算符：
          1. <, >, >=, <=, !=, =, <>
          2. between and
          3. &&/and, or/｜｜, not/！，is null, is not null
          4. in（集合）
            * SELECT * FROM student WHERE age > 20;
            * SELECT * FROM student WHERE age < 20 OR age = 30;
            * SELECT * FROM student WHERE age >= 20 AND age <= 50;
            * SELECT * FROM student WHERE age != 20;
            * SELECT * FROM student WHERE age IN(20, 30, 40);
            * SELECT * FROM student WHERE age BETWEEN 20 AND 30;
            * SELECT * FROM student WHERE english IS NULL;
            * SELECT * FROM student WHERE english IS  NOT NULL;

          5. like (模糊查询)
            * 占位符： _ (单个的字符), % (多个字符)
            * SELECT * FROM student WHERE name LIKE ‘马%’；（姓马的人）
            * SELECT * FROM student WHERE name LIKE ‘_ 华%’； （第二个子是华）
            * SELECT * FROM student WHERE name LIKE ‘_ _ _ ’； （姓名是三个字）
            * SELECT * FROM student WHERE name LIKE ‘%德%’； （姓名中包含德的）

    4. 排序查询
        * 语法：order by 子句
          * order by 排序字段1 排序方法1，排序字段2 排序方法2,......
        * 排序方法：
          * SELECT * FROM student ORDER BY math DESC； （降序）
          * SELECT * FROM student ORDER BY math ASC； （升序）
          * SELECT * FROM student ORDER BY math ASC，english ASC； （只有第一条件相同时，会使用第二条件）

    5. 聚合函数： 将一列数据作为一个整体，进行纵向的计算

      注意：聚合函数计算是会排除NULL值。

      解决方式：

        1. 选择不包含非空的列进行计算
        2. IFNULL函数

      * count：计算个数  选择不包含空的列进行计算：主键/count（ * ）
        * SELECT COUNT(name) FROM student;
        * SELECT COUNT(IFNULL（name， 0）) FROM student;
      * max：计算最大值  SELECT
        * SELECT MAX(math) FROM student;
      * min：计算最小值
        * SELECT MIN(math) FROM student;
      * sum：计算和
        * SELECT SUM(math) FROM student;
      * avg：计算平均数
        * SELECT AVG(math) FROM student;

    6. 分组查询：

      * 语法：group by 分组字段；
      * 注意：
        1. 分组之后查询的字段：分组字段，聚合函数。
        2. WHERE 与 HAVING的区别：
          * WHERE 在分组之前进行限定，如果不满足条件，则不参与分组。
          * HAVING在分组之后急性限定，如果不满足结果，则不会本查处
          * WHERE 后不可以跟聚合函数，HAVING可以进行聚合函数的判断

      * SELECT sex, AVG(math) FROM student GROUP BY sex；(按照性别分组，分别查询男女同学的数学平均分)
      * SELECT sex, AVG(math)， COUNT（id） FROM student GROUP BY sex；（多个聚合函数）        
      * SELECT sex, AVG(math)， COUNT（id） FROM student WHERE math > 70 GROUP BY sex；(分组之前加条件限定)
      * SELECT sex, AVG(math)， COUNT（id） FROM student WHERE math > 70 GROUP BY sex HAVING COUNT（ID）> 2；(分组之前加条件限定)
      * SELECT sex, AVG(math)， COUNT（id）num_of_people（别名） FROM student WHERE math > 70 GROUP BY sex HAVING num_of_people）> 2；(分组之前加条件限定)

    7. 分页查询：
        * 语法：limit 开始的索引， 每页查询的条数；
          * SELECT * FROM student LIMIT 0，3；  （第一页）
          * SELECT * FROM student LIMIT 3，3；  （第二页）
          * SELECT * FROM student LIMIT 6，3；  （第二页）
        * 公式：开始的索引 = （当前的页码 - 1）/ 每页显示的条数
        * LIMIT是MySQL的“方言”。

## 约束 （Constraint）
  * 概念：对表中的数据进行限定，保证数据的正确性，有效性和完整性。
  * 分类：


    1. 主键约束：primary key
    2. 非空约束：not null
    3. 唯一约束：unique
    4. 外键约束：foreign key

  * 非空约束：not null （值不能为空）


    1. 创建表时添加约束
      * CREATE TABLE stu (id INT, name VARCHAR(20) NOT NULL); -- name 为非空
      * ALTER TABLE stu MODIFY name VARCHAR(20); -- 删除非空约束
    2. 创建表后再添加约束
      * ALTER TABLE stu MODIFY name VARCHAR(20) NOT NULL; -- 添加非空约束

  * 唯一约束：unique （值是不能重复）


    1. 创建表是添加唯一约束
      * CREATE TABLE stu (id INT, phone_number VARCHAR(20) UNIQUE);
    2. 删除唯一约束
      * ALTER TABLE stu DROP INDEX phone_number;
    3. 创建表后再唯一约束
      * ALTER TABLE stu MODIFY phone_number VARCHAR(20) UNIQUE;

  * 主键约束：primary key （唯一且不为空）


    1. 注意：
      * 一张表只能有一个字段为主键
      * 主键就是表中记录的唯一标示
    2. 创建表时添加主键约束
      * CREATE TABLE stu (id INT PRIMARY KEY, name VARCHAR(20));
    3. 删除主键
      * ALTER TABLE stu DROP PRIMARY KEY;
    4. 创建表后再主键约束
      * ALTER TABLE stu MODIFY id INT PRIMARY KEY;

  * 自动增长：如果某一列的数值是数值类型，使用auto_incrememt 可以来完成值的自动增长


    1. 创建表时添加主键约束 并且完成自动增长
      * CREATE TABLE stu (id INT PRIMARY KEY AUTO_INCREMENT, name VARCHAR(20));
    2. 删除自动增长
      * ALTER TABLE stu MODIFY id INT;
    3. 创建表添加自动增长
      * ALTER TABLE stu  MODIFY id INT AUTO_INCREMENT;
  * 外键约束：foreign key : 让表与表产生关系，从而保证数据的正确性


    1. 创建表时，可以添加外键
      *  CREATE TABLE employee (..., 列名 INT CONSTRAINT 外键名称 FOREIGN KEY (外键的字段) REFERENCES 主表名称(主表列名));
      *  CREATE TABLE employee (id INT PRIMARY KEY AUTO_INCREMENT, name VARCHAR(20), age INT, dep_id INT CONSTRAINT emp_dept FOREIGN KEY (dep_id) REFERENCES department(id));
    2. 删除外键
      * ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;
      * ALTER TABLE employee DROP FOREIGN KEY emp_dept;
    3. 添加外键：
      * ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键的字段) REFERENCES 主表名称(主表列名));
      * ALTER TABLE employee ADD CONSTRAINT emp_dept FOREIGN KEY (dep_id) REFERENCES department(id));

  * 级联操作：（谨慎使用）


    1. 添加外键，设置级联更新，设置级联删除
      * ALTER TABLE employee ADD CONSTRAINT emp_dept FOREIGN KEY (dep_id) REFERENCES department(id)) ON UPDATE CASCADE ON DELETE CASCADE;

## 数据库的设计
  1. 多表之间的关系

    * 分类：

      1. 一对一 （了解）：人和身份证，一个人只有一个身份证，一个身份证只能对应一个人
      2. 一对多（多对一）：部门和员工，一个部门有多个员工， 一个员工只能对应一个部门
      3. 多对多：学生与课程，一个学生可以选择多门课程，一个课程也可以被很多学生选择

    * 实现关系：
      1. 一对一： 一对一关系实现，可以在任意一方添加唯一外键指向另一方主键。
      2. 一对多（多对一）：在多的一方建立外键，指向一的一方的主键。
      3. 多对多：多对多关系实现需要借助第三张表。中间表至少包含两个字段，这两个字段作为作为第三张表的外键分别指向两张表的主键。

    * 案例：


      CREATE TABLE tab_category (
        cid INT PRIMARY KEY AUTO_INCREMENT,
        cname VARCHAR (100) NOT NULL UNIQUE
      );

      CREATE TABLE tab_route (
        rid INT PRIMARY KEY AUTO_INCREMENT,
        rname VARCHAR (100) NOT NULL UNIQUE,
        price DOUBLE,
        rdata DATE,
        cid INT, FOREIGN KEY (cid) REFERENCES tab_category(cid)
      );
      CREATE TABLE tab_user (
        uid INT PRIMARY KEY AUTO_INCREMENT,
        username VARCHAR(100) NOT NULL UNIQUE,
        password VARCHAR(30) NOT NULL,
        name VARCHAR(100),
        birthday DATE,
        gender CHAR(1) DEFAULT 'M',
        phone_number VARCHAR(11),
        email VARCHAR(100)
      );
      CREATE TABLE tab_favorite (      --中间表
        rid INT,
        uid INT,
        PRIMARY KEY(rid, uid),         --联合主键
        FOREIGN KEY (rid) REFERENCES tab_route(rid),
        ROREIGN KEY (uid) REFERENCES tab_user(uid)  
      );




  2. [数据库设计的范式](https://segmentfault.com/a/1190000013695030)
      * 概念：设计数据库时，需要遵循的一些规范。
      * 分类：
        1. 第一范式（1NF）：每一列都是不可分割的原子数据项。
        2. 第二范式（2NF）：在1NF的基础上，非码属性必须完全依赖于码（在1NF的基础上消除非主属性对主码的部分函数依赖）
            * 概念：
              1. 函数依赖：A-->B，如果通过A属性（属性组）的值，可以确定唯一B属性的值。则称B依赖于A。
                例如： 学号-->姓名，（学号，课程名称）--> 分数。
              2. 完全函数依赖：A-->B，如果A是一个属性组，则B属性值的确定需要依赖于A属性组中所有的属性值。
                例如：（学号，课程名称）--> 分数。
              3. 部分函数依赖：A-->B，如果A是一个属性组，则B属性值的确定只需要依赖于A属性组中某一些值即可。
                例如：（学号，课程名称）--> 姓名。
              4. 传递函数依赖：A-->B，B-->C，，如果通过A属性（属性组）的值，可以确定唯一B属性的值，再通过B属性（属性组）的值可以确定唯一C属性的值，则称C传递函数依赖于A。
                例如：学号-->系名， 系名-->系主任。
              5. 码：如果在一张表中，一个属性或属性组，被其他所有属性或属性组完全依赖，则称这个属性（属性组）为该表的码。
                例如：该表中码为：（学号，课程名称）。
                * 主属性：码属性组中的所有属性
                * 非主属性：除主码属性组的属性
        3. 第三范式（3NF）：在2NF的基础上，任何非主属性不依赖其他非主属性（在2NF的基础上消除传递依赖）。
        4. 巴斯-科德 范式（BCNF）：
        5. 第四范式（4NF）：
        6. 第五范式（5NF）：

## 数据库的备份与还原

  1. 命令行的方式
    * 语法：mysqldump -u用户名 -p密码 数据库名称 > 保存路径
    * 还原：
        1. 登陆数据库
        2. 创建数据库
        3. 使用数据库
        4. 执行文件： source 文件路径；
  2. 图形化工具

## 多表查询
  * 语法：SELECT 列名称，列名称 FROM 表名， 表名, ...;
  * 笛卡尔积：有两个集合A，B。取这两个集合的所有组成情况。要完成多表查询，需要消除无用的数据。
  * 多表查询的分类：
    1. 内连接查询
      * 隐式内连接：

          案例：
              SELECT
                t1.name,      -- 方便加注解 可读性高
                t1.gender,
                t2.name
              FROM
                emp t1,
                department t2
              WHERE
                emp.dept_id = dept.id;

      * 显示内连接：
           * 语法：
                  SELECT 字段列表 FROM 列表1 INNER JOIN 列表2 ON 条件。

              案例：
                  SELECT * FROM emp [INNER] JOIN dept ON emp.dept_id = dept.id;
                  SELECT * FROM emp JOIN dept ON emp.dept_id = dept.id; -- INNER 可省略
      * 内连接查询：
          1. 从哪些表中查询
          2. 条件是什么
          3. 查询哪些字段

    2. 外连接查询：
      * 左外连接：查询的是左表所有数据以及其交集部分。
          * 语法：
                SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件
            案例：
                SELECT
                  t1.* ,t2.name
                FROM
                  emp t1
                LEFT JOIN
                  dept t2
                ON
                  t1.dept_id = t2.id;

      * 右外连接：查询的是右表所有数据以及其交集部分。
          * 语法：
                SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件
            案例：
                SELECT
                  t1.* ,t2.name
                FROM
                  emp t1
                RIGHT JOIN
                  dept t2
                ON
                  t1.dept_id = t2.id;

    3. 子查询：查询中嵌套查询，称嵌套查询为子查询
      基本案例：
          SELECT * FROM emp WHERE emp.salary = (SELECT MAX(salary) FROM emp);

      * 子查询的不同情况：
        1. 子查询的结果是单行单列的：
          * 子查询可以作为条件，使用运算符取判断。运算符： < > = ...
                  SELECT * FROM emp WHERE emp.salary < (SELECT AVG(salary) FROM emp); -- 查询员工小于平均工资的人

        2. 子查询的结果是多行单列的：
          * 子查询可以作为条件，使用运算符IN来判断：
                  SELECT * FROM emp WHERE dept_id IN (SELECT id FROM dept WHERE name = '财务部' OR name = '市场部'); -- 查询财务部和市场部所有员工信息。

        3. 子查询的结果是多行多列的：
          * 子查询可以作为一个虚拟表：
                  SELECT * FROM dept t1, (SELECT * FROM emp WHERE emp.join_date > '2011-11-11') t2  -- 查询员工入职日期是2011-11-11日后之后的员工信息和部门信息。
                  WHERE t1.id = t2.dep_id;

## 事务
  1. 事务的基本介绍：
        * 概念：如果一个包含多个步骤的业务操作，被事务管理，那么这些操作要么同时成功，要么同时失败。
  2. 操作：
        * 开启事务：start transaction；
        * 回滚事务：rollback；
        * 提交事务：commit；    
        * MySQL数据库中事务默认自动提交，Oracle默认手动提交。
        * 查看事务默认提交方式：@@autocommit； 1 代表自动提交， 0 代表手动提交
        * 修改默认事务提交方式：set @@autocommit = 0；
  3. 事务的四大特征：
        * 原子性：不可分割的最小单位，要么同时成功，要么同时失败。
        * 持久性：当事务提交或回滚后，数据库会持久化的保存数据
        * 隔离性：多个事务之间。相互独立。
        * 一致性：事务操作前后数据总量不变。
  4. 事务的隔离级别（了解）：
        * 概念：多个事务之间是相互独立的。但是如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题。
        * 存在的问题：
            1. 脏读：一个事务，读取到另一个事务中没有提交的数据。
            2. 不可重复读（虚读）：在同一个事务中，两次读取到的数据不一样。
            3. 幻读：一个事务操作（DML）数据表中所有记录，另一个事务添加来一条数据，则第一个事务查询不到自己的修改。
        * 隔离级别：
            1. read uncommitted: 读未提交
                * 产生的问题：脏读，不可重复读，幻读。
            2. read committed: 读以提交    （Oracle默认）
                * 产生的问题：不可重复读，幻读。
            3. repeatable read: 可重复读  （MySQL默认）
                * 产生的问题：幻读。
            4. serializable：串行化
                * 可解决所有的问题
            5. 注意： 隔离级别从小到大安全性越来越高，但是效率越来越低。
            6. 数据库查询隔离级别：
                * SELECT @@tx_isolation；
            7. 数据库设置隔离级别：
                * SET GLOBLE TRANSACTION ISOLATION LEVEL 级别字符串；

## DCL：
  * SQL分类：
        1. DDL：操作数据库和表
        2. DML：增删改表中的数据
        3. DQL：查询表中的数据
        4. DCL：管理用户，授权

  * DBA：数据库管理员

  * DCL：管理用户，授权
      1. 管理用户
            * 添加用户
                    语法：CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
                    * CREATE USER 'abc'@'localhost' IDENTIFIED BY 'a123123';
                    * CREATE USER 'abc'@'%' IDENTIFIED BY 'a123123';

            * 删除用户
                    * DROP USER '用户名'@'主机名';
                    * DROP USER 'abc'@'localhost';

            * 查询用户
                    1. 切换到mysql数据库
                      * USE mysql;
                    2. 查询user表
                      * SELECT * from USER;
                    * 注意：%通配符表示可以在任意主机使用用户登陆数据库

            * 修改用户密码
                    * UPDATE USER SET PASSWORD = PASSWORD('新密码') WHERE USER = '用户名';
                    * SET PASSWORD FOR '用户名'@'主机名' = PASSWORD('新密码');

            * MySQL中忘记密码了
                    * net stop mysql(需要管理员运行停止mysql服务)
                    * mysqld --skip-grant-tables (使用无验证启动mysql服务)，然后打开新的窗口直接输入mysql就可以登陆了。
                    * 修改密码
                    * 打开任务管理器手动结束mysql服务，重新启动服务，使用新密码登陆。
      2. 授权
        1. 查询权限
                * SHOW GRANTS FOR '用户名'@'主机名';
                * SHOW GRANTS FOR 'abc'@'localhost';

        2. 授予权限
                * GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';
                * GRANT SELECT ON db3.student TO 'abc'@'localhost';
                * GRANT SELECT, DELETE, UPDATE ON db3.student TO 'abc'@'localhost';
                * GRANT ALL ON *.* TO 'abc'@'localhost'; -- 设置所有权限和所有数据库级表
        3. 撤回权限
                * REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
                * REVOKE UPDATE ON db3.student FROM 'abc'@'localhost';
