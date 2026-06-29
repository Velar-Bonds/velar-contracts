<div align="center">

# VELAR · Contracts

### Contratos inteligentes Soroban de VELAR sobre Stellar

[![Stellar](https://img.shields.io/badge/Built_on-Stellar-black?logo=stellar)](https://stellar.org)
[![Soroban SDK](https://img.shields.io/badge/soroban--sdk-22.0-blue)](https://soroban.stellar.org)
[![Rust](https://img.shields.io/badge/Rust-stable-orange?logo=rust)](https://www.rust-lang.org)

</div>

Cada **bono político** emitido por el TSE es un contrato Soroban individual desplegado
en Stellar. La cadena es la fuente de verdad (monto, fechas, dueño actual, partido
emisor, hash del documento, estado); Postgres queda como índice/cache.

> App y backend: [Velar-Bonds/Velar](https://github.com/Velar-Bonds/Velar) · Docs: [velar-docs](https://github.com/Velar-Bonds/velar-docs)

## Contratos

| Contrato | Descripción |
|---|---|
| `contracts/velar-bond` | El bono como contrato Soroban (testnet). |

## API del contrato `VelarBond`

| Función | Quién | Qué hace |
|---|---|---|
| `initialize(tse, args)` | TSE | Crea el bono con todos sus atributos. |
| `transfer(to)` | Dueño actual | Transfiere el bono a otra wallet. |
| `freeze()` / `unfreeze()` | TSE | Congela / descongela el bono. |
| `set_in_escrow()` / `set_active()` | Dueño | Publica / retira del marketplace. |
| `set_document_hash(hash)` | TSE | Ancla el hash del documento (BytesN<32>). |
| `details()` | Público | Lee todos los atributos. |
| `current_owner()` | Público | Dueño actual on-chain. |
| `status()` | Público | `Active` / `InEscrow` / `Frozen` / `Sold` / `Cancelled`. |
| `tse()` | Público | Dirección del TSE emisor. |

Eventos: `issued`, `transfer`, `frozen`, `unfrozen`, `inescrow`, `active`.

## Desarrollo

```bash
rustup target add wasm32-unknown-unknown
cargo test
cargo build --release --target wasm32-unknown-unknown
# El .wasm queda en target/wasm32-unknown-unknown/release/velar_bond.wasm
```

## Licencia

MIT
