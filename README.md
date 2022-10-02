# reqwest for WebAssembly

An ergonomic, batteries-included HTTP Client for Rust.
This is a fork from the original [reqwest](https://github.com/seanmonstar/reqwest) with support for WebAssembly compilation target.
That allows reqwest apps to run inside the [WasmEdge Runtime](https://github.com/WasmEdge/WasmEdge#readme) as a lightweight and secure alternative to natively compiled apps in Linux container.

For more details and usage examples, please see the upstream [reqwest](https://github.com/seanmonstar/reqwest) source and [these examples](https://github.com/WasmEdge/wasmedge_reqwest_demo).

Note: We do not yet support SSL / TLS connections in reqwest_wasi yet.

- Plain bodies, JSON, urlencoded, multipart
- Customizable redirect policy
- HTTP Proxies
- Cookie Store

## Example

This asynchronous example uses [Tokio](https://tokio.rs) and enables some
optional features, so your `Cargo.toml` could look like this:

```toml
[dependencies]
reqwest_wasi = { version = "0.11", features = ["json"] }
tokio_wasi = { version = "1.21", features = ["full"] }
```

And then the code:

```rust,no_run
use std::collections::HashMap;

#[tokio::main]
async fn main() -> Result<(), Box<dyn std::error::Error>> {
    let resp = reqwest::get("http://eu.httpbin.org/ip")
        .await?
        .json::<HashMap<String, String>>()
        .await?;
    println!("{:#?}", resp);
    Ok(())
}
```

## Blocking Client

There is an optional "blocking" client API that can be enabled:

```toml
[dependencies]
reqwest_wasi = { version = "0.11", features = ["blocking", "json"] }
```

```rust,no_run
use std::collections::HashMap;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    let resp = reqwest::blocking::get("http://eu.httpbin.org/ip")?
        .json::<HashMap<String, String>>()?;
    println!("{:#?}", resp);
    Ok(())
}
```

## License

Licensed under either of

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://apache.org/licenses/LICENSE-2.0)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall
be dual licensed as above, without any additional terms or conditions.
