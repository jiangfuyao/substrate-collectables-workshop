Set Up Substrate UI
===

Similar to the `substrate-node-new` command you used way back at the beginning of this tutorial, when you install Substrate, we also provide you with a `substrate-ui-new` command which will automatically clone the [`substrate-ui` repo](https://github.com/paritytech/substrate-ui/tree/substrate-node-template) on your computer.

与你在本教程开头时使用的 `substrate-node-new` 命令类似，当你安装 Substrate 时，我们还为你提供了一个 `substrate-ui-new` 命令，它将自动将 [`substrate-ui` repo](https://github.com/paritytech/substrate-ui/tree/substrate-node-template) 克隆到你的电脑上。

## Install the Substrate UI

For the best experience working with the Substrate UI repo, you should install `yarn` on your computer following the instructions here:

为了获得使用 Substrate UI repo 的最佳体验，你应该按照以下说明在你的计算机上安装yarn：

[Install Yarn](https://yarnpkg.com/lang/en/docs/install/)

Run the following command in your working folder, as indicated by the usage instructions `substrate-ui-new --help`:

在工作文件夹中运行以下命令，使用说明可见 `substrate-ui-new --help` 所示：

```
substrate-ui-new substratekitties
```

You should see that a new folder `<NAME>-ui` is created where the `substrate-ui` repo is cloned. In that folder, install the required packages using:

你应该看到创建了一个新文件夹 `<NAME>-ui`，其中克隆了  `substrate-ui` repo。在该文件夹中，使用以下命令安装所需的包：

```
cd substratekitties-ui
yarn install
```
(If you get an error using `yarn` on a debian-based system, see the note at the bottom.)

（如果在基于debian的系统上使用纱线时出错，请参阅底部的注释。）

And once the packages are done you can run the UI using:

一旦完成包的安装后，你可以使用以下命令运行 UI：

```
yarn run dev
```

Make sure your node is up and running and open `localhost:8000` in your Chrome.

确保你的节点已启动并运行，并在 Chrome 中打开 `localhost:8000`。

----
_Note_:

The UI uses websockets to connect to the local node instance through an unencrypted connection. Most Browsers disallow this kind of connection for security and privacy reasons, only Chrome allows this connection _if it is connecting to localhost_. That is why we are using Chrome in this workshop. If you want to connect to the browser to a different computer in the network, it must be served through a secured connection.

UI 使用 websockets 通过未加密的连接连接到本地节点实例。出于安全和隐私原因，大多数浏览器不允许此类连接，只有 Chrome *连接到 localhost 时* 才允许此连接。这就是为什么我们在这里使用 Chrome 的原因。如果要将浏览器连接到网络中的其他计算机，则必须通过安全连接提供服务。

_Note_:

You may need to remove the existing `yarn` (a legacy command line tool) and install via `npm`:

你可能需要删除现有 `yarn` 并通过 `npm` 安装：

```
sudo apt remove cmdtest
sudo apt remove yarn
sudo npm install -g yarn
```

`yarn install` should work as expected now.

`yarn install` 应该会按预期工作。

----

