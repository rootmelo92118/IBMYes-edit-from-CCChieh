# IBMYes

本项目包括3部分

1. IBM Cloud Fonudray搭建V2Ray ws
2. 利用Github的Actions 每周重启 IBM Cloud Fonudray
3. Cloudflare 高速节点中转

# 使用IBM Cloud Fonudray搭建V2Ray

首先注册https://cloud.ibm.com/

注册步骤略过

登录后点击右侧 创建资源

![image-20200615192854218](img/README/image-20200615192854218.png)

可以找到Cloud Foundray

![image-20200615193004495](img/README/image-20200615193004495.png)

创建公共应用程序

![image-20200615193052842](img/README/image-20200615193052842.png)

填写相关信息

![image-20200615193202810](img/README/image-20200615193202810.png)

区域必须达拉斯，只有那里有免费的。

![image-20200615193340241](img/README/image-20200615193340241.png)

填写应用名称

一键安装脚本

```shell
wget --no-check-certificate -O install.sh https://raw.githubusercontent.com/CCChieh/IBMYes/master/install.sh && chmod +x install.sh  && ./install.sh
```



# 利用Github的Actions 每周重启 IBM Cloud Fonudray

首先登录IBM Cloud

点击又上角的命令行

在这一步我们主要是记录4个值

 ```
IBM_ACCOUNT // IBM Cloud的登录邮箱和密码
IBM_APP_NAME // 应用的名称
REGION_NUM // 区域编码
RESOURSE_ID // 资源组ID
 ```

具体后面会一步一步完成

![image-20200615175949804](img/README/image-20200615175949804.png)

进入命令行先执行

```shell
ibmcloud login
```

输入邮箱和密码。

之后记录下区域(Region)

![image-20200615180817619](img/README/image-20200615180817619.png)

```
#1. au-syd
#2. in-che
#3. jp-tok
#4. kr-seo
#5. eu-de
#6. eu-gb
#7. us-south
#8. us-east
```

这里需要记下和区域对应的编号也就是`REGION_NUM`，比如我这里是us-south,那么我的区域编号是`7`

接下来获取资源组id`RESOURSE_ID`

```shell
ibmcloud resource groups
```

![image-20200615183425453](img/README/image-20200615183425453.png)

图中所指向便是`RESOURSE_ID`

现在返回github，到本项目

```
https://github.com/CCChieh/IBMYes
```

![image-20200615184239713](img/README/image-20200615184239713.png)

右上角fork到自己的github下，然后进入setting

![image-20200615184327329](img/README/image-20200615184327329.png)

选择Secrets

![image-20200615184426979](img/README/image-20200615184426979.png)

New secret

分别建立四个secret

```
IBM_ACCOUNT // IBM Cloud的登录邮箱和密码
IBM_APP_NAME // 应用的名称
REGION_NUM // 区域编码
RESOURSE_ID // 资源组ID
```



以`IBM_ACCOUNT`为例![image-20200615184703280](img/README/image-20200615184703280.png)

第一行为邮箱，第二行为密码。

这里需要邮箱和密码所以中间换行 ，其他的不需要换行 。

把四个secret补充完成

![image-20200615185015130](img/README/image-20200615185015130.png)

之后点击上方Actions，在这里你就会看到有个IBM Cloud Auto Restart在执行

![image-20200615185614978](img/README/image-20200615185614978.png)

第一次可能因为secret没添加导致workflow执行失败，只需要点下

![image-20200615191100959](img/README/image-20200615191100959.png)

进去后按照下图

![image-20200615191035212](img/README/image-20200615191035212.png)

找到 `Re-run jobs`重新执行一次即可，至此自动重启已经ok了。

> 感谢药油@[My Flavor](https://yaohuo.me/bbs/userinfo.aspx?touserid=24109)，原本打算弄bash在自己服务器定期执行脚本，现在看了他的帖子，发现用Actions是一个更好的选择。

