1、常用命令行工具：

  ①cmd     ②powershell      ③git bash

2、命令行常用命令（在git bash上生效，部分在cmd无用）

    -pwd (print working directory) 查看当前所在路径--绝对路径
    
    -cd(change directory) 切换目标
    
    -ls(list) 查看当前目录下的内容
    
    -mkdir(make directory) 创建目录
    
    -touch 创建文件
    
    -cat 查看文件内容（一次性将内容全部显示）
    
    -less 查看文件内容（显示部分信息）--再次输入‘回车’一行一行显示，‘空格’一页一页显示 ，‘b’一次向上走一页
    
    -rm(remove) 删除文件，-rm -rf 文件夹（循环递进删除文件夹）
    
    -rmdir(remove directory)删除文件夹（只能删除空文件夹，不常用）
    
    -clear 清屏
    
    -q 退出
    
    -mv(move) 移动文件或重命名
    
    -cp(copy) 复制文件
    
    -echo ‘内容’ > 文件名 （输出内容到文件中，每次输入都是覆盖原来的文件）
    
    -echo ‘内容’ >>文件名（输出内容到文件中，每次输入都是追加新内容）

 


3、Git介绍

     Git版本管理工具，有三大区域：
    
     ① 工作目录-----存放项目代码的目录
    
     ②暂存区-----存放工作中更改的文件，避免项目代码丢失。
    
     ③代码仓库-----当开发功能足够成为一个版本时，提交到仓库。其实就是将暂存区中代码复制一份存储到代码仓库中。


​    

     Git常用命令
    
     ① 配置git用户名和密码
    
          git config  --global user.name sun
    
          git config  --global user.email  sun@qq.com
    
    ② 查看当前git的配置
    
          git config --list
    
     ③ 初始化git 仓库
    
          git init
    
     ④ 查看当前仓库的状态
    
          git status
    
      ⑤ 将工作目录中的文件添加到暂存区
    
          git add sun.html（这个命令上传一个文件）     git add  .(这个命令会将当前目标下所有文件上传)    git   add  a.txt  b.txt (如果上传多个，文件名之间用空格)
    
          题外话： 如果当前文件夹内文件很多，但是有些又不想提交。可以通过编辑器（sublime,webstorm等）或命令行创建一个文件 以.gitignore后缀，其内容写入不想提交的文件名即可。此时再通过git add .命令去全部提交时就会有选择提交。
    
      ⑥ 将暂存区中的代码提交到本地仓库，形成一个版本
    
          git  commit -m 备注（如果备注内容带空格，则需要加‘’）
    
       ⑦  查看本地仓库中的历史提交版本
    
          git  log
    
       ⑧  将暂存区中文件删除
    
           git  rm  --cached 文件名
    
       注意： 1、必须保证工作目录中代码和暂存区中代码一致。 2、删除之后，工作目录中仍然有此文件而暂存区没有。git不将管理该文件。
    
        ⑨  用暂存区中的文件覆盖工作目录中的文件
    
           git  checkout -- 文件名
    
        注意： 暂存区和工作目录中均有此文件，该文件依然被git管理
    
        ⑩ 回滚到本地仓库中特定版本并覆盖暂存区和工作目录
    
           git  reset --hard  commitID(commitID可以到git log中查看提交编号)，有种方式：1、全部黏贴  2、只取前6位
    
      注意： 如果有版本1，版本2（后提交），当回滚到版本1时版本2会被自动删除。

　　图示：

　　　　

　　

　　

分支相关命令：

    ① 查看分支  
    
            git  branch (显示结果中 有* 代表当前所在分支)
    
    ②  创建分支
    
            git  branch 分支名称
    
     ③  切换分支  
    
           git  checkout 分支名称
    
     ④  创建并切换分支 
    
          git  checkout -b 分支名称
    
     ⑤ 删除分支 （如果分支没有被合并不允许删除）
    
         git  branch -d 分支名称
    
     ⑥  删除分支（强制删除分支）
    
         git  branch  -D 分支名称
    
      ⑦  合并分支
    
        git  merge 来源分支（意思：当前目录到主分支，将来源分支合并到主分支上。合并后来源分支仍然存在）


　　

 

　　　　

4、github 相关命令介绍

     4.1 模拟一个公共代码仓库
    
           ①先初始化   git  init --bare  sun.git （注意：此时公共代码仓库的文件夹必须以.git为后缀名）
    
    　 4.2  github仓库
    
           ① 为远程仓库地址创建别名
    
              git  remote add origin  https://github.com/sun766/Programming-art.git（此处举例）
    
            注意： 通常我们会把远程仓库地址设置别名为origin
    
           ② 查看远程地址的详细信息
    
             git  remote -v
    
           ③ 查看当前别名所对应的远程仓库地址
    
             git  remote show origin
    
            ④ 从远程仓库获取代码（拉取所有版本到本地）
    
             git clone  origin 
    
             注意： 加入到已有项目的开发中，需要先拉取所有版本到本地再进行开发。
    
            ⑤ 从远程仓库拉取代码（拉取最新版本到本地，开发过程中使用）
    
             git pull origin master
    
              面试题： 说出clone 和 pull 之间区别
    
            ⑥向远程仓库推送代码
    
             git  push origin(远程仓库地址)   master(本地分支名称)：master(远程分支名称)
    
           注意： 推送时一定要在本地代码仓库目录中，如果本地分支同远程分支名称一样，可以只写一个。
    
            ⑦ 删除当前别名所对应的远程仓库地址
    
              git  remote remove origin 
    
            记住： 如果你想重新使用origin 别名，则需要将原来的origin 对应远程地址删除掉。


​            

　　‘’多人协作开发免登录设置”

 　　当不想使用账户和密码进行推送代码时，建议使用SSH协议。

   　　在git bash 中输入ssh-keygen，   自动会在c:/用户/administrator/中生成.ssh文件。其包括三个文件

     　　① id_rsa   ②  id_rsa.pub    ③ known_hosts

   　　 在github账号中settings/SSH and GPG keys，点击New SSH key将②中内容复制粘贴。

 　　   注意： 设置别名时用SSH路径。