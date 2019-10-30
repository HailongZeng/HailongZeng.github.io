---
title: "个人博客Hugo搭建"
date: 2019-10-30T00:14:49-07:00
draft: true
---


https://mogeko.me/2018/018/

# 1、安装Hugo

在终端输入$ brew install hugo进行安装

在终端输入$ hugo version查看是否安装成功

# 2、用Hugo来生成博客

在终端输入$ hugo new site myblog来创建博客

在终端输入$ ls -l查看当前用户文件夹

在终端输入$ cd myblog/进入刚才创建的博客文件夹

在终端输入$ ls -l查看myblog目录的文件

在终端输入$ pwd打印工作目录

# 3、下载并设置主题

主题网站：https://themes.gohugo.io/ 选择一个主题进入，里面有安装步骤

这里以安装m10c主题为例

在终端输入$ git clone https://github.com/vaga/hugo-theme-m10c.git themes/m10c

在终端输入$ cd themes/进入主题文件夹

在终端输入$ cd ..返回上一级文件夹myblog

# 4、在本地启动个人博客

在终端输入$ hugo server -t m10c –buildDrafts进行启动博客

终端会打印信息Web Server is available at xxx地址，在浏览器中打开地址即可查看博客

# 5、实际写一篇文章

在终端输入$ hugo new post/blog.md进行创建

在终端输入$ cd content/进入content目录, post在content目录下

在终端输入$ cd post/进入post目录，blog.md在post目录下

在终端输入$ vim blog.md来修改 或者用Visual Studio Code中Start中Open folder打开blog.md来修改，修改后进行保存

在终端输入$ cd ../..返回到myblog根目录

在终端输入$ hugo server -t m10c –buildDrafts进行启动博客

终端会打印信息Web Server is available at xxx地址，在浏览器中打开地址即可查看博客

修改博客名字可以改变title

# 6、将个人博客部署到远端服务器

首先在github中创建一个新的仓库，即进入Create a new repository，然后填充Repository name

在终端输入$ hugo –theme=m10c –baseUrl=“https://HailongZeng.github.io/" –buildDrafts 进行创建，theme填写你选用的theme，baseUrl填写你刚才创建的库

在终端输入$ ls -l会看到多出来一个public目录

在终端输入$ cd public/进入public目录，再输入$ ls -l查看

在终端输入$ git init初始化仓库, 然后输入$ git add . 将public目录下所有文件加到仓库中

在终端输入$ git commit -m ”我的hugo博客第一次提交“进行提交

在终端输入$ git remote add origin https://github.com/HailongZeng/HailongZeng.github.io.git进行远端连接

在终端输入$ git push -u origin master进行push上去，输入github用户名和密码

