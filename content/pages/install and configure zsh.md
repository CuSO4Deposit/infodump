---
tags:
- Linux
date: 2023-07-16
title: install and configure zsh
categories:
lastMod: 2025-10-12
--- 
## Install

```shell
$ sudo apt-get install zsh
$ cat /etc/shells
# ...
/bin/zsh
# ...
$ chsh -s zsh
```
  
## Configure

Use [Oh-My-Zsh](https://github.com/ohmyzsh/ohmyzsh) to generate quick configuration.

Theme is configured in `.zshrc`, modify `ZSH_THEME` variable. It can also be set to "Random" to reload a random theme each time the shell restarts.

Plugins can be git cloned into `~/.oh-my-zsh/custom/plugins` and add into `.zshrc`, the `plugins=()` list.

After changes to `.zshrc`, run `source .zshrc` to reload.
 