# 与你的节点交互

在本教程结束时，我们将为你创建自己的定制 UI，以便与你的区块链进行交互。与此同时，我们可以使用 [Polkadot-JS](https://polkadot.js.org) Apps UI，它是一个适应你自定义节点的通用界面。

打开 **Chrome** 并导航至：

https://polkadot.js.org/apps/

要将 UI 指向本地节点，你需要调整 **设置**:

```
Settings > remote node/endpoint to connect to > Local Node (127.0.0.1:9944)
```

同时将 `Default Interface Theme` 更改为 `Substrate`。

![An image of the settings in Polkadot-JS Apps UI](./assets/polkadot-js-settings.png)

按下 **Save and Reload** 后，你应该注意到 Polkadot-JS Apps UI 重新生成; 并显示出 *Explorer* 和 *Transfer* 等新选项卡。请注意，你应该在浏览 Polkadot UI 前启动并运行 `./target/substratekitties --dev` 链。

请注意，稍后我们将在 Section 1 > Viewing a Structure 的 "Registering a Custom Struct" 部分中导入带有其他类型定义的 JSON 文件。

让我们进入 **Transfer** 标签页并进行交易。名为 "Alice" 的默认帐户预先存储了大量的 *Units*。

通过创建交易与 "Bob" 分享一些。你应该会在交易完成时看到确认信息，并且 Bob 的余额也会更新。

![First Transfer in Polkadot-JS Apps UI](./assets/first-transfer.png)

可见我们创建，运行并且与我们自己的本地 Substrate 链进行交互的整个过程是有多么快速。

---

**Learn More**

使用 [oo7-substrate library](https://github.com/paritytech/oo7/tree/master/packages/oo7-substrate) 库构建的 [Substrate UI](https://github.com/paritytech/substrate-ui) 是 [Polkadot-JS Apps UI](https://github.com/polkadot-js/apps) 是用于与你的 Substrate 链进行交互的另一个前端界面。

构建自己的 UI 时，请记住参考 [Polkadot-JS API 文档](https://github.com/polkadot-js/api) 和 [oo7 API 文档](https://tomusdrw.github.io/oo7/)。
