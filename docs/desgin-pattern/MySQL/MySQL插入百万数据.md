## MySQL插入百万数据

**注：**`一次性插入千万的，建议分批处理，十几万 几十万的插入。`

### 建表

创建emp表和dept表。

### 设置参数

首先，调用函数时，mysql会报错（This function has none of deterministic, NO SQL,or READS SQL DATA in its declaration and binary logging is enabled(you *might* want to use the less safe log_bin_trust_function_creators variable），设置`log_bin_trust_function_creators = 1`即可。(信任子程序的创建者，禁止创建、修改子程序时对SUPER权限的要求，设置log_bin_trust_routine_creators全局系统变量为1，**此方法重启MySQL后，会失效，有永久方法：百度__**)

```mysql
set GLOBAL log_bin_trust_function_creators=1;
```

### 创建函数

#### 创建字符串

```mysql
delimiter $$
create function rand_string(n INT) RETURNS VARCHAR(255)
BEGIN
	declare chars_str VARCHAR(100) DEFAULT 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ';
	declare return_str VARCHAR(255) DEFAULT '';
	declare i INT DEFAULT 0;
	while i < n DO
	set return_str = concat(return_str,substring(chars_str,floor(1+RAND()*52),1));
	set i = i + 1;
	END	while;
	RETURN return_str;
END $$
```

delimiter $$ 因为在内部也需要`;`，可分号代表代码执行终止，所以需要将终止符号替换成其他符号。  
创建函数,类似Java的

```java
String rand_string(int n){
	String chars_str = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ";
    String return_str = "";
    int i = 0;
  	while(i < n){
        return_str = apened(return_str....);//就是拼接字符串，产生随机字符；
        i++;
    }
    return return_str;
}
```

#### 随机产生部门编号

```mysql
delimiter $$
create FUNCTION rand_num() returns int(5)
BEGIN
	declare i int DEFAULT 0;
	set i = FLOOR(10 + RAND()*10);
	return i;
END $$
```

产生10-20之间的部门号。

### 创建存储过程

#### emp的存储过程

```mysql
delimiter $$
create PROCEDURE insert_emp(in start int(10),IN max_num int (10))
BEGIN
	declare i int DEFAULT 0;
	set autocommit = 0;
	REPEAT
        set i = i + 1;
        insert into emp 
        (empno,ename,job,mgr,hiredate,sal,comm,deptno) VALUES 
        ((start+i),rand_string(6),'salesman',0001,CURDATE(),2000,400,rand_num());
	UNTIL i = max_num END REPEAT;
END $$
```

向emp表插入数据。

#### dept的存储过程

```mysql
delimiter $$
create PROCEDURE insert_dept(in start int(10),IN max_num int (10))
BEGIN
	declare i int DEFAULT 0;
	set autocommit = 0;
	REPEAT
        set i = i + 1;
        insert into dept 
        (deptno,dname,loc) VALUES 
        ((start+i),rand_string(10),rand_string(8));
	UNTIL i = max_num END REPEAT;
END $$
```

向dept表插入数据。

### 调用存储过程

```mysql
delimiter ;
Call insert_dept(1,500000);
Call insert_emp(1,500000);
```

dept / emp 编号从1开始，插入50w条数据。