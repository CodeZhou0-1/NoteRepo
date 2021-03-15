## 在WindowsSever中安装JDK碰到的问题

### 碰到的问题
 - 安装好JDK并且配置好环境变量
 - 但是在CMD中输入 Java -version 提示没有安装
 - 但是在 PowerShell 中输入 Java 提示已经安装
 - 项目也还是运行不了

### 解决方案
- 将 JDK 中 bin 目录下的下图三个文件复制
![image.png](http://codezhou.com/upload/2021/02/image-7b72b71439e24832b7a1e273e681408d.png)
- 粘贴到 C:\Windows\System32 文件中，再次运行项目成功


### 参考链接
- 找不到了..