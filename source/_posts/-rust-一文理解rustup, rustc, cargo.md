---
title: 一文理解rustup, rustc, cargo
date: 2020-05-09 00:39:50
categories: [rust]
---

## rustup

是 rustc 的版本管理工具, 具体命令可以使用 `rustup --help` 进行查看.
下面会列举写比较常用的命令示例.

### rustup update

使用 `rustup update --help` 可以看到如下输出:

```zsh
USAGE:
    rustup update [FLAGS] [toolchain]...

FLAGS:
        --force             Force an update, even if some components are missing
    -h, --help              Prints help information
        --no-self-update    Don't perform self update when running the `rustup update` command

ARGS:
    <toolchain>...    Toolchain name, such as 'stable', 'nightly', or '1.8.0'. For more information see `rustup help
                      toolchain`
```

可以第二个(更新标识)以及第三个参数(更新的工具链), 第三个参数可以选择

- `stable` (更新 `rustc` 到最新的稳定版本)
- `nightly` (更新 `rustc` 到最新的非稳定版本)
- `1.8.0` (更新 `rustc` 到指定某个版本)

> 如果直接运行 `rustup update` 会直接更新所有已经安装的 `toolchain`, 然后再更新 `rustup` 本身

### rustup default

用于指定使用的 rustc toolchain: 

```zsh
# 指定 rustc 的版本为 stable 的最新版本
rustup default stable
```

### rustup self uninstall

卸载 `rustup`

## rustc

`rustc` 是 `Rust编程语言` 的编译器, 会将 `.rs` 类型的文件编译为可执行文件.
与 `gcc` 的功能比较类似, 可以指定相关参数.

下文将要讲到的 `cargo` 实际上就是依赖于 `rustc` 进行各种操作的.

执行 `cargo build --verbose` 命令得到如下输出:

```zsh
(base) ~/demos/rust/hello-rust/cargo_hello 🐳 cargo build --verbose
   Compiling cargo_hello v0.1.0 (/Users/wangbo/demos/rust/hello-rust/cargo_hello)
     Running `rustc --crate-name cargo_hello --edition=2018 src/main.rs --error-format=json --json=diagnostic-rendered-ansi --crate-type bin --emit=dep-info,link -C debuginfo=2 -C metadata=e61da58ee0da174a -C extra-filename=-e61da58ee0da174a --out-dir /Users/wangbo/demos/rust/hello-rust/cargo_hello/target/debug/deps -C incremental=/Users/wangbo/demos/rust/hello-rust/cargo_hello/target/debug/incremental -L dependency=/Users/wangbo/demos/rust/hello-rust/cargo_hello/target/debug/deps`
    Finished dev [unoptimized + debuginfo] target(s) in 1.28s
(base) ~/demos/rust/hello-rust/cargo_hello 🐳
```

可以看到实际上也是调用了 `rustc` ,并指定相关参数执行编译的.

## cargo

`cargo` 是一个依赖版本管理工具. 如同 `Java` 的 `Maven`, `C#` 的 `NuGet`;

具体操作有一本专门的小册讲解其各种命令: [The Cargo Book](https://doc.rust-lang.org/cargo/getting-started/installation.html)
