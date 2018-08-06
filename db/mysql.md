# MySQL

---

## MySQL8.* Install
### mysql/my.ini
```ini
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[mysqld]
# 设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=C:/shaoxing/db/mysql
# 设置mysql数据库的数据的存放目录
datadir=C:/shaoxing/db/mysql\data
# 允许最大连接数
max_connections=20
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```
### Init
```cmd
mysqld remove 删除注册表，重新建立data目录
mysqld --initialize --console 这里会出现密码
mysqld --install
net start mysql
mysql -u root -p
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
net start mysql
```