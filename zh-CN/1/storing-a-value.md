Storing a Value
===

Now that we have our storage value declared in our runtime, we can actually create a function to push a value to it.

现在我们已经在 runtime 中声明了存储值，我们实际上可以创建一个函数来将值推送到存储中。

## Declaring a Public Function

We need to define runtime functions that will set and modify our storage values. This can be done within our `decl_module!` macro, which declares all the entry points that your module handles.

我们需要定义将设置和修改存储值的 runtime 函数。这可以在我们的 `decl_module!` 宏中完成，该宏声明了模块用于处理逻辑的所有入口。

Here is an example of an exposed function declaration:

以下是声明公开函数的示例：

```rust
decl_module! {
    pub struct Module<T: Trait> for enum Call where origin: T::Origin {

        fn my_function(origin, input_bool: bool) -> Result {
            let _sender = ensure_signed(origin)?;

            <MyBool<T>>::put(input_bool);
            
            Ok(())
        }
    }
}
```

## Function Structure

Module functions exposed here should always take the form:

此处公开的模块函数应始终采用以下形式：

```rust
fn foo(origin, bar: Bar, baz: Baz, ...) -> Result;
```

### Origin

The first argument of these functions is always `origin`. `origin` contains information about where the call originated from. This is generally split into three groups:

- Public calls that are signed by an external account.
- Root calls that are allowed to be made only by the governance system.
- Inherent calls that are allowed to be made only by the block authors and validators.

Refer to definition of [Origin](https://substrate.readme.io/docs/glossary#section-origin) in the Substrate Glossary.

这些函数的第一个参数始终是 `origin`。 `origin` 包含有关调用来源的信息。通常分为三组：

- 由外部帐户签名的 public 调用。
- 允许仅由治理系统进行的 root 调用。
- 允许仅由块作者和验证者进行的 inherent 调用。

请参阅 Substrate 术语表中的 [Origin](https://substrate.readme.io/docs/glossary#section-origin) 定义。

### Result

Additionally, these functions must return the `Result` type from the `support::dispatch` module. This means that a successful function call will always return `Ok(())`, otherwise, the logic should catch any errors which may cause a problem and return an `Err()`.

此外，这些函数必须返回 `support::dispatch` 模块中的 `Result` 类型。这意味着成功的函数调用将始终返回 `Ok(())`，否则应捕获可能导致问题的任何错误并返回 `Err()`。

Since these are dispatched functions, there are two extremely important things to remember:

- MUST NOT PANIC: Under no circumstances (save, perhaps, storage getting into an irreparably damaged state) must this function panic.
- NO SIDE-EFFECTS ON ERROR: This function must either complete totally and return `Ok(())`, or it must have no side-effects on storage and return `Err('Some reason')`.

由于这些是调度函数，因此需要记住两件非常重要的事情：

- MUST NOT PANIC: 在任何情况下（保存，或者存储进入一个不可挽回的损坏状态）函数都不能 panic。
- NO SIDE-EFFECTS ON ERROR: 此函数要么完全完成并返回 `Ok(())`，要么它必须对存储没有副作用并返回 `Err('某些原因'）`。

We will talk about these more later. Throughout this tutorial, we will make sure that both of these conditions are satisfied, and we will remind you to do the same.

我们稍后会谈到这些。在本教程中，我们将确保满足这两个条件，并且我们也提醒也这样做。

## Checking for a Signed Message

As mentioned, the first argument in any of these module functions is the `origin`. There are three convenience calls in `system` that do the matching for you and return a convenient result: `ensure_signed`, `ensure_root` and `ensure_inherent`. **You should always match against them as the first thing you do in your function.**

如上所述，任何这些模块函数中的第一个参数是 `origin`。`system` 中有三个方便的调用函数 `ensure_signed`, `ensure_root` 和 `ensure_inherent`，可以调用三者中匹配的函数并返回一个方便的结果。**在你的函数中做的第一件事应该总是从这三个调用函数选择匹配的函数。**

We can use the `ensure_signed()` function from `system::ensure_signed` to check the origin, and "ensure" that the messaged is signed by a valid account. We can even derive the signing account from the result of the function call as shown in the example function above.

我们可以使用 `system` 中的 `ensure_signed()` 函数来检查 origin，并 “ensure” 消息是由有效帐户签名的。我们甚至可以从函数调用的结果中派生签名帐户，如上面的示例函数所示。

## Your Turn!

Use the template to create a `set_value()` function which will allow a user to send a signed message which puts a `u64` into the runtime storage.

使用该模板创建一个 `set_value()` 函数，该函数允许用户发送一条签名消息，该消息将 `u64` 放入 runtime 存储中。

<!-- tabs:start -->

#### ** Template **

[embedded-code](./assets/1.3-template.rs ':include :type=code embed-template')

#### ** Solution **

[embedded-code-final](./assets/1.3-finished-code.rs ':include :type=code embed-final')

<!-- tabs:end -->

---
**Learn More**

If you try to compile the code samples we showed above without importing the required libraries, you will get some errors:

如果你尝试编译我们上面显示的代码示例而不导入所需的库，你将收到一些错误：

```rust
error[E0425]: cannot find function `ensure_signed` in this scope
  --> src/substratekitties.rs:15:27
   |
15 |             let _sender = ensure_signed(origin)?;
   |                           ^^^^^^^^^^^^^ not found in this scope
help: possible candidate is found in another module, you can import it into scope
   |
2  | use system::ensure_signed;
   |
```

As you can see, we added some functionality in our code that we had not yet imported into our module. Rust actually can help us here by suggesting ways to solve these problems. If we listen to Rust, then we can simply add these `use` statements at the very top to get our code to compile again:

如你所见，我们在代码中添加了一些尚未导入模块的功能。Rust 实际上可以通过错误消息里建议来帮助我们解决问题。如果我们听从 Rust，那么我们可以简单地在最顶层添加这些 `use `语句来让我们的代码再次编译通过：

```rust
use system::ensure_signed;
```

As mentioned in "common patterns" section, Rust will be your friend throughout runtime development, and *should mostly* help you overcome any issues in your code. Moving forward we will try to mention whenever you need to import a new library, but don't be worried when the compiler sends you some errors. Instead embrace the helpful suggestions it might be giving you.

正如 “[common patterns](../0/common-patterns-moving-forward.md)” 部分所述，Rust 在整个 runtime 开发过程中将成为你的朋友，并且可以帮助你克服代码中的 *大部分* 问题。

---