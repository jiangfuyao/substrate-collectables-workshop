Storing a Structure
===

If you thought everyone getting their own number was cool, lets try to give them all digital kitties!

First we need to define what properties our kitties have in the form of a `struct`, and then we need to learn how to store these custom `structs` in our runtime storage.

如果你认为每个人都有自己的号码很酷，让我们试着给他们所有的数字 kitty！

## Defining a Custom Struct

You can define a custom struct for your runtime like so:

你可以为 runtime 定义一个自定义结构，如下所示：

```rust
#[derive(Encode, Decode, Default, Clone, PartialEq)]
pub struct MyStruct<A, B> {
    some_number: u32,
    some_generic: A,
    some_other_generic: B,
}
```

This should look pretty normal compared to defining structs in other languages. However you will notice two oddities about this declaration for runtime development.

与在其他语言中定义结构相比，这应该看起来很正常。但是，你会在 runtime 开发中注意到这个声明的两个奇怪之处。

### Using Generics

You will notice that we define our example struct using a generic as one of the types that we store. This will be important when trying to use custom Substrate types like `AccountId` or `Balances` within our struct as we will need to pass in these types every time we use our struct.

你会注意到我们使用泛型定义了我们的示例结构作为我们的一个存储类型。当我们尝试在结构中使用 `Substrate` 中的类型（如 `AccountId` 或 `Balances`）时，这显得非常重要，因为每次我们在使用我们的结构体时都需要传递这些类型。

So if we wanted to store an `AccountId` in `some_generic`, we would need to define our storage item like this:

因此，如果我们想在 `some_generic` 中存储 `AccountId`，我们需要像这样定义我们的存储项：

```rust
decl_storage! {
    trait Store for Module<T: Trait> as Example {
        MyItem: map T::AccountId => MyStruct<T::Balance, T::Hash>;
    }
}
```

For the purposes of clarity, we will name a generic type for `T::AccountId` as `AccountId` or `T::Balance` as `Balance`. You can use comma separate and add more generics as needed following this pattern.

为了清楚起见，我们用名为 `T::AccountId` 的泛型类型作为 `AccountId`，名为将 `T::Balance` 的泛型类型作为 `Balance`。你可以使用逗号分隔，并根据需要在此模式后添加更多泛型。

### Derive Macro

The other thing you will notice is `#[derive(...)]` at the top. This is an attribute provided by the Rust compiler which allows basic implementations of some traits. You can learn more about that [here](https://doc.rust-lang.org/rust-by-example/trait/derive.html). For the purposes of this tutorial you can treat it like magic.

你会注意到的另一件事是顶部的 `＃[derive(...)]`。这是 Rust 编译器提供的属性，允许基本地实现某些 trait。你可以在[这里](https://doc.rust-lang.org/rust-by-example/trait/derive.html)了解更多相关信息。出于本教程的目的，你可以将其视为 magic。

## Custom Struct in Module Function

Now that we have initialized our custom struct in our runtime storage, we can now push values and modify it.

现在我们已经在 runtime 存储中初始化了自定义结构，接着我们可以存储值并对其进行修改。

Here is an example of creating and inserting a struct into storage using a module function:

以下是使用模块函数创建结构并将其插入到存储的示例：

```rust
decl_module! {
    pub struct Module<T: Trait> for enum Call where origin: T::Origin {
        fn create_struct(origin, user: T::AccountId， value: u32, balance: T::Balance, hash: T::Hash) -> Result {
            let _sender = ensure_signed(origin)?;

            let new_struct = MyStruct {
                some_number: value,
                some_generic: balance,
                some_other_generic: hash,
            };

            <MyItem<T>>::insert(user, new_struct);
            Ok(())
        }
    }
}
```

## Your Turn!

Update your storage mapping runtime to store a `Kitty` struct instead of a u64.

更新你 runtime 中的 storage map 以存储 `Kitty` 结构体而不是 u64。

A `Kitty` should have the following properties:

`Kitty` 应该具有以下属性：

- `id` : `Hash`
- `dna` : `Hash`
- `price` : `Balance`
- `gen` : `u64`

We have created a skeleton of the `create_kitty()` function for you, but you will need to add the logic. Include code to create a `new_kitty` using the `Kitty` object and store that object into your runtime storage.

我们已经为你创建了 `create_kitty()` 函数的框架，但你需要添加逻辑。包含使用 `Kitty` object 创建 `new_kitty` 的代码，并将该 object 存储到 runtime 存储中。

To initialize your `T::Hash` and `T::Balance` you can use:

要初始化 `T::Hash` 和 `T::Balance`，你可以使用：

```rust
let hash_of_zero = <T as system::Trait>::Hashing::hash_of(&0);

let my_zero_balance = <T::Balance as As<u64>>::sa(0);
```

We will also have `gen` start as `0`.

我们也需要将 `gen` 初始化 `0`。

<!-- tabs:start -->

#### ** Template **

[embedded-code](./assets/1.6-template.rs ':include :type=code embed-template')

#### ** Solution **

[embedded-code-final](./assets/1.6-finished-code.rs ':include :type=code embed-final')

<!-- tabs:end -->

---
**Learn More**

### Strings in Substrate

 You might expect that one of the properties we add for our kitties is a name! After all, who doesn't name the things they love?

Substrate does not directly support `Strings`. Runtime storage is there to store the state of the business logic on which the runtime operates. It is not to store general data that the UI needs. If you really need to store some arbitrary data into your runtime, you can always create a bytearray (`Vec<u8>`), however the more logical thing to do is to store a hash to a service like IPFS to then use to fetch data for your UI. This is currently beyond the scope of this workshop, but may be added later to support additional metadata about your kitty.

你可能希望我们为 kitties 添加一个名称属性！毕竟，谁不会给他们喜欢的东西取名呢? [译者注： 我还真不会...]

Substrate 不直接支持字符串。Runtime 存储用于存储 runtime 运行的业务逻辑的状态。它不是存储 UI 所需的一般数据。如果你确实需要将一些任意数据存储到 runtime，你总是可以创建一个 bytearray（`Vec<u8>`），但更合乎逻辑的做法是将哈希值存储到 IPFS 之类的服务中，然后获取数据用于 UI 展示。目前这超出了本次研讨会的范围，但可能会在以后添加，以支持有关你的 kitty 的其他元数据。[译者注： 从编程语言角度来说，Substrate 之所以不直接支持 `String` 是因为 runtime 运行在 Wasm 上，而 Rust 编译为 Wasm 时需要启用 `no_std` 配置，也就是无法使用 `libstd` 标准库，这也就导致了标准库中的 `String` 无法在 runtime 逻辑中使用。但实际上可以直接用 `Vec<u8>` 来代替 `String`，因为 `String` 本身的存储就是一个遵循 `UTF8` 格式的 `Vec<u8>`，而 `Vec<u8>` 可以在不需要导入标准库的情况下通过导入 `liballoc` 库来使用，注意该库还只是 `nightly-only`]

---
