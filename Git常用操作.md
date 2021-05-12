## 设置全局配置和单个仓库的用户名和邮箱

### 全局

```shell
git config --global user.name "name"
```

```shell
git config --global user.email "name@example.com"
```

### 单个仓库

```shell
git config user.name "name"
```

```shell
git config user.email "name@example.com"
```

### 查看配置

```shell
git config --list # 查看当前配置, 在当前项目下面查看的配置是全局配置+当前项目的配置, 使用的时候会优先使用当前项目的配置
```

