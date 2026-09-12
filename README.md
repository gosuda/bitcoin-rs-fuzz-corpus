# bitcoin-rs fuzz corpus

Coverage-guided fuzzing corpora for
[bitcoin-rs](https://github.com/gosuda/bitcoin-rs).

Each directory under `corpus/` matches a `cargo-fuzz` target in the main
repository. The scheduled campaign runs the target against its directory,
minimizes the resulting corpus, and commits a change only when the minimized
set changes. Campaign metadata records the exact bitcoin-rs revision and
GitHub Actions run that produced each update.

The fuzz targets, scheduled workflow, and local commands are maintained in the
main repository. See its
[fuzzing guide](https://github.com/gosuda/bitcoin-rs/blob/main/fuzz/README.md)
before contributing inputs or running a local campaign.

## Initial corpus

The initial `block_validate`, `p2p_message`, `script_eval`, and `tx_validate`
inputs were copied from bitcoin-rs commit
`1a125f2486c843c28174df695c9498c6f52e4292`. They were imported and minimized
from [rust-bitcoin/qa-assets](https://github.com/rust-bitcoin/qa-assets) commit
`ffd27e4ee51266673859e3d1314369e780e26a4e` under CC0-1.0.

The initial `utxo_snapshot` input is the bitcoin-rs v4 golden snapshot from the
same bitcoin-rs revision and is available under the repository's MIT OR
Apache-2.0 license.
