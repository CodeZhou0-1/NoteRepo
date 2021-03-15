# Git常用命令的复习和Github的应用同步笔记文章


## Ⅰ. Git常用命令的复习

### ① Git中的基本概念
#### 1.三个区域
Git本地有三个工作区域：
- 工作目录（Working Directory）
- 暂存区(Stage/Index)
- 资源库(Repository或Git Directory)。
- 如果在加上远程的git仓库(Remote Directory)就可以分为四个工作区域。
> ![image.png](http://codezhou.com/upload/2021/03/image-93134cf048854bb995d35cde7dea3cee.png)

> Workspace：工作区，就是你平时存放项目代码的地方

> Index / Stage：暂存区，用于临时存放你的改动，事实上它只是一个文件，保存即将提交到文件列表信息

> Repository：仓库区（或本地仓库），就是安全存放数据的位置，这里面有你提交到所有版本的数据。其中HEAD指向最新放入仓库的版本

> Remote：远程仓库，托管代码的服务器，可以简单的认为是你项目组中的一台电脑用于远程数据交换

#### 2.文件三种状态
git管理的文件有三种状态：
- 已修改（modified）
- 已暂存（staged）
- 已提交(committed)


### ② Git常用命令
- 初始化仓库：`git init`  
- 将修改或新增的文件添加到暂存区： `git add .`
- 将暂存区的内容提交到版本区（本地仓库）：`git commit -m "commit message"`
- 将本地仓库的文件添加到远程仓库（比如Github或者Gitee）：`git push`
- 将远程仓库的内容更新到本地仓库中：`git pull`
- 将远程仓库的代码克隆下来：`git clone <sshUrl>`
- 查看commit的历史：`git log`
- 合并指定分支到当前分支：`git merge <branch>`
- 撤销指定的Commit：`git reset --hard commit_id`
![image.png](http://codezhou.com/upload/2021/03/image-4dd4bc73a24a49889e0f96e9cb8f0f29.png)

- 其他命令的实际应用：
![image.png](http://codezhou.com/upload/2021/03/image-bad486fa119a43ffbcf9abc42e2ab152.png)

![image.png](http://codezhou.com/upload/2021/03/image-7d204065e0204b219d7f1155bacf3fe7.png)
- **Git常用命令图片**：
![image.png](http://codezhou.com/upload/2021/03/image-b1a0e3701a0a4165a4e3e471641916cd.png)

## Ⅱ. 将Git连接到自己的Github上配置公钥
- 设置本机绑定SSH公钥，实现免密码登录
- 进入 C:\Users\Administrator\\.ssh 目录
- 执行`ssh-keygen -t rsa`命令生成rsa公钥
- 将公钥信息(`id_rsa.pub`为公钥) 添加到Github账户中
![image.png](http://codezhou.com/upload/2021/03/image-f965ce2cd0424c888b63893adeedee95.png)
- 然后创建自己的仓库，即可实现便捷的pull和push了


### Ⅲ. 参考文章
- [Git复习文章](https://blog.csdn.net/qq_30081043/article/details/109394817)
- [Git撤销指定Commit](https://www.cnblogs.com/ningkyolei/p/4334990.html)