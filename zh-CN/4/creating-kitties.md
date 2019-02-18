Creating Kitties
===

Now that we have gotten a taste for bonds and accessing runtime storage from our UI, let's actually build some interaction.

现在既然我们已经尝试了绑定并从 UI 访问 runtime 存储，那么让我们实际构建一些交互。

## Calling Our Runtime

In the last section we explored the `runtime` variable created by the Substrate UI. Now let's take a look at the `calls` variable:

在上一节中，我们研究了 Substrate UI 创建的 runtime 变量。现在让我们来看看如何 `calls` 变量：

```
calls.
```

![An image of the `calls` autocomplete](./assets/calls-autocomplete.png)

Again, we see that we automatically gain access to all of our modules, including the one we just created. Diving into the `substratekitties` we find:

我们再次看到，我们可以自动访问所有模块，包括我们刚刚创建的模块。深入了解 `substratekitties`，我们发现：

```
calls.substratekitties.
```

![An image of `calls.substratekitties` autocomplete](./assets/calls-substratekitties-autocomplete.png)

This is a list of all the functions we created in the `decl_module!` macro. Note that our private functions like `_mint()` and `_transfer()` are not here since they are not supposed to be part of our public API.

这是我们在 `decl_module!` 宏中创建的所有函数的列表。请注意，我们的私有函数如 `_mint()` 和 `_transfer()` 不在此处，因为它们不应该是我们的公共 API 的一部分。

## Making a Call

So let's try to make a call to create a new kitty. To do that, we can make a `post()` request to our runtime like so:

所以我们试着创建一个 call 来创建一个新的 kitty。为此，我们可以向 runtime 发出 `post()` 请求，如下所示：

```javascript
post({
    sender: "5GoKvZWG5ZPYL1WUovuHW3zJBWBP5eT8CbqjdRY4Q6iMaDtZ",
    call: calls.substratekitties.createKitty()
}).tie(console.log)
```

In this sample, the `sender` is the address of one of our accounts. We can retrieve this for any of our accounts in the **Address Book** section of the Substrate UI:

在此示例中，`sender` 是我们其中一个帐户的地址。我们可以在 Substrate UI 的 **Address Book** 中检索我们的所有账户：

![An image of the Address Book section](./assets/address-book.png)

When we submit this in our console, we will see a few things happen in the background, and then our transaction is `finalised` and the number of kitties increases:

当我们在控制台中提交此内容时，我们会在后台看到一些事情发生，交易最终 `finalised` 并且 kitties 数量增加：

![An image of creating a kitty from console](./assets/transaction-from-console.png)

## Creating a Transaction Button

Now that we know how to make a call to our runtime, we will want to integrate that into our UX. Again, we will take advantage of components provided to us by the Substrate UI called `TransactionButton` and `SignerBond`.

现在我们知道如何调用我们的 runtime，我们希望将它集成到我们的 UX 中。同样，我们将利用 Substrate UI 提供给我们的名为 `TransactionButton` 和 `SignerBond` 的组件。

If you look at the code for the other sections on the page, you will find examples of how to integrate these parts.

如果你查看页面上其他部分的代码，你将找到如何集成这些部分的示例。

The `SignerBond` creates an input field where a person can write the name of the account they want to sign some message. This account gets placed inside of a `bond`.

`SignerBond` 创建一个输入区域，在里面每个人可以写入他们想要签署某些消息的帐户名称。该账户被置于 `bond` 内。

```javascript
this.account = new Bond;

<SignerBond bond={this.account}/>
```

You can then use this bond to power a `TransactButton`, where we will use the value stored in the bond to power the `sender` field of a transaction:

然后你可以使用此 bond 为 `TransactButton` 提供支持，我们将使用存储在 bond 中的值为交易的 `sender` 字段提供支持：

```javascript
<TransactButton
    content="Submit Transaction"
    icon='send'
    tx={{
        sender: runtime.indices.tryIndex(this.account),
        call: calls.myModule.myFunction()
    }}
/>
```

Because the `TransactionButton` has a dependency on `this.account`, it won't be active until the `SignerBond` has a valid input. Once it does, you can easily submit a transaction on behalf of that user.

因为 `TransactionButton` 依赖于 `this.account`，所以在 `SignerBond` 具有有效输入之前它不会处于活动状态。完成后，你可以代表该用户轻松提交交易。

## Other Components

We won't go deep into each component available through the Substrate UI. However, most of these components should be relatively easy to understand and reuse in your own sections.

我们不会深入了解通过 Substrate UI 提供的每个组件。但是，大多数这些组件应该相对容易理解并能在你自己的代码里重用。

There is very little magic happening beyond what is available in [React components](https://reactjs.org/docs/react-component.html), so that would be a good place to start to expand your knowledge.

除了 [React 组件](https://reactjs.org/docs/react-component.html) 中可见的内容之外，几乎没有什么神奇的事情发生，所以这将是一个你开始扩展知识的好时候。

## Your Turn!

Let's add a `Create Kitty` button to your Substrate UI.

让我们在你的 Substrate UI 中添加一个 `Create Kitty` 按钮。

You will need to create a new bond in your `constructor()`, and use that bond to power a `SignerBond` component.

你需要在 `constructor()` 中创建一个新的 bond，并使用该 bond 为 `SignerBond` 组件提供支持。

Then you will want to connect that `SignerBond` to a `TransactionButton` which makes a `call` to `createKitty()`. You can use the `paw` icon to give your transaction button a little extra flare.

然后，你需要将 `SignerBond` 连接到 `TransactionButton`，后者会调用 `createKitty()`。你可以使用 `paw` 图标为你的交易按钮增加一点额外的效果。

Once you have completed this, test your button by creating some new kitties! Watch your "kitty counter" increase as new kitties enter your system.

完成此操作后，通过创建一些新的 kitty 来测试你的按钮！当新的 kitty 进入你的系统时，观察你的 “kitty counter” 是否增加。

<!-- tabs:start -->

#### ** Template **

[embedded-code](./assets/4.3-template.js ':include :type=code embed-template')

#### ** Solution **

[embedded-code-final](./assets/4.3-finished-code.js ':include :type=code embed-final')

<!-- tabs:end -->
