# 笔记仓库，持续更新

> 不让win版git客户端中的LF自动转CRLF 
```
git config core.autocrlf false
```

> 设置全局变量

```
git config --global user.name "BlueDriver"
git config --global user.email "cpwu@foxmail.com"
git config --list
```

> 设置非全局变量，即在某项目目录下执行设置

```
git config user.name "gitlab's Name"
git config user.email "gitlab@xx.com"
git config --list
```

> `git config --list`查看当前配置, 在当前项目下面查看的配置是全局配置+当前项目的配置, 使用的时候会优先使用当前项目的配置

