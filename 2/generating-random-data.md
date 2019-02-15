Generating Random Data
===

At the end of the last section, we allowed each user to create their own kitty. However, they weren't very unique... Let's fix that.

在最后一节的结尾，我们允许每个用户创建自己的kitty。但是，它们并不是独一无二的... 让我们解决这个问题。

## Generating a Random Seed
If we want to be able to tell these kitties apart, we need to start giving them unique properties! For our app, we need to generate a unique `id` for each kitty and some random `dna`.

如果我们想要区分这些 kitties，我们需要给它们独一无二的属性！对于我们的应用程序而言，我们需要为每个 kitty 和一些随机 `dna` 生成一个唯一的 `id`。

We can securely fetch some randomness from our chain using the `system` module:

我们可以使用 `system` 模块从我们的链中安全地获取一些随机性：

```rust
<system::Module<T>>::random_seed()
```

Substrate uses a safe mixing algorithm that uses the entropy of previous blocks to generate new random data for each subsequent block.

However, since it is dependent on previous blocks, it can take over 80 blocks to fully warm up, and you may notice the seed will not change until then.

Substrate 使用一个安全混合的算法，该算法使用先前块的熵来为每个后续块生成新的随机数据。

但是，由于它依赖于先前的块，因此可能需要超过 80 个块才能完全预热，并且你可能注意到了种子在此之前不会改变。

### Using a Nonce

Since the random seed does not change for multiple transactions in the same block, and since it may not even generate a random seed for the first 80 blocks, it is important that we also introduce a `nonce` which our module can manage. Furthermore, we can also use a user specific property like the `AccountId` to introduce a bit more entropy.

If we are able to hash this combined data, we should get enough randomness for our needs.

由于随机种子不会因同一个块中的多个交易而发生变化，并且它可能不会为前 80 个块生成随机种子，所以我们还引入了一个我们可以管理的 `nonce`。此外，我们还可以使用像 `AccountId` 这样的用户特定属性来引入更多的熵。

如果我们能够 hash 这些组合数据，我们应该就可以为我们的需求提供足够的随机性。

## Hashing Data

A random number generator on substrate will look something like this:

Substrate 上的随机数生成器看起来像这样：

```rust
let sender = ensure_signed(origin)?;
let nonce = <Nonce<T>>::get();
let random_seed = <system::Module<T>>::random_seed();

let random_hash = (random_seed, sender, nonce).using_encoded(<T as system::Trait>::Hashing::hash);

<Nonce<T>>::mutate(|n| *n += 1);
```

`Nonce` will be a new item in our storage which we will simply increment whenever we use it.

We can use this `random_hash` to populate both the `id` and `dna` for our kitty.

`Nonce` 将成为我们存储中的新的一项，只要我们使用它，我们就会简单地增加它。

我们可以使用这个 `random_hash` 来填充我们的 kitty 的 `id` 和 `dna`。

## Checking for Collision

To easily track all of our kitties, it would be helpful to standardize our logic to use a unique id as the global key for our storage items. This means that a single unique key will point to our `Kitty` object, and all other ownership links or maps will point to that key.

为了轻松跟踪我们的所有 kitties，标准化我们的逻辑以使用唯一 id 作为我们存储项的全局密钥将会很有帮助。这意味着单个唯一键将指向我们的 `Kitty` 对象，而且所有其他所有权会链接或映射将指向该键。

The `id` on the `Kitty` object will serve that purpose, but we need to make sure that the `id` for a new kitty is always unique. We can do this with a new storage item `Kitties` which will be a mapping from `id` (`Hash`) to the `Kitty` object.

`Kitty` 对象上的 `id` 将用于此目的，但我们需要确保新 kitty 的 `id` 始终是唯一的。我们可以使用新的存储项目 `Kitties` 来实现这一点，它将是从 `id` (`Hash`) 到 `Kitty` 对象的映射。

With this object, we can easily check for collisions by simply checking whether this storage item already contains a mapping using a particular `id`. Something like:

使用此对象，我们可以通过简单地检查此存储项是否已包含了特定 `id` 的映射来轻松检查冲突。就像这样：

```rust
ensure!(!<Kitties<T>>::exists(new_id), "This new id already exists");
```

However unlikely it is that two randomly generated hashes may collide, it is important to make this check as we may introduce other ways to generate kitties, and our storage structure depends on this uniqueness.

然而两个随机生成的哈希值可能会发生碰撞，因此进行此检查非常重要，因此我们可能会引入其他生成 kitties 的方法，我们的存储结构取决于这种唯一性。

## Your Turn!

Let's update our module to support generating random data for our kitties, and use that random data to create unique `id`s which will be the basis for our storage and logic.

We will introduce a new `Kitties` storage items which will map an `id` to a `Kitty` object. We will also want to create a `KittyOwner` storage item which will map an `id` to the `AccountId` who owns the kitty.

Finally we can update our `OwnedKitty` object to point to this unique `id` rather than have a duplicate copy of the `Kitty` object in our storage.

Now that we have set up all these new storage items, we will need to also update our `create_kitty()` function to correctly update these storage items when a kitty is made.

Follow the instructions on the template for this section to get things running!

让我们更新我们的模块以支持为我们的 kitty 生成随机数据，并使用该随机数据创建唯一的 `id`，这将成为我们存储和逻辑的基础。

我们将介绍一个新的 `Kitties` 存储项，它将 `id` 映射到 `Kitty` 对象。我们还想创建一个 `KittyOwner` 存储项，它将 `id` 映射到拥有 kitty 的用户的 `AccountId`。

最后，我们可以更新 `OwnedKitty` 对象以指向此唯一 `id`，而不是在我们的存储中存储 `Kitty` 对象的副本。

现在我们已经设置了所有这些新的存储项，我们还需要更新我们的 `create_kitty()` 函数，以便在制作 kitty 时正确更新这些存储项。

按照本节模板上的说明操作！

<!-- tabs:start -->

#### ** Template **

[embedded-code](./assets/2.1-template.rs ':include :type=code embed-template')

#### ** Solution **

[embedded-code-final](./assets/2.1-finished-code.rs ':include :type=code embed-final')

<!-- tabs:end -->