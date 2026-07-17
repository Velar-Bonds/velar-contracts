# `VelarBond` — referencia del contrato

Cada bono es un **contrato Soroban único** en Stellar (`lib.rs` + `test.rs`).
Un bono = una instancia de contrato con su propio dueño, estado y metadata. Ser
dueño del bono = ser el `current_owner` del contrato.

## Estado almacenado (`DataKey`)

- `Tse` — `Address` con poder de aprobar/congelar.
- `Owner` — dueño actual del bono.
- `Details` — `BondDetails` completo.
- `InitializedAt` — timestamp de inicialización.

## `Status`

`Active` · `InEscrow` · `Frozen` · `Sold` · `Cancelled`

## Interfaz pública

| Función | Autorización | Descripción |
|---------|--------------|-------------|
| `initialize(tse, args: InitArgs)` | una sola vez | Crea el bono con su metadata y dueño inicial |
| `transfer(to: Address)` | dueño actual | Transfiere la propiedad a `to` |
| `freeze()` | TSE | Congela el bono |
| `unfreeze()` | TSE | Descongela el bono |
| `set_in_escrow()` | TSE | Marca el bono en escrow |
| `set_active()` | TSE | Devuelve el bono a activo |
| `set_document_hash(hash: BytesN<32>)` | TSE | Actualiza el hash del documento |
| `details() -> BondDetails` | lectura | Metadata completa |
| `current_owner() -> Address` | lectura | Dueño actual |
| `status() -> Status` | lectura | Estado on-chain |
| `tse() -> Address` | lectura | Autoridad TSE del bono |

`initialize` recibe un struct `InitArgs` (no parámetros sueltos) porque Soroban
limita las funciones a 10 parámetros y el bono tiene más metadata.

### `InitArgs` / `BondDetails`

`party_id`, `party_owner`/`current_owner`, `bond_id`, `certificate_number`,
`series`, `face_value` (`i128`), `currency` (`Symbol`), `interest_rate_bps`
(`u32`, basis points — 650 = 6.50 %), `issue_date`/`maturity_date` (`u64`),
`document_hash` (`BytesN<32>`, SHA-256 del PDF).

## Errores

| Código | Error | Cuándo |
|--------|-------|--------|
| 1 | `AlreadyInitialized` | Inicializar dos veces |
| 2 | `NotInitialized` | Operar sin inicializar |
| 3 | `NotOwner` | El caller no es el dueño |
| 4 | `NotTse` | El caller no es el TSE |
| 5 | `InvalidStatus` | Transición no permitida |
| 6 | `Frozen` | Bono congelado |
| 7 | `SameOwner` | Transferir al mismo dueño |

## Build & test

```bash
cargo test                                              # tests (src/test.rs)
cargo build --target wasm32-unknown-unknown --release   # WASM optimizado
```

El perfil `[profile.release]` del `Cargo.toml` (opt-level=z, lto, strip,
panic=abort, codegen-units=1) produce un WASM pequeño apto para Stellar.
