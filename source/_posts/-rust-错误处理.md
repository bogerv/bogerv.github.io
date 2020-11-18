---
title: Rust 错误处理
date: 2020-03-12 01:25:50
categories: [rust]
---

错误处理可分为两大类:

- 抛出异常
- 返回值

Rust 选择了返回值处理错误, 并且提供了优雅的解决方案.

## 理解 unwrapping

```Rust
use std::env;

fn main() {
    let mut argv = env::args();
    let arg: String = argv.nth(1).unwrap(); // err1
    let n: i32 = arg.parse().unwrap(); // err2
    println!("{}", 2 * n);
}
```

如果你给这个程序 0 个参数（错误 1）或者 第一个参数并不是整型（错误 2），这个程序也会像第一个例子那样 panic. 因为 unwrap 包含了 panic.

## Option 枚举类型

```Rust
enum Option<T> {
    None,
    Some(T),
}
```

Option 用于表示值**不存在的可能**. 会强迫编译器处理不存在的情况.

返回两个变量:

- None: 表明失败或者缺少值
- Some(T): 元组结构体, 存的 T 类型值

如果使用 `Option<T>` 作为返回值, 则可以使用 match 处理返回结果, 如下实例:

```Rust
// 不会 `panic!` 的整数除法。
fn checked_division(dividend: i32, divisor: i32) -> Option<i32> {
    if divisor == 0 {
        // 失败表示成 `None` 取值
        None
    } else {
        // 结果 Result 被包装到 `Some` 取值中
        Some(dividend / divisor)
    }
}

// 此函数处理可能失败的除法
fn try_division(dividend: i32, divisor: i32) {
    // `Option` 值可以进行模式匹配，就和其他枚举类型一样
    match checked_division(dividend, divisor) {
        None => println!("{} / {} failed!", dividend, divisor),
        Some(quotient) => {
            println!("{} / {} = {}", dividend, divisor, quotient)
        },
    }
}

fn main() {
    try_division(4, 2);
    try_division(1, 0);

    // 绑定 `None` 到一个变量需要类型标注
    let none: Option<i32> = None;
    let _equivalent_none = None::<i32>;

    let optional_float = Some(0f32);

    // 解包 `Some` 将取出被包装的值。
    println!("{:?} unwraps to {:?}", optional_float, optional_float.unwrap());

    // 解包 `None` 将会引发 `panic!`。
    println!("{:?} unwraps to {:?}", none, none.unwrap());
}
```

## Result 类型

```Rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

Result 用于表示**具体失败的原因**.

返回两个变量:

- Ok(T): 表示操作成功, 并包装操作返回的 T 类型值
- Err(E): 表示操作失败, 并包装 E, E 能够解释失败的原因

实例如下:

```Rust
mod checked {
    // 我们想要捕获的数学 “错误”
    #[derive(Debug)]
    pub enum MathError {
        DivisionByZero,
        NegativeLogarithm,
        NegativeSquareRoot,
    }

    pub type MathResult = Result<f64, MathError>;

    pub fn div(x: f64, y: f64) -> MathResult {
        if y == 0.0 {
            // 此操作将会失败，那么（与其让程序崩溃）不如把失败的原因包装在
            // `Err` 中并返回
            Err(MathError::DivisionByZero)
        } else {
            // 此操作是有效的，返回包装在 `Ok` 中的结果
            Ok(x / y)
        }
    }

    pub fn sqrt(x: f64) -> MathResult {
        if x < 0.0 {
            Err(MathError::NegativeSquareRoot)
        } else {
            Ok(x.sqrt())
        }
    }

    pub fn ln(x: f64) -> MathResult {
        if x < 0.0 {
            Err(MathError::NegativeLogarithm)
        } else {
            Ok(x.ln())
        }
    }
}

// `op(x, y)` === `sqrt(ln(x / y))`
fn op(x: f64, y: f64) -> f64 {
    // 这是一个三层的 match 金字塔！
    match checked::div(x, y) {
        Err(why) => panic!("{:?}", why),
        Ok(ratio) => match checked::ln(ratio) {
            Err(why) => panic!("{:?}", why),
            Ok(ln) => match checked::sqrt(ln) {
                Err(why) => panic!("{:?}", why),
                Ok(sqrt) => sqrt,
            },
        },
    }
}

fn main() {
    // 这会失败吗？
    println!("{}", op(1.0, 10.0));
}
```
