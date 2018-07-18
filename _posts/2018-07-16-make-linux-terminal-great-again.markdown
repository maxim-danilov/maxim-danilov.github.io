---
layout: post
title:  "Make Linux terminal great again (Terminator + Oh My ZSH + autosuggestions + highlighting + Agnoster theme + powerline fonts + solarized colors)"
date:   2018-07-16 12:49:10 +0300
categories: jekyll update
---

![image-title-here](/assets/img/0010.png){:class="img-responsive"}

The terminal is part of every software developer’s life. The terminal carry out routine tasks. Most Linux programs have only console interface. If you have a good terminal you can forget about boring graphical interface. :) 

However the default terminal is like an old typewriter. It doesn't have autosuggestions, autocorrect, highlighting. Let's fix it. 

## Install Terminator
Well, to start with we are going to update your terminal emulator. We will use Terminator instead of gnome-terminal. Terminator has appearance settings, hotkeys, text search, arrange terminals in a grid. 

{% highlight bash %}
sudo apt-get install terminator
{% endhighlight %}

## Install Zsh
Next we will update our unix shell. We will use zsh instead bash. Zsh has autocorrect, history search, plugins, improved bash language. 

{% highlight bash %}
sudo apt-get install zsh
{% endhighlight %}

Zsh should be set as your default shell:
{% highlight bash %}
chsh -s $(which zsh)
{% endhighlight %}

Перезапустить терминал В отображающиеся меню zsh выбрать пункт 2 
Restart your terminal. You should see the configuration menu of zsh and choose option 2:
`Populate your ~/.zshrc with the configuration recommended by the system administrator and exit`

Don’t forget to move your configuration and aliases from .bashrc to .zshrc:

## Install Oh My Zsh
Oh my zsh is a framework for managing your zsh. It adds a lot of helper functions, plugins, themes. 
{% highlight bash %}
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
{% endhighlight %}

## Install Powerline fonts 
We need these fonts to show unicode chars in the terminal.
https://github.com/powerline/fonts 
{% highlight bash %}
sudo apt-get install fonts-powerline
{% endhighlight %}

Change theme to Agnoster

You need to change [ZSH_THEME="robbyrussell"] to [ZSH_THEME="agnoster"] in your .zshrc 

Restart your terminal. 

## Install Solarized color
Next we will install the color palette for the terminal. 

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
eval \`dircolors ~/.dir_colors/dircolors\`
{% endhighlight %}

Then we need to activate just installed color palette: 
In your terminator right click on the terminal
Preferences>Profiles>Colors>Foreground and Background>Built-in schemes: Solarized dark
Preferences>Profiles>Colors>Palette>Built-in schemes: Solarized

Restart Terminator. You should see something like this:

## Oh My Zsh plugins
### Autosuggestions 
Autosuggestions work using your zsh history. This greatly increases your typing speed.
https://github.com/zsh-users/zsh-autosuggestions

{% highlight bash %}
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
{% endhighlight %}

Add the plugin to the list of plugins for Oh My Zsh to load (inside ~/.zshrc):
`plugins=(zsh-autosuggestions)`

Press right arrow to use the suggestion. 

### Remove hostname
The username takes some space of your terminal. We’ll remove this.
Add this line to your  ~/.zshrc

{% highlight bash %}
prompt_context() {} 
{% endhighlight %}

Now it looks more compact. 

### Syntax highlighting
{% highlight bash %}
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
{% endhighlight %}
{% highlight bash %}
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
{% endhighlight %}

### Vim 
This plugin for vim fans. Now you can input difficult command easily. :)

{% highlight bash %}
vim .zshrc
{% endhighlight %}
Add `vi-mode` to your plugins array. 

## Conclusion 
We built a beautiful terminal with autosuggestions, autocorrect, highlighting, hotkeys and vim as input method. The terminal has potential for further development.