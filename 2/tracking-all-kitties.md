Tracking All Kitties
===

Now that we have enabled each user to create their own unique kitty, we should probably start tracking them!

It makes sense for our game to track a total number of kitties created, as well as a way to track who owns each kitty.

现在我们已经让每个用户都可以创建自己独一无二的 kitty，我们开始跟踪它们！

我们的游戏将会跟踪创建的 kitty 总数，以及跟踪谁拥有每只 kitty。

## "Verify First, Write Last"

> IMPORTANT: This section is important
> 重要提示：此部分非常重要

As a developer building on Substrate, it is critical that you make a distinction about how you should design your runtime logic versus developing a smart contract on a platform like Ethereum.

作为基于 Substrate 的开发人员，你必须区分如何设计 runtime 逻辑，而不是像以太网那样在平台上开发智能合约。

On Ethereum, if at any point your transaction fails (error, out of gas, etc...), the state of your smart contract will be unaffected. However, on Substrate this is not the case. As soon as a transaction starts to modify the storage of the blockchain, those changes are permanent, even if the transaction would fail at a later time during runtime execution.

在以太坊，如果你的交易在任何时候失败（错误，没有 gas 等...），你的智能合约的状态将不受影响。但是，在 Substrate 上并非如此。一旦交易开始修改区块链的存储，这些更改就是永久性的，即使交易在 runtime 执行期间失败也是如此。

This is necessary for blockchain systems since you may want to track things like the nonce of a user or subtract gas fees for any computation that occurred. Both of these things actually happen in the Ethereum state transition function for failed transactions, but you have never had to worry about managing those things as a contract developer.

这对于区块链系统是必要的，因为你可能想要跟踪用户的 nonce 或者为任何发生的计算减去 gas 费用。对于失败的交易来说，这两件事实际上都发生在以太坊状态转换函数中，但你作为智能合约开发人员，从来不必担心去管理这些事情。

Now that you are a Substrate runtime developer, you will have to be conscious of any changes you make to the state of your blockchain, and ensure that it follows the **"verify first, write last"** pattern. We will be helping you do this throughout the tutorial.

既然现在你是 Substrate runtime 开发人员，你必须察觉到你对区块链状态所做的任何更改，并确保它遵循 **“verify first, write last”** 的模式。我们将在整个教程中帮助你做到这点。

## Creating a List

Substrate does not natively support a list type since it may encourage dangerous habits. In runtime development, list iteration is, generally speaking, evil. Unless explicitly guarded against, it will add unbounded O(N) complexity to an operation that will only charge O(1) fees. As a result, your chain becomes attackable. Instead, a list can be emulated with a mapping and a counter like so:

Substrate 本身不支持 list 类型，因为它可能会导致开发者养成危险的习惯。在 runtime 开发中，列表迭代通常是坏事。除非明确防范了其危险操作，否则它将为仅收取 O(1) 费用的操作添加无限制的 O(N) 复杂性。结果就是你的链变得容易被攻击。相反，可以使用映射和计数器模拟列表，如下所示：

```rust
decl_storage! {
    trait Store for Module<T: Trait> as Example {
        AllPeopleArray get(person): map u32 => T::AccountId;
        AllPeopleCount get(num_of_people): u32;
    }
}
```

Here we are storing a list of people in our runtime represented by `AccountId`s. We just need to be careful to properly maintain these storage items to keep them accurate and up to date.

这里我们将在 runtime 中存储 `AccountId` 表示的人员列表。我们只需要小心谨慎地维护这些存储项目，以确保它们准确和最新。

## Checking for Overflow/Underflow

If you have developed on Ethereum, you may be familiar with all the problems that can happen if you do not perform "safe math". Overflows and underflows are an easy way to cause your runtime to panic or for your storage to get messed up.

如果你曾经在以太坊上开发过，那么如果你不执行 “safe math”，你就会碰到你所熟悉的问题，即 Overflow/Underflow。Overflow 和 Underflow 是可以可以使你的 runtime 出现混乱或导致存储混乱的简单方法。

You must always be proactive about checking for possible runtime errors before you make changes to your storage state. Remember that unlike Ethereum, when a transaction fails, the state is NOT reverted back to before the transaction, so it is your responsibility to ensure that there are no side effects on error.

在更改存储状态之前，你必须始终主动检查可能的 runtime 错误。请记住，与以太坊不同，当交易失败时，状态不会恢复到交易发生之前，因此你有责任确保在错误处理上不会产生任何副作用。

Fortunately, checking for these kinds of errors are quite simple in Rust where primitive number types have [`checked_add()`](https://doc.rust-lang.org/std/primitive.u32.html#method.checked_add) and [`checked_sub()`](https://doc.rust-lang.org/std/primitive.u32.html#method.checked_sub) functions.

幸运的是，在 Rust 中检查这些类型的错误非常简单，其中原始数字类型具有 [`checked_add()`](https://doc.rust-lang.org/std/primitive.u32.html#method.checked_add) 和 [`checked_sub()`](https://doc.rust-lang.org/std/primitive.u32.html#method.checked_sub) 函数。

Let's say we wanted to add an item to our `AllPeopleArray`, we should first check that we can successfully increment the `AllPeopleCount` like so:

假设我们想要向 `AllPeopleArray` 中添加一项，我们首先要检查我们是否可以成功增加 `AllPeopleCount`，如下所示：

```rust
let all_people_count = Self::num_of_people();

let new_all_people_count = all_people_count.checked_add(1).ok_or("Overflow adding a new person")?;
```

Using `ok_or` is the same as writing:

使用 `ok_or` 与下面的代码相同：

```rust
let new_all_people_count = match all_people_count.checked_add(1) {
    Some (c) => c,
    None => return Err("Overflow adding a new person"),
};
```

However, `ok_or` it is more is more clear and readable than `match`; you just need to make sure to remember the `?` at the end!

但是，`ok_or` 比 `match` 更清晰可读; 你只需要确保记住 `?` 在末尾！

If we were successfully able to increment `AllPeopleCount` without an overflow, then it will simply assign the new value to `new_all_people_count`. If not, our module will return an `Err()` which can be gracefully handled by our runtime. The error message will also appear directly in our node's console output.

如果我们成功地能够在没有上溢的情况下递增 `AllPeopleCount`，那么它就会将新值分配给 `new_all_people_count`。如果失败，我们的模块将返回一个 `Err()`，它可以由我们的 runtime 优雅地处理。错误消息也将直接显示在节点的控制台输出中。

## Updating our List in Storage

Now that we have checked that we can safely increment our list, we can finally push changes to our storage. Remember that when you update your list, the "last index" of your list is one less than the count. For example, in a list with 2 items, the first item is index 0, and the second item is index 1.

现在我们已经检查过我们可以安全地增加列表，我们最终可以将更改推送到存储中。请记住，当你更新列表时，列表的 “最后一个索引” 比计数少一个。例如，在包含 2 个项目的列表中，第一个项目是索引 0，第二个项目是索引 1。

A complete example of adding a new person to our list of people would look like:

将新人添加到我们的人员列表中的完整示例如下所示：

```rust
fn add_person(origin, new_person: T::AccountId) -> Result {
    let sender = ensure_signed(origin)?;

    let all_people_count = Self::num_of_friends();
    
    let new_all_people_count = all_people_count.checked_add(1).ok_or("Overflow adding a new person")?;

    <AllPeopleArray<T>>::insert(all_people_count, new_people);
    <AllPeopleCount<T>>::put(new_all_people_count);

    Ok(())
}
```

We should probably add collision detection to this function too! Do you remember how to do that?

我们也应该为这个函数添加碰撞检测！你还记得怎么做吗？

## Deleting From Our List

One problem that this `map` and `count` pattern introduces is holes in our list when we try to remove elements from the middle. Fortunately, the order of the list we want to manage in this tutorial is not important, so we can use a "swap and pop" method to efficiently mitigate this issue.

当我们尝试从中间删除元素时，映射和计数模式引入的一个问题就是会在列表中留下空位。幸运的是，我们想要在本教程中管理的列表顺序并不重要，因此我们可以使用 “swap and pop” 的方法来有效地缓解此问题。

The "swap and pop" method switches the position of the item we want to remove and the last item in our list. Then, we can simply remove the last item without introducing any holes to our list.

“swap and pop” 方法交换删除项的位置以及列表中的最后一项。然后，我们可以简单地删除最后一项而不会在我们的列表中引入任何空位。

Rather than run a loop to find the index of the item we want to remove each time we remove an item, we will use a little extra storage to keep track of each item and its position in our list.

我们不会在每次删除时运行循环来查找删除项的索引，而是使用一些额外的存储来跟踪列表中每个项及其所在的位置。

We won't introduce the logic for "swap and pop" until later, but we will ask you to start tracking the index of each item using an `Index` storage like this example:

我们现在不会引入 “swap and pop” 的逻辑，但是我们会要求你使用一个 `Index` 存储项来跟踪列表中每项的索引，如下所示：

```rust
AllPeopleIndex: map T::AccountId => u32;
```

This is really just an inverse of the content in `AllPeopleArray`. Note that we do not need a getter function here since this storage item is used internally, and does not need to be exposed as part of our modules API.

这实际上只是 `AllPeopleArray` 中内容的反转。请注意，我们这里不需要 getter 函数，因为此存储项只在内部使用，并且不需要作为模块 API 的一部分公开。

## Your Turn!

Now that you know what is necessary to keep track of all of your kitties, let's update your runtime to do this.

Follow the template to add new storage items, and update your `create_kitty` function to **"verify first, write last"** whenever a new kitty is created.

现在你已经知道了跟踪所有 kitties 的必要性，让我们更新你的 runtime 来执行此操作。

按照模板添加新的存储项，并在创建新的 kitty 时将 `create_kitty` 函数更新为 **"verify first, write last"**。

<!-- tabs:start -->

#### ** Template **

[embedded-code](./assets/2.3-template.rs ':include :type=code embed-template')

#### ** Solution **

[embedded-code-final](./assets/2.3-finished-code.rs ':include :type=code embed-final')

<!-- tabs:end -->

---
**Learn More**

Talk about cloning and borrowing...

[TODO: make this a page]

谈论 Rust 中 clone 和 borrow...

---