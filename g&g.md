# Git/Github

+ Repository / Git Project ---> 所有编辑的记录文件夹
+ commit ---> 类似于截图，所有文件的
+ branch

**使用 git**

1. 初始化

```bash
$ git init
```

2. 设置用户信息

```bash
$ git config --global user.name "xxx"
$ git config --global user.email "xxx@gmail.com"
```

3. 保存到本地的服务器（未保存文件呈红色，保存的文件呈绿色）

```bash
# . 表示保存所有的文件
$ git add .
```

4. 推到远程服务器，双引号内的表示备注（此次改变的内容和目的）

```bash
$ git commit -m "First initial commit"
```

**使用 github**

1. New repository
2. 使用 git 连接 github 

```bash
$ git remote add origin xxx # repository 的 url
```

3. 使用 git 把修改的文件 push 到 github

```bash
$ git branch # 查看在那个 branch 上面
```

```bash
$ git push origin master
```

+ merge
+ Pull
+ .gitignore
+ README
+ push