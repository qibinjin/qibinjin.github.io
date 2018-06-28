---
layout: post
title: 用jekyll配合Github部署自己的个人博客
date: 2018-06-28 21:21:00
tags: code
author: Kim
---

<p>最近在自己弄Django相关的东西，发现通过Django可以轻松地实现很多功能。表单验证，用户管理，后台管理等等都有现成的中间件。</p>

<p>但是由于自己实在是没有审美，导致写的前台惨不忍睹。由此萌生了一个想法有没有可以快速美观的建站方案？？</p>

<p>答案当然是有，谷歌从来没让人失望，发现可以用过github配合jekyll或者hexo快速建立自己的网站，我这里使用的是jekyll所以hexo的部分可以看下别人的分享。</p>
<p>需要做的事<br>1.注册Github账户<br>2.建立一个仓库repository<br>3.在自己的电脑上配置jekyll环境<br>4.选择并下载自己喜欢的模板<br>5.使用模板并预览修改并提交</p>
<p>第一，二步我就默认都没问题了吧，如果有问题可以直接谷歌或者百度一下，注意的地方就是建立远程仓库的时候要使用username.github.io的格式这样可以自动帮你建立成需要个人页面的仓库并将这个地址作为你的域名。</p>
<p>配置jekyll环境，我是在ubuntu上配置的jekyll需要ruby环境</p>

```shell
sudo apt-get install ruby

安装完ruby后会确认下是否有gems可以在终端打gem如果没有提示未找到命令就是成功了


接下来直接用gem安装jekyll，会将大部分的依赖安装好，博主在这里报ERROR: Failed to build gem native extension在git上查找发现再装一下ruby-dev即可

sudo apt-get install ruby-dev
sudo gem install jekyll 

```
<p>到这里就可以new 一个blog 测试了</p>
```shell
jekyll new blog
jekyll serve 
```
<p>上述命令是建立一个新的博客，也可以使用 jekyll new .(注意这里是点) 可以在当前目录生成新的博客，如果当前目录有文件可以使用jekyll new . --force</p>
<p>到这里基本就算完成了，后续可以到<a href="http://jekyllthemes.org/">jekyllthemes</a>网站找模板下载到本地。</p>
<amp-img src="assets/images/net_pic.jpg" alt="Download" height="400" width="652"></amp-img>
<p>上图1为跳转到git仓库可以用fork的方式将模板代码克隆到自己的git上，2 可以下载打包好的模板代码</p>
<p>当下载到本地后还会有些问题，因为每个模板使用的依赖不同，所以在下载好模板后可以</p>
<p>所以在cd 到对应目录后执行jekyll serve或者jekyll build的时候会报错，这时候不要着急首先看看是不是缺少某个组件，如果是按照名字gem install即可</p>
<p>常见的有jekyll-sitemap和jekyll-paginate</p>
<p>上面的步骤都完成后就已经可以运行起模板了，但是还没有内容，内容部分可以参考readme.md里面会有比较详细的使用方法～</p>


