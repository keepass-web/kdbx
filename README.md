# @keepass-web/kdbx

KDBX 3.1 and 4.x parser and serializer for the [keepass-web](https://github.com/keepass-web) project.

Handles KDBX files reporting 0x67 format.

## Specification

See [SPEC.md](./SPEC.md).

## Usage

Published to npm under the `@keepass-web` scope with provenance signing. Depends
on [`@keepass-web/chacha20`](https://github.com/keepass-web) (ChaCha20/Salsa20)
and [`@keepass-web/argon2`](https://github.com/keepass-web) (Argon2d/Argon2id);
AES, HMAC, SHA, and GZip come from WebCrypto and the Web Streams compression
API, so the package is isomorphic (browser and Node) and otherwise
dependency-free.

```ts
import { Kdbx, Credentials, createEntry, appendChild, getText, getChild } from '@keepass-web/kdbx';

// Create a new KDBX 4.x database (ChaCha20 + Argon2id by default).
const credentials = Credentials.fromPassword('correct horse battery staple');
const db = await Kdbx.create(credentials, { databaseName: 'My Vault' });
appendChild(db.getRootGroup(), createEntry({ title: 'GitHub', username: 'octocat', password: 's3cret' }));
const bytes = await db.save();

// Load it back.
const reopened = await Kdbx.load(bytes, credentials);
const entry = getChild(reopened.getRootGroup(), 'Entry')!;
```

`Kdbx.create` accepts `version` (3 or 4), `cipher` (`aes`/`chacha20`), `kdf`
(`argon2id`/`argon2d`/`aes`), `compression`, and KDF tuning. The canonical state
of a database is its `<KeePassFile>` XML tree (`db.root`); `Protected="True"`
values are held as plaintext in memory and (re-)encrypted with the inner random
stream on save.

## Development

```sh
npm ci
npm run typecheck
npm run lint
npm test
```
