# 用七牛云实现自动上传Typora文章中的图片

### Ⅰ.场景
- 用Typora写笔记，每次写好之后将文章直接复制到博客网站发布，图片无法加载出来，原因是Typora的图片都是保存的图片的本地路径。

- 那么，就想能不能让我将图片复制到Typora的时候，自动将图片上传到云服务的OSS然后可以通过自己的域名随时随地访问图片



### Ⅱ. Typora设置

- 先是在Typora中设置点开 最上面的文件，点倒数第二个**偏好设置**



![img](http://img.codezhou.com/img/v2-d1802fab5daa54185fb4e2ae21ef543e_720w.png)

- 然后按照图片所示设置

![image-20210502145944127](http://img.codezhou.com/img/image-20210502145944127.png)



### Ⅲ.  七牛云设置

- 第一步注册账号：注册账号之后，需要实名验证，我这需要一天多认证完成，实名认证完成之后就可以进行下面的操作
- 找到对象存储

![image-20210502150303494](http://img.codezhou.com/img/image-20210502150303494.png)

- 新建空间

![image-20210502150333718](http://img.codezhou.com/img/image-20210502150333718.png)

![image-20210502150424126](http://img.codezhou.com/img/image-20210502150424126.png)



### Ⅳ. 两者整合设置

- 请先在桌面上新建一个txt文本，并将下面的json文件复制进去

```json
{
  "picBed": {
    "uploader": "qiniu",
    "qiniu": {
      "accessKey": "",
      "secretKey": "",
      "bucket": "", // 存储空间名
      "url": "http://", // 自定义域名
      "area":  "z2", // 存储区域编号
      "options": "", // 网址后缀，比如？imgslim
      "path": "img/" // 自定义存储路径，比如 img/
    }
  },
  "picgoPlugins": {}
}
```

- 然后我们现在需要通过我们注册的**七牛云**账号来获取上面的配置信息
- accessKey / secretKey 在 秘钥管理（悬浮在），里面能找到

![image-20210502150747042](http://img.codezhou.com/img/image-20210502150747042.png)





![img](https://pic1.zhimg.com/80/v2-d649de0144c8361d7a60e5b8a60e5df4_720w.jpg)



- bucket 就是你创建的空间名称

![image-20210502150817978](http://img.codezhou.com/img/image-20210502150817978.png)



- url一栏的获取，图中域名可以填入url那，但是建议使用自己的域名，下面介绍如何绑定自己的域名

![image-20210502151331913](http://img.codezhou.com/img/image-20210502151331913.png)





### Ⅴ. 配置自己的域名访问图片

- 第一步点击域名管理

  ![image-20210502151551708](http://img.codezhou.com/img/image-20210502151551708.png)

- 然后绑定域名

![image-20210502151653200](http://img.codezhou.com/img/image-20210502151653200.png)

- 随便创建自己的一个二级域名即可，如图我创建了我的域名下的img二级域名

![image-20210502151804677](http://img.codezhou.com/img/image-20210502151804677.png)

- 复制CNAME去云解析DNS那里去配置，我的域名在阿里云买的所以我这演示阿里云怎么配置

![image-20210502152028287](http://img.codezhou.com/img/image-20210502152028287.png)

- 然后如下配置

![image-20210502152257065](http://img.codezhou.com/img/image-20210502152257065.png)

- 好了这个配置完成了

### Ⅵ. 最后一步

- 配置的完整样子如下图



![image-20210502151506911](http://img.codezhou.com/img/image-20210502151506911.png)



- 确定配置文件都填写好了之后，点开typora，点击文件里的偏好设置，设置图像信息



![img](https://pic2.zhimg.com/80/v2-8942c771afdb5849ecbd1a5312312e91_720w.png)





![img](https://pic2.zhimg.com/80/v2-70151bf2c1fb83770e4fa5d52f867a3d_720w.jpg)



- 点开配置文件，将之前建立的txt文本复制、粘贴进配置文件里面，保存并退出。



![image-20210502152640475](http://img.codezhou.com/img/image-20210502152640475.png)



- 验证图片上传功能

![img](https://pic2.zhimg.com/80/v2-7ce4c664637f1800207a605a35514ed9_720w.jpg)



- 当看到下面的页面的时候代表已经成功设置好了，现在可以去体验了



![image-20210502152539912](http://img.codezhou.com/img/image-20210502152539912.png)

### Ⅶ. 参考文章

- https://zhuanlan.zhihu.com/p/137426939