# Specification

## KDBX Format

The KDBX format is documented by the KeePass project.

- **KDBX 4.x**: https://keepass.info/help/kb/kdbx_4.html
- **KDBX 3.1**: https://keepass.info/help/kb/kdbx_4.html (see version notes)
- **File format internals**: https://keepass.info/help/kb/kdbx_4b.html

### Secondary signatures

| Byte | Format |
|------|--------|
| `0x67` | KDBX 3.1 and 4.x — implemented here |
| `0x66` | KDBX pre-release — implemented upon request |
| `0x65` | KeePass 1.x `.kdb` — implemented upon request, separate repository |

### Cryptographic dependencies

| Primitive | Source |
|-----------|--------|
| AES-256-CBC (outer encryption, KDBX 3.1) | WebCrypto |
| ChaCha20 (outer encryption, KDBX 4.x) | `@keepass-web/chacha20` |
| AES-KDF (key derivation, KDBX 3.1) | WebCrypto |
| Argon2d / Argon2id (key derivation, KDBX 4.x) | `@keepass-web/argon2` |
| HMAC-SHA256 (block authentication, KDBX 4.x) | WebCrypto |
| Salsa20 (inner random stream, KDBX 3.1) | `@keepass-web/chacha20` |
| ChaCha20 (inner random stream, KDBX 4.x) | `@keepass-web/chacha20` |

### Test corpus

Two components:

- To test correctness, custom files created with KeePassXC with fully known contents, covering every edge case.
- To test interoperability, borrowing from KeePassXC `tests/data` and fixtures from `keepass-rs`, `File::KDBX`, and `KeePassJava2`.
