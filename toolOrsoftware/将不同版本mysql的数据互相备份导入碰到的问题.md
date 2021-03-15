## 将不同版本mysql的数据互相备份导入碰到的问题

### Ⅰ. 场景
- 需要将项目部署在云服务器的数据库迁移到实体服务器的数据库上
- 但是云服务器的Mysql数据库版本是8而实体服务器数据库是5.7
- 本来想在实体服务器安装 Mysql8 版本的，但是安装过程中各种报错
- 尝试用DirectX修复工具修复添加一些dll文件后，安装 Mysql8 依旧报错
- 发现安装 Mysql8 需要的一些环境太多很多报错还是选择了安装 Mysql5.7

### Ⅱ. 问题
- 于是从 Mysql8 中导出数据库备份Sql文件，但是导入到 Mysql5.7 中的时候报错，可能是版本有些语法不一样。
- 本来还是想将 Mysql5.7 换成 Mysql8 但是这个项目实体服务器安装 Mysql 还是很多报错，且修复提示可能要重启服务器，但是这个服务器部署了其他项目，不好重启。
- 所以只能从其他方面解决这个问题

### Ⅲ. 解决

- 使用Navicat的数据传输功能
![image.png](http://codezhou.com/upload/2021/02/image-5cdf15dd64154512973ce00a8bbbd5ef.png)
- Navicat的数据传输备份成 Sql 文件可以选择以哪个版本的 Mysql 备份（Navict YES！）
![image.png](http://codezhou.com/upload/2021/02/image-cb85a2b0afb34849881b63fe51e1f0f0.png)

- 选择好备份到哪个文件，点击下一步开始备份即
- 得到备份好的文件导入 Mysql5.7 版本成功