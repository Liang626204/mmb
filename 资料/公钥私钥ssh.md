## github/码云公私钥和ssh详解

### 1. 为什么要生成公钥私钥？

github和码云等有两种认证方式，第一种是基于https的认证方式，这种方式的好处就是不用配置ssh-key，逻辑上相对简单，但是操作上每次都需要进行用户名和密码的配置。第二种就是基于ssh的认证方式。

使用https的认证方式，每次都必须输入密码，非常麻烦。好在SSH还提供了公钥登录，可以省去输入密码的步骤。

所谓"公钥登录"，原理很简单，就是用户将自己的公钥储存在远程主机上。登录的时候，远程主机会向用户发送一段随机字符串，用户用自己的私钥加密后，再发回来。远程主机用事先储存的公钥进行解密，如果成功，就证明用户是可信的，不再要求用户输入密码。

### 2. 如何生成公私钥？ssh-keygen

传送门： [生成/添加SSH公钥](https://gitee.com/help/articles/4181#article-header0)

码云提供了基于SSH协议的Git服务，在使用SSH协议访问项目仓库之前，需要先配置好账户/项目的SSH公钥。

你可以按如下命令来生成 sshkey:

```shell
ssh-keygen -t rsa -C "xxxxx@xxxxx.com"  
# 按三次回车你就会看见这个东西
fdipzone@ubuntu:~$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/fdipzone/.ssh/id_rsa): 这里输入要生成的文件名
Enter passphrase (empty for no passphrase):                       这里输入密码 
Enter same passphrase again:                                      这里重复输入密码
Your identification has been saved in /home/fdipzone/.ssh/id_rsa.
Your public key has been saved in /home/fdipzone/.ssh/id_rsa.pub.
The key fingerprint is:
f2:76:c3:6b:26:10:14:fc:43:e0:0c:4d:51:c9:a2:b0 fdipzone@ubuntu
The key's randomart image is:
+--[ RSA 2048]----+
|    .+=*..       |
|  .  += +        |
|   o oo+         |
|  E . . o        |
|      ..S.       |
|      .o .       |
|       .o +      |
|       ...oo     |
|         +.      |
+-----------------+
```

+ 参数 -t rsa 表示使用rsa算法进行加密，执行后，会在C:\Users\\**(当前用户).ssh目录下找到id_rsa(私钥)和id_rsa.pub(公钥)
+ 在git bash 窗口，可以通过命令`cd ~/.ssh`，访问到生成公私钥的文件夹
+ 输入命令`ls`可以查看当前文件夹的文件
+ 输入命令`cat id_rsa.pub`可打开当前文件，然后复制该文件内容
+ 上述操作可直接通过可视化的windows资源管理器直接访问操作

**注意**

+ 如果你输入命令出现了

  ```bash
  $ ssh-keygen -t rsa
  Generating public/private rsa key pair.
  Enter file in which to save the key (/c/Users/blur_/.ssh/id_rsa):
  /c/Users/blur_/.ssh/id_rsa already exists.
  Overwrite (y/n)?
  ```

+ 它提示什么，就输入什么，不要直接回车，如果想overwrite覆盖，那就输入y

+ 最后的最后，输入命令`ssh -T git@gitee.com`

  会弹出

  ```bash
  Are you sure you want to continue connecting (yes/no)? yes
  # 然后再重新输入刚才的命令
  $ ssh-keygen -t rsa 
  $ Hi Harry! You've successfully authenticated, but GITEE.COM does not provide shell access.
  ```

+ 在新生成密钥之后，在.ssh文件夹中少了一个known_hosts文件，本来密钥文件应该是三个，现在是两个，便报了这样的错误，此时选择yes回车之后，便可，同时生成了缺少了的known_hosts文件。
+ 至此，就可以畅通无阻的上传远端了。