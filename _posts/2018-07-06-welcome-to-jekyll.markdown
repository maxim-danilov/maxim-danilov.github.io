---
layout: post
title:  "Turning Vim into an JavaScript IDE"
date:   2018-07-06 14:50:30 +0300
categories: jekyll update
---
Integrated development environment (IDE) is an important tool for software developers. In simple terms, IDE is a powerful text editor for creating programs. 

One of the best IDE for JavaScript is WebStorm. WebStorm is able to understand your code, suggest a good autocomplete, debug and refactor your code. But such IDE have a few disadvantages: sometimes it’s slow, takes much space, limited settings, high resource consumption.

Vim is a lightweight, fast, and free text editor. Vim is well known for its hotkeys that make coding faster and more comfortable. Developers created a lot of Vim plugins for Vim. 

So we're going to build a fast, flexible, lightweight IDE using Vim with plugins. Let’s to define the necessary functions of IDE:
* Syntax highlighting 
* Snippets 
* Lint (analyzes source code to flag programming errors, bugs)
* Code formatting tool 
* Autocomplete

> The commands below are intended for Linux. I use Ubuntu 16.04.

## Install Vim
We are going to use modern Vim plugins which require Vim 8 (latest version). Install Vim 8 using:

{% highlight bash %}
sudo add-apt-repository ppa:jonathonf/Vim
{% endhighlight %}

{% highlight bash %}
sudo apt update
{% endhighlight %}

{% highlight bash %}
sudo apt install vim
{% endhighlight %}

{% highlight bash %}
vim –version
{% endhighlight %}

## Install Vundle
Vundle is a plugin manager for Vim (as npm for JavaScript). 
Install Vundle using git clone:

{% highlight bash %}
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
{% endhighlight %}

{% highlight bash %}
vim .vimrc
{% endhighlight %}

Put this at the top of your `~/.vimrc` to use Vundle:
{% highlight plain %}
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
" Plugin 'L9'
" Git plugin not hosted on GitHub
Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Install L9 and avoid a Naming conflict if you've already installed a
" different version somewhere else.
" Plugin 'ascenator/L9', {'name': 'newL9'}

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line
{% endhighlight %}

Launch Vim and run `:PluginInstall` 

## Install the syntax highlighting plugin
A syntax hignlignter assists programmers when reading and analysing code. We will install this plugin:
[https://github.com/pangloss/vim-javascript](https://github.com/pangloss/vim-javascript)

We need add this line to `.vimrc` into plugins section:

`Plugin 'pangloss/vim-javascript'`
Then we need to install the plugin using vundle. Start Vim and type `:PluginInstall` command. The plugin is installed:

Before:

After:

Now it looks better.

## Snippets
Snippets make coding faster. You can quickly insert loops, conditions, functions and other language constructions by using hotkeys. 

We need these plugins:
[https://github.com/garbas/vim-snipmate](https://github.com/garbas/vim-snipmate)
[https://github.com/grvcoelho/vim-javascript-snippets](https://github.com/grvcoelho/vim-javascript-snippets)

Add to `.vimrc`:
{% highlight plain %}
Plugin 'MarcWeber/vim-addon-mw-utils'
Plugin 'tomtom/tlib_vim'
Plugin 'garbas/vim-snipmate'
{% endhighlight %}

Also remap the hotkey to `Ctrl+j`
{% highlight plain %}
imap <C-J> <esc>a<Plug>snipMateNextOrTrigger
smap <C-J> <Plug>snipMateNextOrTrigger
{% endhighlight %}

It works! 

## Lint
A lint checks a source code for bugs, stylistic errors. It's an important tool for writing high quality code. 

ALE is a modern lint plugin for Vim. ALE checks your code in the real time mode (while you edit your code). Actually ALE is a layer between Vim and a lint tool. We will use ESlint as a lint tool for a JS code.
 
Add to `.vimrc`:
{% highlight plain %}
Plugin 'w0rp/ale'
{% endhighlight %}

Run `:PluginInstall` 


### Configure ESLint:
{% highlight bash %}
mkdir lintTest
{% endhighlight %}

{% highlight bash %}
npm init
{% endhighlight %}

{% highlight bash %}
npm install eslint --save-dev
{% endhighlight %}

{% highlight bash %}
./node_modules/.bin/eslint --init
{% endhighlight %}

{% highlight bash %}
vim index.js
{% endhighlight %}

The lint works but I would like to adjust messages style. I added these lines to `.vimrc`:
{% highlight plain %}
let g:ale_echo_msg_error_str = 'E'
let g:ale_echo_msg_warning_str = 'W'
let g:ale_echo_msg_format = '[%linter%] %s [%severity%]'
{% endhighlight %}

## Line numbers
Line numbers are disabled by default. 

Add this to `.vimrc`:
{% highlight plain %}
set number
{% endhighlight %}


## Code formatter
A code formatter (a code beautifier) converts your code into correct format. You can quickly write some code then to format it with just one click. ESLint and ALE will do this job. 

These options activate code formatting on saving a file:
{% highlight bash %}
let b:ale_fixers = ['eslint']
let g:ale_fix_on_save = 1
{% endhighlight %}

## Autocomplete
Autocomplete increases your typing speed. It predicts the word a user intends to enter. 
Install plugin dependencies:

{% highlight bash %}
sudo apt-get install build-essential cmake
sudo apt-get install python-dev python3-dev
{% endhighlight %}

Install the plugin:
{% highlight bash %}
Plugin 'Valloric/YouCompleteMe'
{% endhighlight %}
{% highlight bash %}
:PluginInstall
{% endhighlight %}

YCM has a client-server architecture. The commands above installs the client. Here we install YCM server:
{% highlight bash %}
cd ~/.vim/bundle/YouCompleteMe
./install.py --all
{% endhighlight %}

Cool, it works! 


## Conclusion 
We just made a good tool to write a JS code and edit any config files. It's lightweight, fast, сustomizable and I can use it on any device via SSH. 

