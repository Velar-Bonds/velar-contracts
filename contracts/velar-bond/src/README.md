# Código fuente del contrato

El contrato `VelarBond` (`lib.rs` + `test.rs`) vive hoy en el monorepo público
[Velar-Bonds/Velar](https://github.com/Velar-Bonds/Velar/tree/main/contracts/velar-bond/src).

Para traerlo a este repo, desde la raíz del repo cloná y copiá los dos archivos:

```bash
git clone --depth 1 https://github.com/Velar-Bonds/Velar.git /tmp/velar-mono
cp /tmp/velar-mono/contracts/velar-bond/src/lib.rs  contracts/velar-bond/src/lib.rs
cp /tmp/velar-mono/contracts/velar-bond/src/test.rs contracts/velar-bond/src/test.rs
rm contracts/velar-bond/src/README.md
git add -A && git commit -m "feat: agregar fuente del contrato VelarBond" && git push
```

Después, `cargo test` y el CI deberían pasar en verde.
