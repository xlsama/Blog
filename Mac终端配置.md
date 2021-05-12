# Mac 终端配置

## 安装 oh-my-zsh

1. 下载 oh-my-zsh

```zsh
git clone https://github.com/ohmyzsh/ohmyzsh.git ~/.oh-my-zsh
```

2. 创建 `.zshrc` 配置文件

```zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc
```

3. 修改默认 shell

```zsh
chsh -s $(which zsh)
```

> `echo $SHELL` 可以查看当前使用的 shell

## 安装主题

1. 下载 pure 主题

```zsh
git clone https://gitee.com/URmyLucky/pure2020.git ~/.oh-my-zsh/custom/pure
```

```zsh
ln -s ~/.oh-my-zsh/custom/pure/async.zsh ~/.oh-my-zsh/custom/async.zsh
```

2. 编辑 `.zshrc` ,将 ZSH_THEME 修改如下:

```zsh
ZSH_THEME='refined'
```

3. 更新 `.zshrc`

```zsh
source ~/.zshrc
```

## 安装插件

### 安装 zsh-syntax-highlighting

1. 下载到 `oh-my-zsh` 的插件文件夹

```zsh
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

2. 添加插件到 `.zshrc`

```zsh
plugins=(
  git
  zsh-syntax-highlighting
)
```

## 配置代理

### 添加代理

将下面内容添加到 `.zshrc` 中

```zsh
# where proxy
proxy () {
  export http_proxy="http://127.0.0.1:1080"
  export https_proxy="http://127.0.0.1:1080"
  curl cip.cc
  echo "http代理已开启🇺🇸"
}

# where noproxy
noproxy () {
  unset http_proxy
  unset https_proxy
  curl cip.cc
  echo "http代理已关闭🇨🇳"
}
```

### 使用

![image-20210403102502491](https://i.loli.net/2021/05/12/clxeUndOLpj2XvW.png)