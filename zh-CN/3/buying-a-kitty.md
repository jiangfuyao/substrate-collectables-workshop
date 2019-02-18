# 购买一个 Kitty

现在我们可以设置 kitty 的价格并转让 kitty 的所有权，我们拥有构建 `buy_kitty` 功能所需的一切。

## 检查用于出售的 Kitty

在我们允许用户执行 `buy_kitty()` 函数之前，我们应该确保 kitty 确实可以出售。我们已经简化了我们的示例，任何默认价格为 0 的 Kitty 都不会被出售。所有者可以轻松地调用 `set_price()`， 设置他们 kitty 的价值为 0，并将其从市场上撤下。

你可以使用该类型公开的函数轻松检查 `T::Balance` 是否为零：

```rust
let my_value = <T::Balance as As<u64>>::sa(0);
ensure!(my_value.is_zero(), "Value is not zero");
// `ensure` will succeed and execution continues here
```

如果你想改善这一点，我们可能会将价格定为 `Option<T::Balance>`，其中 0 将是有效价格，而由 `None` 表示不能出售... 但我们会将其当作挑战，留给读者自己实现。

## 进行付款

到目前为止，我们的链完全独立于 `Balances` module 提供的内部货币。`Balances` module 使我们能够完全管理每个用户的内部货币，这意味着我们需要谨慎使用它。

幸运的是，`Balances` module 暴露出了一个名为 `make_transfer()` 的公有函数，它允许你安全地将 units 从一个帐户转移到另一个帐户，检查是否有足够的余额，上溢，下溢，甚至是因为获得 tokens 的账户创建。

此函数既可以 “verifies” 又可以 “writes”，因此你需要谨慎地将其作为 module 逻辑的一部分包含在内。

```rust
// end of verifications

<balances::Module<T>>::make_transfer(&from, &to, value)?;

// beginning of writing to storage
```

## 轮到你了！

按照提供的模板编程，在必要的代码中完成 `buy_kitty()` 函数。随意在 Polkadot-JS Apps UI 中测试你的新函数是否存在任何错误。

<!-- tabs:start -->

#### ** Template **

[embedded-code](./assets/3.3-template.rs ':include :type=code embed-template')

#### ** Solution **

[embedded-code-final](./assets/3.3-finished-code.rs ':include :type=code embed-final')

<!-- tabs:end -->
