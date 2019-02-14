Common Patterns Moving Forward
===

## The Rust Compiler is Your Friend

One of the many advantages of using a strongly typed programming language like Rust

[TODO: Make it seem like RUST is your friend and will help you add code when needed]

使用强类型编程语言（如 Rust）的众多优点之一

[TODO: 让 RUST 成为你的朋友，在需要时帮助你添加代码]

## Making Updates to Your Runtime

Before we jump into creating a custom Substrate runtime, you should be familiar with a few patterns which will help you iterate and run your code.

在我们开始创建自定义 Substrate runtime 之前，你应该熟悉一些可以帮助你迭代和运行代码的模式。

Your Substrate runtime code is compiled into two versions:

- A [WebAssembly](https://webassembly.org/) (Wasm) image
- A standard binary executable

你的 Substrate runtime 代码被编译为两个版本:

- WebAssembly（Wasm）image
- 标准二进制可执行文件

The Wasm file is used as a part of the compilation of the standard binary, so it is important that you always compile your Wasm image first before you build the executable.

Wasm 文件用作标准二进制文件编译的一部分，因此在构建可执行文件之前首先编译 Wasm image 非常重要。

The pattern should be:

模式应该是:

```bash
./build.sh               // Build Wasm
cargo build --release    // Build binary
```

Additionally, when you make changes to your node, the blocks produced in the past by older versions of your node persist. You may notice that when restarting your node, block production simply picks up where it left off.

此外，当你对节点进行更改时，之前旧版本节点生成的块仍然存在。你可能会注意到，当重启节点时，块只会从中断处继续生成。

However, if your changes to the runtime are significant, you may need to purge your chain of all the previous blocks with this handy command:

但是，如果你对 runtime 的更改很重要，则可能需要使用以下命令清除你链上所有先前块:

```bash
./target/release/substratekitties purge-chain --dev
```

After all this, then you will be able to start up your node again, fresh, with all the latest changes:

完成所有这些后，你将能够再次启动带有最近更改的节点:

```bash
./target/release/substratekitties --dev
```

Remember this pattern; you will be using it a lot.

记住这种模式; 你会经常使用它。

---
**Learn More**

Substrate is unique in that it is built to support Wasm and all languages that compile to Wasm. This allows for next generation features like live runtime upgrades and ...

[TODO: Talk about Wasm and runtime upgrades]

Substrate 的独特之处在于它是为了支持 Wasm 和所有能编译为 Wasm 的语言而构建的。这允许下一代特性，如实时 runtime 升级和 ...

[TODO：谈谈 Wasm 和 runtime 升级]

---