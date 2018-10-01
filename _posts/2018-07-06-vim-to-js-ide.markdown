---
layout: single
title:  "Turning Vim into a JavaScript IDE"
date:   2018-07-06 14:50:30 +0300
---

Integrated development environment (IDE) is an important tool for software developers. In simple terms, IDE is a powerful text editor for creating programs. 

![image-title-here](/assets/img/0005.gif){:class="img-responsive"}

Vim is a lightweight, fast, and free text editor. Vim is well known for its hotkeys that make coding faster and more comfortable. Developers created a lot of plugins for Vim. 

So we're going to build a fast, flexible, lightweight IDE using Vim with plugins. Let’s define the necessary functions of IDE:
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
{% highlight viml %}
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

Plugin 'VundleVim/Vundle.vim'
Plugin 'tpope/vim-fugitive'
Plugin 'git://git.wincent.com/command-t.git'
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}

call vundle#end()            " required
filetype plugin indent on    " required
{% endhighlight %}

Launch Vim and run `:PluginInstall` 

![image-title-here](/assets/img/0001.png){:class="img-responsive"}

## Install the syntax highlighting plugin
A syntax highlighting assists programmers when reading and analyzing code. We will install this plugin:
[https://github.com/pangloss/vim-javascript](https://github.com/pangloss/vim-javascript)

We need add this line to `.vimrc` into the plugins section:

`Plugin 'pangloss/vim-javascript'`

![image-title-here](/assets/img/0002.png){:class="img-responsive"}

Then we need to install the plugin using vundle. Start Vim and type `:PluginInstall` command. The plugin is installed:

![image-title-here](/assets/img/0003.png){:class="img-responsive"}

Before:
![image-title-here](/assets/img/0004.png){:class="img-responsive"}

After:
![image-title-here](/assets/img/0008.png){:class="img-responsive"}

Now it looks better.

## Snippets
Snippets make coding faster. You can quickly insert loops, conditions, functions and other language constructions by using hotkeys. 

We need to these plugins:
[https://github.com/garbas/vim-snipmate](https://github.com/garbas/vim-snipmate)
[https://github.com/grvcoelho/vim-javascript-snippets](https://github.com/grvcoelho/vim-javascript-snippets)

Add to `.vimrc`:
{% highlight plain %}
Plugin 'MarcWeber/vim-addon-mw-utils'
Plugin 'tomtom/tlib_vim'
Plugin 'garbas/vim-snipmate'
Plugin 'grvcoelho/vim-javascript-snippets'
{% endhighlight %}

Also remap the hotkey to `Ctrl+j`
{% highlight plain %}
imap <C-J> <esc>a<Plug>snipMateNextOrTrigger
smap <C-J> <Plug>snipMateNextOrTrigger
{% endhighlight %}

![image-title-here](/assets/img/0001.gif){:class="img-responsive"}

It works! 

## Lint
A lint checks a source code for bugs, stylistic errors. It's an important tool for writing high-quality code. 

ALE is a modern lint plugin for Vim. ALE checks your code in the real time mode (while you edit your code). Actually, ALE is a layer between Vim and a lint tool. We will use ESlint as a lint tool for a JS code.
 
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
cd lintTest
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

![image-title-here](/assets/img/0002.gif){:class="img-responsive"}

The lint works but I would like to adjust messages style. I added these lines to `.vimrc`:
{% highlight plain %}
let g:ale_echo_msg_error_str = 'E'
let g:ale_echo_msg_warning_str = 'W'
let g:ale_echo_msg_format = '[%linter%] %s [%severity%]'
{% endhighlight %}

![image-title-here](/assets/img/0006.png){:class="img-responsive"}

## Line numbers
Line numbers are disabled by default. 

Add this to `.vimrc`:
{% highlight plain %}
set number
{% endhighlight %}

![image-title-here](/assets/img/0007.png){:class="img-responsive"}

## Code formatter
A code formatter (a code beautifier) converts your code into the correct format. You can quickly write some code and to format it with just one click. ESLint and ALE will do this job. 

These options activate code formatting on saving a file:
{% highlight bash %}
let b:ale_fixers = ['eslint']
let g:ale_fix_on_save = 1
{% endhighlight %}

![image-title-here](/assets/img/0003.gif){:class="img-responsive"}

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

![image-title-here](/assets/img/0004.gif){:class="img-responsive"}

## File explorer
NERDTree is a file system explorer
[https://github.com/scrooloose/nerdtree](https://github.com/scrooloose/nerdtree)

{% highlight bash %}
Plugin 'scrooloose/nerdtree'
{% endhighlight %}

I added this to my `.vimrc` to open `NERDTree` with Ctrl+n:
{% highlight bash %}
" NERDTress Ctrl+n
map <C-n> :NERDTreeToggle<CR>
" NERDTress File highlighting
{% endhighlight %}

These line configure different highlighting for different file extensions:
{% highlight bash %}
function! NERDTreeHighlightFile(extension, fg, bg, guifg, guibg)
 exec 'autocmd filetype nerdtree highlight ' . a:extension .' ctermbg='. a:bg .' ctermfg='. a:fg .' guibg='. a:guibg .' guifg='. a:guifg
 exec 'autocmd filetype nerdtree syn match ' . a:extension .' #^\s\+.*'. a:extension .'$#'
endfunction
call NERDTreeHighlightFile('jade', 'green', 'none', 'green', '#151515')
call NERDTreeHighlightFile('ini', 'yellow', 'none', 'yellow', '#151515')
call NERDTreeHighlightFile('md', 'blue', 'none', '#3366FF', '#151515')
call NERDTreeHighlightFile('yml', 'yellow', 'none', 'yellow', '#151515')
call NERDTreeHighlightFile('config', 'yellow', 'none', 'yellow', '#151515')
call NERDTreeHighlightFile('conf', 'yellow', 'none', 'yellow', '#151515')
call NERDTreeHighlightFile('json', 'yellow', 'none', 'yellow', '#151515')
call NERDTreeHighlightFile('html', 'yellow', 'none', 'yellow', '#151515')
call NERDTreeHighlightFile('styl', 'cyan', 'none', 'cyan', '#151515')
call NERDTreeHighlightFile('css', 'cyan', 'none', 'cyan', '#151515')
call NERDTreeHighlightFile('coffee', 'Red', 'none', 'red', '#151515')
call NERDTreeHighlightFile('js', 'Red', 'none', '#ffa500', '#151515')
call NERDTreeHighlightFile('php', 'Magenta', 'none', '#ff00ff', '#151515')
{% endhighlight %}

![image-title-here](/assets/img/0009.png){:class="img-responsive"}

## System clipboard
[System copy](https://github.com/christoomey/vim-system-copy) provides copying/pasting text to the system clipboard. 

Install dependencies: 
{% highlight bash %}
apt-get install xsel
{% endhighlight %}

Add `Plugin 'christoomey/vim-system-copy'` to `.vimrc`, run `:PluginInstall`.

Use `cp` to copy into your system clipboard and `cv` to paste from system clipboard.

## My current .vimrc
{% highlight viml %}
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim

call vundle#begin()

Plugin 'VundleVim/Vundle.vim'

"Highlighting:
Plugin 'pangloss/vim-javascript'

"Snippets:
Plugin 'MarcWeber/vim-addon-mw-utils'
Plugin 'tomtom/tlib_vim'
Plugin 'garbas/vim-snipmate'
Plugin 'grvcoelho/vim-javascript-snippets'
Plugin 'terryma/vim-multiple-cursors'

"Lint:
Plugin 'w0rp/ale'

"Autocomplete:
Plugin 'Valloric/YouCompleteMe'

"File explorer:
Plugin 'scrooloose/nerdtree'

"System clipboard:
Plugin 'christoomey/vim-system-copy'

call vundle#end()            " required


"""vim configuration:
"Indentation
filetype plugin indent on
set tabstop=2
set shiftwidth=2
set expandtab

"Line numbers:
set number

"No wrap:
set nowrap


"""Plugins configuration:
"Snippets:
imap <C-J> <esc>a<Plug>snipMateNextOrTrigger
smap <C-J> <Plug>snipMateNextOrTrigger

"Lint:
let g:ale_echo_msg_error_str = 'E'
let g:ale_echo_msg_warning_str = 'W'
let g:ale_echo_msg_format = '[%linter%] %s [%severity%]'

"Code formatter:
let b:ale_fixers = ['eslint']
let g:ale_fix_on_save = 1

"File explorer:
" NERDTress Ctrl+n
map <C-n> :NERDTreeToggle<CR>
" NERDTress File highlighting
function! NERDTreeHighlightFile(extension, fg, bg, guifg, guibg)
 exec 'autocmd filetype nerdtree highlight ' . a:extension .' ctermbg='. a:bg .' ctermfg='. a:fg .' guibg='. a:guibg .' guifg='. a:guifg
 exec 'autocmd filetype nerdtree syn match ' . a:extension .' #^\s\+.*'. a:extension .'$#'
endfunction
call NERDTreeHighlightFile('jade', 'green', 'none', 'green', '#151515')
call NERDTreeHighlightFile('ini', 'yellow', 'none', 'yellow', '#151515')
call NERDTreeHighlightFile('md', 'blue', 'none', '#3366FF', '#151515')
call NERDTreeHighlightFile('yml', 'yellow', 'none', 'yellow', '#151515')
call NERDTreeHighlightFile('config', 'yellow', 'none', 'yellow', '#151515')
call NERDTreeHighlightFile('conf', 'yellow', 'none', 'yellow', '#151515')
call NERDTreeHighlightFile('json', 'yellow', 'none', 'yellow', '#151515')
call NERDTreeHighlightFile('html', 'yellow', 'none', 'yellow', '#151515')
call NERDTreeHighlightFile('styl', 'cyan', 'none', 'cyan', '#151515')
call NERDTreeHighlightFile('css', 'cyan', 'none', 'cyan', '#151515')
call NERDTreeHighlightFile('coffee', 'Red', 'none', 'red', '#151515')
call NERDTreeHighlightFile('js', 'Red', 'none', '#ffa500', '#151515')
call NERDTreeHighlightFile('php', 'Magenta', 'none', '#ff00ff', '#151515')
{% endhighlight %}

## Conclusion 
We just made a good tool to write a JS code and edit any config files. It's lightweight, fast, сustomizable and I can use it on any device via SSH. 

