构建 fzf
============

> **语言**：[English](BUILD.md) | 简体中文（本文档）
>
> 本译文中的命令、Makefile 目标、环境变量名与许可证名均保持原文。

构建说明
------------------

### 前置条件

- Go 1.23 或更高版本

### 使用 Makefile

```sh
# 在 target 目录中为你的平台构建 fzf 二进制文件
make

# 构建 fzf 二进制文件并复制到 bin 目录
make install

# 使用 goreleaser 为所有平台构建 fzf 二进制文件与归档包
make build

# 发布 GitHub release
make release
```

> [!WARNING]
> Makefile 使用 git 命令来确定 `fzf --version` 所需的版本与修订信息。
> 因此，如果你构建 fzf 的环境无法获取其 git 信息，
> 就必须手动设置 `$FZF_VERSION` 与 `$FZF_REVISION`。
>
> 例如 `FZF_VERSION=0.24.0 FZF_REVISION=tarball make`

> [!TIP]
> 若要在构建时启用性能剖析选项，请设置 `TAGS=pprof`
>
> ```sh
> TAGS=pprof make clean install
> fzf --profile-cpu /tmp/cpu.pprof --profile-mem /tmp/mem.pprof \
>     --profile-block /tmp/block.pprof --profile-mutex /tmp/mutex.pprof
> ```

运行测试
-------------

```sh
# 运行 go 单元测试
make test

# 运行集成测试（需要在 tmux 中运行）
make itest

# 运行单个测试用例
ruby test/runner.rb --name test_something
```

使用的第三方库
--------------------------

- [rivo/uniseg](https://github.com/rivo/uniseg)
    - 采用 [MIT](https://raw.githubusercontent.com/rivo/uniseg/master/LICENSE.txt) 许可
- [mattn/go-shellwords](https://github.com/mattn/go-shellwords)
    - 采用 [MIT](http://mattn.mit-license.org) 许可
- [mattn/go-isatty](https://github.com/mattn/go-isatty)
    - 采用 [MIT](http://mattn.mit-license.org) 许可
- [tcell](https://github.com/gdamore/tcell)
    - 采用 [Apache License 2.0](https://github.com/gdamore/tcell/blob/master/LICENSE) 许可
- [fastwalk](https://github.com/charlievieth/fastwalk)
    - 采用 [MIT](https://raw.githubusercontent.com/charlievieth/fastwalk/master/LICENSE) 许可

许可证
-------

[MIT](LICENSE)
