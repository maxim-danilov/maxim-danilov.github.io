---
layout: single
title: "Make Linux terminal great again (Terminator + Oh My ZSH + autosuggestions + highlighting + Agnoster theme + powerline fonts + solarized colors)"
date: 2018-07-16 12:49:10 +0300
---

The terminal is part of every software developer’s life. The terminal carries out routine tasks. Most Linux programs have only console interface. If you have a good terminal you can forget about a boring graphical interface. :) 

![image-title-here](/assets/img/0010_c.png){:class="img-responsive"}

However, the default terminal is like an old typewriter. It doesn't have autosuggestions, autocorrect, highlighting. Let's fix it. 

## Install Terminator
Well, to start with we are going to update your terminal emulator. We will use Terminator instead of gnome-terminal. Terminator has appearance settings, hotkeys, text search, arrange terminals in a grid. 

{% highlight bash %}
sudo apt-get install terminator
{% endhighlight %}

## Install Zsh
Next, we will update our Unix shell. We will use zsh instead of bash. Zsh has autocorrection, history search, plugins, improved bash language. 

{% highlight bash %}
sudo apt-get install zsh
{% endhighlight %}

Zsh should be set as your default shell:
{% highlight bash %}
chsh -s $(which zsh)
{% endhighlight %}

Restart your terminal. You should see the configuration menu of zsh and choose option 2:
`Populate your ~/.zshrc with the configuration recommended by the system administrator and exit`

![image-title-here](/assets/img/0011.png){:class="img-responsive"}

Don’t forget to move your configuration and aliases from .bashrc to .zshrc:
![image-title-here](/assets/img/0012.png){:class="img-responsive"}

## Install Oh My Zsh
Oh my zsh is a framework for managing your zsh. It adds a lot of helper functions, plugins, themes. 
{% highlight bash %}
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
{% endhighlight %}

## Install Powerline fonts 
We need [these fonts](https://github.com/powerline/fonts) to show Unicode chars in the terminal.

{% highlight bash %}
sudo apt-get install fonts-powerline
{% endhighlight %}

## Change theme to Agnoster

You need to change `ZSH_THEME="robbyrussell"` to `ZSH_THEME="agnoster"` in your .zshrc 

Restart your terminal. 

## Install Solarized color
Next, we will install the color palette for the terminal. 

{% highlight bash %}
git clone git://github.com/sigurdga/gnome-terminal-colors-solarized.git ~/.solarized
{% endhighlight %}
{% highlight bash %}
cd ~/.solarized
{% endhighlight %}
{% highlight bash %}
./install.sh
{% endhighlight %}

Choose option 1 (dark theme). Then choose option 1 to download dircolors-solarized. After installation, open .zshrc and add the line:

{% highlight bash %}
eval `dircolors ~/.dir_colors/dircolors`
{% endhighlight %}

Then we need to activate just installed color palette: 
In your terminator right click on the terminal

`Preferences>Profiles>Colors>Foreground and Background>Built-in schemes`: `Solarized dark`

`Preferences>Profiles>Colors>Palette>Built-in schemes`: `Solarized`

![image-title-here](/assets/img/0013.png){:class="img-responsive"}

Restart Terminator. You should see something like this:

![image-title-here](/assets/img/0014.png){:class="img-responsive"}

## Oh My Zsh plugins
### Autosuggestions 
Autosuggestions work using your zsh history. This greatly increases your typing speed.
https://github.com/zsh-users/zsh-autosuggestions

{% highlight bash %}
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
{% endhighlight %}

Add the plugin to the list of plugins for Oh My Zsh to load (inside ~/.zshrc):
`plugins=(zsh-autosuggestions)`

![image-title-here](/assets/img/0006.gif){:class="img-responsive"}

Press the right arrow to use the suggestion. 

### Remove hostname
The username takes some space of your terminal. We’ll remove this.
Add this line to your ~/.zshrc

{% highlight bash %}
prompt_context() {} 
{% endhighlight %}

![image-title-here](/assets/img/0015.png){:class="img-responsive"}

Now it looks more compact. 

### Syntax highlighting
{% highlight bash %}
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
{% endhighlight %}
{% highlight bash %}
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
{% endhighlight %}

![image-title-here](/assets/img/0016.png){:class="img-responsive"}

### Vim 
This plugin for vim fans. 

{% highlight bash %}
export ZSH="/home/ghz/.oh-my-zsh"

ZSH_THEME="agnoster"

plugins=(
  git
  zsh-autosuggestions
  vi-mode
)

source $ZSH/oh-my-zsh.sh

# Colors:
eval `dircolors ~/.dir_colors/dircolors`

# Aliases:
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME=$HOME/gems' >> ~/.bashrc
echo 'export PATH=$HOME/gems/bin:$PATH' >> ~/.bashrc

# My aliases:
alias lg='ll | grep '
alias hg='history | grep '
alias hv='history | vim -'
alias copy='xsel -ib'
alias mv="mv -v"
alias rm="rm -vi"
alias cp="cp -v"
alias install="sudo apt install "
alias update="sudo apt update"

# Remove hostname:
prompt_context() {} 

# Highlighting:
source /home/ghz/.oh-my-zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

# My functions:
mkdircd () {
    mkdir "$@"
    if [ "$1" = "-p" ]; then
        cd "$2"
    else
        cd "$1"
    fi
}

pathtofile () {
  case "$1" in
    /*) printf '%s\n' "$1";;
    *) printf '%s\n' "$PWD/$1";;
  esac
}
{% endhighlight %}
Add `vi-mode` to your plugins array. 

Now you can input difficult command easily. :)

## My .zshrc
{% highlight viml %}
vim .zshrc
export ZSH="/home/ghz/.oh-my-zsh"

ZSH_THEME="agnoster"

plugins=(
  git
  zsh-autosuggestions
  vi-mode
)

source $ZSH/oh-my-zsh.sh

# Colors:
eval `dircolors ~/.dir_colors/dircolors`

# Aliases:
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
echo 'export GEM_HOME=$HOME/gems' >> ~/.bashrc
echo 'export PATH=$HOME/gems/bin:$PATH' >> ~/.bashrc

# My aliases:
alias lh='ls -a | egrep "^\."'

# Remove hostname
prompt_context() {} 

# Highlighting
source /home/ghz/.oh-my-zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
{% endhighlight %}


## Conclusion 
We built a beautiful terminal with autosuggestions, autocorrect, highlighting, hotkeys and vim as an input method. The terminal has a potential for further development.
