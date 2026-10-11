# clone-lin-geth-copygas

LIN clone of [ethereum/go-ethereum](https://github.com/ethereum/go-ethereum) tag `v1.16.9` `memoryCopierGas` / `gasKeccak256` (CopyGas=3 and KECCAK256 word gas, LGPL-3.0).

This repository holds **only the LIN copy**. Auditable proofs live in [kbelludoo/lin-open](https://github.com/kbelludoo/lin-open):

- Kernel: `src/lin_geth_copygas.lin`
- Proofs: `examples/geth_copygas/`
- Event: `docs/events/EVENT_GETH_COPYGAS_CLONE_LIN.rulel`
- Harness: `python3 test/prove_geth_copygas_external.py`

## Class

**EXPERIMENTAL** i64-scale. Not a Geth node. `memoryCopierGas` is a closure over `stack.Back(stackpos)`; LIN lifts that to an integer `size`. Negative size fail-closes to 0.

KECCAK256 word gas (`Keccak256WordGas * words`) is included as a sibling opcode addend. It is distinct from CREATE2's `(InitCodeWordGas + Keccak256WordGas) * words`.

## Goldens

| size | COPY (`CopyGas=3`) | KECCAK256 (`Keccak256WordGas=6`) |
|---|---:|---:|
| 0 | 0 | 0 |
| 1 | 3 | 6 |
| 32 | 3 | 6 |
| 33 | 6 | 12 |
| 1024 | 96 | 192 |
| 49152 | 4608 | 9216 |

Formula: `CopyGas * ((size+31)/32)` with `CopyGas=3`. KECCAK256 uses `Keccak256WordGas=6`.

## License

Algorithm: LGPL-3.0 (go-ethereum). This LIN transcription is published so auditors can hash the clone independently of lin-open proofs.
