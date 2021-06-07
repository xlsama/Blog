# 命令

## which

查看可执行文件的位置



## chsh

修改默认shell

```shell
chsh -s /usr/zsh
```

修改之后重新登录一下



## nohup

**nohup** 英文全称 no hang up（不挂起），用于在系统后台不挂断地运行命令，退出终端不会影响程序的运行。

**nohup** 命令，在默认情况下（非重定向时），会输出一个名叫 nohup.out 的文件到当前目录下，如果当前目录的 nohup.out 文件不可写，输出重定向到 **$HOME/nohup.out** 文件中。



### **语法**

```shell
nohup Command [ Arg … ] [　& ]
```

### 参数说明：

**Command**：要执行的命令。

**Arg**：一些参数，可以指定输出文件。

**&**：让命令在后台执行，终端退出后命令仍旧执行。



以下命令在后台执行 root 目录下的 `runoob.sh` 脚本：

```shell
nohup /root/runoob.sh &
```

在终端如果看到以下输出说明运行成功：

```shell这时我们打开 root 目录 可以看到生成了 nohup.out 文件。如果要停止运行，你需要使用以下命令查找到 nohup 运行脚本到 PID，然后使用 kill 命令来删除：
appending output to nohup.out
```

这时我们打开 root 目录 可以看到生成了 nohup.out 文件。

如果要停止运行，你需要使用以下命令查找到 nohup 运行脚本到 PID，然后使用 kill 命令来删除：

```shell
ps -aux | grep "runoob.sh" 
```

参数说明：

- **a** : 显示所有程序
- **u** : 以用户为主的格式来显示
- **x** : 显示所有程序，不区分终端机

另外也可以使用 **ps -def | grep "runoob.sh**" 命令来查找。

找到 PID 后，就可以使用 kill PID 来删除。

```shell
kill -9  进程号PID
```
