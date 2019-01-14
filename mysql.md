# MySql

## 常用命令
1. `desc;` 查看表结构
2. `show variables like 'char%';`	查看字符集设置
3. `source file.sql;`	导入数据，要在file.sql目录下进行
4. centOS下，`etc/my.cfg`，设置MySQL字符集
5. `service mysqld restart`	重启MySQL
6. `mysql -u root -p`	登录MySQL
7. `select version();`	查看MySQL版本（要先登录MySQL）
8. `status;` 查看MySQL状态（版本、字符集等）

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
2. 
