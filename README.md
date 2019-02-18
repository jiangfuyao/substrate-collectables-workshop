<div align="right">
    Language: :us:
    <a title="Chinese" href="docs/zh-CN/README.md">:cn:</a>
</div>

# [Substrate Collectables][main link]

> The interactive hands-on build-your-first-blockchain with [Substrate][] workshop
>
> 使用 [Substrate][] 交互式地构建你的第一条区块链

![A screenshot of Substrate kitties](./media/substrate-collectables.png)

## What is this?

This is an interactive hands-on self-paced workshop. You will learn how to build your first blockchain using [Substrate][], the OpenSource [Rust][] Blockchain Development Kit by [Parity][]. Through the lessons of the workshop, you will build a collectables blockchain -- a chain that creates assets, and allows you to interact with and managing ownership of them.

这是一个交互式的自我实践项目，你能学到如何使用 [Substrate][] - 由 [Parity][] 开源的 [Rust][] 区块链开发组件来构建你的第一条区块链。通过这个实践项目，你将构建一条能创建资产，可交互和管理其所有权的区块链。

As such, this material will focus on the logic of building the said chain. It won't cover the networking, consensus or economic incentive aspects of blockchains. Fortunately, Substrate comes with decent networking and consensus engines built in, so we can just focus on the chain logic.

因此，该材料将着重于构建所述链的逻辑。它不会设计区块链的网络，共识和经济激励方面。幸运的是，Substrate 内置了不错的网络和共识引擎，这使得我们可以专注于链的逻辑。

Substrate is built using [Rust][], a modern statically typed systems programming language. We won't go into the details of the language within this workshop. The language is quite easy to read and follow and if you have programmed before, you shouldn't have too much trouble following what is going on and finishing the exercises even if [Rust][] is new to you.

Substrate 是使用 Rust 构建的，Rust 是一种现代静态类型的系统编程语言。我们不会在该项目里详细介绍该语言的细节。Rust 语言非常容易阅读，如果你之前有过编程经验，那么即使你不熟悉 Rust，也不会在完成练习时遇到太多麻烦。[**译者注**: 我并不同意 “有过编程经验的开发者在编写 Rust 时不会遇到太多麻烦” 这种观点，我认为需要至少有过 C++/Haskell 的编程经验者才比较容易上手 Rust，因为 Rust 从 C++，Haskell 和其他编程语言里借鉴了很多优秀的部分，并基于这些构建了属于自身的独有特点]

## How do I do this?

Just go through the material chapter by chapter, do one exercise at a time. While the material is meant for you to be able to do on your own, we highly recommend you to get together and work on it with others, in learning groups or hosted workshops. It is totally normal to get stuck from time to time or to not understand what the material is attempting to explain. In those situations it helps a lot to have others around to talk to about it and resolve that frustration. That said, we highly appreciate any [feedback regarding the material, and where you might got stuck][feedback].

只需逐章阅读材料，并完成每章的练习。虽然这些材料是为了让你能够独立完成，但我们强烈建议你与学习小组里的其他人一起合作。不时陷入困境或不理解材料中所解释的内容是完全正常的。在这时候，与周围的其他人交流会对问题的解决很有帮助。我们非常感谢 [有关该材料的任何反馈，以及你可能遇到的问题][feedback]。

# [Let's go!](/0/introduction.md)

---
**NOTE**

Substrate is a rapidly evolving project, which means that breaking changes may cause you problems when trying to follow these instructions. Feel free to [contact us](https://substrate.readme.io/v1.0.0/docs/feedback) with any problems you encounter.

**注意**

Substrate 是一个快速发展的项目，这意味着在尝试遵循这些说明时，breaking changes 可能会导致你遇到问题。如果你遇到任何问题，请随时[与我们联系](https://substrate.readme.io/v1.0.0/docs/feedback)。

---

## How to contribute

* Open [issues on our GitHub](https://github.com/shawntabrizi/substrate-collectables-workshop/issues)

* Fork and clone the [repository](https://github.com/shawntabrizi/substrate-collectables-workshop)

* Launch a local development server with a tool like [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) for [Visual Studio Code](https://code.visualstudio.com/)

* Contribute by commiting and pushing changes to a branch of your origin fork and creating a Pull Request to the upstream repository

## Acknowledgements

Open source projects like Substrate and this workshop could not be successful without the collective minds and collaborative effort of the development community.

The Substratekitties workshop stands on the backs of giants like [Cryptokitties](https://www.cryptokitties.co/), [Cryptozombies](https://cryptozombies.io/), [Docsify](https://docsify.js.org/), [Monaco Editor](https://microsoft.github.io/monaco-editor/), [David Revoy's Cat Avatar Generator](https://framagit.org/Deevad/cat-avatar-generator), and numerous volunteers to report errors and bugs along the way.

We hope this educational material teaches you something new, and in turn, you teach others too.

---

[main link]: https://shawntabrizi.github.io/substrate-collectables-workshop/
[feedback]: https://substrate.readme.io/v1.0.0/docs/feedback
[Substrate]: https://www.parity.io/substrate/
[Substrate docs]: https://substrate.readme.io/
[Parity]: https://www.parity.io/
[Rust]: https://www.rust-lang.org/
