Owning Multiple Kitties
===

Have you noticed a problem with our runtime? What would happen if the same user called the `create_kitty()` function?

Right now our storage can only track one kitty per user, but our total supply of kitties will keep going up! Let's fix that by enabling users to own multiple kitties!

你是否注意到我们的 runtime 存在问题？如果同一个用户调用 `create_kitty()` 函数会发生什么？

现在我们的存储只能跟踪每个用户的一个 kitty，但我们的 kitty 总供应量将继续上升！让我们通过让用户拥有多个 kitty 来解决这个问题！

## Using Tuples to Emulate Higher Order Arrays

We are going to need to introduce some more complex storage items to represent ownership of multiple items across multiple users...

我们需要引入一些更复杂的存储项来表示多个用户对多个项目的所有权。

Fortunately, for our needs, using a [tuple](https://doc.rust-lang.org/rust-by-example/primitives/tuples.html) of `AccountId` and `Index` can pretty much solve our problems here

幸运的是，根据我们的需求，使用 `AccountId` 和 `Index` 元组几乎就可以解决我们的问题了。

Here is how we could build a "friends list" unique to each person using such a structure:

以下是我们如何使用这样的结构构建每个人独有的 “朋友列表”：

```rust
MyFriendsArray get(my_friends_array): map (T::AccountId, u32) => T::AccountId;
MyFriendsCount get(my_friends_count): map T::AccountId => u32;
```

This should emulate a more standard two-dimensional array like:

这应该模拟一个标准的二维数组，如：

```rust
MyFriendsArray[AccountId][Index] -> AccountId
```

Also we can get the number of friends for a user like:

我们也可以获取用户的朋友数量：

```rust
MyFriendsArray[AccountId].length()
```

## Relative Index

Just as before, we can optimize the computational work our runtime needs to do by indexing the location of items. The general approach to this would be to reverse the mapping of `MyFriendsArray`, and create a storage item like this:

与以前一样，我们可以通过索引项目的位置来优化 runtime 需要执行的计算工作。一般的方法是反转 `MyFriendsArray` 的映射，并创建一个这样的存储项：

```rust
MyFriendsIndex: map (T::AccountId, T::AccountId) => u32;
```

Where the `AccountId`s would represent the user, their friend, and the returned value would be the index of `MyFriendsArray` for that user where the friend is stored.

如果 `AccountId` 代表用户和他们的朋友, 那么返回值将是 `MyFriendsArray` 的索引，即该用户的朋友在朋友列表中的索引。

However, since our kitties all have unique identifiers as a `Hash`, and cannot be owned by more than one user, we can actually simplify this structure:

但是，由于我们的 kitty 都具有作为 `Hash` 的唯一标识符，并且不能被多个用户所拥有，所以我们实际上可以简化此结构

```rust
MyKittiesIndex: map T::Hash => u32;
```

This index tells us for a given kitty, where to look in the *owners* array for that item.

这个索引告诉了我们对于一个给定的 kitty，可以在哪里查看该项目的 *所有者* 数组。

## Your Turn!

There should be nothing too surprising here... just doing some more accounting.

Follow the template to introduce our final set of storage items, and make sure the `create_kitty()` function manages these storage items correctly. It should feel very similar to the previous section.

这里应该没有什么太令人惊讶的了... 只是做了一会儿会计。

按照模板引入了我们最终的一系列存储项，并确保 `create_kitty()` 函数正确管理这些存储项。它应该与上一节非常相似。

<!-- tabs:start -->

#### ** Template **

[embedded-code](./assets/2.4-template.rs ':include :type=code embed-template')

#### ** Solution **

[embedded-code-final](./assets/2.4-finished-code.rs ':include :type=code embed-final')

<!-- tabs:end -->

---
**Learn More**

Talk about Option<T::AccountID>

[TODO: make this a page]

谈谈 Option<T::AccountID>

---