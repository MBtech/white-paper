---
layout: post
title: "Bash tab completion"
categories:
  - guide
tags:
  - guide
---
Bash completion is a functionality through which bash presents possible options when users press the `tab` key while typing a command. It not only saves user time by reducing the keystrokes but can also give user the available options so that they don't have to remember the exact flag or option. The completion script uses builtin bash commands `complete` and `compgen` to define the suggestions that should be shown for an executable.

Recently, I started contributing to a small project called [googliser](https://github.com/teracow/googliser) that lets you get google images using CLI. Here's the bash completion spec that I wrote for googliser.

`OPTS` lists all the command line options and `compgen` would show the options that match based on what you has already typed from these set of options.

`COMP_WORDS` is an array of all words typed after the name of the program.
`COMP_CWORD` is an index in `COMP_WORDS` that points to the word at the current cursor when tab was pressed.  

Lastly, `_filedir` is a function available in bash-completion lib. to complete filenames/paths and it can handle spaces in the filenames as well. You can check if you have that function available or not `declare -f _filedir` (I found out about it through this [answer](https://stackoverflow.com/questions/23998364/bash-completion-script-to-complete-file-path-after-certain-arguments-options)). You probably would need to install that lib for MacOS. I added that function so that for command line arguments that require filepaths, our bash completion script has the ability to suggest them.

```bash
#!/usr/bin/env bash
_GoogliserCompletion()
{
    # Pointer to current completion word.
    # By convention, it's named "cur" but this isn't strictly necessary.
    local cur

    OPTS='-d -E -h -L -q -s -S -z -a -b -G -i -l -m -n -o -p -P -r -R -t -T -u --debug \
    --exact-search --help --lightning --links-only --no-colour --no-color --safesearch-off \
    --quiet --random --reindex-rename --save-links --skip-no-size --aspect-ratio \
    --border-pixels --colour --color --exclude-links --exclude-words --format --gallery \
    --input-links --input-phrases --lower-size --minimum-pixels --number --output --parallel \
    --phrase --recent --retries --sites --thumbnails --timeout --title --type --upper-size --usage-rights'

    COMPREPLY=()   # Array variable storing the possible completions.
    cur=${COMP_WORDS[COMP_CWORD]}
    prev=${COMP_WORDS[COMP_CWORD-1]}
    case "$cur" in
        -*)
        COMPREPLY=( $( compgen -W "${OPTS}" -- ${cur} ) );;
    esac

    # Display file completion for options that require files as arguments
    case "$prev" in
        --input-links|--exclude-links|-i|--input-phrases)
        _filedir ;;
    esac

    return 0
}

complete -F _GoogliserCompletion -o filenames googliser
```

## Registering the completion script:
When you are simply experimenting with the completion spec you can do `source <path to completion spec>` and then try out the changes. But in order to have the changes applied across new terminals and without manually doing the source, you need to register you bash completion spec. Here's how you can do it on Mac or in Linux.

### On MacOS (Tested with Catalina)
Copy your bash completion spec to `/usr/local/etc/bash_completion.d/`

```bash
brew install bash-completion
echo "[ -f /usr/local/etc/bash_completion ] && . /usr/local/etc/bash_completion" >> ~/.bash_profile
source ~/.bash_profile
```

Or if you are doing these steps in a script then use `. ~/.bash_profile` instead of the last command shown above.

### On Linux (Test with Ubuntu 18.04.2)
Copy your bash completion spec to `/etc/bash_completion.d/` and it should be automatically reloaded for any new bash terminal

For your current terminal you can simply source

`source <path to your completion script>`

### Bash completion specs with zsh
Since in macOS Catalina, zsh became the default shell I decided to add zsh support for googliser.
You can add the following lines to `~/.zshrc` to enable this (thanks to this [answer](https://stackoverflow.com/questions/3249432/can-a-bash-tab-completion-script-be-used-in-zsh))

```bash
autoload -U +X compinit && compinit
autoload -U +X bashcompinit && bashcompinit
source /path/to/your/bash_completion_script
```

**References:**
- [Programmable Completion Builtins](https://www.gnu.org/software/bash/manual/html_node/Programmable-Completion-Builtins.html)
- [Introduction to Programmable Completion](https://www.tldp.org/LDP/abs/html/tabexpansion.html)
- [Bash Completion Mac](https://sourabhbajaj.com/mac-setup/BashCompletion/)
- [Bash Completion Tutorial](https://iridakos.com/programming/2018/03/01/bash-programmable-completion-tutorial)
