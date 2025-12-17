<p align="center">
  <img src="https://raw.githubusercontent.com/ApiliumCode/aingle/main/assets/aingle.svg" alt="AIngle Logo" width="200"/>
</p>

<h1 align="center">lmdb-rs-apilium</h1>

<p align="center">
  <strong>Rust bindings for LMDB - optimized for AIngle</strong>
</p>

<p align="center">
  <a href="https://crates.io/crates/lmdb-rkv"><img src="https://img.shields.io/crates/v/lmdb-rkv.svg" alt="Crates.io"/></a>
  <a href="https://docs.rs/lmdb-rkv"><img src="https://docs.rs/lmdb-rkv/badge.svg" alt="Documentation"/></a>
  <a href="https://github.com/ApiliumCode/lmdb-rs-apilium/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-Apache--2.0-blue.svg" alt="License"/></a>
  <a href="https://github.com/ApiliumCode/lmdb-rs-apilium/actions"><img src="https://github.com/ApiliumCode/lmdb-rs-apilium/workflows/CI/badge.svg" alt="CI Status"/></a>
</p>

---

## Overview

Idiomatic and safe Rust APIs for interacting with the [Symas Lightning Memory-Mapped Database (LMDB)](http://symas.com/mdb/). This fork is optimized for use within the AIngle distributed systems framework.

## Features

- **Memory-mapped I/O** - Direct memory access for high performance
- **ACID transactions** - Full transaction support with MVCC
- **Zero-copy reads** - Access data directly without copying
- **Nested transactions** - Hierarchical transaction support
- **Cursors** - Efficient iteration over key-value pairs
- **Cross-platform** - Linux, macOS, Windows support

## Installation

```toml
[dependencies]
lmdb-rkv = "0.1"
```

## Quick Start

```rust
use lmdb::{Environment, Database, WriteFlags};
use std::path::Path;

fn main() -> Result<(), Box<dyn std::error::Error>> {
    // Open environment
    let env = Environment::new()
        .set_max_dbs(1)
        .open(Path::new("./data"))?;

    // Open database
    let db = env.open_db(None)?;

    // Write transaction
    {
        let mut txn = env.begin_rw_txn()?;
        txn.put(db, b"key", b"value", WriteFlags::empty())?;
        txn.commit()?;
    }

    // Read transaction
    {
        let txn = env.begin_ro_txn()?;
        let value = txn.get(db, b"key")?;
        println!("Value: {:?}", String::from_utf8_lossy(value));
    }

    Ok(())
}
```

## Crates

| Crate | Description |
|-------|-------------|
| `lmdb-rkv` | High-level Rust API |
| `lmdb-rkv-sys` | Low-level FFI bindings |

## Building from Source

```bash
# Clone with submodules
git clone --recursive https://github.com/ApiliumCode/lmdb-rs-apilium.git
cd lmdb-rs-apilium

# Build
cargo build

# Run tests
cargo test
```

## Performance

LMDB provides exceptional read performance through memory-mapped files:

| Operation | Performance |
|-----------|-------------|
| Read | O(1) - Direct memory access |
| Write | O(log n) - B+ tree insertion |
| Iteration | Sequential disk access |

## Part of AIngle

This crate is part of the [AIngle](https://github.com/ApiliumCode/aingle) ecosystem - a Semantic DAG framework for IoT and distributed AI applications.

## License

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.

---

<p align="center">
  <sub>Maintained by <a href="https://apilium.com">Apilium Technologies</a> - Tallinn, Estonia</sub>
</p>
