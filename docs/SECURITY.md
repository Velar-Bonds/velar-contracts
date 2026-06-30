# Seguridad · velar-contracts

## Estado

| Item | Estado |
|------|--------|
| Auditoría externa | ⏳ Pendiente |
| Bug bounty | ⏳ No activo |
| Mainnet | ❌ No recomendado |

## Modelo de confianza

- **TSE** puede congelar y anclar documentos — diseño intencional regulatorio.
- **Dueño** puede transferir y cambiar estado marketplace (`InEscrow` / `Active`).
- La app Velar usa **custodia asistida**; las secret keys no están en este repo.

## Reportar vulnerabilidades

Abrir issue **privado** o contactar maintainers del org [Velar-Bonds](https://github.com/Velar-Bonds).

No publicar exploits en issues públicos antes de mitigación.

## Checklist pre-mainnet

- [ ] Auditoría Soroban por firma reconocida
- [ ] Multisig TSE on-chain
- [ ] Timelock en `freeze` / upgrades
- [ ] Plan de pausa de emergencia documentado
