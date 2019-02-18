Running a Custom Node
===

Now that you have successfully installed the Substrate framework on your machine, we can quickly spin up a custom Substrate node using pre-configured templates.

现在你已经在计算机上成功安装了 Substrate 框架，我们可以使用预先配置的模板快速启动自定义 Substrate 节点。

In your terminal window, navigate to your working directory and run `substrate-node-new --help` to view usage instructions. We will choose "substratekitties" as the name of our project for the purposes of this walkthrough, but you can modify this name for future projects you work on. Now run the following, where `<AUTHOR>` is your first name:

在终端窗口中，切换到你的工作目录并运行 `substrate-node-new --help` 以查看使用说明。出于本项目的目的，我们将选择 “baseskitties” 作为项目的名称，但你可以为自己的项目修改此名称。现在运行以下命令，其中 `<AUTHOR>` 是你的名字：

```bash
substrate-node-new substratekitties <AUTHOR>
```

> **NOTE**: If you want to peek behind the magic of `substrate-node-new` you can take a look [here](https://github.com/paritytech/substrate-up/blob/master/substrate-node-new).
>
> **注意**: 如果你想要了解 `substrate-node-new` 的具体内容，你可以看看 [这里](https://github.com/paritytech/substrate-up/blob/master/substrate-node-new)。

Once your custom node is done compiling, you should be able to run the following command to start the node:

完成自定义节点编译后，你应该能够运行以下命令来启动节点：

```bash
cd substratekitties
./target/release/substratekitties --dev
```

If you are successful, you should see blocks being produced.

如果你成功了，你应该能看到正在生成的块。

![An image of the node producing new blocks](./assets/building-blocks.png)

---
**Learn More**

Using the `--dev` flag tells your node binaries to run a specific chain

使用 `--dev` 标志会告诉你的节点二进制文件去运行特定的链

---