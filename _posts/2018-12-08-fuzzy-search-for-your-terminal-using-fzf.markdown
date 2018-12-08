---
layout: single
title:  "Fuzzy search using fzf"
date:   2018-12-08 18:37:00 +0500
---
Fzf is a smart command line tool to find files and folders. Just forget about typing file paths.

 For example: 
* I don't have to write paths for any files or folders because fzf do this job using a fuzzy search
* Actually fzf is a very usable console interface to filter 
 any lists, files, command history, processes, hostnames, bookmarks, git commits, etc.
* You can write your own tools with fzf to increase your productivity

It's easy to install fzf using git: [https://github.com/junegunn/fzf#using-git](https://github.com/junegunn/fzf#using-git)

After the installation is complete you will be  able to use in your terminal:
* <CTRL+T> list files + folders in current directory (recursively)
* <CTRL+R> search history of shell commands 
* <ALT+C> fuzzy change directory (works without cd command)
These default features are very usable.

More important is configuring functions that will help you to build your software products. My current functions:
* open file by text (recursively)
* open file by name (recursively)
* interactive cd

Below the code of the bash functions:

{% highlight bash %}
# open file by text
# sudo apt-get install silversearcher-ag
vg() {
  local file
  local line

  read -r file line <<<"$(ag --nobreak --noheading $@ | fzf -0 -1 | awk -F: '{print $1, $2}')"

  if [[ -n $file ]]
  then
     vim $file +$line
  fi
}

# open file by name
vf() {
  local files
  IFS=$'\n' files=($(fzf-tmux --query="$1" --multi --select-1 --exit-0))
  [[ -n "$files" ]] && ${EDITOR:-vim} "${files[@]}"
}

# interactive cd
function cd() {
    if [[ "$#" != 0 ]]; then
        builtin cd "$@";
        return
    fi
    while true; do
        local lsd=$(echo ".." && ls -p | grep '/$' | sed 's;/$;;')
        local dir="$(printf '%s\n' "${lsd[@]}" |
            fzf --reverse --preview '
                __cd_nxt="$(echo {})";
                __cd_path="$(echo $(pwd)/${__cd_nxt} | sed "s;//;/;")";
                echo $__cd_path;
                echo;
                ls -p --color=always "${__cd_path}";
        ')"
        [[ ${#dir} != 0 ]] || return 0
        builtin cd "$dir" &> /dev/null
    done
}
{% endhighlight %}

As you can see 'open file by text' requires silversearcher-ag. Put these functions to your .zshrc/.bashrc file to call them from anywhere.

Hope fzf and the functions will help you and make the world better.
