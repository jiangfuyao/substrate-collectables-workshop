Set the Price of a Kitty
===

The core of our collectables chain is done! Users can now create and track ownership of their Substratekitties.

我们链的核心已经完成！用户现在可以创建和追踪他们的 Substratekitties 的所有权。

But if we want to make our runtime more like a game, we will need to introduce other functions like buying and selling. We will start those features by first enabling users to update the price of their kitty.

但如果我们想让我们的 runtime 更像游戏，我们需要引入其他功能，如买卖。我们将首先让用户更新其 kitty 的价格来启动这些功能。

## Updating a Stored Struct

Every `Kitty` object has a `price` attribute that we have set to 0 as default. If we want to update the price of a kitty, we will need to pull down the `Kitty` object, update the price, and push it back into storage.

每个 `Kitty` 对象都有一个我们默认设置为 0 的 `price` 属性。如果我们想要更新 kitty 的价格，我们需要下拉 `Kitty` 对象，更新价格，并将其推回存储中。

Remember that Rust expects you to declare a variable as mutable (`mut`) if the value is going to be updated, so we should do that here:

请记住，如果要更新值，Rust 希望你将变量声明为 mutable (`mut`)，因此我们应该在此处执行此操作：

```rust
let mut object = Self::get_object(object_id);
object.value = new_value;

<Objects<T>>::insert(object_id, object);
```

## Permissioned Functions

Any user can call our `create_kitty()` function with a signed message, but as we create functions which modify objects, we should check that only the appropriate users are successful in making those calls.

任何用户都可以使用签名消息调用我们的 `create_kitty（）` 函数，但是当我们创建修改对象的函数时，我们应该确认只有适当的用户才能成功进行这些调用。

For modifying a `Kitty` object, we will need to get the `owner` of kitty, and `ensure` that it is the same as the `sender`.

要修改 `Kitty` 对象，我们需要获取 kitty 的 `owner`，并 `ensure` 它与 `sender` 相同。

`KittyOwner` stores a mapping to an `Option<T::AccountId>` since a given `Hash` may not point to a generated and owned `Kitty` yet. This means, whenever we fetch the `owner` of a kitty, we need to resolve the possibility that it returns `None`. This could be caused by bad user input or even some sort of problem with our runtime, but checking will help prevent these kinds of problems.

`KittyOwner` 将映射存储到 `Option<T::AccountId>`，因为给定的 `Hash` 可能不会指向一个生成和拥有的 `Kitty`。这意味着，每当我们获取 kitty 的所有者时，我们需要解决它返回 `None` 的可能性。这可能是由于用户输入错误甚至是 runtime 的某些问题造成的，但检查有助于防止出现这类问题

An ownership check for our module will look something like this:

我们模块的所有权检查将如下所示：

```rust
let owner = Self::owner_of(object_id).ok_or("No owner for this object")?;

ensure!(owner == sender, "You are not the owner");
```

## Sanity Checks

We are going to start letting users call public functions that our runtime exposes, and that means opportunity for our users to give poor input unintentionally or even maliciously.

我们开始让用户调用我们在 runtime 中暴露出的公共函数，这意味着我们的用户有机会无意或者甚至是恶意地提供不良输入。

We need to ensure that our runtime is consistently doing sanity checks so that we do not allow users to break our chain. If we are creating a function which updates the value of an object, the first thing we better do is make sure the object exists at all.

我们需要确保我们的 runtime 始终进行健全性检查，以至于不允许用户破坏我们的链。如果我们要创建一个更新对象的值的函数，我们做的第一件事最好就是确保对象存在。

```rust
ensure!(<MyObject<T>>::exists(object_id));
```

## Your Turn!

You now have all the tools necessary to update your `Kitty` object. Remember, **"verify first, write last"**. Make sure you are performing all the appropriate checks before you actually change storage, and do not assume that the user is giving you good data to work with!

Follow the template in this section to help you create the `set_price()` function.

现在拥有更新 `Kitty` 对象所需的所有工具了。请记住，**"verify first, write last"**。确保在实际更改存储之前执行所有相应的检查，并且不要假设用户为你提供了良好的数据！

按照本节中的模板来帮助你创建 `set_price()` 函数。

<!-- tabs:start -->

#### ** Template **

[embedded-code](./assets/3.1-template.rs ':include :type=code embed-template')

#### ** Solution **

[embedded-code-final](./assets/3.1-finished-code.rs ':include :type=code embed-final')

<!-- tabs:end -->