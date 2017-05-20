# Oracle SQL 语句基本使用方法

```sql
-- 新建表空间
create tablespace myTablespace
datafile 'D:\db\myTablespace.dbf'
size 100M   -- 指定大小
autoextend on -- 自动扩展开
next 32M    -- 每次增长32M
maxsize 1024M   -- 最大1024M

-- 删除表空间
drop tablespace myTablespace including contents and datafiles -- 删除表空间同时删除数据库文件

-- 创建用户
create user dbj
identified by dbj123
default tablespace myTablespace

-- 查询用户
select username,default_tablespace from dba_users where username='dbj'

-- 修改用户
alter user dbj
identified by dbj456
default tablespace mySpace

-- 赋予用户权限
grant create session,create sequence to dbj with admin option

-- 移除系统权限
revoke create session from dbj

-- 赋予对象权限
grant select on te to dbj

-- 角色三种类型
-- connect 只有最基本的权限范围
-- resource  开发人员的权限，可以建表，建索引
-- dba 拥有所有权限

-- 将系统角色赋予用户
grant dba to dbj

-- 自定义角色，用户角色
create role role_dbj

-- 再赋予角色权限
grant create session to role_dbj
grant insert on te to role_dbj
```