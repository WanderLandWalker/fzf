fzf 的 Vim 集成
===================

> **语言**：[English](README-VIM.md) | 简体中文（本文档）
>
> 本文档为 README-VIM.md 的简体中文译本。所有 Vim 命令、函数名、变量名、可取值与技术标识符均保持原文。

<!-- vim-markdown-toc GFM -->

* [安装](#安装)
* [概览](#概览)
* [`:FZF[!]`](#fzf)
    * [配置](#配置)
        * [示例](#示例)
            * [`g:fzf_colors` 说明](#gfzf_colors-说明)
* [`fzf#run`](#fzfrun)
* [`fzf#wrap`](#fzfwrap)
    * [`fzf#wrap` 支持的全局选项](#fzfwrap-支持的全局选项)
* [技巧](#技巧)
    * [在终端缓冲区中运行 fzf](#在终端缓冲区中运行-fzf)
    * [在 Vim 弹出窗口中启动 fzf](#在-vim-弹出窗口中启动-fzf)
    * [在 tmux/Zellij 弹出窗口中启动 fzf](#在-tmuxzellij-弹出窗口中启动-fzf)
    * [隐藏状态栏](#隐藏状态栏)
* [许可证](#许可证)

<!-- vim-markdown-toc -->

安装
------------

安装 fzf 之后，只需在 Vim 配置文件中把相应的目录加入
`&runtimepath`，即可在 Vim 中启用它。具体路径
可能因包管理器而异。

```vim
" 如果是通过 Homebrew 安装的
set rtp+=/usr/local/opt/fzf

" 如果是在 Apple Silicon 上通过 Homebrew 安装的
set rtp+=/opt/homebrew/opt/fzf

" 如果你把 fzf 克隆到了 ~/.fzf 目录
set rtp+=~/.fzf
```

如果你使用 [vim-plug](https://github.com/junegunn/vim-plug)，同样的写法可以
写成：

```vim
" 如果是通过 Homebrew 安装的
Plug '/usr/local/opt/fzf'

" 如果是在 Apple Silicon 上通过 Homebrew 安装的
Plug '/opt/homebrew/opt/fzf'

" 如果你把 fzf 克隆到了 ~/.fzf 目录
Plug '~/.fzf'
```

但如果你希望使用 GitHub 上最新的 Vim 插件文件，而不是软件包中
附带的那一份，请这样写：

```vim
Plug 'junegunn/fzf'
```

该 Vim 插件会直接使用系统上可用的 fzf 二进制文件。如果在
`$PATH` 中找不到 fzf，它会询问你是否需要为你下载最新的二进制文件。

为了确保你始终拥有最新版本的二进制文件，请像这样设置
更新后钩子：

```vim
Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
```

概览
-------

fzf 的 Vim 插件提供两个核心函数，以及构建于二者之上的 `:FZF` 命令 ——
一个基础的文件选择器命令。

1. **`fzf#run([spec dict])`**
    - 在 Vim 中按照给定的 spec（规格）启动 fzf
    - `:call fzf#run({'source': 'ls'})`
2. **`fzf#wrap([spec dict]) -> (dict)`**
    - 接受一个供 `fzf#run` 使用的 spec，并返回其扩展版本，
      其中附带了用于照顾全局偏好（`g:fzf_xxx`）的额外选项
        - `:echo fzf#wrap({'source': 'ls'})`
    - 我们通常会先把 spec 用 `fzf#wrap` *包装*一下，再传给 `fzf#run`
        - `:call fzf#run(fzf#wrap({'source': 'ls'}))`
3. **`:FZF [fzf_options string] [path string]`**
    - 基础的模糊文件选择器
    - 为不想写 VimScript 来实现自定义命令的人提供的参考实现
    - 如果你想要更多这类命令，请看看 [fzf.vim](https://github.com/junegunn/fzf.vim) 项目。

其中最重要的是 `fzf#run`，不过先讲 `:FZF` 命令会更容易理解全貌。

`:FZF[!]`
---------

```vim
" 在当前目录下查找文件
:FZF

" 在你的主目录下查找文件
:FZF ~

" 带上 fzf 命令行选项
:FZF --reverse --info=inline /tmp

" 带感叹号的版本以全屏模式启动 fzf
:FZF!
```

与 [ctrlp.vim](https://github.com/kien/ctrlp.vim) 类似，用回车键、
`CTRL-T`、`CTRL-X` 或 `CTRL-V`，可以分别在当前窗口中、
在新标签页中、在水平分屏中，或在垂直分屏中打开选中的文件。

注意，环境变量 `FZF_DEFAULT_COMMAND` 与
`FZF_DEFAULT_OPTS` 在此处同样适用。

### 配置

- `g:fzf_action`
    - 自定义用于以不同方式打开选中文件的额外键位绑定
- `g:fzf_layout`
    - 决定 fzf 窗口的大小与位置
- `g:fzf_colors`
    - 自定义 fzf 的颜色，使其与当前的配色方案相匹配
- `g:fzf_history_dir`
    - 启用历史记录功能

#### 示例

```vim
" 这是默认的额外键位绑定
let g:fzf_action = {
  \ 'ctrl-t': 'tab split',
  \ 'ctrl-x': 'split',
  \ 'ctrl-v': 'vsplit' }

" 一个动作也可以是一个用来处理选中行的函数引用
function! s:build_quickfix_list(lines)
  call setqflist(map(copy(a:lines), '{ "filename": v:val, "lnum": 1 }'))
  copen
  cc
endfunction

let g:fzf_action = {
  \ 'ctrl-q': function('s:build_quickfix_list'),
  \ 'ctrl-t': 'tab split',
  \ 'ctrl-x': 'split',
  \ 'ctrl-v': 'vsplit' }

" 默认的 fzf 布局
if exists('$TMUX') || exists('$ZELLIJ')
  " Vim 插件会尝试通过 --popup 选项在 tmux 或 Zellij 的 popup 中打开 fzf
  " （需要较新的 fzf 与 tmux/zellij），并使用下列参数：
  let g:fzf_layout = { 'popup': '90%,60%' }
else
  " 如果 --popup 选项不可用，它会在 Vim 内的一个弹出窗口中打开
  " （位于屏幕中央）
  let g:fzf_layout = { 'window': { 'width': 0.9, 'height': 0.6 } }
endif

" 下面是更多的布局示例：

" - 位于底部 40% 的 tmux 或 Zellij popup
let g:fzf_layout = { 'popup': 'bottom,40%' }

" - 位于顶部的 tmux 或 Zellij popup，尺寸不同
let g:fzf_layout = { 'popup': 'top,90%,40%' }

" - Vim 弹出窗口：位于当前窗口的中央（相对定位）
let g:fzf_layout = { 'window': { 'width': 0.9, 'height': 0.6, 'relative': v:true } }

" - Vim 弹出窗口：锚定在当前窗口的底部
let g:fzf_layout = { 'window': { 'width': 0.9, 'height': 0.6, 'relative': v:true, 'yoffset': 1.0 } }

" - Vim 分屏窗口：down / up / left / right
let g:fzf_layout = { 'down': '40%' }

" - 通过 Vim 命令打开的 Vim 窗口
let g:fzf_layout = { 'window': 'enew' }
let g:fzf_layout = { 'window': '-tabnew' }
let g:fzf_layout = { 'window': '10new' }

" 自定义 fzf 的颜色，使其与你的配色方案相匹配
" - fzf#wrap 会把这个变量翻译成一组 `--color` 选项
let g:fzf_colors =
\ { 'fg':      ['fg', 'Normal'],
  \ 'bg':      ['bg', 'Normal'],
  \ 'query':   ['fg', 'Normal'],
  \ 'hl':      ['fg', 'Comment'],
  \ 'fg+':     ['fg', 'CursorLine', 'CursorColumn', 'Normal'],
  \ 'bg+':     ['bg', 'CursorLine', 'CursorColumn'],
  \ 'hl+':     ['fg', 'Statement'],
  \ 'info':    ['fg', 'PreProc'],
  \ 'border':  ['fg', 'Ignore'],
  \ 'prompt':  ['fg', 'Conditional'],
  \ 'pointer': ['fg', 'Exception'],
  \ 'marker':  ['fg', 'Keyword'],
  \ 'spinner': ['fg', 'Label'],
  \ 'header':  ['fg', 'Comment'] }

" 启用按命令分别记录的历史
" - 历史文件会存放在指定的目录中
" - 设置之后，CTRL-N 与 CTRL-P 会被绑定到 'next-history' 与
"   'previous-history'，而不是 'down' 与 'up'。
let g:fzf_history_dir = '~/.local/share/fzf-history'
```

##### `g:fzf_colors` 说明

`g:fzf_colors` 是一个把 fzf 元素映射到颜色规格
列表的字典：

    element: [ component, group1 [, group2, ...] ]

- `element` 是要应用颜色的 fzf 元素：

  | 元素                        | 说明                                                   |
  | ---                         | ---                                                   |
  | `fg`  / `bg`  / `hl`        | 条目（前景色 / 背景色 / 高亮）                          |
  | `fg+` / `bg+` / `hl+`       | 当前条目（前景色 / 背景色 / 高亮）                      |
  | `preview-fg` / `preview-bg` | 预览窗口的文字与背景                                  |
  | `hl`  / `hl+`               | 高亮的子串（普通 / 当前）                              |
  | `gutter`                    | 左侧 gutter 的背景                                    |
  | `pointer`                   | 指向当前行的指针（`>`）                                |
  | `marker`                    | 多选标记（`>`）                                       |
  | `border`                    | 窗口周围的边框（`--border` 与 `--preview`）            |
  | `header`                    | 页首（`--header` 或 `--header-lines`）                 |
  | `info`                      | 信息行（匹配计数）                                     |
  | `spinner`                   | 流式输入指示器                                         |
  | `query`                     | 查询串                                                 |
  | `disabled`                  | 搜索被禁用时的查询串                                   |
  | `prompt`                    | 查询串之前的提示符（`> `）                             |
  | `pointer`                   | 指向当前行的指针（`>`）                                |

- `component` 指定在考察后面各个高亮组时，要从中提取哪个分量（`fg` / `bg`）

- `group1 [, group2, ...]` 是一组高亮组，会（按
  顺序）在其中查找匹配的颜色定义

例如，考虑下面这条规格：

```vim
  'prompt':  ['fg', 'Conditional', 'Comment'],
```

这意味着我们将为 **prompt（提示符）** 着色：
- 优先使用 `Conditional` 的 `fg` 属性（若存在），
- 否则使用 `Comment` 高亮组的 `fg` 属性（若存在），
- 否则回退到 **prompt** 的默认颜色设置。

你可以通过打印 `fzf#wrap()` 函数的返回值，
来检查根据该设置所生成的颜色选项：

```vim
:echo fzf#wrap()
```

`fzf#run`
---------

`fzf#run()` 函数是 Vim 集成的核心。它接受单个
字典参数，即 *spec*，并据此启动 fzf 进程。至少需要指定
`sink` 选项，用来告诉它如何处理被选中的条目。

```vim
call fzf#run({'sink': 'e'})
```

我们还没有指定 `source`，所以这等价于在命令行上
不带标准输入管道地启动 fzf；fzf 会遍历当前
目录下的文件系统来获取文件列表。（如果设置了
`$FZF_DEFAULT_COMMAND`，fzf 会改用该命令的输出。）当你选中一项时，
它会用 sink（即 `:e` 命令）打开它。如果你想在新标签页中打开，
可以传入 `:tabedit` 命令作为 sink。

```vim
call fzf#run({'sink': 'tabedit'})
```

你可以用任意 shell 命令作为 source 来生成列表。下面的
例子会列出由 git 管理的文件。它等价于在 shell 上运行
`git ls-files | fzf`。

```vim
call fzf#run({'source': 'git ls-files', 'sink': 'e'})
```

fzf 的选项可以通过 spec 字典中的 `options` 条目指定。

```vim
call fzf#run({'sink': 'tabedit', 'options': '--multi --reverse'})
```

如果你不希望 fzf 窗口占满整个屏幕，还可以传入布局选项。

```vim
" 允许使用 up / down / left / right / window
call fzf#run({'source': 'git ls-files', 'sink': 'e', 'left': '40%'})
call fzf#run({'source': 'git ls-files', 'sink': 'e', 'window': '30vnew'})
```

`source` 不一定非得是外部 shell 命令，你也可以传入一个 Vim
数组作为 source。在下面这个例子中，我们把配色
方案的名字作为 source，来实现一个配色方案选择器。

```vim
call fzf#run({'source': map(split(globpath(&rtp, 'colors/*.vim')),
            \               'fnamemodify(v:val, ":t:r")'),
            \ 'sink': 'colo', 'left': '25%'})
```

下表汇总了所有可用的选项。

| 选项名                     | 类型          | 说明                                                                    |
| -------------------------- | ------------- | ---------------------------------------------------------------------- |
| `source`                   | 字符串        | 用于生成 fzf 输入的外部命令（例如 `find .`）                            |
| `source`                   | 列表          | 作为 fzf 输入的 Vim 列表                                                |
| `sink`                     | 字符串        | 用于处理被选中项的 Vim 命令（例如 `e`、`tabe`）                         |
| `sink`                     | 函数引用      | 针对每个被选中项调用的函数                                              |
| `sinklist`（或 `sink*`）   | 函数引用      | 与 `sink` 类似，但一次性接受输出行的列表                                |
| `exit`                     | 函数引用      | 以 fzf 的退出状态码调用的函数（例如 0、1、2、130）                      |
| `options`                  | 字符串/列表   | 传给 fzf 的选项                                                         |
| `dir`                      | 字符串        | 工作目录                                                                |
| `up`/`down`/`left`/`right` | 数值/字符串   | （布局）窗口的位置与大小（例如 `20`、`50%`）                            |
| `popup`/`tmux`             | 字符串        | （布局）`--popup` 的选项（例如 `90%,70%`）                              |
| `window`（Vim 8 / Neovim） | 字符串        | （布局）用于打开 fzf 窗口的命令（例如 `vertical aboveleft 30new`）      |
| `window`（Vim 8 / Neovim） | 字典          | （布局）弹出窗口的设置（例如 `{'width': 0.9, 'height': 0.6}`）          |

`options` 条目既可以是字符串，也可以是列表。对于简单场景，
字符串就够用了，但更推荐使用列表类型以避免转义问题。

```vim
call fzf#run({'options': '--reverse --prompt "C:\\Program Files\\"'})
call fzf#run({'options': ['--reverse', '--prompt', 'C:\Program Files\']})
```

当 `window` 条目是字典时，fzf 会在弹出窗口中启动。此时
允许使用以下选项：

- 必填：
    - `width` [浮点数范围 [0 ~ 1]] 或 [整数范围 [8 ~ ]]
    - `height` [浮点数范围 [0 ~ 1]] 或 [整数范围 [4 ~ ]]
- 可选：
    - `yoffset` [浮点数，默认 0.5，范围 [0 ~ 1]]
    - `xoffset` [浮点数，默认 0.5，范围 [0 ~ 1]]
    - `relative` [布尔值，默认 v:false]
    - `border` [字符串，默认 `rounded`（在 Windows 上为 `sharp`）]：边框样式
        - `rounded` / `sharp` / `horizontal` / `vertical` / `top` / `bottom` / `left` / `right` / `no[ne]`

`fzf#wrap`
----------

我们已经看到，`:FZF` 命令的诸多方面都可以通过一组全局选项
变量来配置：打开文件的方式（`g:fzf_action`）、
窗口的位置与大小（`g:fzf_layout`）、调色板
（`g:fzf_colors`）等等。

那么，怎样才能让我们自定义的 `fzf#run` 调用也遵循这些变量呢？
只需在把 spec 字典传给 `fzf#run` 之前，先用 `fzf#wrap`
*"包装"* 一下即可。

- **`fzf#wrap([name string], [spec dict], [fullscreen bool]) -> (dict)`**
    - 所有参数都是可选的。通常我们只需要传入一个 spec 字典。
    - `name` 用于管理历史文件。如果未定义
      `g:fzf_history_dir`，它会被忽略。
    - `fullscreen` 可以是 `0` 或 `1`（默认为 0）。

`fzf#wrap` 接受一个 spec，并返回它的扩展版本（同样
是一个字典），其中附带了用于照顾全局偏好的额外选项。你可以
像这样检查它的返回值：

```vim
echo fzf#wrap({'source': 'ls'})
```

在 *"包装"* 好 spec 之后，我们把它传给 `fzf#run`。

```vim
call fzf#run(fzf#wrap({'source': 'ls'}))
```

现在它支持 `CTRL-T`、`CTRL-V` 与 `CTRL-X` 键位绑定（可通过
`g:fzf_action` 配置），并且会按照 `g:fzf_layout`
的设置来打开 fzf 窗口。

为了方便使用，我们来定义 `LS` 命令。

```vim
command! LS call fzf#run(fzf#wrap({'source': 'ls'}))
```

输入 `:LS` 看看效果。

我们希望让 `:LS!`（带感叹号的版本）以全屏方式打开 fzf，就像
`:FZF!` 那样。请在命令定义中加上 `-bang`，并用 `<bang>` 的值来设置
`fzf#wrap` 的最后一个 `fullscreen` 参数（参见 `:help <bang>`）。

```vim
" 在 :LS! 时，<bang> 求值为 '!'，于是 '!0' 变成 1
command! -bang LS call fzf#run(fzf#wrap({'source': 'ls'}, <bang>0))
```

如果我们的 `:LS` 命令还能接受一个目录参数，
使得 `:LS /tmp` 这样的用法成为可能，那就会更实用。

```vim
command! -bang -complete=dir -nargs=? LS
    \ call fzf#run(fzf#wrap({'source': 'ls', 'dir': <q-args>}, <bang>0))
```

最后，如果你启用了 `g:fzf_history_dir`，你也许想给我们的命令
指定一个唯一的名字，并把它作为第一个参数传给 `fzf#wrap`。

```vim
" 该命令的查询历史会以 'ls' 为名存放在 g:fzf_history_dir 中。
" 如果未定义 g:fzf_history_dir，这个名字会被忽略。
command! -bang -complete=dir -nargs=? LS
    \ call fzf#run(fzf#wrap('ls', {'source': 'ls', 'dir': <q-args>}, <bang>0))
```

### `fzf#wrap` 支持的全局选项

- `g:fzf_layout`
- `g:fzf_action`
    - **仅在没有提供自定义 `sink`（或 `sinklist`）时生效**
        - 通常来说，带有自定义 sink 意味着每个条目并不是普通的文件路径
          （例如配色方案名），因此我们不能盲目套用同样的策略
          （即 `tabedit some-color-scheme` 是没有意义的）
- `g:fzf_colors`
- `g:fzf_history_dir`

技巧
----

### 在终端缓冲区中运行 fzf

当 fzf 被配置为在 Vim 或 Neovim 内的终端缓冲区中启动时，你
可能会发现默认的 ANSI 颜色不太一样。这种情况下，请在常规 Vim 中
使用 `g:terminal_ansi_colors`、在 Neovim 中使用 `g:terminal_color_x`
来配置颜色。

```vim
" 对应 seoul256 配色方案的终端颜色
if has('nvim')
  let g:terminal_color_0 = '#4e4e4e'
  let g:terminal_color_1 = '#d68787'
  let g:terminal_color_2 = '#5f865f'
  let g:terminal_color_3 = '#d8af5f'
  let g:terminal_color_4 = '#85add4'
  let g:terminal_color_5 = '#d7afaf'
  let g:terminal_color_6 = '#87afaf'
  let g:terminal_color_7 = '#d0d0d0'
  let g:terminal_color_8 = '#626262'
  let g:terminal_color_9 = '#d75f87'
  let g:terminal_color_10 = '#87af87'
  let g:terminal_color_11 = '#ffd787'
  let g:terminal_color_12 = '#add4fb'
  let g:terminal_color_13 = '#ffafaf'
  let g:terminal_color_14 = '#87d7d7'
  let g:terminal_color_15 = '#e4e4e4'
else
  let g:terminal_ansi_colors = [
    \ '#4e4e4e', '#d68787', '#5f865f', '#d8af5f',
    \ '#85add4', '#d7afaf', '#87afaf', '#d0d0d0',
    \ '#626262', '#d75f87', '#87af87', '#ffd787',
    \ '#add4fb', '#ffafaf', '#87d7d7', '#e4e4e4'
  \ ]
endif
```

### 在 Vim 弹出窗口中启动 fzf

你可以通过在 `g:fzf_layout` 中设置 `window` 键，
让 fzf 在 Vim 的弹出窗口中启动。

```vim
" 必填：
" - width [浮点数范围 [0 ~ 1]] 或 [整数范围 [8 ~ ]]
" - height [浮点数范围 [0 ~ 1]] 或 [整数范围 [4 ~ ]]
"
" 可选：
" - xoffset [浮点数，默认 0.5，范围 [0 ~ 1]]
" - yoffset [浮点数，默认 0.5，范围 [0 ~ 1]]
" - relative [布尔值，默认 v:false]
" - border [字符串，默认 'rounded']：边框样式
"   - 'rounded' / 'sharp' / 'horizontal' / 'vertical' / 'top' / 'bottom' / 'left' / 'right'
let g:fzf_layout = { 'window': { 'width': 0.9, 'height': 0.6 } }
```

### 在 tmux/Zellij 弹出窗口中启动 fzf

除了在 Vim 内部的窗口中打开，fzf 也可以在复用器（multiplexer）的 popup 中启动，
方法是把 `--popup` 选项的值放进 `popup` 键。`tmux` 同样被接受，
正如 `--tmux` 是 `--popup` 的别名一样。

该布局适用于 tmux 3.3 及以上，或 Zellij 0.44 及以上搭配 fzf
0.71.0 及以上。在 tmux 3.7 及以上搭配 fzf 0.74.0 及以上的环境中，
以及在 Zellij 中，它都是默认方式 —— 此时窗格是非模态的：Vim 会保持启动
fzf 时所在的窗口可见，你可以在 fzf 打开期间切换回去。低于上述版本时，
tmux 给出的是一个无法离开的 popup，因此那里默认使用 Vim 内部的窗口。
在 tmux 上，显式指定 `--border` 样式同样会得到一个模态 popup 而非浮动窗格，
因为 tmux 浮动窗格的原生边框无法去除。如果你希望保留浮动窗格及其原生边框，
就不要加 `--border`。Zellij 无论哪种方式都会保留浮动窗格，并在 fzf 自己绘制
边框时隐藏其原生边框。请自行设置 `g:fzf_layout` 来选择其中一种。

```vim
if exists('$TMUX') || exists('$ZELLIJ')
  " 可用选项参见 `man fzf` 中的 `--popup` 选项
  " [center|top|bottom|left|right][,SIZE[%]][,SIZE[%]]
  let g:fzf_layout = { 'popup': '90%,70%' }
else
  " 当不在复用器中时，配置 Vim 的弹出窗口
  let g:fzf_layout = { 'window': { 'width': 0.9, 'height': 0.7 } }
endif
```

### 隐藏状态栏

当 fzf 在终端缓冲区中启动时，该缓冲区的 file type 会被设为
`fzf`。因此你可以设置 `FileType fzf` 自动命令来定制该窗口的
各项设置。这只适用于在 Vim 内部打开的布局，而不适用于默认情况下
tmux 或 Zellij 的窗格 —— 那并不是一个缓冲区。

例如，如果你在屏幕底部打开 fzf（例如 `{'down':
'40%'}`），你可能会希望暂时禁用状态栏，让画面更清爽。

```vim
let g:fzf_layout = { 'down': '30%' }
autocmd! FileType fzf
autocmd  FileType fzf set laststatus=0 noshowmode noruler
  \| autocmd BufLeave <buffer> set laststatus=2 showmode ruler
```

[许可证](LICENSE)
------------------

MIT License（MIT）

Copyright (c) 2013-2026 Junegunn Choi
