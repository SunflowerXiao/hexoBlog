---
title: 服务器创建新用户
date: 2019-10-28 14:41:46
categories:
   - 服务器
---

服务器上的root用户在linux系统上有非常大的权限，因此非常不建议你在日常服务器配置开发中使用root账号。 root账号固有的部分权限是即使在偶然的情况下也可以造成毁灭性的更改。

建议设置一个拥有指定权限的用户账号用于日常使用。

### 创建新用户

当你使用root账户登录上服务器之后，就可以开始创建新用户了。执行下面的语句，就创建了一个叫做demo的用户。

```bash
adduser demo
```

按下回车执行语句后，会提问几个问题，第一个会让你设置demo账户的密码,建议输入一个安全性强的密码。其他问题都是可选的，如果你不想回答直接enter到最后即可。

### 给demo账户赋特权

现在我们有了一个拥有常规账户权限的demo用户。但是有时候我们需要执行一些管理员权限才能够执行的任务

为了避免用户注销普通用户并以root账户重新登录，我们可以为demo账号设置root特权，通过sudo放在每个命令之前，这将使我们的普通用户可以以管理特权运行。

将demo用户添加到sudo用户组，就可以将这些权限赋给demo用户。默认的，在sudo用户组下面的用户都被允许执行sudo命令。

执行下面的命令添加demo用户到sudo用户组

```bash
gpasswd -a demo sudo
```

现在就可以以超级用户的身份运行命令了。


### 添加Public Key 验证

为创建的demo用户设置public key验证。通过请求私有的sshkey登录可以提高服务器的安全性。

#### 生成Key Pair

如果你还没有一组由公有和私有秘钥构成的SSH key pair, 需要生成一个。如果你已经有了这个key，可以直接看下一步 *复制公有秘钥*

在本机电脑上打开cmd,执行如下命令：

```
local$ ssh-keygen
```

假设你的本机账户是 `localuser`, 你将会看到如下的输出：

```
ssh-keygen output
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/localuser/.ssh/id_rsa):
```

直接回车或者键入一个新名字， 然后系统会让你设置该秘钥的密码。你可以输入密码或者直接回车。

注意： 如果你没有设置密码，那么无需验证就可以使用私钥进行身份验证，如果输入了密码，就需要输入设置的密码来进行验证。

执行完上述步骤后会生成一个私有秘钥 `id_rsa` 和一个公有秘钥 `id_rsa.pub`, 在 你当前用户根目录下的`.ssh` 目录下。 私有秘钥不能和任何不能登入你服务器的人分享。


### 复制公钥

生成了SSH KEY pair后，就需要复制密钥到服务器。这里介绍两个简单的方法来实现。

注意： 如果服务在创建的时候选择了ssh key的方式，ssh-copy-id方式将不会有效

#### 方法1：使用ssh-copy-id

如果你的本机可以执行ssh-copy-id命令，你可以使用它来 给你的服务器或者其他任何你已经登录认证了的机器安装公钥。

执行 `ssh-copy-id` 语句通过指定服务器的 用户和ip， 在这里用户就是上面刚刚创建的demo：

```
local$ ssh-copy-id demo@SERVER_IP_ADDRESS
```

回车之后会让你输入服务器密码，输入完之后公钥就被添加到远程服务器的`.ssh/authorized_keys` 文件中了。现在可以使用相应的私钥登录服务器。

#### 方法2： 手动安装 key

假设你已经生成了SSH key pair，使用下面的命令在本机执行来显示你的公钥。

```
local$ cat ~/.ssh/id_rsa.pub
```

这个会 打印出你的公共密钥，看起来跟下面类似：

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDBGTO0tsVejssuaYR5R3Y/i73SppJAhme1dH7W2c47d4gOqB4izP0+fRLfvbz/tnXFz4iOP/H6eCV05hqUhF+KYRxt9Y8tVMrpDZR2l75o6+xSbUOMu6xN+uVF0T9XzKcxmzTmnV7Na5up3QM3DoSRYX/EP3utr2+zAqpJIfKPLdA74w7g56oYWI9blpnpzxkEd3edVJOivUkpZ4JoenWManvIaSdMTJXMy3MtlQhva+j9CgguyVbUkdzK9KKEuah+pFZvaugtebsU+bllPTB0nlXGIJk98Ie9ZtxuY3nCKneB+KjKiXrAvXUPCI9mWkYS/1rggpFmu3HbXBnWSUdf localuser@machine.local
```

复制公共密钥，添加到远程服务器

##### 添加公钥到远程账户

启用使用SSH密钥进行身份验证为新的远程使用，需要添加公钥到用户根目录的指定文件

__在服务器上__, 使用root账户登录，使用一下命令切换账户

```
su -demo
```

现在你已经进入了demo用户的根目录， 创建一个新的目录叫做.ssh， 并且给他赋予777权限

```
mkdir .ssh
chmod 700 .ssh
```

现在使用文本编辑器打开.ssh目录下名为`authorized_keys`的文件，我们这里使用nano

```
nano .ssh/authorized_keys
```

执行这个命令，这里没有这个文件的话会创建。  粘贴公钥到文本编辑器中。

执行`ctrl + x` 退出编辑， 选择 `Y` 保存， `ENTER`确认文件名。


编辑完该文件后，设置文件权限为600

```
chmod 600 .ssh/authorized_keys
```

退出当前（demo）登录账户， 返回到root账户

```
exit
```

现在demo账户已经可以使用ssh 进行登陆了，使用私有密钥进行认证

[原文处处](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-14-04)

