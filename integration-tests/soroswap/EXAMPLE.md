# Two legs sharing one Soroswap pair

If both legs of a split trade use the same XLM/USDC pair, the first leg moves the reserves seen by the second. [Kanatoko](https://crates.io/crates/kanatoko) runs the aggregator built from this checkout against saved Soroswap code and reserves, so both legs trade in the same pool.

| On the captured pair | USDC output |
| --- | ---: |
| One 90.4722000 XLM leg, quoted in isolation | 11.4149918 |
| Two such legs, both quoted at the opening reserves | 22.8299836 |
| Two legs executed in sequence | 22.6064897 |

The second test sets the aggregate minimum one unit above the sequential quote. It checks error 8 and the trader, aggregator and pool balances and pair reserves. Retrying with the exact minimum succeeds. The Protocol 30 captures are from mainnet ledgers 65646134 (split) and 65646133 (minimum). Authorization is mocked locally.

[Read the Rust assertions](tests/soroswap.rs#L160).

## Reproduce locally

Use Rust 1.95.0 with the `wasm32v1-none` target. From the repo root, build the contracts, fetch the locked test dependencies once, then run the test offline.

```sh
cargo +1.95.0 build --locked --manifest-path Cargo.toml --target wasm32v1-none --profile contract-release -p aggregator-contract --target-dir integration-tests/soroswap/target
cargo +1.95.0 fetch --locked --manifest-path integration-tests/soroswap/Cargo.toml
cargo +1.95.0 test --locked --offline --manifest-path integration-tests/soroswap/Cargo.toml
```
