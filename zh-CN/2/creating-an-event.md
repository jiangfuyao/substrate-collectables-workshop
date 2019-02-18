Creating an Event
===

On Substrate, [**Transactions**](https://substrate.readme.io/docs/glossary#section-transaction) are handled differently than you might have been used to on Ethereum. Even though a transaction may be finalized, it does not necessarily imply that the function executed by that transaction fully succeeded.

To know that, we should emit an [**`Event`**](https://substrate.readme.io/docs/glossary#section-events) at the end of the function to not only report success, but to tell the "off-chain world" that some particular state transition has happened.

在Substrate上，[**Transactions**](https://substrate.readme.io/docs/glossary#section-transaction) 的处理方式与以往在以太坊上的处理方式不同。即使 transaction 可能已经完成，但并不一定意味着该 transaction 执行的函数完全成功。

为了知道执行函数是否完全成功，我们应该在函数结束时发出一个 [**`Event`**](https://substrate.readme.io/docs/glossary#section-events)，不仅要报告成功，还要告诉 “off-chain world” 某些特定的状态转换已经发生了。

## Declaring an Event

Substrate provides a `decl_event!` macro which allows you to easily create events which you can deposit in your runtime logic.

Substrate 提供了一个 `decl_event!` 宏，它允许你轻松创建可以存储在 runtime 逻辑中的 event。

Here is an example of an event declaration:

以下是声明 event 的示例：

```rust
decl_event!(
    pub enum Event<T>
    where
        <T as system::Trait>::AccountId,
        <T as system::Trait>::Balance
    {
        MyEvent(u32, Balance),
        MyOtherEvent(Balance, AccountId),
    }
);
```

Again, if we want to use some custom Substrate types, we need to integrate generics into our event definition.

同样，如果我们想要使用一些自定义 Substrate 类型，我们需要将泛型集成到我们的 event 定义中。

## Adding an Event Type

The `decl_event!` macro will generate a new `Event` type which you will need to expose in your module. This type will need to inherit some traits like so:

`decl_event!` 宏将生成一个新的 `Event` 类型，你需要在模块中暴露出它。这种类型需要继承一些特征，如下所示：

```rust
pub trait Trait: balances::Trait {
    type Event: From<Event<Self>> + Into<<Self as system::Trait>::Event>;
}
```

## Depositing an Event

In order to use events within your runtime, you need to add a function which deposits those events. Since this is a common pattern in runtime development, the [**`decl_module!`**](https://github.com/paritytech/wiki/pull/272) macro can automatically add a default implementation of this to your module.

为了在 runtime 中使用 event，你需要添加一个存放这些 event 的函数。由于这是 runtime 开发中的常见模式，因此 `decl_module!` 宏可以自动添加一个默认实现到你的模块中。

Simply add a new function to your module like so:

只需在模块中添加一个新函数，如下所示：

```rust
fn deposit_event<T>() = default;
```

If your events do not use any generics (e.g. just Rust primitive types), you should omit the `<T>` like this:

如果你的 event 不使用任何泛型（例如只是 Rust 原始类型），你应该省略 `<T>`，如下所示：

```rust
fn deposit_event() = default;
```

The `decl_module!` macro will detect this line, and replace it with the appropriate function definition for your runtime.

`decl_module!` 宏将会检测出此行，在你的 runtime 中用合适的函数定义来替换它。

### Calling `deposit_event()`

Now that you have things set up within your module, you may want to actually deposit the event at the end of your function.

现在你已经在模块中构建了东西，你可能想要在函数结束时 deposit event。

Doing that is relatively simple, you just need to provide the values that go along with your `Event` definition:

这样做相对简单，你只需提供与 `Event` 定义一起提供的值：

```rust
let my_value = 1337;
let my_balance = <T::Balance as As<u64>>::sa(1337);

Self::deposit_event(RawEvent::MyEvent(my_value, my_balance);
```

## Updating `lib.rs` to Include Events

The last step to get your runtime to compile is to update the `lib.rs` file to include the new `Event` type you have defined.

让 runtime 编译成功的最后一步是更新 `lib.rs` 文件以包含你定义的新事件类型。

In your modules `Trait` implementation, you need to include:

在你的模块的 `Trait` 实现中，你需要包括：

```rust
// `lib.rs`
...
impl mymodule::Trait for Runtime {
    type Event = Event;
}
...
```

Finally, you need to also include the `Event` or `Event<T>` type to your module's definition in the `construct_runtime!` macro. Which one you include depends again on whether your event uses any generics.

最后，你还需要在 `construct_runtime!` 宏中包含 `Event` 或`Event<T>` 类型到你的模块定义中。你包含哪一个取决于你的 event 是否使用任何泛型。

```rust
// `lib.rs`
...
construct_runtime!(
    pub enum Runtime with Log(InternalLog: DigestItem<Hash, Ed25519AuthorityId>) where
        Block = Block,
        NodeBlock = opaque::Block,
        InherentData = BasicInherentData
    {
        ...
        MyModule: mymodule::{Module, Call, Storage, Event<T>},
    }
);
...
```

## Your Turn!

You will need to update your module to support events. We have taught you all the different parts, you will just need to piece it together.

Use the instructions in the template of this section to help you get your module talking! Remember you will need to update `lib.rs` too!

你需要更新模块以支持 event。我们已经教了你所有不同的部分，你只需要把它拼凑起来。

使用本节模板中的说明来帮助你的模块实际运行起来! 记住你也需要更新 `lib.rs`！

<!-- tabs:start -->

#### ** Template **

[embedded-code-template](./assets/2.2-template.rs ':include :type=code embed-template')

#### ** Solution **

[embedded-code-final](./assets/2.2-finished-code.rs ':include :type=code embed-final')

<!-- tabs:end -->
