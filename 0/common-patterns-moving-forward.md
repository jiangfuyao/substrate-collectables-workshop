Common Patterns Moving Forward
===

## The Rust Compiler is Your Friend

One of the many advantages of using a strongly typed programming language like Rust

[TODO: Make it seem like RUST is your friend and will help you add code when needed]

## Making Updates to Your Runtime

Before we jump into creating a custom Substrate runtime, you should be familiar with a few patterns which will help you iterate and run your code.

Your Substrate runtime code is compiled into two versions:

 - A [WebAssembly](https://webassembly.org/) (Wasm) image
 - A standard binary executable

The Wasm file is used as a part of the compilation of the standard binary, so it is important that you always compile your Wasm image first before you build the executable.

The pattern should be:

```bash
./build.sh               // Build Wasm
cargo build --release    // Build binary
```

Additionally, when you make changes to your node, the blocks produced in the past by older versions of your node persist. You may notice that when restarting your node, block production simply picks up where it left off.

However, if your changes to the runtime are significant, you may need to purge your chain of all the previous blocks with this handy command:

```bash
./target/release/substratekitties purge-chain --dev
```

After all this, then you will be able to start up your node again, fresh, with all the latest changes:

```bash
./target/release/substratekitties --dev
```

Remember this pattern; you will be using it a lot.

---
**Learn More**

Substrate is unique in that it is built to support Wasm and all languages that compile to Wasm. This allows for next generation features like live runtime upgrades and ...

[TODO: Talk about Wasm and runtime upgrades]

---