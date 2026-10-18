fzf 进阶示例
======================

> **语言**：[English](ADVANCED.md) | 简体中文（本文档）
>
> 本文档为 ADVANCED.md 的简体中文译本。所有命令、选项、Vim 变量名与脚本代码均保持原文。

* *最后更新：2025/02/02*
* *需要 fzf 0.59.0 或更高版本*

---

<!-- vim-markdown-toc GFM -->

* [引言](#引言)
* [显示模式](#显示模式)
    * [`--height`](#--height)
    * [`--tmux`](#--tmux)
* [动态重新加载列表](#动态重新加载列表)
    * [通过按 CTRL-R 更新进程列表](#通过按-ctrl-r-更新进程列表)
    * [在多个数据源之间切换](#在多个数据源之间切换)
    * [用单个键位绑定切换](#用单个键位绑定切换)
* [与 Ripgrep 集成](#与-ripgrep-集成)
    * [把 fzf 用作二级过滤器](#把-fzf-用作二级过滤器)
    * [把 fzf 用作交互式 Ripgrep 启动器](#把-fzf-用作交互式-ripgrep-启动器)
    * [切换到仅使用 fzf 的搜索模式](#切换到仅使用-fzf-的搜索模式)
    * [在 Ripgrep 模式与 fzf 模式之间切换](#在-ripgrep-模式与-fzf-模式之间切换)
    * [用单个键位绑定在 Ripgrep 模式与 fzf 模式之间切换](#用单个键位绑定在-ripgrep-模式与-fzf-模式之间切换)
    * [同时控制 Ripgrep 搜索与 fzf 搜索](#同时控制-ripgrep-搜索与-fzf-搜索)
* [追踪日志](#追踪日志)
* [Git 对象的键位绑定](#git-对象的键位绑定)
    * [`git status` 中列出的文件](#git-status-中列出的文件)
    * [分支](#分支)
    * [提交哈希](#提交哈希)
* [配色主题](#配色主题)
    * [fzf 主题炼丹场](#fzf-主题炼丹场)
    * [从 Vim 配色方案生成 fzf 配色主题](#从-vim-配色方案生成-fzf-配色主题)

<!-- vim-markdown-toc -->

引言
------------

fzf 是一个交互式的 [Unix 过滤器][filter]程序，其设计目的是
与其他 Unix 工具配合使用。它从标准输入读取条目列表，
让你从中筛选出一个子集，再把选中的条目写到
标准输出。你可以把它理解为 *grep* 的交互式版本，
而且即使你还不了解它的任何选项，它就已经很好用了。

```sh
# 1. ps:   把进程列表喂给 fzf
# 2. fzf:  用模糊匹配算法交互式地选中一个进程
# 3. awk:  从选中的那一行取出 PID
# 3. kill: 用该 PID 杀掉进程
ps -ef | fzf | awk '{print $2}' | xargs kill -9
```

[filter]: https://en.wikipedia.org/wiki/Filter_(software)

上面这个例子简明地概括了 fzf 的核心概念，不过一旦你
掌握了它丰富的各种特性，就能用它构建出复杂得多的交互式工作流。

- 完整的选项与特性列表请运行 `man fzf`
- 最新的新增内容请参阅 [CHANGELOG.md](CHANGELOG.md)

本文将通过一些示例带你熟悉 fzf 的进阶特性。

显示模式
-------------

### `--height`

fzf 默认以全屏模式打开，但这并不总是符合你的需要。
使用 fzf 时，你往往希望仍然能看到终端中当前的上下文。
`--height` 这个选项用于以非全屏模式在光标下方打开 fzf，
这样你依然可以看到它上面此前的命令及其输出。

```sh
fzf --height=40%
```

![image](https://user-images.githubusercontent.com/700826/113379893-c184c680-93b5-11eb-9676-c7c0a2f01748.png)

你可能还想试试其他布局选项，例如
`--layout=reverse`、`--info=inline`、`--border`、`--margin` 等。

```sh
fzf --height=40% --layout=reverse
fzf --height=40% --layout=reverse --info=inline
fzf --height=40% --layout=reverse --info=inline --border
fzf --height=40% --layout=reverse --info=inline --border --margin=1
fzf --height=40% --layout=reverse --info=inline --border --margin=1 --padding=1
```

![image](https://user-images.githubusercontent.com/700826/113379932-dfeac200-93b5-11eb-9e28-df1a2ee71f90.png)

*（完整的选项列表请查阅手册页）*

但你肯定不想每次使用 fzf 都重复输入 `--height=40% --layout=reverse
--info=inline --border --margin=1 --padding=1`。你可以写一个包装脚本或 shell 别名，
不过还有一个更省事的办法。像这样定义 `$FZF_DEFAULT_OPTS` 即可：

```sh
export FZF_DEFAULT_OPTS="--height=40% --layout=reverse --info=inline --border --margin=1 --padding=1"
```

### `--tmux`

（需要 tmux 3.3 或更高版本）

如果你在用 tmux，可以通过 `--tmux` 选项在 tmux 的 popup 中打开 fzf。

```sh
# 在屏幕中央打开 fzf，popup 的宽高各占 70%
fzf --tmux 70%
```

![image](https://github.com/junegunn/fzf/assets/700826/9c365405-c700-49b2-8985-60d822ed4cff)

如果你不在 tmux 中，`--tmux` 选项会被静默忽略。因此，如果你想避免
以全屏方式打开 fzf，可以同时指定 `--height` 与 `--tmux`。

```sh
# 后指定的 --tmux 在 tmux 中优先于 --height 生效。
# 如果你不在 tmux 中，--tmux 被忽略，于是改用 --height。
fzf  --height 70% --tmux 70%
```

你还可以按下面的格式指定 popup 窗口的位置、宽度与高度：

* `[center|top|bottom|left|right][,SIZE[%]][,SIZE[%][,border-native]]`

```sh
# 宽 100%，高 60%
fzf --tmux 100%,60% --border horizontal
```

![image](https://github.com/junegunn/fzf/assets/700826/f80d3514-d69f-42f2-a8de-a392a562bfcf)

```sh
# 靠右（宽 50%）
fzf --tmux right
```

![image](https://github.com/junegunn/fzf/assets/700826/4033ade4-7efa-421b-a3fb-a430d197098a)

```sh
# 靠左（宽 40%，高 70%）
fzf --tmux left,40%,70%
```

![image](https://github.com/junegunn/fzf/assets/700826/efe43881-2bf0-49ea-ab2e-1377f778cd52)

> [!TIP]
> 你可能还想看看我的那些支持这种 popup
> 窗口布局的 tmux 插件。
>
> - https://github.com/junegunn/tmux-fzf-url
> - https://github.com/junegunn/tmux-fzf-maccy

动态重新加载列表
-----------------------------

fzf 可以通过 `reload` 绑定，使用任意程序来动态更新候选列表
（`reload` 的设计文档见
[这里][reload]）。

[reload]: https://github.com/junegunn/fzf/issues/1750

### 通过按 CTRL-R 更新进程列表

这个例子展示了如何设置一个绑定，以便在不重启 fzf 的情况下
动态更新列表。

```sh
(date; ps -ef) |
  fzf --bind='ctrl-r:reload(date; ps -ef)' \
      --header=$'Press CTRL-R to reload\n\n' --header-lines=2 \
      --preview='echo {}' --preview-window=down,3,wrap \
      --layout=reverse --height=80% | awk '{print $2}' | xargs kill -9
```

![image](https://user-images.githubusercontent.com/700826/113465047-200c7c00-946c-11eb-918c-268f37a900c8.png)

- 初始命令是 `(date; ps -ef)`。它会打印当前日期
  与时间，以及进程列表。
- 通过 `--header` 选项，你可以把任意信息显示为固定的页首。
- 为了禁止选中前两行（`date` 和 `ps` 的表头），我们使用了
  `--header-lines=2` 选项。
- `--bind='ctrl-r:reload(date; ps -ef)'` 把 CTRL-R 绑定到 `reload` 动作，
  该动作会运行 `date; ps -ef`，于是我们可以通过按 CTRL-R
  来更新进程列表。
- 我们用了简单的 `echo {}` 预览选项，这样即使某一行太长，
  也能在下方的预览窗口中看到它的完整内容。

### 在多个数据源之间切换

你并不局限于只用一条 reload 绑定。设置多条绑定，
就可以在不同的数据源之间切换。

```sh
find * | fzf --prompt 'All> ' \
             --header 'CTRL-D: Directories / CTRL-F: Files' \
             --bind 'ctrl-d:change-prompt(Directories> )+reload(find * -type d)' \
             --bind 'ctrl-f:change-prompt(Files> )+reload(find * -type f)'
```

![image](https://user-images.githubusercontent.com/700826/113465073-4af6d000-946c-11eb-858f-2372c0955f67.png)

![image](https://user-images.githubusercontent.com/700826/113465072-46321c00-946c-11eb-9b6f-cda3951df579.png)

### 用单个键位绑定切换

上面的例子用了两个不同的键位绑定来在两种模式间切换，
但我们能不能只用一个键位绑定呢？

要让一个键位绑定在每次按下时表现出不同的行为，我们需要：

1. 一种保存当前状态的方法。也就是"我们当前处于哪种模式？"
2. 以及一种根据状态动态执行不同动作的方法。

下面的例子展示了如何 1. 把当前模式记录在 prompt 字符串中，2. 并利用这个信息
（`$FZF_PROMPT`）通过 `transform` 动作决定要执行哪些动作。

```sh
fd --type file |
  fzf --prompt 'Files> ' \
      --header 'CTRL-T: Switch between Files/Directories' \
      --bind 'ctrl-t:transform:[[ ! $FZF_PROMPT =~ Files ]] &&
              echo "change-prompt(Files> )+reload(fd --type file)" ||
              echo "change-prompt(Directories> )+reload(fd --type directory)"' \
      --preview '[[ $FZF_PROMPT =~ Files ]] && bat --color=always {} || tree -C {}'
```

与 Ripgrep 集成
-------------------

### 把 fzf 用作二级过滤器

* 需要 [bat][bat]
* 需要 [Ripgrep][rg]

[bat]: https://github.com/sharkdp/bat
[rg]: https://github.com/BurntSushi/ripgrep

fzf 对列表做过滤的速度相当快，你几乎不需要考虑它的性能。
但对于在许多大文件中搜索文本内容这件事，它并非合适的工具；那种情况下
你确实应该使用 [Ripgrep][rg] 这类工具。

在接下来的例子中，Ripgrep 是主过滤器，负责在文件中搜索给定的
文本，而 fzf 被用作二级模糊过滤器，为这个工作流增加交互性。
我们还用 [bat][bat] 在预览窗口中显示匹配到的那一行。

这是一段 bash 脚本，在其他不兼容的 shell 上无法按预期运行。
为了规避兼容性问题，我们把这段代码保存成一个名为 `rfv` 的脚本文件。

```bash
#!/usr/bin/env bash

# 1. 用 Ripgrep 在文件中搜索文本
# 2. 用 fzf 交互式地缩小范围
# 3. 在 Vim 中打开该文件
rg --color=always --line-number --no-heading --smart-case "${*:-}" |
  fzf --ansi \
      --color "hl:-1:underline,hl+:-1:underline:reverse" \
      --delimiter : \
      --preview 'bat --color=always {1} --highlight-line {2}' \
      --preview-window 'up,60%,border-bottom,+{2}+3/3,~3' \
      --bind 'enter:become(vim {1} +{2})'
```

然后带一个初始查询串来运行它。

```sh
# 让脚本可执行
chmod +x rfv

# 以初始查询 "algo" 运行
./rfv algo
```

> Ripgrep 会执行初次搜索，列出所有包含
`algo` 的行。然后我们再用 fzf 进一步缩小范围。

![image](https://user-images.githubusercontent.com/700826/113683873-a42a6200-96ff-11eb-9666-26ce4091b0e4.png)

我知道这段代码信息量很大，我们来逐段拆解。

- Ripgrep 按下面的格式打印匹配行
  ```
  man/man1/fzf.1:54:.BI "--algo=" TYPE
  man/man1/fzf.1:55:Fuzzy matching algorithm (default: v2)
  man/man1/fzf.1:58:.BR v2 "     Optimal scoring algorithm (quality)"
  src/pattern_test.go:7:  "github.com/junegunn/fzf/src/algo"
  ```
  以 `:` 分隔的第一个词元是文件路径，第二个词元是
  匹配行的行号。它们分别对应预览命令中的 `{1}`
  与 `{2}`。
    - `--preview 'bat --color=always {1} --highlight-line {2}'`
- 由于我们是用 `--color=always` 选项运行 `rg` 的，所以应当通过设置
  `--ansi` 告诉 fzf 去解析输入中的 ANSI 颜色码。
- 我们使用 `--color` 选项自定义 fzf 给各类文本元素着色的方式。
  `-1` 表示让 fzf 保留输入中的原始颜色。可用的颜色选项参见 `man fzf`。
- `--preview-window` 选项的值由以 `,`
  分隔的 5 个部分组成
    1. `up` —— 预览窗口的位置
    1. `60%` —— 预览窗口的大小
    1. `border-bottom` —— 只在底部绘制预览窗口边框
    1. `+{2}+3/3` —— 预览内容的滚动偏移量
    1. `~3` —— 固定的页首行数
- 我们来拆解后两个。我们希望预览窗口能以某个滚动偏移量显示 bat 的输出，
  使得匹配行位于预览窗口中央附近。
    - `+{2}` —— 基础偏移量取自第二个词元
    - `+3` —— 我们在基础偏移上再加 3 行，用来补偿
      `bat` 输出的页首部分
        - ```
          ───────┬──────────────────────────────────────────────────────────
                 │ File: CHANGELOG.md
          ───────┼──────────────────────────────────────────────────────────
             1   │ CHANGELOG
             2   │ =========
             3   │
             4   │ 0.26.0
             5   │ ------
          ```
    - `/3` 会微调偏移量，使匹配行显示在窗口中三分之一的位置
    - `~3` 让顶部三行成为固定页首，这样无论滚动偏移量是多少，它们始终可见
- 我们没有用 shell 脚本来处理 fzf 的最终输出，而是使用了
  [fzf 0.38.0][0.38.0] 中新增的 `become(...)` 动作，让 fzf 转变为
  一个新进程，用 `vim` 打开该文件（`vim {1}`）并把光标移到对应行（`+{2}`）。

[0.38.0]: https://github.com/junegunn/fzf/blob/master/CHANGELOG.md#0380

### 把 fzf 用作交互式 Ripgrep 启动器

我们已经了解到可以把 `reload` 动作绑定到某个按键（例如
`--bind=ctrl-r:execute(ps -ef)`）。在接下来的例子中，我们要**把
`reload` 动作绑定到 `change` 事件**上，这样每当用户*修改* fzf 上的
查询串时，就会触发 `reload` 动作。

下面是上述 `rfv` 脚本的一个变体。每当用户更新 fzf 上的查询串，
fzf 都会重启 Ripgrep。搜索与过滤完全由 Ripgrep 完成，
fzf 只是提供交互界面。因此我们牺牲了“模糊”能力，
但在较大的项目上性能会更好，而且随着你缩小结果范围，还能释放内存。

```bash
#!/usr/bin/env bash

# 1. 用 Ripgrep 在文件中搜索文本
# 2. 用 reload 动作交互式地重启 Ripgrep
# 3. 在 Vim 中打开该文件
RG_PREFIX="rg --column --line-number --no-heading --color=always --smart-case "
INITIAL_QUERY="${*:-}"
fzf --ansi --disabled --query "$INITIAL_QUERY" \
    --bind "start:reload:$RG_PREFIX {q} || true" \
    --bind "change:reload:sleep 0.1; $RG_PREFIX {q} || true" \
    --delimiter : \
    --preview 'bat --color=always {1} --highlight-line {2}' \
    --preview-window 'up,60%,border-bottom,+{2}+3/3,~3' \
    --bind 'enter:become(vim {1} +{2})'
```

![image](https://user-images.githubusercontent.com/700826/113684212-f9ff0a00-96ff-11eb-8737-7bb571d320cc.png)

- 我们没有采用通常的 `rg ... | fzf` 形式来启动 fzf，而是通过
  `start:reload` 绑定立即启动初次 Ripgrep 进程，
  以保证代码风格一致。
- 过滤不再是 fzf 的职责，因此使用了 `--disabled`。
- reload 命令中的 `{q}` 会求值为 fzf 提示符上的查询串。
- reload 命令中的 `sleep 0.1` 用于"去抖动"（debouncing）。这个微小的延迟
  能减少我们输入查询串过程中产生的中间 Ripgrep 进程数量。

### 切换到仅使用 fzf 的搜索模式

在上一个例子中，由于我们把搜索功能完全交给了 Ripgrep，
因此失去了模糊匹配的能力。但我们可以通过把 `reload` 动作
从 `change` 事件上*"解绑"*（unbind），动态切换到仅使用 fzf 的搜索模式。

```sh
#!/usr/bin/env bash

# 用 Ripgrep 与 fzf 做两阶段过滤
#
# 1. 用 Ripgrep 在文件中搜索文本
# 2. 用 reload 动作交互式地重启 Ripgrep
#    * 按 alt-enter 切换到仅使用 fzf 的过滤模式
# 3. 在 Vim 中打开该文件
RG_PREFIX="rg --column --line-number --no-heading --color=always --smart-case "
INITIAL_QUERY="${*:-}"
fzf --ansi --disabled --query "$INITIAL_QUERY" \
    --bind "start:reload:$RG_PREFIX {q}" \
    --bind "change:reload:sleep 0.1; $RG_PREFIX {q} || true" \
    --bind "alt-enter:unbind(change,alt-enter)+change-prompt(2. fzf> )+enable-search+clear-query" \
    --color "hl:-1:underline,hl+:-1:underline:reverse" \
    --prompt '1. ripgrep> ' \
    --delimiter : \
    --preview 'bat --color=always {1} --highlight-line {2}' \
    --preview-window 'up,60%,border-bottom,+{2}+3/3,~3' \
    --bind 'enter:become(vim {1} +{2})'
```

* 第 1 阶段：用 Ripgrep 过滤
![image](https://user-images.githubusercontent.com/700826/119213880-735e8a80-bafd-11eb-8493-123e4be24fbc.png)
* 第 2 阶段：用 fzf 过滤
![image](https://user-images.githubusercontent.com/700826/119213887-7e191f80-bafd-11eb-98c9-71a1af9d7aab.png)

- 我们加上了 `--prompt` 选项，用来表明 fzf 起初运行在
  "Ripgrep 启动器模式"。
- 我们添加了 `alt-enter` 绑定，它会
    1. 解绑 `change` 事件，于是按键时不再重启 Ripgrep
    2. 把提示符改为 `2. fzf>`
    3. 启用 fzf 的搜索功能
    4. 清空用于启动 Ripgrep 进程的当前查询串
    5. 并解绑 `alt-enter` 自身，因为这只是一次性事件
- 我们恢复了 `--color` 选项，用于自定义第二阶段中匹配片段的显示方式

### 在 Ripgrep 模式与 fzf 模式之间切换

[fzf 0.30.0][0.30.0] 新增了 `rebind` 动作，于是我们可以把先前
通过 `unbind` "解绑"掉的绑定再"重新绑定"回来。

这是上一个例子的改进版本，允许我们通过 CTRL-R 与
CTRL-F 在 Ripgrep 启动器模式与仅使用 fzf 的过滤模式之间来回切换。

```sh
#!/usr/bin/env bash

# 在 Ripgrep 启动器模式（CTRL-R）与 fzf 过滤模式（CTRL-F）之间切换
rm -f /tmp/rg-fzf-{r,f}
RG_PREFIX="rg --column --line-number --no-heading --color=always --smart-case "
INITIAL_QUERY="${*:-}"
fzf --ansi --disabled --query "$INITIAL_QUERY" \
    --bind "start:reload($RG_PREFIX {q})+unbind(ctrl-r)" \
    --bind "change:reload:sleep 0.1; $RG_PREFIX {q} || true" \
    --bind "ctrl-f:unbind(change,ctrl-f)+change-prompt(2. fzf> )+enable-search+rebind(ctrl-r)+transform-query(echo {q} > /tmp/rg-fzf-r; cat /tmp/rg-fzf-f)" \
    --bind "ctrl-r:unbind(ctrl-r)+change-prompt(1. ripgrep> )+disable-search+reload($RG_PREFIX {q} || true)+rebind(change,ctrl-f)+transform-query(echo {q} > /tmp/rg-fzf-f; cat /tmp/rg-fzf-r)" \
    --color "hl:-1:underline,hl+:-1:underline:reverse" \
    --prompt '1. ripgrep> ' \
    --delimiter : \
    --header '╱ CTRL-R (ripgrep mode) ╱ CTRL-F (fzf mode) ╱' \
    --preview 'bat --color=always {1} --highlight-line {2}' \
    --preview-window 'up,60%,border-bottom,+{2}+3/3,~3' \
    --bind 'enter:become(vim {1} +{2})'
```

- 为了在切换模式时恢复查询串，我们把当前查询
  保存在 `/tmp/rg-fzf-{r,f}` 文件中，并通过 [fzf 0.36.0][0.36.0] 新增的
  `transform-query` 动作来恢复查询串。
- 另外注意，我们在 `start` 事件（fzf 启动时触发一次）
  上解绑了 `ctrl-r` 绑定。

[0.30.0]: https://github.com/junegunn/fzf/blob/master/CHANGELOG.md#0300
[0.36.0]: https://github.com/junegunn/fzf/blob/master/CHANGELOG.md#0360

### 用单个键位绑定在 Ripgrep 模式与 fzf 模式之间切换

与前面的版本不同，我们只用一个热键来在 ripgrep 与 fzf 模式之间切换。
这是通过把 `$FZF_PROMPT` 当作 `transform` 动作中的状态来实现的，
该特性由 [fzf 0.45.0][0.45.0] 引入。关于这个特性的更详细说明
可以参见前面的一节 —— [用单个键位绑定切换](#用单个键位绑定切换)。

[0.45.0]: https://github.com/junegunn/fzf/blob/master/CHANGELOG.md#0450

使用 `transform` 动作时，占位符（`\{q}`）应当做转义处理，
以避免被立即求值。

```sh
#!/usr/bin/env bash

# 在 Ripgrep 模式与 fzf 过滤模式之间切换（CTRL-T）
rm -f /tmp/rg-fzf-{r,f}
RG_PREFIX="rg --column --line-number --no-heading --color=always --smart-case "
INITIAL_QUERY="${*:-}"
fzf --ansi --disabled --query "$INITIAL_QUERY" \
    --bind "start:reload:$RG_PREFIX {q}" \
    --bind "change:reload:sleep 0.1; $RG_PREFIX {q} || true" \
    --bind 'ctrl-t:transform:[[ ! $FZF_PROMPT =~ ripgrep ]] &&
      echo "rebind(change)+change-prompt(1. ripgrep> )+disable-search+transform-query:echo \{q} > /tmp/rg-fzf-f; cat /tmp/rg-fzf-r" ||
      echo "unbind(change)+change-prompt(2. fzf> )+enable-search+transform-query:echo \{q} > /tmp/rg-fzf-r; cat /tmp/rg-fzf-f"' \
    --color "hl:-1:underline,hl+:-1:underline:reverse" \
    --prompt '1. ripgrep> ' \
    --delimiter : \
    --header 'CTRL-T: Switch between ripgrep/fzf' \
    --preview 'bat --color=always {1} --highlight-line {2}' \
    --preview-window 'up,60%,border-bottom,+{2}+3/3,~3' \
    --bind 'enter:become(vim {1} +{2})'
```

### 同时控制 Ripgrep 搜索与 fzf 搜索

`search` 与 `transform-search` 动作允许你用任意查询串触发一次 fzf 搜索。
这让 fzf 不必严格依赖提示符上的输入，从而支持自定义搜索语法。

在下面的例子中，先用 `transform` 动作有条件地触发
针对 ripgrep 的 `reload`，随后再触发针对 fzf 的 `search`。查询串的
第一个词用于启动 Ripgrep 进程来生成初始结果，
而查询串的其余部分则交给 fzf 做二级过滤。

```sh
#!/usr/bin/env bash

export TEMP=$(mktemp -u)
trap 'rm -f "$TEMP"' EXIT

INITIAL_QUERY="${*:-}"
TRANSFORMER='
  rg_pat={q:1}      # 第一个词传给 ripgrep
  fzf_pat={q:2..}   # 其余的传给 fzf

  if ! [[ -r "$TEMP" ]] || [[ $rg_pat != $(cat "$TEMP") ]]; then
    echo "$rg_pat" > "$TEMP"
    printf "reload:sleep 0.1; rg --column --line-number --no-heading --color=always --smart-case %q || true" "$rg_pat"
  fi
  echo "+search:$fzf_pat"
'
fzf --ansi --disabled --query "$INITIAL_QUERY" \
    --with-shell 'bash -c' \
    --bind "start,change:transform:$TRANSFORMER" \
    --color "hl:-1:underline,hl+:-1:underline:reverse" \
    --delimiter : \
    --preview 'bat --color=always {1} --highlight-line {2}' \
    --preview-window 'up,60%,border-line,+{2}+3/3,~3' \
    --bind 'enter:become(vim {1} +{2})'
```

追踪日志
-----------

fzf 可以运行长时间执行的预览命令，并在命令完成之前渲染部分结果。
而当你在 `--preview-window` 选项中指定 `follow` 标志时，
fzf 会对结果执行类似"`tail -f`"的操作，自动滚动到底部。

```bash
# 加上 "follow" 后，预览窗口会自动滚动到底部。
# "\033[2J" 是用于清屏的 ANSI 转义序列。
# fzf 读到这段序列时会清除之前的预览内容。
fzf --preview-window follow --preview 'for i in $(seq 100000); do
  echo "$i"
  sleep 0.01
  (( i % 300 == 0 )) && printf "\033[2J"
done'
```

![image](https://user-images.githubusercontent.com/700826/113473303-dd669600-94a3-11eb-88a9-1f61b996bb0e.png)

不得不承认，刚才那个例子有点无聊。下面这个用于浏览
Kubernetes Pod 的例子就比较实用了。

```bash
pods() {
  command='kubectl get pods --all-namespaces' fzf \
    --info=inline --layout=reverse --header-lines=1 \
    --prompt "$(kubectl config current-context | sed 's/-context$//')> " \
    --header $'╱ Enter (kubectl exec) ╱ CTRL-O (open log in editor) ╱ CTRL-R (reload) ╱\n\n' \
    --bind 'start,ctrl-r:reload:$command' \
    --bind 'ctrl-/:change-preview-window(80%,border-bottom|hidden|)' \
    --bind 'enter:execute:kubectl exec -it --namespace {1} {2} -- bash' \
    --bind 'ctrl-o:execute:${EDITOR:-vim} <(kubectl logs --all-containers --namespace {1} {2})' \
    --preview-window up:follow \
    --preview 'kubectl logs --follow --all-containers --tail=10000 --namespace {1} {2}' "$@"
}
```

![image](https://user-images.githubusercontent.com/700826/113473547-1d7a4880-94a5-11eb-98ef-9aa6f0ed215a.png)

- 预览窗口会*"追踪"*该 Pod 的日志
    - 长时间持有大量日志会消耗很多内存，因此我们用
      `--tail=10000` 限制了初始日志的数量。
- `execute` 绑定让你无需离开 fzf 就能运行任意命令
    - 在某个 Pod 上按回车即可 `kubectl exec` 进入它
    - 按 CTRL-O 在你的编辑器中打开日志
- 按 CTRL-R 重新加载 Pod 列表
- 反复按 CTRL-/ 可在多组预览
  窗口选项之间轮换
    1. `80%,border-bottom`
    1. `hidden`
    1. `|` 之后的空字符串表示回退到 `--preview-window` 的默认选项

Git 对象的键位绑定
----------------------------

你常常需要把各种 Git 对象的标识符放到
命令行上。例如，我们经常会写下面这样的命令：

```sh
git checkout [SOME_COMMIT_HASH or BRANCH or TAG]
git diff [SOME_COMMIT_HASH or BRANCH or TAG] [SOME_COMMIT_HASH or BRANCH or TAG]
```

[fzf-git.sh](https://github.com/junegunn/fzf-git.sh) 项目定义了一组
基于 fzf 的 Git 对象键位绑定。我强烈推荐你去看看，因为它们真的非常有用。

### `git status` 中列出的文件

<kbd>CTRL-G</kbd><kbd>CTRL-F</kbd>

![image](https://user-images.githubusercontent.com/700826/113473779-a9d93b00-94a6-11eb-87b5-f62a8d0a0efc.png)

### 分支

<kbd>CTRL-G</kbd><kbd>CTRL-B</kbd>

![image](https://user-images.githubusercontent.com/700826/113473758-87dfb880-94a6-11eb-82f4-9218103f10bd.png)

### 提交哈希

<kbd>CTRL-G</kbd><kbd>CTRL-H</kbd>

![image](https://user-images.githubusercontent.com/700826/113473765-91692080-94a6-11eb-8d38-ed4d41f27ac1.png)

配色主题
------------

你可以通过 `--color` 选项自定义 fzf 给各类文本元素着色的方式。下面是
几个配色主题。注意，你需要一个能显示 24 位颜色的终端模拟器。

```sh
# junegunn/seoul256.vim (dark)
export FZF_DEFAULT_OPTS='--color=bg+:#3F3F3F,bg:#4B4B4B,border:#6B6B6B,spinner:#98BC99,hl:#719872,fg:#D9D9D9,header:#719872,info:#BDBB72,pointer:#E12672,marker:#E17899,fg+:#D9D9D9,preview-bg:#3F3F3F,prompt:#98BEDE,hl+:#98BC99'
```

![seoul256](https://user-images.githubusercontent.com/700826/113475011-2c192d80-94ae-11eb-9d17-1e5867bae01f.png)

```sh
# junegunn/seoul256.vim (light)
export FZF_DEFAULT_OPTS='--color=bg+:#D9D9D9,bg:#E1E1E1,border:#C8C8C8,spinner:#719899,hl:#719872,fg:#616161,header:#719872,info:#727100,pointer:#E12672,marker:#E17899,fg+:#616161,preview-bg:#D9D9D9,prompt:#0099BD,hl+:#719899'
```

![seoul256-light](https://user-images.githubusercontent.com/700826/113475022-389d8600-94ae-11eb-905f-0939dd535837.png)

```sh
# morhetz/gruvbox
export FZF_DEFAULT_OPTS='--color=bg+:#3c3836,bg:#32302f,spinner:#fb4934,hl:#928374,fg:#ebdbb2,header:#928374,info:#8ec07c,pointer:#fb4934,marker:#fb4934,fg+:#ebdbb2,prompt:#fb4934,hl+:#fb4934'
```

![gruvbox](https://user-images.githubusercontent.com/700826/113475042-494dfc00-94ae-11eb-9322-cd03a027305a.png)

```sh
# arcticicestudio/nord-vim
export FZF_DEFAULT_OPTS='--color=bg+:#3B4252,bg:#2E3440,spinner:#81A1C1,hl:#616E88,fg:#D8DEE9,header:#616E88,info:#81A1C1,pointer:#81A1C1,marker:#81A1C1,fg+:#D8DEE9,prompt:#81A1C1,hl+:#81A1C1'
```

![nord](https://user-images.githubusercontent.com/700826/113475063-67b3f780-94ae-11eb-9b24-5f0d22b63399.png)

```sh
# tomasr/molokai
export FZF_DEFAULT_OPTS='--color=bg+:#293739,bg:#1B1D1E,border:#808080,spinner:#E6DB74,hl:#7E8E91,fg:#F8F8F2,header:#7E8E91,info:#A6E22E,pointer:#A6E22E,marker:#F92672,fg+:#F8F8F2,prompt:#F92672,hl+:#F92672'
```

![molokai](https://user-images.githubusercontent.com/700826/113475085-8619f300-94ae-11eb-85e4-2766fc3246bf.png)

### fzf 主题炼丹场

由 [Vitor Mello](https://github.com/vitormv) 创建的
[fzf 主题炼丹场](https://vitormv.github.io/fzf-themes/)是一个可以
交互式创建 fzf 主题的网页。

### 从 Vim 配色方案生成 fzf 配色主题

fzf 的 Vim 插件可以根据 `g:fzf_colors` 变量，从当前的配色方案
生成 `--color` 选项。详细的说明可以参见
[这里](README-VIM.zh-CN.md#gfzf_colors-说明)。

下面是一个例子。把这段内容加入你的 Vim 配置文件。

```vim
let g:fzf_colors =
\ { 'fg':         ['fg', 'Normal'],
  \ 'bg':         ['bg', 'Normal'],
  \ 'preview-bg': ['bg', 'NormalFloat'],
  \ 'hl':         ['fg', 'Comment'],
  \ 'fg+':        ['fg', 'CursorLine', 'CursorColumn', 'Normal'],
  \ 'bg+':        ['bg', 'CursorLine', 'CursorColumn'],
  \ 'hl+':        ['fg', 'Statement'],
  \ 'info':       ['fg', 'PreProc'],
  \ 'border':     ['fg', 'Ignore'],
  \ 'prompt':     ['fg', 'Conditional'],
  \ 'pointer':    ['fg', 'Exception'],
  \ 'marker':     ['fg', 'Keyword'],
  \ 'spinner':    ['fg', 'Label'],
  \ 'header':     ['fg', 'Comment'] }
```

然后你可以通过打印 `fzf#wrap()` 的结果，
看看由此生成的 `--color` 选项。

```vim
:echo fzf#wrap()
```

用下面这条命令，可以把 `export FZF_DEFAULT_OPTS="..."` 这一行
追加到当前文件的末尾。

```vim
:call append('$', printf('export FZF_DEFAULT_OPTS="%s"', matchstr(fzf#wrap().options, "--color[^']*")))
```
