---
title: 前端开发环境搭建（macOS）
date: 2018-03-22 11:07:11
tags: ['开发环境']
---
### 代理

chrome插件：[switchOmega](https://www.switchyomega.com/)

GFWList：[地址](https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt)

可视化代理工具： [~~shadowsocks-qt5~~](https://github.com/shadowsocks/shadowsocks-qt5/wiki/%E5%AE%89%E8%A3%85%E6%8C%87%E5%8D%97) [shadowsocks](https://github.com/shadowsocks)

### 终端

zsh:

```bash
sudo apt-get install zsh //ubuntu
```
``` bash
brew install zsh zsh-completions //macOS
```

oh-my-zsh：

``` bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```
auto-autosuggestions：

``` bash
git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
```

添加插件：

``` shell
plugins=(git z zsh-autosuggestions)
```

### 前端开发环境搭建

#### nodejs

使用``nvm``来安装nodejs：

``` bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash
```

安装完nvm之后，会在``.bashrc``中加如下面的代码，如果``.zshrc``里面如果没有载入``.bashrc``的，是无效的，手动把下面代码剪切到``.zshrc``中。

``` shell
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

``nvm``安装好之后，用下面命令安装最新版本nodejs:

``` bash
nvm install node
```

#### yarn

#### ubuntu
``` bash
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
```

通过以上指令添加yarn源，然后直接``apt-get``安装

#### macOS
``` bash
brew install zsh zsh-completions
```

安装好只有好必要的话可以讲``yarn bin``目录添加到环境变量中

``` shell
export PATH=$HOME/.yarn/bin:$HOME/bin:/usr/local/bin:$PATH
```

#### vscode

[官网地址](https://code.visualstudio.com/)

### GIT-alais
``` text
[alias]
        st = status
        co = checkout
        ci = commit
        pr = pull --rebase
        ps = push
        br = branch
        unstage = reset HEAD
        last = log -1
        lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-comm$
        d = difftool
        g = log --graph --all
```