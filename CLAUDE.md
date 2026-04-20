# CLAUDE.md — verus

Verus: formal verification toolchain for Rust programs using SMT-based automated theorem
proving. Used by Project-Metic as the primary Rust verification backend, and as the target
language for the `verus-proof-synthesis` and `VeriStruct` research tools.

Upstream: verus-lang/verus (open source).

## What This Repo Is

The Verus verifier — used by Project-Metic for formally verifying Rust components in the
binary analysis and compiler pipeline, and as the benchmark target for proof synthesis research.

## Key Invariants

- **sorry_count == 0 is the correctness standard.** Any Verus proof that uses `assume` or
  `assume(false)` is not considered verified by Metic standards. sorry-free proofs only.
- **Metic IR properties verified in Verus feed Lean 4 obligations.** Verus results that
  establish memory safety or ISA-level properties may be referenced in Lean 4 proof obligations
  submitted to Infera. Verus output is not independently a Metic certificate.
- **Toolchain version pinning.** Verus versions are coupled to specific `rustc` nightly builds.
  Pin the toolchain in `rust-toolchain.toml`. Never use `stable` for Verus verification.

## Dev Commands

```bash
# Build Verus
./build.sh

# Verify a Rust file
./source/target/release/verus --crate-type lib input.rs

# Run Verus test suite
./tools/run-tests.sh

# Install via cargo
cargo install --path source/cargo-verus/
```

## Tech Stack

- Rust, Z3 (SMT solver), rustc nightly

## Integration with Project-Metic

- `verismo` and COCONUT-SVSM provide reference targets for verified low-level Rust
- `verus-proof-synthesis` (AutoVerus, VeruSAGE) targets this toolchain
- `VeriStruct` uses Verus for data structure verification benchmarks
- Verus-verified properties can feed into Lean 4 PO submissions to Infera

## What NOT to Do

- Do not use `assume(false)` in production proofs
- Do not use a stable Rust toolchain for Verus verification
- Do not treat Verus output alone as a Metic certificate without Infera co-signing
