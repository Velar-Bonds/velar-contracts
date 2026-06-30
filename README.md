<div align="center">

# VELAR · Contracts

### Contratos inteligentes Soroban para bonos políticos en Stellar

[![Stellar](https://img.shields.io/badge/Stellar-testnet-7D00FF?logo=stellar&logoColor=white)](https://stellar.org)
[![Soroban SDK](https://img.shields.io/badge/soroban--sdk-22.0-0284FF)](https://soroban.stellar.org)
[![Rust](https://img.shields.io/badge/Rust-stable-orange?logo=rust)](https://www.rust-lang.org)
[![CI](https://github.com/Velar-Bonds/velar-contracts/actions/workflows/ci.yml/badge.svg)](https://github.com/Velar-Bonds/velar-contracts/actions)

[App](https://github.com/Velar-Bonds/Velar) · [Spec](https://github.com/Velar-Bonds/velar-spec) · [Docs](https://github.com/Velar-Bonds/velar-docs)

</div>

---

## Visión general

Cada **bono político** puede desplegarse como contrato Soroban **`VelarBond`**: metadata, dueño y estado viven on-chain. La app [Velar](https://github.com/Velar-Bonds/Velar) orquesta mint Classic Asset + deploy Soroban + escrow Trustless Work.

| Modo | Cuándo |
|------|--------|
| **Classic Asset only** | Dev sin `SOROBAN_VELAR_BOND_WASM_HASH` |
| **Classic + Soroban** | Prod / demo con WASM desplegado |

---

## Contratos

| Crate | Ruta | Descripción |
|-------|------|-------------|
| **VelarBond** | [`contracts/velar-bond/`](./contracts/velar-bond/) | Bono individual: owner, status, metadata, TSE admin |

---

## API · `VelarBond`

| Función | Autorización | Efecto |
|---------|--------------|--------|
| `initialize(tse, args)` | TSE | Crea bono con metadata completa |
| `transfer(to)` | Dueño | Transfiere propiedad |
| `freeze()` / `unfreeze()` | TSE | Congela / descongela |
| `set_in_escrow()` / `set_active()` | Dueño | Marketplace / retiro |
| `set_document_hash(hash)` | TSE | Ancla SHA-256 del PDF |
| `details()` | Lectura pública | Metadata completa |
| `current_owner()` | Lectura pública | Address dueño |
| `status()` | Lectura pública | `Active` · `InEscrow` · `Frozen` · `Sold` · `Cancelled` |
| `tse()` | Lectura pública | Address TSE |

### Eventos

`issued` · `transfer` · `frozen` · `unfrozen` · `inescrow` · `active`

### Errores (`repr u32`)

| Código | Significado |
|--------|-------------|
| 1 | Ya inicializado |
| 2 | No inicializado |
| 3 | No es el dueño |
| 4 | No es el TSE |
| 5 | Estado inválido |
| 6 | Bono congelado |
| 7 | Mismo dueño |

---

## Desarrollo local

```bash
# Toolchain (ver rust-toolchain.toml)
rustup target add wasm32-unknown-unknown

# Tests unitarios Soroban
cargo test

# Build WASM release
cargo build --release --target wasm32-unknown-unknown

# Artefacto:
# target/wasm32-unknown-unknown/release/velar_bond.wasm
```

---

## Despliegue testnet

Guía paso a paso: [`docs/DEPLOY.md`](./docs/DEPLOY.md)

Resumen:

```bash
# Instalar Stellar CLI
cargo install --locked stellar-cli --features opt

# Desplegar e invocar initialize (ver DEPLOY.md)
stellar contract deploy --wasm target/wasm32-unknown-unknown/release/velar_bond.wasm --network testnet
```

En **Velar API**, configurar:

```env
SOROBAN_VELAR_BOND_WASM_HASH=<hash del WASM desplegado>
SOROBAN_TSE_ADDRESS=G...
```

---

## Seguridad

- [`docs/SECURITY.md`](./docs/SECURITY.md) — modelo de amenazas y auditoría pendiente  
- **No usar en mainnet** sin revisión externa del contrato y multisig TSE.

---

## Relación con el spec

Estados Soroban ↔ Postgres: [velar-spec · states](https://github.com/Velar-Bonds/velar-spec/blob/main/docs/states.md)

---

## Licencia

[MIT](./LICENSE)
