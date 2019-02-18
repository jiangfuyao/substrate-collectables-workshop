Playing Our Game
===

If you have made it this far, **congratulations**!

如果你已经完成到这一步了，那么 **恭喜你**！

You have now completed the development of your Substratekitties runtime module. If you followed our instructions carefully and made all of the right checks, your code should look something like what we have shown here.

你现在已经完成了 Substratekitties runtime 模块的开发。如果你仔细按照我们的说明进行了所有正确的检查，那么你的代码应该与我们在此处显示的内容类似。

This would be a good time to check your work using the Polkadot-JS Apps UI, ensuring you have not run into any errors or introduced any problems.

现在是使用 Polkadot-JS Apps UI 检查成果的好时机，确保你没有遇到任何错误或引入任何问题。

## Manual Tests

You should run the following manual tests:

- Fund multiple users with tokens so they can all participate
- Have each user create multiple kitties
- Try to transfer a kitty from one user to another using the right and wrong owner
- Try to set the price of a kitty using the right and wrong owner
- Buy a kitty using an owner and another user
- Use too little funds to purchase a kitty
- Overspend on the cost of the kitty and ensure that the balance is reduced appropriately
- Breed a kitty and check that the new DNA is a mix of the old and new
- After all of these actions, confirm that all users have the right number of kitties, the total kitty count is correct, and any other storage variables are correctly represented

你应该运行以下手动测试：

- 使用 token 为多个用户提供资金，以便他们都可以参与
- 让每个用户创建多个 kitties
- 通过使用正确和错误的所有者，尝试将 kitty 从一个用户转移给另一个用户
- 通过使用正确和错误的所有者，尝试设置 kitty 的价格
- 使用所有者和其他用户购买 kitty
- 使用不足的资金购买 kitty
- 高价购买 kitty，确保余额被适当减少
- 培育 kitty，检查新 DNA 是新旧混合物
- 完成所有这些操作后，确认所有用户都具有正确数量的 kitty，确认 kitty 总数正确，并且所有其他存储变量都被正确表示

## Challenge

Our challenge to the reader is to extend the Substratekitties runtime and include additional functions and features you might want to see in this runtime module.

我们向读者提出的挑战是扩展 Substratekitties runtime，并包含你可能希望在此 runtime 模块中看到的其他功能和特性。

Here are some ideas:

- Track the parents of a kitty during when `breed_kitty()` is called. Maybe as just an event...?

- Limit the number of kitties that can be created by `create_kitty()`, and/or add a price curve to make each new kitty cost more to create.

- Add a cost to breeding kitties that the person receiving the new kitty has to pay. Make sure funds are correctly sent to each user. Make sure a person can breed using their own kitties.

- Add a kitty "fight" where two kitties can compete to win a game based on a random number and some weighted statistic. The winning kitty has their stats increase, giving them a better chance to win the next round.

- Add a gene mutation algorithm to kitty breeding which will introduce traits that neither of the parent kitties had.

- Introduce an auction system for owners who do not want to simply set a fix price to purchase their kitty. Auctions close after a period of time has passed where no one has placed a bid.

以下是一些想法：

- 在调用 `breed_kitty()` 期间跟踪 kitty 的父母。也许只是一个 event...？

- 限制 `create_kitty()` 可以创建的 kitties 数量，和/或添加价格曲线以使得创建每个新的 kitty 成本更高。

- 培育新 kitty 时收到这只新 kitty 的人必须支付一定的费用。确保资金正确发送给每个用户。确保每个人都可以使用自己的 kitty 进行培育。

- 添加一个 kitty “fight”，其中两个 kitties 可以竞争赢得基于随机数和一些加权统计的游戏。胜利的 kitty 的数据有所增加，这使他们有更好的机会赢得下一轮比赛。

- 添加基因突变算法到 kitty 培育中，这将引入父母 kitties 都没有的特征。

- 为那些不想简单地设定定价以购买 kitty 的所有者推出拍卖系统。如果没有人投标则一段时间后拍卖结束。

What other ideas can you think of?

你还能想到其他什么想法？

In the next section we will be creating a custom UI for this game using Substrate JavaScript libraries. This custom UI is sensitive to changes in naming conventions, so you may need to copy the final Substratekitties code to ensure compatibility.

在下一节中，我们将使用 Substrate JavaScript 库为此游戏创建自定义 UI。此自定义 UI 对命名约定的更改很敏感，因此你可能需要复制最终的 Substratekitties 代码以确保兼容性。

<!-- tabs:start -->

#### ** Solution **

[embedded-code-final](./assets/3.5-finished-code.rs ':include :type=code embed-final')

<!-- tabs:end -->