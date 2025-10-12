---
tags:
- Linux
- Neovim
- Vim
date: 2023-07-17
title: install lunarvim on ubuntu
categories:
lastMod: 2025-10-12
--- 
## Install prerequisites
  
### Neovim
  
#### Approach 1: makedeb package

The neovim in Debian official repo is v0.6, but lunarvim acquires v0.8+.

[kde - Can&#39;t install neovim v0.8.0+ on Ubuntu - Super User](https://superuser.com/a/1776825) provides a solution: install neovim from the [makedeb package registry](https://mpr.makedeb.org/packages/neovim)

[Installing Packages | makedeb Docs](https://docs.makedeb.org/using-the-mpr/installing-packages/)

```shell
> bash -ci "$(wget -qO - 'https://shlink.makedeb.org/install')"
> wget -qO - 'https://proget.makedeb.org/debian-feeds/prebuilt-mpr.pub' | gpg --dearmor | sudo tee /usr/share/keyrings/prebuilt-mpr-archive-keyring.gpg 1> /dev/null
> echo "deb [arch=amd64 signed-by=/usr/share/keyrings/prebuilt-mpr-archive-keyring.gpg] https://proget.makedeb.org prebuilt-mpr $(lsb_release -cs)" | sudo tee /etc/apt/sources.list.d/prebuilt-mpr.list
> sudo apt update
```

`mist` doesn't work in my case so I create the package using makedeb.

```shell
> git clone https://mpr.makedeb.org/neovim.git
> cd neovim
> makedeb -si
```

After long building process, test neovim version:

```shell
> nvim -v
NVIM v0.9.1
Build type: Debug
LuaJIT 2.1.0-beta3
```
  
#### Approach 2: AppImage

[Installing Neovim · neovim/neovim Wiki · GitHub](https://github.com/neovim/neovim/wiki/Installing-Neovim#appimage-universal-linux-package)

```shell
> curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim.appimage
> chmod u+x nvim.appimage
> ./nvim.appimage --appimage-extract
> sudo mv squashfs-root /
> sudo ln -s /squashfs-root/AppRun /usr/bin/nvim
> nvim -v
NVIM v0.9.1
Build type: Release
LuaJIT 2.1.0-beta3
```
  
### make

```shell
> sudo apt-get install make
```
  
### pip

```shell
> sudo apt-get install pip
```
  
### node.js, npm

**(Update on 1st Sept. 2023: The following method is now deprecated. Visit [GitHub - nodesource/distributions: NodeSource Node.js Binary Distributions](https://github.com/nodesource/distributions) for details.**



[GitHub - nodesource/distributions: NodeSource Node.js Binary Distributions](https://github.com/nodesource/distributions#debinstall)

```shell
> curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash - &&\
sudo apt-get install -y nodejs
```
  
### cargo

[Other Installation Methods - Rust Forge](https://forge.rust-lang.org/infra/other-installation-methods.html)

```shell
> curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
  
### lazygit

https://github.com/jesseduffield/lazygit#installation

```shell
> LAZYGIT_VERSION=$(curl -s "https://api.github.com/repos/jesseduffield/lazygit/releases/latest" | grep -Po '"tag_name": "v\K[^"]*')
> curl -Lo lazygit.tar.gz "https://github.com/jesseduffield/lazygit/releases/latest/download/lazygit_${LAZYGIT_VERSION}_Linux_x86_64.tar.gz"
> tar xf lazygit.tar.gz lazygit
> sudo install lazygit /usr/local/bin
```
  
## Install LunarVim

```shell
> LV_BRANCH='release-1.3/neovim-0.9' bash <(curl -s https://raw.githubusercontent.com/LunarVim/LunarVim/release-1.3/neovim-0.9/utils/installer/install.sh)
```

add the lvim dir to env:

```shell
> vim ~/.zshrc # or .bashrc, etc. according to the shell used
```

add the following into it:

```
export PATH="/home/<username>/.local/bin:$PATH"
```

then run:

```shell
> source .zshrc
```

Now calling `lvim` will invoke LunarVim. 
 