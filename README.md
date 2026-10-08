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

`corpus/verdicts.json` pins the expected native-codec verdict per
`tx_validate`/`block_validate` seed (QAC-05,
[docs/contracts/qa-corpus.md](https://github.com/gosuda/bitcoin-rs/blob/main/docs/contracts/qa-corpus.md)).
bitcoin-rs's differential gate enforces it bidirectionally; the daily
publish-corpus job regenerates it after applying campaign output. Manual
refresh: `BITCOIN_RS_FUZZ_CORPUS=<checkout>/corpus CORPUS_VERDICTS_WRITE=1
cargo test -p bitcoin-rs-primitives --test differential`.

## Initial corpus

The initial `block_validate`, `p2p_message`, `script_eval`, and `tx_validate`
inputs were copied from bitcoin-rs commit
`1a125f2486c843c28174df695c9498c6f52e4292`. They were imported and minimized
from [rust-bitcoin/qa-assets](https://github.com/rust-bitcoin/qa-assets) commit
`ffd27e4ee51266673859e3d1314369e780e26a4e` under CC0-1.0.

The initial `utxo_snapshot` input is the bitcoin-rs v4 golden snapshot from the
same bitcoin-rs revision and is available under the repository's MIT OR
Apache-2.0 license.

## Imported reference corpora

This repository is the single home for seed corpora; the main repository's
`fuzz/corpus/` directory was retired into it (bitcoin-rs PRs
[#1396](https://github.com/gosuda/bitcoin-rs/pull/1396) and
[#1402](https://github.com/gosuda/bitcoin-rs/pull/1402)). Those inputs include
corpora imported and minimized from
[bitcoin/bitcoin](https://github.com/bitcoin/bitcoin) commit
`9dfde64cc3262329051fd05fffe40eecc786a99f` (MIT) and
[btcsuite/btcd](https://github.com/btcsuite/btcd) commit
`b48125d0a3565b1441522ee10422f7815db03216` (ISC) on top of the qa-assets set.

Per-source provenance, upstream pins, licenses, and the refresh rule are owned
by the main repository's
[fuzz/CORPUS_PROVENANCE.md](https://github.com/gosuda/bitcoin-rs/blob/main/fuzz/CORPUS_PROVENANCE.md).
`.reference-inventory.json` at the repository root tracks which seeds the
reference importer owns so a pin refresh can remove stale ones; it is
maintained by `scripts/import-reference-corpora.sh` and is not for hand edits.
