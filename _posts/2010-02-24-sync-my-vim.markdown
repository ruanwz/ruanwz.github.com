---
layout: post
title: 同步我的VIM
--- 
{{ page.title }}
================

24 Feb 2010

我基本上只用VIM来作编辑器。VIM有一个优势，就是当你通过SSH登录到远程主机上时，使用VIM作编辑器的效率比其他方式都要高。

要让VIM工作地更好，需要安装很多插件，我常用的有project,vim-ruby,vim-rails,snipmate.vim等。这些插件有的已经没有多少改动，比如project,直接在vim.org下载一次，安装完就不再更新。有的则是不断更新，需要我到github上取得最新的版本。

每次登录一个新的主机，我一般都会先装好VIM，为了避免反复的配置，我决定把vim的配置放到github上。方法如下：

* 建立~/gitrepos目录，我把放在github上的插件都克隆到这个目录下
* 把~/.vim目录移到~/gitrepos/vimfiles目录下，提交到github上。
* 在~/.vimrc首部加入以下一行重新指定路径,比如我的是这样的：

`set runtimepath=~/gitrepos/snipmate.vim,~/gitrepos/vimfiles,~/gitrepos/vim-rails,~/gitrepos/vim-ruby,~/gitrepos/git-vim,$VIMRUNTIME,~/gitrepos/snipmate.vim/after`

注意，一般把插件的目录都放在$VIMRUNTIME前面就可以了，但是有些插件(snipmate.vim)还需要在after目录中执行其他的操作，因此也要把after目录加到最后。

* 把.vimrc也复制到~/gitrepos/vimfiles/目录下

这样一来，在新主机上只需要执行git clone, 然后再把.vimrc放到~/目录下就可以把VIM配置好了。.还有一个好处就是你在某台主机上作了修改，都可以用git push提交到github上，其他主机就可以用git pull同步更新了。

有兴趣的可以参考一下我放在github上的[vimfiles](http://github.com/ruanwz/vimfiles)设置。
