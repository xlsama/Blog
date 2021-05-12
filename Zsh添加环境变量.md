# Zsh添加环境变量

给 Linux/Unix 系统增加环境变量，是使用 `export` 命令。

为了永久性生效，则需要考虑加入到登录的 `profile`中。

这个时候要考虑你当前使用的 `shell`,比如默认的 `bash shell`，则可编辑用户根目录下的隐藏文件 `./bash_profile`

对于`zsh` 而言，需要编辑 `.zshrc` 这个文件 `vim ~/.zshrc`

在最后面加上一句

```shell
export PATH=/usr/local/nodejs/bin:$PATH
```

解释：

环境变量中，各个值是以冒号分隔开的。上面的语句表示给 `PATH` 这个变量重新赋值，让它等于

```shell
usr/local/nodejs/bin
```

同时后面加上原来的 `$PATH`