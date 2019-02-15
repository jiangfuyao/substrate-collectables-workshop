Creating a Module
===

## Creating a New Module

To start, we need to create a new module for our runtime. For that we will work with an empty module template which we will place in a new `substratekitties.rs` file:

首先，我们需要为 runtime 创建一个新模块。为此，我们将使用一个空模块模板，并把它放在一个新的 `substratekities.rs` 文件中：

```
substratekitties
|
+-- runtime
    |
    +-- src
        |
        +-- lib.rs
        |
        +-- * substratekitties.rs
```

**substratekitties<span>.</span>rs**

```rust
use support::{decl_storage, decl_module};

pub trait Trait: system::Trait {}

decl_storage! {
    trait Store for Module<T: Trait> as KittyStorage {
        // Declare storage and getter functions here
    }
}

decl_module! {
    pub struct Module<T: Trait> for enum Call where origin: T::Origin {
        // Declare public functions here
    }
}
```

You can see that this template allows us to start writing the most basic parts of our module, the public functions and the storage.

你可以看到，该模板允许我们开始编写模块的最基本部分, 公共函数和存储。

But before we even start doing that, we should include this file into our overall runtime which is defined in the `lib.rs` file located in the same directory.

但在我们开始这样做之前，我们应该将此文件包含在我们的整个 runtime 中，runtime 被定义在同一目录中的 `lib.rs` 文件中。

## Updating our Runtime

If you take a closer look at the `lib.rs` file, you will notice it contains details about all the modules that make up your runtime. For each module, we:

- Import the Rust file containing the module
- Implement its `Trait`
- Include the module into the `construct_runtime!` macro

如果仔细查看 `lib.rs` 文件，你会注意到它包含有关构成 runtime 的所有模块细节。对于每个模块，我们：

- 导入包含该模块的 Rust 文件
- 实施其 `Trait`
- 将模块包含在 `construct_runtime!` 宏中

So we will need to do the same here.

所以我们需要在这里做同样的事情。

To include the new module file we created, we can add the following line (indicated with the `// Add this line` comment) near the top of our file:

要包含我们创建的新模块文件，我们可以在文件顶部附近添加以下行（注释用 `// Add this line` 表示）:

```rust
// `lib.rs`
...
pub type BlockNumber = u64;

pub type Nonce = u64;

// Add this line
mod substratekitties;
...
```

Since we have not defined anything in our module, our `Trait` implementation is also very simple. We can include this line after the other trait implementations:

由于我们没有在模块中定义任何内容，因此我们的 `Trait` 实现也非常简单。我们可以在其他 trait 实现之后包含这一行：

```rust
// `lib.rs`
...
impl sudo::Trait for Runtime {
	type Event = Event;
	type Proposal = Call;
}

// Add this line
impl substratekitties::Trait for Runtime {}
...
```

Finally, we can add this line at the end of our `construct_runtime!` definition:

最后，我们可以在 `construct_runtime!` 的末尾添加这一行定义：

```rust
// `lib.rs`
...
construct_runtime!(
	pub enum Runtime with Log(InternalLog: DigestItem<Hash, Ed25519AuthorityId>) where
		Block = Block,
		NodeBlock = opaque::Block,
		UncheckedExtrinsic = UncheckedExtrinsic
	{
		System: system::{default, Log(ChangesTrieRoot)},
		Timestamp: timestamp::{Module, Call, Storage, Config<T>, Inherent},
		Consensus: consensus::{Module, Call, Storage, Config<T>, Log(AuthoritiesChange), Inherent},
		Aura: aura::{Module},
		Indices: indices,
		Balances: balances,
		Sudo: sudo,
		// Add this line
		Substratekitties: substratekitties::{Module, Call, Storage},
	}
);
...
```

Note that we have added three `types` to this definition (`Module`, `Call`, `Storage`), all of which are produced by the macros defined in our template.

请注意，我们在此定义中添加了三种类型（`Module`，`Call`，`Storage`），所有这些类型都是由模板中定义的宏生成的。

As is, this code is valid and should compile. Give it a shot with:

这样，这段代码是有效的，并且应该能编译通过。试一试：

```bash
./build.sh
cargo build --release
```

## Your Turn!

If you have not already, follow the instructions on this page to set up your `substrate-node-template`. If you completed everything successfully, you should be able to compile your code successfully without errors:

如果你还没有准备好，请按照此页面上的说明设置你的 `substrate-node-template`。如果你已成功完成所有操作，则应该能够成功编译代码而不会出现错误：

```bash
./build.sh
cargo build --release
```

At the end of every section in this tutorial, your code should compile without errors. Most of the changes throughout this tutorial will take place in the `substratekitties.rs` file, but we will need to update the `lib.rs` file one more time at a later point.

在本教程的每个部分的末尾，你的代码在编译时应该没有错误。本教程中的大多数更改都将在 `substratekities.rs` 文件中进行，但我们需要稍后再次更新 `lib.rs` 文件。

Now it's time to start adding some of our own logic!

现在是时候开始添加一些我们自己的逻辑了！

<!-- tabs:start -->

#### ** Solution **

[embedded-code](./assets/1.1-finished-code.rs ':include :type=code embed-final')

<!-- tabs:end -->

---
**Learn More**

Check out the documentation for the `construct_runtime!` macro [here](https://substrate.readme.io/docs/construct_runtime).

查看 `construct_runtime!` 宏的[文档](https://substrate.readme.io/docs/construct_runtime)

---
