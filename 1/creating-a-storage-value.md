Creating a Storage Value
===

Let's add the most simple logic we can to our runtime: a function which stores a variable.

To do this, we will first need to define a storage variable for a [**Storage Item**](https://substrate.readme.io/docs/glossary#section-storage-items) in the [**`decl_storage!`**](https://wiki.parity.io/decl_storage) macro. This allows for type-safe usage of the Substrate storage database, so you can keep things around between blocks.

让我们将最简单的逻辑添加到 runtime：一个存储变量的函数。

为此，我们首先需要在 `decl_storage！` 宏中为 [**Storage Item**](https://substrate.readme.io/docs/glossary#section-storage-items) 定义存储变量。Substrate 存储数据库允许类型安全地使用，因此你可以在块之间保持一致。

## Declaring a Storage Value

Substrate natively supports all the primitive types available in Rust (`bool`, `u8`, `u32`, etc..) and as well some custom types specific to Substrate (`AccountId`, `Balance`, `Hash`, [and more](https://github.com/paritytech/oo7/blob/master/packages/oo7-substrate/src/types.js)...)

Substrate 本身支持 Rust 中可用的所有原始类型（`bool`，`u8`，`u32` 等）以及一些 Substrate 中的特定自定义类型 (`AccountId`, `Balance`, `Hash`, [and more](https://github.com/paritytech/oo7/blob/master/packages/oo7-substrate/src/types.js)...)

You can declare a simple storage item like this:

你可以声明一个简单的 storage item，如下所示：

```rust
decl_storage! {
    trait Store for Module<T: Trait> as Example {
        MyU32: u32;
        MyBool get(my_bool_getter): bool;
    }
}
```

Here we have defined two variables: a `u32` and a `bool` with a getter function named `my_bool_getter`. The `get` parameter is optional, but if you add it to your storage item it will expose a getter function with the name specified (`fn getter_name() -> Type`).

To store these basic storage values, you need to import the `support::StorageValue` module.

这里我们定义了两个变量：一个 `u32` 和一个带有名为 `my_bool_getter` 的 getter 函数的 `bool`。`get` 参数是可选的，但如果将其添加到 storage item，它将公开具有指定名称的 getter 函数（`fn getter_name() -> Type`）。

要存储这些基本存储值，您需要导入 `support::StorageValue` 模块。

### Working with a Storage Value

The functions used to access a `StorageValue` are defined in the [`srml/support` folder](https://github.com/paritytech/substrate/blob/master/srml/support/src/storage/generator.rs#L98):

用于访问 `StorageValue` 的函数在 [`srml/support` 文件夹](https://github.com/paritytech/substrate/blob/master/srml/support/src/storage/generator.rs#L98) 中定义：

```rust
/// Get the storage key.
fn key() -> &'static [u8];

/// true if the value is defined in storage.
fn exists<S: Storage>(storage: &S) -> bool {
    storage.exists(Self::key())
}

/// Load the value from the provided storage instance.
fn get<S: Storage>(storage: &S) -> Self::Query;

/// Take a value from storage, removing it afterwards.
fn take<S: Storage>(storage: &S) -> Self::Query;

/// Store a value under this key into the provided storage instance.
fn put<S: Storage>(val: &T, storage: &S) {
    storage.put(Self::key(), val)
}

/// Mutate this value
fn mutate<R, F: FnOnce(&mut Self::Query) -> R, S: Storage>(f: F, storage: &S) -> R;

/// Clear the storage value.
fn kill<S: Storage>(storage: &S) {
    storage.kill(Self::key())
}
```

So if you want to "put" the value of `MyU32`, you could write:

所以如果你想 “put” `MyU32` 的值，你可以写：

```rust
<MyU32<T>>::put(1337);
```

If you wanted to "get" the value of `MyBool`, you could write either:

如果你想 “get” `MyBool` 的值，你可以选择以下任一一种写法：

```rust
let my_bool = <MyBool<T>>::get();
let also_my_bool = Self::my_bool_getter();
```

We will show you exactly how to integrate these calls into your module in the next section.

我们将在下一节中向你展示如何将这些调用集成到你的模块中。

## Your Turn!

Create a storage value called `Value` which stores a `u64`.

Make sure to import any required libraries required by the compiler. Your code should compile successfully.

创建一个名为 `Value` 的存储值，用于存储 `u64`。

确保导入编译器所需的任何库。你的代码能成功编译。

<!-- tabs:start -->

#### ** Template **

[embedded-code](./assets/1.2-template.rs ':include :type=code embed-template')

#### ** Solution **

[embedded-code-final](./assets/1.2-finished-code.rs ':include :type=code embed-final')

<!-- tabs:end -->
