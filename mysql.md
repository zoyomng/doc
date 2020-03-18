### MySQL使用介绍

mysql新建用户，修改权限

- 登录：`mysql -u root -p  `

- 查看现有用户(mysql8.0.1)

```
mysql> select host,user,authentication_string from mysql.user;

 +-----------+------------------+----------------------------------------------------------------+
 | host | user | authentication_string |
 +-----------+------------------+----------------------------------------------------------------+
 | localhost | mysql.infoschema | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
 | localhost | mysql.session | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
 | localhost | mysql.sys | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE |
 | localhost | root | $A$005$e!42 )Tf+4M{4W>MkFY9ktIVPhgVemeQsSQnuiGLRiH/909Zyaj9XF3/3Yk2 |
 +-----------+------------------+----------------------------------------------------------------+
```





- 新建用户

　格式:`create user "username"@"host" identified by "password";`

　eg：

1. mysql->create user 'test'@'localhost' identified by '123';

2. mysql->create user 'test'@'192.168.7.22' identified by '123';

3. mysql->create user 'test'@'%' identified by '123';
       /*host="localhost"为本地登录用户，host="ip"为ip地址登录，host="%"，为外网ip登录*/

- 删除用户

　格式：`drop user 'username'@'host';`

- 授权

　格式：`grant privileges on databasename.tablename to 'username'@'host' IDENTIFIED BY 'PASSWORD';`

1. GRANT命令说明：
   　priveleges(权限列表),可以是all priveleges, 表示所有权限，也可以是select、update等权限，多个权限的名词,相互之间用逗号分开。

   　　 on用来指定权限针对哪些库和表。

   　　　　 *.* 中前面的*号用来指定数据库名，后面的*号用来指定表名。

    　　　　to 表示将权限赋予某个用户, 如 jack@'localhost' 表示jack用户，@后面接限制的主机，可以是IP、IP段、域名以及%，%表示任何地方。注意：这里%有的版本不包括本地，以前碰到过给某个用户设置了%允许任何地方登录，但是                  在本地登录不了，这个和版本有关系，遇到这个问题再加一个localhost的用户就可以了。

   　　       identified by指定用户的登录密码,该项可以省略。

    　　       WITH GRANT OPTION 这个选项表示该用户可以将自己拥有的权限授权给别人。注意：经常有人在创建操作用户的时候不指定WITH GRANT OPTION选项导致后来该用户不能使用GRANT命令创建用户或者给其它用户授权。
    
                 备注：可以使用GRANT重复给用户添加权限，权限叠加，比如你先给用户添加一个select权限，然后又给用户添加一个insert权限，那么该用户就同时拥有了select和insert权限。

2. 授权原则说明：

　　　　权限控制主要是出于安全因素，因此需要遵循一下几个经验原则：

   　　　 a、只授予能满足需要的最小权限，防止用户干坏事。比如用户只是需要查询，那就只给select权限就可以了，不要给用户赋予update、insert或者delete权限。

　		   b、创建用户的时候限制用户的登录主机，一般是限制成指定IP或者内网IP段。

　　　	c、初始化数据库的时候删除没有密码的用户。安装完数据库的时候会自动创建一些用户，这些用户默认没有密码。

　　　	d、为每个用户设置满足密码复杂度的密码。

   　　　 e、定期清理不需要的用户。回收权限或者删除用户。

　eg：
   /*授予用户通过外网IP对于该数据库的全部权限*/

　　`grant all privileges on `test`.* to 'test'@'%' ;`

 /*授予用户在本地服务器对该数据库的全部权限*/

　　`grant all privileges on `test`.* to 'test'@'localhost';   `

   grant select on test.* to 'user1'@'localhost';  /*给予查询权限*/

   grant insert on test.* to 'user1'@'localhost'; /*添加插入权限*/

   grant delete on test.* to 'user1'@'localhost'; /*添加删除权限*/

   grant update on test.* to 'user1'@'localhost'; /*添加权限*/

　 flush privileges; /*刷新权限*/


- 查看权限

```
 show grants;
+---------------------------------------------------------------------+
| Grants for root@localhost                                           |
+---------------------------------------------------------------------+
| GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION |
| GRANT PROXY ON ''@'' TO 'root'@'localhost' WITH GRANT OPTION        |
+---------------------------------------------------------------------+
2 rows in set (0.00 sec)
```

查看某个用户的权限：

``` show grants for 'jack'@'%';
+-----------------------------------------------------------------------------------------------------+
| Grants for jack@%                                                                                   |
+-----------------------------------------------------------------------------------------------------+
| GRANT USAGE ON *.* TO 'jack'@'%' IDENTIFIED BY PASSWORD '*9BCDC990E611B8D852EFAF1E3919AB6AC8C8A9F0' |
+-----------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)
```

- 删除权限

  ```
  revoke privileges on databasename.tablename from 'username'@'host';
  revoke delete on test.* from 'jack'@'localhost';
  ```

- 更改用户名

　　`mysql> rename user 'jack'@'%' to 'jim'@'%';`

- 修改密码

1. 用set password命令

   ```
   mysql> SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123456');
   
   Query OK, 0 rows affected (0.00 sec)
   ```

2. 用mysqladmin [root@rhel5 ~]# mysqladmin -uroot -p123456 password 1234abcd

　　备注： 格式：mysqladmin -u用户名 -p旧密码 password 新密码

3. 用update直接编辑user表

   (10)pycharm中python3.6+pymysql+mysql8.0.1连接报错　

   pymysql.err.OperationalError: (1045, u"Access denied for user 'root'@'localhost' (using password: No)")

解决方法：　

在cmd命令行连接mysql, 通过mysql -u root -p dong1990

 

然后输入ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'dong1990';
