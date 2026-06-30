# Despliegue · VelarBond (testnet)

## 1. Compilar

```bash
cargo build --release --target wasm32-unknown-unknown
```

## 2. Instalar Stellar CLI

```bash
cargo install --locked stellar-cli --features opt
stellar network use testnet
stellar keys generate alice --fund
```

## 3. Desplegar WASM

```bash
WASM=target/wasm32-unknown-unknown/release/velar_bond.wasm

stellar contract deploy \
  --wasm "$WASM" \
  --source alice \
  --network testnet
```

Guardar el **Contract ID** devuelto.

## 4. Instalar hash en Velar API

Tras deploy, obtener el WASM hash:

```bash
stellar contract install --wasm "$WASM" --network testnet
```

Configurar en `apps/api/.env`:

```env
SOROBAN_VELAR_BOND_WASM_HASH=<wasm_hash>
SOROBAN_TSE_ADDRESS=<cuenta TSE custodial G...>
```

## 5. Verificar

```bash
stellar contract invoke \
  --id <CONTRACT_ID> \
  --source alice \
  --network testnet \
  -- status
```

Enlace explorer: `https://stellar.expert/explorer/testnet/contract/<CONTRACT_ID>`
