Introduction
===

[CryptoKitties](https://www.cryptokitties.co/) is a popular Ethereum dApp that at one point accounted for more than 20% of all incoming transactions on the Ethereum blockchain. In this tutorial, we will try to follow in their footsteps and create the next viral application on Substrate.

In this section, we will show you how to create a runtime which allows users create and own non-fungible tokens.

[CryptoKitties](https://www.cryptokitties.co/) 是一款流行的以太坊 dApp，它在以太网区块链的所有发起交易中占据了20％以上的份额。在本教程中，我们将尝试跟随他们的脚步并在 Substrate上 创建下一个如病毒传播式的应用程序。

在本节中，我们将向你展示如何创建一个 runtime, 使得该 runtime 允许用户创建和拥有不可替换的 tokens。

## Non-Fungible Token

CryptoKitties and other similar dApps use *non-fungible tokens* to represent their assets. A non-fungible token (NFT) is a token which is unique and distinguishable in nature. Whereas you can trade your Bitcoin for someone elses Bitcoin without really changing anything, when you create a kitty on CryptoKitties, you are generating a one of a kind, unique and irreplaceable item.

CryptoKitties 和其他类似的 dApp 使用 *不可替代的 tokens* 来表示其资产。不可替代的 token（NFT）是一种独特且可区分的 token。尽管你可以在没有真正改变任何东西的情况下与他人交易比特币，但是​​当你在 CryptoKitties 上创建 kitty 时，你将生成一种独特且不可替代的 token。

NFTs are particularly exciting not just because they are unique, but because of the things you can do with them! You can own a token, trade your tokens, buy or sell tokens, track the specific history of a token, and even have tokens interact with one another.

NFTs 特别令人兴奋，不只是因为它们是独一无二的，而是你可以用它们做的事情令人兴奋！ 你可以拥有 token，交易 token，买入或卖出 token，跟踪 token 的特定历史记录，甚至可以让 token 彼此交互。

## ERC-721

[ERC-721](http://erc721.org/) is an extremely popular Ethereum token standard for non-fungible tokens. Since this standard is widely used, and there have been numerous smart contracts built using this standard, we will be using it as a rough basis for how our app should be structured.

Specifically, we will look to the [OpenZeppelin implementation](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/token/ERC721/ERC721.sol), avoiding some of the more unnecessary features like "token approvals".

对不可替代的 tokens 来说， [ERC-721](http://erc721.org/) 是一款非常受欢迎的以太坊 token 标准。由于该标准被广泛使用，并且已经使用该标准建立了许多智能合约，所以我们会使用它作为粗略基础来构建我们的应用程序。

具体来说，我们将关注 [OpenZeppelin 实现](https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/token/ERC721/ERC721.sol)，避免一些不必要的功能，如 “token approvals”。
