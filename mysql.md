# MySql

## 常用命令
1. `desc;` 查看表结构
2. `show variables like 'char%';`	查看字符集设置
3. `show charset` 查看支持的字符集
4. `source file.sql;`	导入数据，要在file.sql目录下进行
5. centOS下，`etc/my.cfg`，设置MySQL字符集
6. `service mysqld restart`	重启MySQL
7. `mysql -u root -p`	登录MySQL
8. `select version();`	查看MySQL版本（要先登录MySQL）
9. `status;` 查看MySQL状态（版本、字符集等）

## 数据备份

1. 备份ckb数据库下的books表到books.sql文件中：

```mysql
 mysqldump -u root -p ckb books > books.sql; 
```

然后会提示Enter password，下同

2. 备份quesionnaire数据库到questionnaire_bak文件中：
```mysql
mysqldump -u root -p --database questionnaire > questionnaire_bak;
```

3. 备份指定的多个数据库：
```mysql
mysqldump -u root -p --databases db1 db2 db3 > bak.sql
```
4. 备份全部数据库：
```mysql
mysqldump -u root -p --all-databases > all_database_sql
```

## 还原数据

1. 还原某个数据库：
```mysql
mysql -u root -p < all_database_sql
```
2. 向指定数据库导入表：
```mysql
mysql -u root -p db < books.sql
```

##  注意事项

1. MySQL在 5.5.3 之后增加了 **utf8mb4** 字符编码，mb4即 most bytes 4。简单说 utf8mb4 是 utf8 的超集并完全兼容utf8，能够用四个字节存储更多的字符。而utf8 是 utf8mb3 的别名。标准的 UTF-8 字符集编码是可以用 1~4 个字节去编码21位字符，但是MySQL其实实现的utf8只是使用3个字节而已， `utf8mb4才是真正意义上的 utf8`，`emoji`表情要用utf8mb4存

##  MySQL5.7修改字符集

配置文件位置：

```mysql
 /etc/mysql/mysql.conf.d/mysqld.cnf
```

设置为utf8mb4：

```mysqll
# Copyright (c) 2014, 2016, Oracle and/or its affiliates. All rights reserved.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; version 2 of the License.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301 USA

#
# The MySQL  Server configuration file.
#
# For explanations see
# http://dev.mysql.com/doc/mysql/en/server-system-variables.html
#------------------------------------------------ add 1
[client]
default-character-set=utf8mb4
#------------------------------------------------ add 2

[mysqld]
pid-file	= /var/run/mysqld/mysqld.pid
socket		= /var/run/mysqld/mysqld.sock
datadir		= /var/lib/mysql
#log-error	= /var/log/mysql/error.log
# By default we only accept connections from localhost
#bind-address	= 127.0.0.1
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
#------------------------------------------------ add 2
character-set-server=utf8mb4
#------------------------------------------------ add 2
```

上面中两处add是要加的内容，加完之后重启MySQL，然后登陆MySQL，执行`status`查看当前状态。