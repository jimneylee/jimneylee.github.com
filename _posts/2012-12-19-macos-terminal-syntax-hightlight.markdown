---
author: jimneylee
layout: post
title: "MacOS Terminal终端配置ls和vi颜色高亮"
date: 2012-12-19 10:56:06
comments: true
tags:
- macod
- Terminal
- hightlight
---

* step1: 

```bash
$ alias ls='ls -G'
```

* step2: 

```bash
$ vi .bash_profile
```

```
export PS1='\e[0:35m⌘\e[m \e[0:36m\w/\e[m \e[0:33m`git branch 2> /dev/null | g    rep -e ^* | sed -E  s/^\\\\\*\ \(.+\)$/\(\\\\\1\)\ /`\e[m'
export CLICOLOR=1
export LSCOLORS=GxFxCxDxBxegedabagaced
```

* step3:
```bash
$ cp /usr/share/vim/vimrc ./.vimrc
$ vi .vimrc
```
```
set ai " auto indenting set history=100 " keep 100 lines of history set ruler " >show the cursor position syntax on " syntax highlighting set hlsearch " highlight >the last searched term filetype plugin on " use the file type plugins " When >editing a file, always jump to the last cursor position autocmd BufReadPost * \ >if ! exists("g:leave_my_cursor_position_alone") | \ if line("'\"") > 0 && line >("'\"") <= line("$") | \ exe "normal g'\"" | \ endif | \ endif
```

* step4: restart teminal and enjoy it! :)