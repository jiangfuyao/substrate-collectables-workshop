Storing a Structure
===

If you thought everyone getting their own number was cool, lets try to give them all digital kitties!

First we need to define what properties our kitties have in the form of a `struct`, and then we need to learn how to store these custom `structs` in our runtime storage.

## Defining a Custom Struct

You can define a custom struct for your runtime like so:

```rust
#[derive(Encode, Decode, Default, Clone, PartialEq)]
pub struct MyStruct<A, B> {
    some_number: u32,
    some_generic: A,
    some_other_generic: B,
}
```

This should look pretty normal compared to defining structs in other languages. However you will notice two oddities about this declaration for runtime development.

### Using Generics

You will notice that we define our example struct using a generic as one of the types that we store. This will be important when trying to use custom Substrate types like `AccountId` or `Balances` within our struct as we will need to pass in these types every time we use our struct.

So if we wanted to store an `AccountId` in `some_generic`, we would need to define our storage item like this:

```rust
decl_storage! {
    trait Store for Module<T: Trait> as Example {
        MyItem: map T::AccountId => MyStruct<T::Balance, T::Hash>;
    }
}
```

For the purposes of clarity, we will name a generic type for `T::AccountId` as `AccountId` or `T::Balance` as `Balance`. You can use comma separate and add more generics as needed following this pattern.

### Derive Macro

The other thing you will notice is `#[derive(...)]` at the top. This is an attribute provided by the Rust compiler which allows basic implementations of some traits. You can learn more about that [here](https://doc.rust-lang.org/rust-by-example/trait/derive.html). For the purposes of this tutorial you can treat it like magic.

## Custom Struct in Module Function

Now that we have initialized our custom struct in our runtime storage, we can now push values and modify it.

Here is an example of creating and inserting a struct into storage using a module function:

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

A `Kitty` should have the following properties:

- `id` : `Hash`
- `dna` : `Hash`
- `price` : `Balance`
- `gen` : `u64`

We have created a skeleton of the `create_kitty()` function for you, but you will need to add the logic. Include code to create a `new_kitty` using the `Kitty` object and store that object into your runtime storage.

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

你可能希望我们为 kitties 添加一个名称属性！毕竟，谁不会给他们喜欢的东西取名呢? [**译者注**： 我还真不会...]

Substrate 不直接支持字符串。Runtime 存储用于存储 runtime 运行的业务逻辑的状态。它不是存储 UI 所需的一般数据。如果你确实需要将一些任意数据存储到 runtime，你总是可以创建一个 bytearray（`Vec<u8>`），但更合乎逻辑的做法是将哈希值存储到 IPFS 之类的服务中，然后获取数据用于 UI 展示。目前这超出了本次研讨会的范围，但可能会在以后添加，以支持有关你的 kitty 的其他元数据。[**译者注**： 从编程语言角度来说，Substrate 之所以不直接支持 `String` 是因为 runtime 运行在 Wasm 上，而 Rust 编译为 Wasm 时需要启用 `no_std` 配置，也就是无法使用 `libstd` 标准库，这也就导致了标准库中的 `String` 无法在 runtime 逻辑中使用。但实际上可以直接用 `Vec<u8>` 来代替 `String`，因为 `String` 本身的存储就是一个遵循 `UTF8` 格式的 `Vec<u8>`，而 `Vec<u8>` 可以在不需要导入标准库的情况下通过导入 `liballoc` 库来使用，注意该库还只是 `nightly-only`]

---
