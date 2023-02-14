---
layout: post
title: Build develop environment on a new mac
date: 2023-02-10 17:00:00
description: clash + neovim + cpp + Rust + Python
tags: env
categories: env-build
giscus_comments: true
---

I have an intention to collect all my configure files and procedures into a script for a long time. I've bought a new Macbook Air M1 this year whose configuration is a little different from Linux. Basically, I need my favorite tools installed on my devices, including [Alacritty](https://github.com/alacritty/alacritty){:target="_blank"}, [tmux](https://github.com/tmux/tmux/wiki){:target="_blank"}, [Neovim](https://github.com/neovim/neovim){:target="_blank"}, [clashX](https://github.com/yichengchen/clashX/releases). Other tools like brew, xcode-select and iterm2 are also necessary.

Since I am in China now, I will attach mirrors for quick installations.

## 1. Package manager

### Official installation

First of all, we need to get the famous package manger -- [Homebrew](https://brew.sh/), which is very simple:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Install from mirror

1. In order to use the tuna's mirror, we need to export following environment variables to our shell:

```bash
export HOMEBREW_INSTALL_FROM_API=1
export HOMEBREW_API_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/api"
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles"
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"
```

2. After that, get script from tuna mirrors:

```bash
git clone --depth=1 https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/install.git brew-install && /bin/bash brew-install/install.sh && rm -rf brew-install
```

3. For Apple Silicon CPU devices, we need to add shell env manually,

```bash
test -r ~/.bash_profile && echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.bash_profile
test -r ~/.zprofile && echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
```

### Replace upstream

If we already have brew installed on our mac and want to replace the upstream source to Chinese mirrors, we can set `HOMEBREW_API_DOMAIN` and `HOMEBREW_BREW_GIT_REMOTE` in our `~/.zshrc`:

```bash
export HOMEBREW_API_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/api"
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"
brew update
```

### Reset upstream

```bash
unset HOMEBREW_BREW_GIT_REMOTE
git -C "$(brew --repo)" remote set-url origin https://github.com/Homebrew/brew

unset HOMEBREW_API_DOMAIN
unset HOMEBREW_CORE_GIT_REMOTE
BREW_TAPS="$(BREW_TAPS="$(brew tap 2>/dev/null)"; echo -n "${BREW_TAPS//$'\n'/:}")"
for tap in core cask{,-fonts,-drivers,-versions} command-not-found; do
    if [[ ":${BREW_TAPS}:" == *":homebrew/${tap}:"* ]]; then
        brew tap --custom-remote "homebrew/${tap}" "https://github.com/Homebrew/homebrew-${tap}"
    fi
done
```

<strong>Note: if we need to reset upstream, we also need to remove env variables `HOMEBREW_BREW_GIT_REMOTE` and `HOMEBREW_CORE_GIT_REMOTE` in profiles before executing `brew update`.</strong>

## 2. Install tools

### xcode-select

```bash
xcode-select --install
```

We can also download from [apple developer site](https://developer.apple.com/download/all/?q=xcode), and select the corresponding version of **Command LIne Tools for Xcode.**

### Necessary tools

```bash
brew install git alacritty iterm2 neovim python3 rust ruby golang zsh
```

### Git config

```bash
git config --global user.name=""
git config --global user.email=""
git config --global core.editor=nvim
git config --global core.autocrlf=false
git config --global http.sslverify=false

// For mirros users
git config --global url."https://github.com.cnpmjs.org/".insteadOf https://github.com/
git config --global url."git@git.zhlh6.cn".insteadOf git@github.com
```

### Generate a new SSH key

```bash
ssh-keygen -t rsa -b 4096 -C "xxx@gmail.com"
```

### Test our SSH connection

After we add our SSH key to github we can test it.

```bash
// ssh -T git@HOSTNAME
ssh -T git@github.com
```



## 3. Shell configuration

### Fonts

```bash
brew tap homebrew/cask-fonts
brew install font-hack-nerd-font
brew tap homebrew/cask-fonts && brew install --cask font-meslo-lg-nerd-font
```

### oh-my-zsh and powerline 10K

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

**China mirrors:**

```bash
sh -c "$(curl -fsSL https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh)"
```

```bash
git clone --depth=1 https://gitee.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Set `ZSH_THEME="powerlevel10k/powerlevel10k"` in `~/.zshrc`

### zsh plugins

+ **zsh-autosuggestions**

  ```bash
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  ```
  ```bash
  git clone https://gitee.com/githubClone/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  ```

+ **zsh-syntax-highlighting**

  ```bash
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  ```
  
  ```bash
  git clone https://gitee.com/mirror-github/zsh-syntax-highlighting ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  ```

Addd plugins to `.zshrc` `plugins=(git zsh-syntax-highlighting zsh-autosuggestions)`

### Alacritty config

Override configuration file of Alacritty in `~/.config/alacritty/alacritty.yaml`

```yaml
# Font configuration (changes require restart)
font:
  # Normal (roman) font face
  normal:
    family: MesloLGS NF
    # The `style` can be specified to pick a specific face.
    style: Regular

  # Bold font face
  bold:
    family: MesloLGS NF
    style: Bold
  # Italic font face
  italic:
    family: MesloLGS NF
    style: Italic

  # Point size
  size: 14.0

  # Offset is the extra space around each character. `offset.y` can be thought of
  # as modifying the line spacing, and `offset.x` as modifying the letter spacing.
  offset:
    x: 0
    y: 0

  # Glyph offset determines the locations of the glyphs within their cells with
  # the default being at the bottom. Increasing `x` moves the glyph to the right,
  # increasing `y` moves the glyph upwards.
  glyph_offset:
    x: 0
    y: 0

# mods: Control|Shift|Option|Command
# Support for nvim Alt siwtch
key_bindings:
  - { key: J, mods: Alt, chars: "\x1bj" }
  - { key: K, mods: Alt, chars: "\x1bk" }
```

### Iterm2 pop-up window

+ Settting -> Keys -> Hotkeys **Create a Dedicated Hotkey Window...**
+ Set Hotkey to `Ctrl+\`
+ 

## 4. ClashX

Release version: https://github.com/yichengchen/clashX/releases.

