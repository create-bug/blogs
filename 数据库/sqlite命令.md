django默认使用sqlite，创建项目后使用sqlite3命令查看对应数据库内容。

启动： sqlite3 db.sqlite3
查看当前tables： .tables
显示表头信息： .headers ON
查看tables 表中的值： select * from tables;
插入： INSERT INTO tables(name, value) VALUES('device_provisoned',1);
退出： .exit
