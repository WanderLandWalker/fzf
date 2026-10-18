<div align="center">
  <img src="https://raw.githubusercontent.com/junegunn/i/master/fzf-color.png" alt="fzf - a command-line fuzzy finder">
  <a href="https://github.com/junegunn/fzf/actions"><img src="https://github.com/junegunn/fzf/actions/workflows/linux.yml/badge.svg?branch=master" alt="Build Status"></a>
  <a href="http://github.com/junegunn/fzf/releases"><img src="https://img.shields.io/github/v/tag/junegunn/fzf" alt="Version"></a>
  <a href="https://github.com/junegunn/fzf?tab=MIT-1-ov-file#readme"><img src="https://img.shields.io/github/license/junegunn/fzf" alt="License"></a>
  <a href="https://github.com/junegunn/fzf/graphs/contributors"><img src="https://img.shields.io/github/contributors/junegunn/fzf" alt="Contributors"></a>
  <a href="https://github.com/sponsors/junegunn"><img src="https://img.shields.io/github/sponsors/junegunn" alt="Sponsors"></a>
  <a href="https://github.com/junegunn/fzf/stargazers"><img src="https://img.shields.io/github/stars/junegunn/fzf?style=flat" alt="Stars"></a>
</div>

---

<kbd align="center">
  <br/>
  <a href="https://commitgoods.com/collections/fzf"><img src="https://junegunn.github.io/fzf/images/fzf-mugs.jpg" width="80%" alt="fzf merch"></a>
  <br/>
  <br/>
  表达对 fzf 的喜爱 —— T 恤、马克杯与贴纸现已开售！
  <br/>
  <br/>
  <a href="https://commitgoods.com/collections/fzf">commitgoods.com/collections/fzf</a>
  <br/>
  <br/>
</kbd>

---

> **语言**：[English](README.md) | 简体中文（本文档）
>
> 本文档为 fzf 官方 README 的简体中文译本。所有命令、选项、环境变量名、函数名与链接均保持原文，以便直接对照执行。

fzf 是一个通用的命令行模糊查找器，也是一个交互式终端工具箱。

<img src="https://raw.githubusercontent.com/junegunn/i/master/fzf-preview.png" width=640>

无论你是要选择文件、浏览命令历史、预览数据、通过模糊匹配浏览复杂的数据集，
还是构建自定义菜单与工作流，fzf 都提供了所需的构件，把 shell 脚本变成丰富的终端应用。

亮点
----------

- **可移植** // 以单一二进制文件分发，安装简便
- **速度快** // 经过优化，可在毫秒级处理数百万条目
- **可编程** // 事件驱动架构，可用于构建自定义终端界面与工作流
- **开箱即用** // 内置对 Bash、Zsh、Fish、Nushell、Vim 与 Neovim 的集成

目录
-----------------

<!-- vim-markdown-toc GFM -->

* [安装](#安装)
    * [使用 Homebrew](#使用-homebrew)
    * [使用 Mise](#使用-mise)
    * [Linux 软件包](#linux-软件包)
    * [Windows 软件包](#windows-软件包)
    * [使用 git 安装](#使用-git-安装)
    * [二进制发行包](#二进制发行包)
    * [配置 Shell 集成](#配置-shell-集成)
    * [Vim/Neovim 插件](#vimneovim-插件)
* [升级 fzf](#升级-fzf)
* [构建 fzf](#构建-fzf)
* [用法](#用法)
    * [使用查找器](#使用查找器)
    * [显示模式](#显示模式)
        * [`--height` 模式](#--height-模式)
        * [`--popup` 模式](#--popup-模式)
    * [搜索语法](#搜索语法)
    * [环境变量](#环境变量)
    * [自定义外观](#自定义外观)
    * [选项](#选项)
    * [演示](#演示)
* [示例](#示例)
* [命令行键位绑定](#命令行键位绑定)
* [模糊补全](#模糊补全)
    * [文件与目录](#文件与目录)
    * [进程 ID](#进程-id)
    * [主机名](#主机名)
    * [环境变量 / 别名](#环境变量--别名)
    * [自定义 bash 与 zsh 的模糊补全](#自定义-bash-与-zsh-的模糊补全)
        * [自定义补全所用的 fzf 选项](#自定义补全所用的-fzf-选项)
        * [自定义路径与目录的补全来源](#自定义路径与目录的补全来源)
        * [支持的命令（bash）](#支持的命令bash)
        * [自定义模糊补全](#自定义模糊补全)
    * [fish 的模糊补全](#fish-的模糊补全)
    * [Nushell 的模糊补全](#nushell-的模糊补全)
* [Vim 插件](#vim-插件)
* [进阶主题](#进阶主题)
    * [针对不同类型输入进行调优](#针对不同类型输入进行调优)
    * [性能](#性能)
    * [执行外部程序](#执行外部程序)
    * [转变为另一个进程](#转变为另一个进程)
    * [重新加载候选列表](#重新加载候选列表)
        * [1. 按 CTRL-R 刷新进程列表](#1-按-ctrl-r-刷新进程列表)
        * [2. 按 CTRL-D 或 CTRL-F 切换数据源](#2-按-ctrl-d-或-ctrl-f-切换数据源)
        * [3. 交互式 ripgrep 集成](#3-交互式-ripgrep-集成)
    * [预览窗口](#预览窗口)
    * [预览图片](#预览图片)
* [提示](#提示)
    * [遵循 `.gitignore`](#遵循-gitignore)
    * [Fish shell](#fish-shell)
    * [fzf 主题炼丹场](#fzf-主题炼丹场)
* [相关项目](#相关项目)
* [许可证](#许可证)
* [周边商品](#周边商品)
* [赞助者 :heart:](#赞助者-heart)

<!-- vim-markdown-toc -->

安装
------------

### 使用 Homebrew

你可以使用 [Homebrew](https://brew.sh/)（在 macOS 或 Linux 上）安装 fzf。

```sh
brew install fzf
```

> [!IMPORTANT]
> 如需配置 Shell 集成（键位绑定与模糊补全），
> 请参阅[下文的配置 Shell 集成](#配置-shell-集成)。

fzf 也可[通过 MacPorts][portfile] 安装：`sudo port install fzf`

[portfile]: https://github.com/macports/macports-ports/blob/master/sysutils/fzf/Portfile

### 使用 Mise

你可以使用 [mise](https://github.com/jdx/mise) 安装 fzf。

```sh
mise use -g fzf@latest
```

### Linux 软件包

| 包管理器      | Linux 发行版             | 命令                               |
| --------------- | ----------------------- | ---------------------------------- |
| APK             | Alpine Linux            | `sudo apk add fzf`                 |
| APT             | Debian 9+/Ubuntu 19.10+ | `sudo apt install fzf`             |
| Conda           |                         | `conda install -c conda-forge fzf` |
| DNF             | Fedora                  | `sudo dnf install fzf`             |
| Nix             | NixOS 等                | `nix-env -iA nixpkgs.fzf`          |
| Pacman          | Arch Linux              | `sudo pacman -S fzf`               |
| pkg             | FreeBSD                 | `pkg install fzf`                  |
| pkgin           | NetBSD                  | `pkgin install fzf`                |
| pkg_add         | OpenBSD                 | `pkg_add fzf`                      |
| Portage         | Gentoo                  | `emerge --ask app-shells/fzf`      |
| Spack           |                         | `spack install fzf`                |
| XBPS            | Void Linux              | `sudo xbps-install -S fzf`         |
| Zypper          | openSUSE                | `sudo zypper install fzf`          |

> [!IMPORTANT]
> 如需配置 Shell 集成（键位绑定与模糊补全），
> 请参阅[下文的配置 Shell 集成](#配置-shell-集成)。

[![Packaging status](https://repology.org/badge/vertical-allrepos/fzf.svg?columns=3)](https://repology.org/project/fzf/versions)

### Windows 软件包

在 Windows 上，fzf 可通过 [Chocolatey][choco]、[Scoop][scoop]、
[Winget][winget] 与 [MSYS2][msys2] 获取：

| 包管理器       | 命令                                  |
| --------------- | ------------------------------------- |
| Chocolatey      | `choco install fzf`                   |
| Scoop           | `scoop install fzf`                   |
| Winget          | `winget install fzf`                  |
| MSYS2 (pacman)  | `pacman -S $MINGW_PACKAGE_PREFIX-fzf` |

[choco]: https://chocolatey.org/packages/fzf
[scoop]: https://github.com/ScoopInstaller/Main/blob/master/bucket/fzf.json
[winget]: https://github.com/microsoft/winget-pkgs/tree/master/manifests/j/junegunn/fzf
[msys2]: https://packages.msys2.org/base/mingw-w64-fzf

### 使用 git 安装

或者，你也可以把本仓库 "git clone" 到任意目录，然后运行
[install](https://github.com/junegunn/fzf/blob/master/install) 脚本。

```sh
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

该安装脚本会向你的 shell 配置文件追加若干行，用于修改
`$PATH` 并配置 Shell 集成。

### 二进制发行包

你可以从发布页面下载官方的 fzf 二进制文件。

* https://github.com/junegunn/fzf/releases

### 配置 Shell 集成

将下面相应的一行加入你的 shell 配置文件。

* bash
  ```sh
  # 配置 fzf 键位绑定与模糊补全
  eval "$(fzf --bash)"
  ```
* zsh
  ```sh
  # 配置 fzf 键位绑定与模糊补全
  source <(fzf --zsh)
  ```
* fish
  ```fish
  # 配置 fzf 键位绑定
  fzf --fish | source
  ```
* Nushell —— Nushell 不支持把内容通过管道传给 `source`，因此安装
  脚本会在 autoload 目录中生成一个文件。如果你没用安装脚本，可以手动配置：
  ```nu
  # 生成集成脚本
  mkdir ($nu.default-config-dir | path join "autoload")
  fzf --nushell | save -f ($nu.default-config-dir | path join "autoload" "_fzf_integration.nu")
  ```

> [!NOTE]
> `--bash`、`--zsh`、`--fish` 与 `--nushell` 选项仅在较新版本的 fzf 中提供。
> 如果你使用的是旧版 fzf，或者希望有更精细的控制，可以在
> [/shell](/shell) 目录中单独 source 各个脚本文件。
> 文件的位置可能因你使用的包管理器而异。
> 更多信息请参阅相应的包文档。
> （例如 `apt show fzf`）

> [!TIP]
> 你可以在 source 脚本时，把对应的 `*_COMMAND` 变量设为空字符串，
> 从而禁用 CTRL-T、CTRL-R 或 ALT-C 绑定。例如，要禁用 CTRL-R 与 ALT-C：
>
> * bash: `FZF_CTRL_R_COMMAND= FZF_ALT_C_COMMAND= eval "$(fzf --bash)"`
> * zsh: `FZF_CTRL_R_COMMAND= FZF_ALT_C_COMMAND= source <(fzf --zsh)`
> * fish: `fzf --fish | FZF_CTRL_R_COMMAND= FZF_ALT_C_COMMAND= source`
> * nushell：在你的 `env.nu` 中加入：
>   `$env.FZF_CTRL_R_COMMAND = ""; $env.FZF_ALT_C_COMMAND = ""`
>
> 在 source 脚本之后再设置这些变量不会有任何效果。

### Vim/Neovim 插件

如果你使用 [vim-plug](https://github.com/junegunn/vim-plug)，请在你的
Vim 配置文件中加入：

```vim
Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'
```

* `junegunn/fzf` 提供基础的库函数
    * `fzf#install()` 用于确保你安装了最新的二进制文件
* `junegunn/fzf.vim` 是[一个独立项目](https://github.com/junegunn/fzf.vim)，
  提供各种实用的命令

想进一步了解 Vim 集成，请参阅 [README-VIM.zh-CN.md](README-VIM.zh-CN.md)（中译）。

> [!TIP]
> 如果你使用 Neovim 并且偏好基于 Lua 的插件，可以看看
> [fzf-lua](https://github.com/ibhagwan/fzf-lua)。

升级 fzf
-------------

fzf 正在积极开发中，你可能偶尔需要升级它。请根据你的安装方式
参照下面的说明操作。

- git: `cd ~/.fzf && git pull && ./install`
- brew: `brew update; brew upgrade fzf`
- macports: `sudo port upgrade fzf`
- chocolatey: `choco upgrade fzf`
- vim-plug: `:PlugUpdate fzf`

构建 fzf
------------

参见 [BUILD.zh-CN.md](BUILD.zh-CN.md)（中译）。

用法
-----

fzf 会启动交互式查找器，从 STDIN 读取列表，并把
选中的条目写入 STDOUT。

```sh
find * -type f | fzf > selected
```

如果没有 STDIN 管道，fzf 会遍历当前
目录下的文件系统来获取文件列表。

```sh
vim $(fzf)
```

> [!NOTE]
> 你可以覆盖这一默认行为
> * 方式一：把 `$FZF_DEFAULT_COMMAND` 设为一个能生成所需列表的命令
> * 方式二：在 `$FZF_DEFAULT_OPTS` 中设置 `--walker`、`--walker-root` 与 `--walker-skip` 选项

> [!WARNING]
> 更稳妥的做法是使用 `xargs`，不过上面这种写法更易懂，
> 所以我们还是先这么展示了
> ```sh
> fzf --print0 | xargs -0 -o vim
> ```

> [!TIP]
> fzf 还能把自己转变为另一个进程。
>
> ```sh
> fzf --bind 'enter:become(vim {})'
> ```
>
> *更多信息参见[转变为另一个进程](#转变为另一个进程)。*

### 使用查找器

- `CTRL-K` / `CTRL-J`（或 `CTRL-P` / `CTRL-N`）上下移动光标
- 按 `Enter` 选中条目，按 `CTRL-C` / `CTRL-G` / `ESC` 退出
- 在多选模式（`-m`）下，用 `TAB` 与 `Shift-TAB` 标记多个条目
- 支持 Emacs 风格的键位绑定
- 鼠标：滚动、单击、双击；在多选模式下支持 shift-单击与 shift-滚动

### 显示模式

fzf 默认以全屏模式运行，但也提供其他显示模式。

#### `--height` 模式

使用 `--height HEIGHT[%]` 时，fzf 会在光标下方以指定高度启动。

```sh
fzf --height 40%
```

`reverse` 布局与 `--border` 搭配这个选项效果很好。

```sh
fzf --height 40% --layout reverse --border
```

在高度前加上 `~` 表示设置的是最大高度。

```sh
# 会用尽可能少的行数来显示列表
seq 3 | fzf --height ~100%
seq 3000 | fzf --height ~100%
```

高度值也可以是负数。

```sh
# 屏幕高度减 3
fzf --height -3
```

#### `--popup` 模式

使用 `--popup` 选项时，fzf 会在一个弹出窗口中启动
（需要 tmux 3.3+ 或 Zellij 0.44+）。

```sh
# --popup [center|top|bottom|left|right][,SIZE[%]][,SIZE[%][,border-native]]

fzf --popup center         # 居中，宽高各占 50%
fzf --popup 80%            # 居中，宽高各占 80%
fzf --popup 100%,50%       # 居中，宽 100%，高 50%
fzf --popup left,40%       # 靠左，宽 40%
fzf --popup left,40%,90%   # 靠左，宽 40%，高 90%
fzf --popup top,40%        # 靠上，高 40%
fzf --popup bottom,80%,40% # 靠下，宽 80%，高 40%
```

当你不在 tmux 或 Zellij 中时，`--popup` 会被静默忽略。

> [!NOTE]
> 如果你受困于不支持 popup 的旧版 tmux，
> 或者你想在常规的 tmux 窗格中打开 fzf，可以看看
> [fzf-tmux](bin/fzf-tmux) 脚本。

> [!TIP]
> 你可以把这些选项加入 `$FZF_DEFAULT_OPTS`，让它们默认生效。例如，
>
> ```sh
> # 在 tmux 或 Zellij 中用 popup 打开，否则回退到 --height 模式
> export FZF_DEFAULT_OPTS='--height 40% --popup bottom,40% --layout reverse --border top'
> ```

### 搜索语法

除非另有指定，fzf 默认以“扩展搜索模式”启动，在该模式下你可以输入
多个以空格分隔的搜索词。例如 `^music .mp3$ sbtrkt
!fire`

| 词法片段  | 匹配类型                                     | 说明                                |
| --------- | -------------------------------------------- | ----------------------------------- |
| `sbtrkt`  | 模糊匹配（fuzzy-match）                      | 匹配 `sbtrkt` 的条目                |
| `'wild`   | 精确匹配（exact-match，加引号）              | 包含 `wild` 的条目                  |
| `'wild'`  | 精确边界匹配（exact-boundary-match，两端加引号） | 在单词边界处包含 `wild` 的条目   |
| `^music`  | 前缀精确匹配（prefix-exact-match）           | 以 `music` 开头的条目               |
| `.mp3$`   | 后缀精确匹配（suffix-exact-match）           | 以 `.mp3` 结尾的条目                |
| `!fire`   | 反向精确匹配（inverse-exact-match）          | 不包含 `fire` 的条目                |
| `!^music` | 反向前缀精确匹配（inverse-prefix-exact-match） | 不以 `music` 开头的条目           |
| `!.mp3$`  | 反向后缀精确匹配（inverse-suffix-exact-match） | 不以 `.mp3` 结尾的条目            |

如果你不喜欢模糊匹配，也不希望给每个词都"加上引号"，
可以用 `-e` 或 `--exact` 选项启动 fzf。注意，设置了 `--exact` 之后，
以 `'` 开头表示对该词"取消引号"。

单个竖线字符相当于 OR 运算符。例如，下面这条
查询会匹配以 `core` 开头、并以 `go`、`rb`
或 `py` 结尾的条目。

```
^core go$ | rb$ | py$
```

### 环境变量

- `FZF_DEFAULT_COMMAND`
    - 当输入为 tty 时使用的默认命令
    - 例如 `export FZF_DEFAULT_COMMAND='fd --type f'`
- `FZF_DEFAULT_OPTS`
    - 默认选项
    - 例如 `export FZF_DEFAULT_OPTS="--layout=reverse --inline-info"`
- `FZF_DEFAULT_OPTS_FILE`
    - 如果你更倾向用文件来管理默认选项，可以把这个变量设为
      该文件的路径
    - 例如 `export FZF_DEFAULT_OPTS_FILE=~/.fzfrc`

> [!WARNING]
> Shell 集成并不使用 `FZF_DEFAULT_COMMAND`，因为两者的需求
> 略有差异。
>
> * `CTRL-T` 会运行 `$FZF_CTRL_T_COMMAND` 来获取文件与目录列表
> * `ALT-C` 会运行 `$FZF_ALT_C_COMMAND` 来获取目录列表
> * `vim ~/**<tab>` 会以（`~/`）作为第一个参数调用 `fzf_compgen_path()`
> * `cd foo**<tab>` 会以（`foo`）作为第一个参数调用 `fzf_compgen_dir()`
>
> 可用的选项在本文档后面部分介绍。

### 自定义外观

fzf 的用户界面完全可定制，提供了大量
配置选项。如果想快速上手，可以先从 `--style` 选项的样式预设
开始 —— `default`、`full` 或 `minimal`。

```sh
fzf --style full \
    --preview 'fzf-preview.sh {}' --bind 'focus:transform-header:file --brief {}'
```

| 预设值     | 截图                                                                                   |
| :---      | :---                                                                                   |
| `default` | <img src="https://raw.githubusercontent.com/junegunn/i/master/fzf-style-default.png"/> |
| `full`    | <img src="https://raw.githubusercontent.com/junegunn/i/master/fzf-style-full.png"/>    |
| `minimal` | <img src="https://raw.githubusercontent.com/junegunn/i/master/fzf-style-minimal.png"/> |

下面是一个基于 `full` 预设的示例：

<img src="https://raw.githubusercontent.com/junegunn/i/master/fzf-4-borders.png"/>

<details>

```sh
git ls-files | fzf --style full \
    --border --padding 1,2 \
    --border-label ' Demo ' --input-label ' Input ' --header-label ' File Type ' \
    --preview 'fzf-preview.sh {}' \
    --bind 'result:transform-list-label:
        if [[ -z $FZF_QUERY ]]; then
          echo " $FZF_MATCH_COUNT items "
        else
          echo " $FZF_MATCH_COUNT matches for [$FZF_QUERY] "
        fi
        ' \
    --bind 'focus:transform-preview-label:[[ -n {} ]] && printf " Previewing [%s] " {}' \
    --bind 'focus:+transform-header:file --brief {} || echo "No file selected"' \
    --bind 'ctrl-r:change-list-label( Reloading the list )+reload(sleep 2; git ls-files)' \
    --color 'border:#aaaaaa,label:#cccccc' \
    --color 'preview-border:#9999cc,preview-label:#ccccff' \
    --color 'list-border:#669966,list-label:#99cc99' \
    --color 'input-border:#996666,input-label:#ffcccc' \
    --color 'header-border:#6699cc,header-label:#99ccff'
```

</details>

### 选项

完整的选项列表请查看手册页（`fzf --man` 或 `man fzf`）。

### 演示
如果你习惯通过视频学习，可以看看 [@samoshkin](https://github.com/samoshkin) 录制的这段屏幕录像，来了解 `fzf` 的各项功能。

<a title="fzf - command-line fuzzy finder" href="https://www.youtube.com/watch?v=qgG5Jhi_Els">
  <img src="https://i.imgur.com/vtG8olE.png" width="640">
</a>

示例
--------

* [示例 Wiki 页面](https://github.com/junegunn/fzf/wiki/examples)
    * *免责声明：该页面上的示例由社区维护，
      并未经过充分测试*
* [fzf 进阶示例](ADVANCED.zh-CN.md)

命令行键位绑定
-----------------------------

[配置 Shell 集成](#配置-shell-集成)后，你就可以在 bash、zsh、fish 与 Nushell 中
使用下列键位绑定了。

- `CTRL-T` - 把选中的文件与目录粘贴到命令行上
    - 列表是用 `--walker file,dir,follow,hidden` 选项生成的
        - 你可以把 `FZF_CTRL_T_COMMAND` 设为一个能生成所需列表的自定义命令，从而覆盖该行为
        - 也可以在 `FZF_CTRL_T_OPTS` 中设置 `--walker*` 选项
    - 设置 `FZF_CTRL_T_OPTS` 可向 fzf 传递额外选项
      ```sh
      # 使用 bat 预览文件内容（https://github.com/sharkdp/bat）
      export FZF_CTRL_T_OPTS="
        --walker-skip .git,node_modules,target
        --preview 'bat -n --color=always {}'
        --bind 'ctrl-/:change-preview-window(down|hidden|)'"
      ```
    - 在 source 脚本时把 `FZF_CTRL_T_COMMAND` 设为空字符串即可禁用该功能
- `CTRL-R` - 把从历史中选中的命令粘贴到命令行上。
    - 用 `TAB` 选择多条命令。
    - 如果你想按时间顺序查看命令，再按一次 `CTRL-R`
      即可在按相关度排序与按时间顺序之间切换
    - 按 `ALT-R` 可切换到“原始”模式，在该模式下你可以看到某个匹配项周围的其他条目。
      在此模式下，你可以按 `CTRL-N` 与 `CTRL-P` 只在匹配项之间移动。
    - 按 `CTRL-/` 或 `ALT-/` 可切换是否换行显示
    - 按 `SHIFT-DELETE` 可删除选中的命令（bash 与 fish）
    - 仅 Fish shell：
      - 按 `ALT-ENTER` 可重新格式化并插入选中的命令
      - 按 `ALT-T` 可循环切换命令前缀（时间戳、日期时间、无）
    - 设置 `FZF_CTRL_R_OPTS` 可向 fzf 传递额外选项
      ```sh
      # 按 CTRL-Y 使用 pbcopy 把命令复制到剪贴板
      export FZF_CTRL_R_OPTS="
        --bind 'ctrl-y:execute-silent(echo -n {2..} | pbcopy)+abort'
        --color header:italic
        --header 'Press CTRL-Y to copy command into clipboard'"
      ```
      ```fish
      # Fish shell：把日期时间设为默认前缀
      set -gx FZF_CTRL_R_OPTS "--with-nth 1,3.. --bind 'alt-t:change-with-nth(2..|3..|1,3..)'"

      # 或者默认不显示前缀
      set -gx FZF_CTRL_R_OPTS "--with-nth 3.. --bind 'alt-t:change-with-nth(2..|1,3..|3..)'"
      ```
    - 在 source 脚本时把 `FZF_CTRL_R_COMMAND` 设为空字符串即可禁用该功能
    - 目前还不支持通过非空的 `FZF_CTRL_R_COMMAND` 进行自定义覆盖，这么做会输出一条警告
- `ALT-C` - cd 进入选中的目录
    - 列表是用 `--walker dir,follow,hidden` 选项生成的
    - 设置 `FZF_ALT_C_COMMAND` 可覆盖默认命令
        - 或者在 `FZF_ALT_C_OPTS` 中设置 `--walker-*` 选项
    - 设置 `FZF_ALT_C_OPTS` 可向 fzf 传递额外选项
      ```sh
      # 在预览窗口中打印目录树结构
      export FZF_ALT_C_OPTS="
        --walker-skip .git,node_modules,target
        --preview 'tree -C {}'"
      ```
    - 在 source 脚本时把 `FZF_ALT_C_COMMAND` 设为空字符串即可禁用该功能

这些绑定的显示模式可以通过
`FZF_{CTRL_T,CTRL_R,ALT_C}_OPTS` 分别配置，也可以通过 `FZF_DEFAULT_OPTS` 全局配置。
（例如 `FZF_CTRL_R_OPTS='--popup bottom,60% --height 60% --border top'`）

更多技巧可以在[这个 wiki 页面](https://github.com/junegunn/fzf/wiki/Configuring-shell-key-bindings)找到。

模糊补全
----------------

Shell 集成还为 bash、zsh、fish 与 Nushell 提供了模糊补全。

### 文件与目录

当光标前的单词以触发序列结尾（默认为 `**`）时，
即可触发文件与目录的模糊补全。

- `COMMAND [DIRECTORY/][FUZZY_PATTERN]**<TAB>`

```sh
# 当前目录下的文件
# - 你可以用 TAB 键选择多个条目
vim **<TAB>

# 上级目录下的文件
vim ../**<TAB>

# 上级目录下匹配 `fzf` 的文件
vim ../fzf**<TAB>

# 你的主目录下的文件
vim ~/**<TAB>


# 当前目录下的子目录（单选）
cd **<TAB>

# ~/github 下匹配 `fzf` 的子目录
cd ~/github/fzf**<TAB>
```

### 进程 ID

为 kill 命令提供了 PID 的模糊补全。

```sh
# 可以用 <TAB> 或 <Shift-TAB> 键选择多个进程
kill -9 **<TAB>
```

### 主机名

对于 ssh 命令，提供了主机名的模糊补全。主机名
提取自 /etc/hosts 与 ~/.ssh/config。

```sh
ssh **<TAB>
```

### 环境变量 / 别名

```sh
# bash 与 zsh
unset **<TAB>
export **<TAB>
unalias **<TAB>

# fish
set <SHIFT-TAB>
```

### 自定义 bash 与 zsh 的模糊补全

#### 自定义补全所用的 fzf 选项

```sh
# 用 ~~ 代替默认的 ** 作为触发序列
export FZF_COMPLETION_TRIGGER='~~'

# 传给 fzf 命令的选项
export FZF_COMPLETION_OPTS='--border --info=inline'

# 路径补全的选项（例如 vim **<TAB>）
export FZF_COMPLETION_PATH_OPTS='--walker file,dir,follow,hidden'

# 目录补全的选项（例如 cd **<TAB>）
export FZF_COMPLETION_DIR_OPTS='--walker dir,follow'

# 通过 _fzf_comprun 函数对 fzf 选项做进阶定制
# - 该函数的第一个参数是命令名。
# - 你需要确保把其余参数（$@）传给 fzf。
_fzf_comprun() {
  local command=$1
  shift

  case "$command" in
    cd)           fzf --preview 'tree -C {} | head -200'   "$@" ;;
    export|unset) fzf --preview "eval 'echo \$'{}"         "$@" ;;
    ssh)          fzf --preview 'dig {}'                   "$@" ;;
    *)            fzf --preview 'bat -n --color=always {}' "$@" ;;
  esac
}
```

#### 自定义路径与目录的补全来源

```sh
# 使用 fd（https://github.com/sharkdp/fd）来生成路径候选列表。
# - 该函数的第一个参数（$1）是开始遍历的基准路径
# - 详见源码（completion.{bash,zsh}）。
_fzf_compgen_path() {
  fd --hidden --follow --exclude ".git" . "$1"
}

# 使用 fd 来生成目录补全的列表
_fzf_compgen_dir() {
  fd --type d --hidden --follow --exclude ".git" . "$1"
}
```

#### 支持的命令（bash）

在 bash 上，模糊补全仅对一组预定义的命令启用
（运行 `complete | grep _fzf` 可查看列表）。不过你也可以借助
`_fzf_setup_completion` 辅助函数为其他命令启用它。

```sh
# 用法: _fzf_setup_completion path|dir|var|alias|host COMMANDS...
_fzf_setup_completion path ag git kubectl
_fzf_setup_completion dir tree
```

#### 自定义模糊补全

_**（自定义补全 API 仍处于实验阶段，未来可能变更）**_

对于名为 _"COMMAND"_ 的命令，可以使用
`_fzf_complete` 辅助函数来定义 `_fzf_complete_COMMAND` 函数。

```sh
# 为 "doge" 命令自定义模糊补全
#   例如 doge **<TAB>
_fzf_complete_doge() {
  _fzf_complete --multi --reverse --prompt="doge> " -- "$@" < <(
    echo very
    echo wow
    echo such
    echo doge
  )
}
```

- `--` 之前的参数是传给 fzf 的选项。
- `--` 之后，原样传递原本的补全参数即可（`"$@"`）。
- 然后，写一组用于生成补全候选的命令，
  并通过进程替换（`< <(...)`）把它的输出喂给该函数。

zsh 会按照命名约定自动识别该函数，但在 bash 中你必须
用 `complete` 命令手动把该函数与命令关联起来。

```sh
[ -n "$BASH" ] && complete -F _fzf_complete_doge -o default -o bashdefault doge
```

如果你需要对 fzf 的输出做后处理，可以按如下方式定义
`_fzf_complete_COMMAND_post`。

```sh
_fzf_complete_foo() {
  _fzf_complete --multi --reverse --header-lines=3 -- "$@" < <(
    ls -al
  )
}

_fzf_complete_foo_post() {
  awk '{print $NF}'
}

[ -n "$BASH" ] && complete -F _fzf_complete_foo -o default -o bashdefault foo
```

### fish 的模糊补全

fish 的模糊补全与 bash、zsh 有以下几点不同：

- 它不需要 `**` 这样的触发序列。取而代之的是，它在
  `Shift-TAB` 时激活（取代原生的分页搜索模式），而 `TAB` 则保留
  fish 原生的补全行为。
- 它依赖 fish 原生的补全系统来填充候选列表，
  而不是递归遍历文件系统。如需递归搜索，请改用 `CTRL-T` 绑定。
- 如果当前命令行的词法单元是通配符模式，它会在
  通配符展开的路径列表上进行搜索（而不是像原生行为那样把所有结果插到命令行上）。
  由于展开是由 shell 执行的，结果数量存在上限。
- 唯一支持的配置变量是 `FZF_COMPLETION_OPTS` 与
  `FZF_EXPANSION_OPS`。
- 自定义补全函数所用的函数名为
  `fzf_complete`，它只接受 fzf 选项作为参数，也可以
  在不带任何重定向输入的情况下调用，从而只修改 fzf 选项、同时呈现原生补全结果。
  出于与其他 shell 的兼容性考虑，这里也提供了一个名为 `_fzf_complete` 的函数，
  它可以在 fzf 选项之后接受 ` -- $argv` 这样的命令行参数。

对于 fish 补全未覆盖的命令，更好的做法是先创建
常规的 fish 补全函数（这样 `TAB` 与
`Shift-TAB` 都能用），只在需要为特定命令修改
fzf 选项、或希望 `Shift-TAB` 给出不同结果时才创建 fzf 补全函数：

```fish
# 自定义 git 补全
function _fzf_complete_git
  # 为所有 git 补全显示当前分支的页首文本
  set -lx -- FZF_COMPLETION_OPTS --header="'"(git branch --show-current 2>/dev/null)"'"

  # 参数少于 3 个，或正在补全选项时，不做其他改动
  if not set -q argv[3]; or string match -q -- '-*' $argv[-1]
    fzf_complete
    return
  end

  # 检查子命令
  switch $argv[2]
    case checkout diff log show
      # 为子命令 checkout、diff、log、show 设置预览，并显示所有分支与提交
      begin
        git branch --all --format='%(refname:short)'
        git log --all --oneline --color=always
      end | fzf_complete --no-multi --ansi --accept-nth=1 --query=$argv[-1] --preview='git show --color=always {1}'

    case add rm mv
      # 仅为子命令 add、rm、mv 设置预览
      # fish 补全列表中的特殊字符会被转义，因此必须使用 r 标志。
      fzf_complete --preview="git diff --color=always {r1}"

    case '*'
      # 其他子命令不做改动
      fzf_complete
  end
end
```

与 bash、zsh 类似，在插入命令行之前可以对 fzf 的输出进行处理，
方法是定义一个名为 `_fzf_complete_COMMAND_post` 或
`_fzf_post_complete_COMMAND` 的函数：

```fish
function _fzf_complete_foo
  ls -sh --zero --color=always | fzf_complete --read0 --print0 --ansi --no-multi-line --header-lines=1
end

function _fzf_post_complete_foo
  while read -lz result
    string escape -n -- $result | string trim -l -c '\ ' | string split -m 1 -f 2 ' '
  end
end
```

### Nushell 的模糊补全

Nushell 中的模糊补全通过
[外部补全器](https://www.nushell.sh/cookbook/external_completers.html)
机制实现。与 bash、zsh 相比有一些差异：

- 需要 Nushell 0.116.1 或更新版本。补全运行在行编辑器
  线程上，以便 fzf 能够接管终端；因此当缺少 `**` 触发器时，
  先前配置的外部补全器（例如 Carapace）会被内联调用。
- 该脚本会在加载时包装当时配置的那个外部补全器，
  所以请在其他外部补全器配置之后再加载它。上文所述的 autoload 方式
  运行在 `config.nu` 之后，因此已经满足这一点。
- 在 Nushell >= 0.103.0 上，内置命令（如 `cd`、`ls`）不再调用外部补全器。
  `**<TAB>` 形式的模糊补全只适用于外部命令。
- 可以通过 `config.nu` 中的 `$env.FZF_COMPLETERS` 记录来定义自定义补全器。
  每一项都是一个闭包，接收前缀与命令 spans，返回
  候选字符串列表，或者返回一个 `{ candidates: [...], opts: [...] }` 记录以携带自定义 fzf 选项：
  ```nu
  $env.FZF_COMPLETERS = {
      pacman: {|prefix, spans|
          let sub = $spans | skip 1 | first
          let candidates = (if ($sub =~ "-[SF]") { ^pacman -Slq | lines
          } else if ($sub =~ "-[QR]") { ^pacman -Qq | lines
          } else { [] })
          { candidates: $candidates, opts: ["--preview", "pacman -Si {}"] }
      }
  }
  ```
  更多示例参见 [shell/completion-examples.nu](shell/completion-examples.nu)。
- 支持以下环境变量：
  `FZF_COMPLETION_TRIGGER`、`FZF_COMPLETION_OPTS`、
  `FZF_COMPLETION_PATH_OPTS`、`FZF_COMPLETION_DIR_OPTS`、
  `FZF_COMPLETION_DIR_COMMANDS`。

Vim 插件
----------

参见 [README-VIM.zh-CN.md](README-VIM.zh-CN.md)（中译）。

进阶主题
---------------

### 针对不同类型输入进行调优

由于 fzf 是一个通用的文本过滤器，它的算法被设计成
在任意类型的输入上都能"大体上"工作良好。不过，坦白说并不存在
真正放之四海而皆准的方案，你可能需要根据输入的类型
调整算法与部分设置。为了让这个过程更轻松，fzf 针对一些常见的输入类型提供了若干
"scheme"（方案）。

| 方案                | 说明                                                |
| :---               | :---                                                |
| `--scheme=default` | 为任意类型的输入而设计的通用方案                          |
| `--scheme=path`    | 适用于文件路径                                      |
| `--scheme=history` | 适用于命令历史，或任何时间顺序很重要的输入                 |

（详见 `fzf --man`）

### 性能

fzf 很快。在大多数使用场景下性能都不会成为问题。不过，
你可能需要了解那些会影响性能的选项。

- `--ansi` 会让 fzf 提取并解析输入中的 ANSI 颜色码，这会
  让初始扫描变慢。因此不建议把它加入
  你的 `$FZF_DEFAULT_OPTS`。
- `--nth` 会让 fzf 变慢，因为它必须对每一行做分词。
- 应优先使用纯字符串形式的 `--delimiter`，而不是正则表达式
  分隔符。
- `--with-nth` 会让 fzf 变慢，因为 fzf 必须对每一行做分词与重组。

### 执行外部程序

你可以设置键位绑定来启动外部进程而无需离开
fzf（`execute`、`execute-silent`）。

```bash
# 按 F1 用 less 打开文件而不离开 fzf
# 按 CTRL-Y 把该行复制到剪贴板并中止 fzf（需要 pbcopy）
fzf --bind 'f1:execute(less -f {}),ctrl-y:execute-silent(echo {} | pbcopy)+abort'
```

详见手册页的 *KEY/EVENT BINDINGS*（键位/事件绑定）一节。

### 转变为另一个进程

`become(...)` 与上文介绍的 `execute(...)`/`execute-silent(...)` 类似，
但不同之处在于：它不是执行命令后再返回 fzf，
而是让 fzf 转变为该命令所对应的新进程。

```sh
fzf --bind 'enter:become(vim {})'
```

与看上去等价的命令替换 `vim "$(fzf)"` 相比，这种做法
有以下几个优点：

* 当你用 <kbd>CTRL-C</kbd> 终止 fzf 时，Vim 不会打开一个空文件
* 当你在空结果上按 <kbd>ENTER</kbd> 时，Vim 不会打开一个空文件
* 即使多选结果中含有空白字符也能正确处理
  ```sh
  fzf --multi --bind 'enter:become(vim {+})'
  ```

公允地说，用 `fzf --print0 | xargs -0 -o vim` 代替 `vim "$(fzf)"`
也能解决上面提到的所有问题。尽管如此，`become(...)` 在不同场景下
仍然能提供额外的好处。

* 你可以设置多个绑定，用不同的方式处理结果，
  而无需任何包装脚本
  ```sh
  fzf --bind 'enter:become(vim {}),ctrl-e:become(emacs {})'
  ```
  * 以前你必须用 `--expect=ctrl-e`，然后检查 fzf 输出的第一行
* 你可以利用 fzf 的字段索引表达式
  轻松构造后续命令
  ```sh
  # 用 Vim 打开文件并跳到指定行
  git grep --line-number . |
      fzf --delimiter : --nth 3.. --bind 'enter:become(vim {1} +{2})'
  ```

### 重新加载候选列表

通过把 `reload` 动作绑定到某个按键或事件，你可以让 fzf 动态
重新加载候选列表。详见 https://github.com/junegunn/fzf/issues/1750 。

#### 1. 按 CTRL-R 刷新进程列表

```sh
ps -ef |
  fzf --bind 'ctrl-r:reload(ps -ef)' \
      --header 'Press CTRL-R to reload' --header-lines=1 \
      --height=50% --layout=reverse
```

#### 2. 按 CTRL-D 或 CTRL-F 切换数据源

```sh
FZF_DEFAULT_COMMAND='find . -type f' \
  fzf --bind 'ctrl-d:reload(find . -type d),ctrl-f:reload(eval "$FZF_DEFAULT_COMMAND")' \
      --height=50% --layout=reverse
```

#### 3. 交互式 ripgrep 集成

下面这个例子把 fzf 用作 ripgrep 的选择界面。我们把
`reload` 动作绑定到 `change` 事件上，这样你每次在 fzf 中输入时，ripgrep
进程都会用更新后的查询串（由占位符表达式 `{q}` 表示）重新启动。
另外请注意，我们使用了 `--disabled` 选项，这样 fzf
就不会再做二次过滤。

```sh
: | rg_prefix='rg --column --line-number --no-heading --color=always --smart-case' \
    fzf --bind 'start:reload:$rg_prefix ""' \
        --bind 'change:reload:$rg_prefix {q} || true' \
        --bind 'enter:become(vim {1} +{2})' \
        --ansi --disabled \
        --height=50% --layout=reverse
```

如果 ripgrep 没有找到任何匹配项，它会以非零退出码退出，
fzf 会就此向你发出警告。为了屏蔽该警告信息，我们在命令后面加上了
`|| true`，这样它总会以 0 退出。

更精巧的示例参见
["把 fzf 用作交互式 Ripgrep 启动器"](ADVANCED.zh-CN.md#把-fzf-用作交互式-ripgrep-启动器)。

### 预览窗口

设置 `--preview` 选项后，fzf 会自动以当前行作为参数启动一个外部进程，
并在分屏窗口中显示结果。
执行命令时使用你的 `$SHELL`，形式为 `$SHELL -c COMMAND`。
该窗口可以用鼠标或自定义键位绑定来滚动。

```bash
# {} 会被替换为焦点所在行的单引号包裹字符串
fzf --preview 'cat {}'
```

预览窗口支持 ANSI 颜色，因此你可以使用任何能对文件内容
做语法高亮的程序，例如
[Bat](https://github.com/sharkdp/bat) 或
[Highlight](https://gitlab.com/saalen/highlight)：

```bash
fzf --preview 'bat --color=always {}' --preview-window '~3'
```

你可以用 `--preview-window` 选项自定义预览窗口的大小、位置与边框，
用 `--color` 选项自定义它的前景色与背景色。例如，

```bash
fzf --height 40% --layout reverse --info inline --border \
    --preview 'file {}' --preview-window up,1,border-horizontal \
    --bind 'ctrl-/:change-preview-window(50%|hidden|)' \
    --color 'fg:#bbccdd,fg+:#ddeeff,bg:#334455,preview-bg:#223344,border:#778899'
```

完整的选项列表请查看手册页（`man fzf`）。

更多进阶示例可以在[这里](ADVANCED.zh-CN.md)找到。

> [!WARNING]
> 由于 fzf 是通用文本过滤器而非文件查找器，因此**把 `--preview` 选项
> 加入你的 `$FZF_DEFAULT_OPTS` 并不是个好主意**。
>
> ```sh
> # *********************
> # ** 千万不要这样做！ **
> # *********************
> export FZF_DEFAULT_OPTS='--preview "bat --style=numbers --color=always --line-range :500 {}"'
>
> # bat 只能处理文件列表这一种输入
> ps -ef | fzf
> seq 100 | fzf
> history | fzf
> ```

### 预览图片

fzf 可以通过以下协议之一在预览窗口中显示图片：

* [Kitty graphics protocol](https://sw.kovidgoyal.net/kitty/graphics-protocol/)
* [iTerm2 inline images protocol](https://iterm2.com/documentation-images.html)
* [Sixel](https://en.wikipedia.org/wiki/Sixel)

更多信息参见 [bin/fzf-preview.sh](bin/fzf-preview.sh) 脚本。

```sh
fzf --preview 'fzf-preview.sh {}'
```

提示
----

### 遵循 `.gitignore`

你可以使用 [fd](https://github.com/sharkdp/fd)、
[ripgrep](https://github.com/BurntSushi/ripgrep) 或
[the silver searcher](https://github.com/ggreer/the_silver_searcher)
来遍历文件系统，同时遵循 `.gitignore`。

```sh
# 把 fd 的输出喂给 fzf
fd --type f --strip-cwd-prefix | fzf

# 把 fd 设为 fzf 的默认数据源
export FZF_DEFAULT_COMMAND='fd --type f --strip-cwd-prefix'

# 现在不带管道的 fzf 会用 fd 命令来生成列表
fzf

# 让 CTRL-T 也使用同一个命令
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"
```

如果你希望该命令跟随符号链接，并且不排除
隐藏文件，请使用下面的命令：

```sh
export FZF_DEFAULT_COMMAND='fd --type f --strip-cwd-prefix --hidden --follow --exclude .git'
```

### Fish shell

与 bash、zsh 不同，fish 的 `CTRL-T` 键位绑定会
把命令行上的最后一个词法单元作为递归搜索的根目录。例如，
在下面这条命令行的末尾按下 `CTRL-T`

```sh
ls /var/
```

会列出 `/var/` 下的所有文件与目录。

当使用自定义的 `FZF_CTRL_T_COMMAND` 时，请使用未展开的 `$dir` 变量
来启用这一特性。当最后一个词法单元不是有效目录时，`$dir` 默认为 `.`。示例：

```sh
set -g FZF_CTRL_T_COMMAND "command find -L \$dir -type f 2> /dev/null | sed '1d; s#^\./##'"
```

### fzf 主题炼丹场

由 [Vitor Mello](https://github.com/vitormv) 创建的
[fzf 主题炼丹场](https://vitormv.github.io/fzf-themes/)是一个可以
交互式创建 fzf 主题的网页。

相关项目
----------------

https://github.com/junegunn/fzf/wiki/Related-projects

[许可证](LICENSE)
------------------

MIT License（MIT）

Copyright (c) 2013-2026 Junegunn Choi

周边商品
-----

在这里购买 fzf 的 T 恤、马克杯与贴纸：https://commitgoods.com/collections/fzf

赞助者 :heart:
----------------

我要感谢所有赞助本项目的朋友，是你们让我得以持续改进 fzf。

如果你也想赞助本项目，请访问 https://github.com/sponsors/junegunn。
