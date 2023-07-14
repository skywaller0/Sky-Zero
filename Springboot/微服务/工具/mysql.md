# mysql8

docker安装命令
```
docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:8.0.25 --lower_case_table_names=1
```
安装后会存在远程连接的问题，需要修改配置文件
```
1.进入容器
docker exec -it mysql bash
2.连接并切换数据库
mysql -uroot -p123456
use mysql
3.进行授权远程连接
Grant ALL ON *.* TO 'root'@'%';//远程连接
flush privileges;//刷新权限
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;//更改加密规则
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';//设置密码
flush privileges;//刷新权限
```

数据库最好专库专用，不要混用，否则会出现权限问题