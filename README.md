# Caliptra DOE (Data Object Exchange)

AES-256-CBC de-obfuscation engine for secure device secrets from the [CHIPS Alliance Caliptra Root of Trust](https://github.com/chipsalliance/caliptra-rtl).

## Features

- **Algorithm**: AES-256 encryption/decryption with CBC mode
- **Key sizes**: 128-bit and 256-bit
- **Purpose**: De-obfuscate device secrets (UDS, Field Entropy, HEK) at boot time
- **Bus**: AHB-Lite slave (32-bit)
- **KeyVault**: Write de-obfuscated secrets directly to KeyVault
- **Operations**: UDS decrypt, FE decrypt, HEK decrypt (OCP-aware), Clear/Zeroize
- **Security**: Flow locking (one-shot per boot), zeroize command, OCP lock awareness

## Architecture

```
doe_ctrl (AHB-Lite slave wrapper)
├── doe_reg (register file with interrupt controller)
└── doe_cbc (CBC wrapper)
    ├── doe_fsm (secret flow controller: UDS/FE/HEK states)
    └── doe_core_cbc (AES cipher core with IV management)
        ├── doe_encipher_block (AES forward rounds)
        ├── doe_decipher_block (AES inverse rounds)
        ├── doe_key_mem (round key expansion, 10/14 rounds)
        ├── doe_sbox (4x parallel forward S-box)
        └── doe_inv_sbox (4x parallel inverse S-box)
```

## Operations

| Command | Code | Description |
|---------|------|-------------|
| DOE_UDS | 0x1 | De-obfuscate Unique Device Secret (128-bit) |
| DOE_FE | 0x2 | De-obfuscate Field Entropy (128-bit) |
| DOE_CLEAR | 0x3 | Clear/zeroize obfuscated secrets |
| DOE_HEK | 0x4 | De-obfuscate Hardware Embedded Key (256-bit, OCP-aware) |

## Upstream

Extracted from [chipsalliance/caliptra-rtl](https://github.com/chipsalliance/caliptra-rtl) `src/doe`. See `upstream.yaml` for commit tracking.

## License

Apache-2.0 — see [LICENSE](LICENSE).
