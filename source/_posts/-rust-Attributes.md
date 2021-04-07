# Rust Attributes

我们在浏览其他`Rust`项目源码的时候, 都会看到类似 `#[derive(Default, Copy, Clone, Debug, PartialEq, Eq)]` 或 `#[cfg(feature = "std")]` 等格式的代码

## 00 内部属性 (Inner Attributes)

一个属性声明在一个元素中, 对此元素（比如一般为 crate）整体生效. 内部属性用 `#![]` 声明.

## 01 外部属性 (Outer Attributes)

一个属性声明在一个元素之前, 对跟在后面的这个元素生效. 外部属性用 `#[]` 声明.

> Rust 中, 有些属性可以/只能作内部属性使用, 有些属性可以/只能作外部属性使用

## 02 Meta Item Attribute Syntax

描述了属性语法的基本结构

| 语法样式名称         | 语法看起来的样子                                  |
| -------------------- | ------------------------------------------------- |
| MetaWord             | no_std                                            |
| MetaNameValueStr     | doc = "example"                                   |
| MetaListPaths        | allow(unused, clippy::inline_always)              |
| MetaListIdents       | macro_use(foo, bar)                               |
| MetaListNameValueStr | link(name = "CoreFoundation", kind = "framework") |

> Rust 代码中看到的所有属性语法都是上述五种中的一种或其组合

## 03 `active` 和 `insert` 属性

一个属性, 要么是 `active` 的, 要么是 `insert` 的

`active` 属性是指, 在处理属性（预处理代码）的过程中, `active` 属性会将它们自己删除, 留下所作用的元素

`insert` 属性是指, 在处理属性（预处理代码）的过程中, `insert` 属性会将它们自己保留

- `cfg` 和 `cfg_attr` 属性是 `active` 的
- 当编译为 `test` 模式时, `test` 属性是 `insert` 的编译为非 `test` 模式时, test 属性是 `active` 的
- 属性宏是 `active` 的
- 所有其它属性是 `insert` 的

## 04 属性分类

> 宏属性和派生宏辅助属性**可以自定义**
>
> 工具属性和内建属性, 我们**只能用, 不能自定义**

- Macro attributes - 宏属性
- Derive macro helper attributes - 派生宏辅助属性
- Tool attributes - 工具属性
- Built-in attributes - 内建属性

### 4.1 宏属性

也叫属性宏, 属于过程宏的一种

定义过程宏的时候，使用 `#[proc_macro_attribute]`

```Rust
// 定义宏
#[proc_macro_attribute]
pub fn return_as_is(_attr: TokenStream, item: TokenStream) -> TokenStream {
  item
}

// 使用宏
#[return_as_is]
fn invoke() {}
```

### 4.2 派生宏辅助属性

```Rust
// 先定义派生宏
#[proc_macro_derive(HelperAttr, attributes(helper))]
pub fn derive_helper_attr(_item: TokenStream) -> TokenStream {
  TokenStream::new()
}

// 看如何使用
#[derive(HelperAttr)]
struct Struct {
  #[helper] field: ()
}
```

> 里面那个 `#[helper]` 就是一个派生宏辅助属性

### 4.3 工具属性

```rust
// Tells the rustfmt tool to not format the following element.
#[rustfmt::skip]
struct S {
}

// Controls the "cyclomatic complexity" threshold for the clippy tool.
#[clippy::cyclomatic_complexity = "100"]
pub fn f() {}
```

> 目前 rustc 只认识两个外部工具 (及它们内部的属性) : `rustfmt` 和 `clippy`
>
> 不能在自己的工具中定义 Tool Attribute

### 4.4 内建属性

Rust 内建了 14 类属性

- 条件编译
  - cfg
  - cfg_attr
- 测试
  - test
  - ignore
  - should_panic
- 派生
  - derive
- 宏相关
  - macro_export
  - macro_use
  - proc_macro
  - proc_macro_derive
  - proc_macro_attribute
- 诊断
  - allow, warn, deny, forbid - lint 相关标志开关, 各种 lint 见附录.
  - deprecated
  - must_use
- ABI, 链接, 符号, 和 FFI
  - link
  - link_name
  - no_link
  - repr
  - crate_type
  - no_main
  - export_name
  - link_section
  - no_mangle
  - used
  - crate_name
- 代码生成
  - inline
  - cold
  - no_builtins
  - target_feature
- 文档
  - doc
- 预引入
  - no_std
  - no_implicit_prelude
- 模块
  - path
- 限制
  - recursion_limit
  - type_length_limit
- 运行时
  - panic_handler
  - global_allocator
  - windows_subsystem
- 语言特性
  - feature - 经常会碰到这里面一些陌生的 feature 名称, 需要根据具体的 rustc 版本和所使用的库文档进行查阅.
- 类型系统
  - non_exhaustive

> 上面的属性中, 很多属性, 其内容都可以单独开一篇文章来讲解. 比如, 条件编译相关的属性, FFI 相关属性等.
