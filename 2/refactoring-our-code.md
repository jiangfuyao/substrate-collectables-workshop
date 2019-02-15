Refactoring Our Code
===

Our `create_kitty()` function is pretty bulky and has code we will probably want to use later when we introduce kitty breeding and other ways to "mint" new kitties.

我们的 `create_kitty()` 函数非常庞大，而且里面有些代码可能会在稍后被引入 kitty 育种和其他方法来 “mint” 新的 kitty。

We will take this opportunity to teach you about writing private internal functions which are not directly exposed through your runtime's API, but are still accessible by your module.

我们将借此机会教你编写私有的内部函数，这些函数不是通过 runtime 的 API 直接公开的，但仍可以被你的模块访问。

## Public Interfaces and Private Functions

Within your runtime, you are able to include an implementation of your runtime module like so:

在 runtime 中，你可以像这样包含 runtime 模块的实现：

```rust
impl<T: Trait> Module<T> {
    // Your functions here
}
```

Functions in this block are usually public interfaces or private functions. Public interfaces should be labeled `pub` and generally fall into inspector functions that do not write to storage and operation functions that do. Private functions are your usual private utilities unavailable to other modules.

此块中的函数通常是公共接口或私有函数。公共接口应标记为 `pub`，并且通常属于检查器函数，这些函数不会写入存储和操作函数。私有函数通常是你在其他模块不可用的私有工具。

You can call functions defined here using the `Self::function_name()` pattern you have seen before. Here is an intentionally overcomplicated example:

你可以使用之前看到的 `Self::function_name()` 模式调用此处定义的函数。这是一个故意复杂化的例子：

```rust
decl_module! {
    pub struct Module<T: Trait> for enum Call where origin: T::Origin {
        fn adder_to_storage(origin, num1: u32, num2: u32) -> Result {
            let _sender = ensure_signed(origin)?;
            let result = _adder(num1, num2);

            _store_value(result)?;

            Ok(())
        }
    }
}

impl<T: Trait> Module<T> {
    fn _adder(num1: u32, num2: u32) -> u32 {
        let final_answer = num1.checked_add(num2).ok_or("Overflow when adding")?;
    }

    fn _store_value(value: u32) -> Result {
        <myStorage<T>>::put(value);
        
        Ok(())
    }
}
```

Remember that we still need to follow a "verify first, write last"  pattern, so it is important to not daisy chain private functions which do writes to storage where there is a chance one will throw an error.

请记住，我们仍然需要遵循 "verify first, write last" 模式，因此不要天真地直接调用私有函数，这些私有函数写入存储时，有可能会抛出错误。

## Your Turn!

For us, the process of creating a new kitty from a `Kitty` object and updating all the storage variables is something that we should make reusable so that we can create other kitties whilst taking advantage of the same code.

对于我们来说，从 `Kitty` 对象创建新 kitty 并更新所有存储变量的过程是我们应该重复使用的过程，这样我们就可以在利用相同代码的同时创建其他 kitty。

Whereas the `create_kitty()` function assumes that the kitty generated will always be given to the user who submitted the transaction, our reusable `_mint()` function should not make that assumption.

虽然 `create_kitty()` 函数假定生成的 kitty 将始终被给予提交交易的用户，但我们的可重用 `_mint()` 函数不应该做出这样的假设。

The `create_kitty()` function also generates the property of the new kitty, but we should not move this to the `_mint()` function. There may be multiple ways to generate a kitty object in the future. This function should only ensure that the kitty being created is unique, and that all the storage items are properly updated.

`create_kitty()` 函数也生成新 kitty 的属性，但是我们不应该将它移动到 `_mint()` 函数。将来可能有多种方法可以生成 kitty 对象。该函数应仅确保正在创建的 kitty 是唯一的，并且所有存储项都已正确更新。

Our template provides the function declaration for `_mint()`, and will guide you to refactor the code and update any variables to match the new schema. It might be good to double check your work in the Polkadot-JS Apps UI once again before you move on.

我们的模板为 `_mint()` 提供了函数声明，并将指导你重构代码并更新变量以匹配新模式。在继续之前，最好再次在 Polkadot-JS Apps UI 中仔细检查你的工作是否正确。

<!-- tabs:start -->

#### ** Template **

[embedded-code](./assets/2.6-template.rs ':include :type=code embed-template')

#### ** Solution **

[embedded-code-final](./assets/2.6-finished-code.rs ':include :type=code embed-final')

<!-- tabs:end -->
