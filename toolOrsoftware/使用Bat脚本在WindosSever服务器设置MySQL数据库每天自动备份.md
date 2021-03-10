## 使用Bat脚本在WindosSever服务器设置MySQL数据库每天自动备份

### Ⅰ. 前言
  项目部署在 Windows Sever 系统服务器上面，MySQL数据库要设置每天凌晨自动备份当天的数据

### Ⅱ. 使用Bat脚本备份
- 新建MysqlBackup.txt，接着复制以下代码稍作修改，完成后将txt改成bat后缀
```Bat
@echo off
set "Ymd=%date:~,4%%date:~5,2%%date:~8,2%"
"D:\Program Files\MySQL\MySQL Server 5.7\bin\mysqldump" --opt -u root --password=123 dbname > D:\MysqlBackup\dbname_%Ymd%.sql
@echo on
```
- 代码简解：
    1.` "D:\Program Files\MySQL\MySQL Server 5.7\bin\mysqldump" ` 是mysql安装路径
    2. `root` 和 `123` 对应数据库用户名、密码，`dbname` 对应的是要打算备份的数据库名字
    3. `D:\MysqlBackup\dbname_%Ymd%.sql` 对应备份文件目录和自定义名称。
   
- 此时备份Bat脚本已经编写完成可以测试下是否可以成功备份，如果可以则开始下一步将其设为Windows的定时任务

### Ⅲ. 将Bat脚本设置为定时任务
- 右击“计算机”>>“管理”>>“工具”>>“任务计划程序”后，点击创建任务：
![image.png](http://codezhou.com/upload/2021/03/image-52fef3d642c343d5a06141b42474277d.png)

- 给定时任务命名
![image.png](http://codezhou.com/upload/2021/03/image-cbf55f8f4fda46999141bc6274d4f248.png)

- 设置触发器每天几点执行
![image.png](http://codezhou.com/upload/2021/03/image-9309748ff2364db2ad3e0253f33afb46.png)

- 选择要执行的Bat脚本
![image.png](http://codezhou.com/upload/2021/03/image-d8c621d77dda4eb0b35d57a7ef0da14e.png)

- 最后可以在任务计时程序那里查看或编辑定时任务
![image.png](http://codezhou.com/upload/2021/03/image-bfddd0e7fa6144c79a67f21b7137bb0c.png)

### Ⅳ.参考文章
- [windows server环境mysql数据库每天自动备份](https://blog.csdn.net/baidu_24248003/article/details/86535436)