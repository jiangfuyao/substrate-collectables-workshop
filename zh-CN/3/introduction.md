Introduction
===

At this point you have learned most of the core pieces of Substrate runtime development. However, our runtime is not very interesting as is.

此时，你已经了解了 Substrate runtime 开发的大部分核心内容。但是，我们的 runtime 不是很有趣。

This next section will teach you how you can add additional functions to your runtime to enable some of the most popular features of the original CryptoKitties game:

- Transferring a kitty
- Buying a kitty
- Breeding a kitty

下一节将教你如何向 runtime 添加其他功能，以启用原始 CryptoKitties 游戏的一些最流行的功能：

- 转让 kitty
- 买一只 kitty
- 养一只 kitty

To enable these features, we will need to interact with other modules like the `Balances` module from the SRML. We will also continue to work more with Substrate specific types.

要启用这些功能，我们需要与 SRML 中的 `Balances` 模块等其他模块进行交互。我们还将继续使用 Substrate 特定类型进行更多工作。
