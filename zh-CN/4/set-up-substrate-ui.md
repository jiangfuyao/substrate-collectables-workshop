# 创建 Substrate UI

与你在本教程开头时使用的 `substrate-node-new` 命令类似，当你安装 Substrate 时，我们还为你提供了一个 `substrate-ui-new` 命令，它将自动将 [`substrate-ui` repo](https://github.com/paritytech/substrate-ui/tree/substrate-node-template) 克隆到你的电脑上。

## 安装 Substrate UI

为了获得使用 Substrate UI repo 的最佳体验，你应该按照以下说明在你的计算机上安装yarn：

[Install Yarn](https://yarnpkg.com/lang/en/docs/install/)

在工作文件夹中运行以下命令，使用说明如 `substrate-ui-new --help` 所示：

```bash
substrate-ui-new substratekitties
```

You should see that a new folder `<NAME>-ui` is created where the `substrate-ui` repo is cloned. In that folder, install the required packages using:

你应该看到创建了一个新文件夹 `<NAME>-ui`，它就是 `substrate-ui` repo 的克隆。在该文件夹中，使用以下命令安装所需的包：

```bash
cd substratekitties-ui
yarn install
```

（如果在基于debian的系统上使用 `yarn` 时出错，请参阅底部的注释。）

一旦完成包的安装后，你可以使用以下命令运行 UI：

```bash
yarn run dev
```

确保你的节点已启动并运行，并在 Chrome 中打开 `localhost:8000`。

----

*注意*：

UI 使用 websockets 通过未加密的连接连接到本地节点实例。出于安全和隐私原因，大多数浏览器不允许此类连接，只有 Chrome *连接到 localhost 时* 才允许此连接。这就是为什么我们在这里使用 Chrome 的原因。如果要将浏览器连接到网络中的其他计算机，则必须通过安全连接提供服务。

*注意*：

你可能需要删除现有 `yarn` 并通过 `npm` 安装：

```bash
sudo apt remove cmdtest
sudo apt remove yarn
sudo npm install -g yarn
```

`yarn install` 应该会按预期工作。
