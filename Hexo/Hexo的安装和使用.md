
##安装nodejs
从nodejs网站上下载node.pkg
##安装Hexo
安装Hexo
```Shell
~$sudo npm install -g hexo
```
##准备网站目录并使用hexo进行初始化
在本地机器上准备一个文件目录，例如：~/MyBlogs
将执行目录切换到该目录下并执行初始化命令
```Shell
~/MyBlogs$hexo init
```
此时，该目录为hexo的执行目录，并自动建立一个hellowolrd.md文件
##生成helloworld博客
在hexo的工作目录下执行博客生成命令
```Shell
~/MyBlogs$hexo g
```
##博客预览
在hexo的工作目录下执行服务器运行命令
```Shell 
~/MyBlogs$hexo s
```
此时，在浏览其中输入http://localhost:4000 即可看到博客预览
##添加新文件
在hexo的工作目录下执行服务器运行命令
```Shell
~/MyBlogs$hexo new myfilename
```
则在目录~/MyBlogs/source/\_posts下生成名为myfilename的文件。编辑该文件即可。
##添加主题
https://github.com/tommy351/hexo/wiki/Themes
目录下列出了各种主题及安装方法，安装后，更改_config.yml文件中的theme选项值(注意":"后有一个半角空格)。

Themes|description|Install Commands
------|-----------|----------------
Landscape|Hexo2.4+默认主题|--
Light|历史版本Hexo的默认主题|git clone git://github.com/tommy351/hexo-theme-light.git themes/light
Phase|最漂亮的Hexo主题|git clone git://github.com/tommy351/hexo-theme-phase.git themes/phase
Memoir|一个简单的Hexo主题|git clone git://github.com/yhben/hexo-theme-memoir.git themes/memoir
Persona Dark|基于Light的漂亮的深色主题|git clone git://github.com/thiagopnts/hexo-persona-dark.git themes/persona-dark
Pithiness|支持fancybox的主题|
paperbox|基于Light的纸张式主题|
DamnClean|基于Light的轻量主题|
Modernist|简单的支持fancybox的主题，增加多说评论插件|
Perry|轻量主题，单列|
flag|基于Light的简单主题|
pacman|响应式的主题|
pacmanhover|基于pacman改变颜色并添加浮动效果|
pacmanblue|基于pacman的深蓝主题|
Greyshade|模仿Octopress的Greyshade和主题|
Sagi|另一款Hexo主题|
Raytaylorism|alpha效果的主题|
tyrant|基于Light的简单主题|
MoHexo|基于bootstrap的主题|
writing|基于Light的简单主题|
Striped|html5/css3/响应式主题|
winterlan|基于Light的有动态页面的主题|
Coolblue|模仿Styleshout's的Coolblue的主题|
mame|另一款轻量级主题|
forked mame|mame的一个修改版本|
sidebar|基于Light的一个简单主题|
iOS7|一个ios7风格的主题|
chenall|一个模块化的HEXO主题，多功能|

