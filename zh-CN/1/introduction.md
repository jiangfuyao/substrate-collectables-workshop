Introduction
===

In this section, we will show you the basics of creating a custom runtime:

- How to use runtime storage
- How to expose runtime functions
- How to use the Polkadot-JS Apps UI to interact with your runtime

在本节中，我们将向你展示创建自定义 runtime 的基础知识：

- 如何使用 runtime 存储
- 如何公开 runtime 函数
- 如何使用 Polkadot-JS Apps UI 与你的 runtime 进行交互

## What is a Runtime?

In short, the [*Runtime*](https://substrate.readme.io/docs/glossary#section-runtime) is block execution logic of a blockchain, sometimes referred to as the state transition function [**STF**](https://substrate.readme.io/docs/glossary#section-stf-state-transition-function-). In [Substrate](https://substrate.readme.io/docs/glossary#section-substrate), this is stored on-chain in an implementation-neutral, machine-executable format as a WebAssembly binary. Other systems tend to express it only in human-readable format (e.g. Ethereum) or not at all (e.g. Bitcoin).

简而言之，[*Runtime*](https://substrate.readme.io/docs/glossary#section-runtime) 是区块链的块执行逻辑，有时称为状态转换函数 [**STF**](https://substrate.readme.io/docs/glossary#section-stf-state-transition-function-)。在 Substrate 中，它以实现中立，机器可执行的格式存储在链上，作为WebAssembly 二进制文件。其他系统倾向于仅以人类可读的格式来表达（例如 Ethereum）或者就根本不存在（例如 Bitcoin）。

## What is a Module?

Your blockchain's runtime is composed of multiple features and functionalities which work together to power your blockchain. Things like:

- Account Management
- Token Balances
- Governance
- Runtime Upgrades
- and more...

你的区块链 runtime 由多个特性和功能组成，这些特性和功能共同为区块链提供支持。像：

- 帐户管理
- Token 余额
- 治理
- Runtime 升级
- 和更多...

These are all modules that are provided [here](https://github.com/paritytech/substrate/tree/master/srml) in the codebase for you to easily include into your runtime. These default set of modules provided by Substrate are known as the Substrate Runtime Module Library [**SRML**](https://substrate.readme.io/docs/glossary#section-srml-substrate-runtime-module-library-)

[这些](https://github.com/paritytech/substrate/tree/master/srml)是代码库中提供的所有模块，你可以轻松地将其包含在 runtime 中。Substrate 提供的这些默认模块集被称为 Substrate Runtime Module Library [**SRML**](https://substrate.readme.io/docs/glossary#section-srml-substrate-runtime-module-library-)

With the Substrate framework, you are able to easily create and include new modules into your runtime. That is what we will be doing in this tutorial!

使用 Substrate 框架，你可以轻松地在 runtime 中创建和包含新的模块。这就是我们在本教程中将要做的！

## Rust

At the moment, Substrate and Runtime development uses the [Rust programming language](https://www.parity.io/why-rust/).

目前，Substrate 和 Runtime 开发使用 [Rust编程语言](https://www.parity.io/why-rust/)。

This tutorial is **not** a course in learning Rust, but we should go over some of the basic differences you may encounter when following this guide compared to programming in other languages.

本教程 **不是** 学习 Rust 的课程，但我们应该回顾一下在使用本指南时与用其他语言编程时相比可能遇到的一些基本差异。

### Ownership and Borrowing

From the [Rust docs](https://doc.rust-lang.org/book/ownership.html):

> Ownership is Rust’s most unique feature, and it enables Rust to make memory safety guarantees without needing a garbage collector.
>
> - Each value in Rust has a variable that’s called its owner.
> - There can only be one owner at a time.
> - When the owner goes out of scope, the value will be dropped.

You will see throughout the tutorial we will add an ampersand (`&`) in front of some variables which implies that we are borrowing the value. This is useful if we need to reuse the value multiple times throughout a function.

你将在整个教程中看到我们将在一些变量前面添加一个与号（＆），这意味着我们正在借用该值。如果我们需要在整个函数中多次重用该值，这将非常有用。

It basically hinders you from doing stupid mistakes in handling memory - so be thankful if the Rust compiler advises you not to do a certain thing.

它基本上阻止了你在处理内存时会犯的一些愚蠢错误 - 所以如果 Rust 编译器建议你不要做某件事，请心存感激。

### Traits

From the [Rust docs](https://doc.rust-lang.org/book/traits.html):

> Traits abstract over behavior that types can have in common.

If you are familiar with interfaces, Traits are [Rust's sole notion of an interface](https://blog.rust-lang.org/2015/05/11/traits.html).

如果你熟悉 interfaces，Traits 是 [Rust 唯一的 interface 概念](https://blog.rust-lang.org/2015/05/11/traits.html)。[**译者注**： 我不认同把 traits 说成是 interface，不过对常用 OOP 范式的开发者来看，它在使用上的确更接近 interface 的概念，而对常用 FP 范式的开发者来说，它在使用上更接近于 Haskell 中的 typeclass 概念]

### Recoverable Errors with Result

You will learn later that module functions must return the `Result` type which allows us to handle errors within our functions. The returned `Result` is either `Ok()` for success or `Err()` for a failure.

稍后你将了解到模块函数必须返回 `Result` 类型，这允许我们处理函数中的错误。返回的结果是 `Ok()` 时表示成功，`Err()` 时表示失败。

Throughout this tutorial we will use the question mark operator (`?`) at the end of functions which return a `Result`. When calling a function like this, for example `my_function()?`, Rust simply expands the code to:

在本教程中，我们将在返回 `Result` 的函数末尾使用问号运算符(`?`)。当调用这样的函数时，例如 `my_function()?`, Rust 编译器简单地将代码扩展为:

```rust
match my_function() {
    Ok(value) => value,
    Err(msg) => return Err(msg),
}
```

You can learn more about [this here](https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html).

你可以在[此处](https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html)了解更多相关信息。

Typical error of these kinds are

这些类型的典型错误是

```
error[E0382]: borrow of moved value: `s`
error[E0382]: use of moved value: `s`
error[E0502]: cannot borrow `s` as immutable because it is also borrowed as mutable
error[E0499]: cannot borrow `s` as mutable more than once at a time
```

For example, the following code will not compile:

例如，以下代码将无法编译：

```rust
fn main() {
    let mut s = String::from("hello");
    let t1 = &s;      // t1 is an immutable reference to the String
    let t2 = &mut s;  // t2 is a mutable reference to the String
    t2.push_str(&t1); // We want to append t2 to t1.
                      //
		      // This is broken since both are references to the same underlying string.
}
```
Borrowing error:

```
error[E0502]: cannot borrow `s` as mutable because it is also borrowed as immutable
 --> src/main.rs:4:14
  |
3 |     let t1 = &s;
  |              -- immutable borrow occurs here
4 |     let t2 = &mut s;
  |              ^^^^^^ mutable borrow occurs here
5 |     t2.push_str(&t1);
  |                 --- immutable borrow later used here
```

An easy fix for this particular error is cloning the string instead of having another reference to it.

简单修复这种特定类型错误的方法是克隆字符串而不是对它进行引用。

```rust
fn main() {
    let mut s = String::from("hello");
    let t1 = s.clone();
    let t2 = &mut s;
    t2.push_str(&t1);
}
```

Works!

### Macros

From the [Rust docs](https://doc.rust-lang.org/book/macros.html):

> While functions and types abstract over code, macros abstract at a syntactic level.

More simply, macros are code that write code, usually to simplify or make code more readable.

更简单地说，宏是编写代码的代码，通常用于简化代码或使代码更具可读性。

Substrate uses a lot of macros throughout the Runtime development process, and they are quite specific in the syntax they support and quite poor in the errors they return.

Substrate 在整个 Runtime 开发过程中使用了很多宏，它们支持十分特有的语法，而且它们返回的错误可读性相当差。[**译者注**： 可以使用 [cargo-expand 工具](https://github.com/dtolnay/cargo-expand) 来展开宏，以便理解具体调用逻辑]

---
**Learn More**

 how does Result/? work maybe?

 Introduce Traits -> Connection to interfaces?

[TODO: make this a page]

---
