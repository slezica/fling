# Fling

Share sensitive information with your colleagues, by agreeing on a secret and sending encrypted data through the local
network.


### Installation

Just grab `bin/fling` from this repository. You'll need `python3` and the `cryptography` module installed.


### Usage

Bob wants to share a super important file with Alice, but doesn't want to upload it anywhere. Knowing they are both on
the same local network, they agree on a secret phrase (usually by yelling across the room).

Then Bob runs:

```bash
cat super_important_file | fling "the secret"
```

At the same time, Alice runs:

```bash
fling --catch "the secret"
```

Voilá! The file will be sent over the local network, encrypted using the shared secret.

You can fling stuff other than files, of course. Bob can pipe anything he wants to `fling` via stdin, and Alice can
redirect stdout on her side.


### How?

When Bob runs `fling` with the secret, the tool starts broadcasting the secret's `sha256` hash over UDP. When Alice runs
`fling --catch`, she starts listening for UDP broadcasts containing that hash.

By matching their hashes, the two `fling` instances will find each other. The data will be encrypted with the secret
(the preimage of the hash being broadcasted), sent and decrypted on the other side.

Note that, if the secret is weak, it's easy for an attacker to obtain the data using brute force.


### Future Work

1. Use a key derivation algorithm instead of a single hash, to slow down brute-froce attacks.

