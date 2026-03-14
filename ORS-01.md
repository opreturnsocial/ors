# ORS-01: Base Protocol Specification

**Status:** MVP Specification | **Version:** 0.1 (Draft)

## What is ORS?

ORS (OP_RETURN SOCIAL) is a minimal, permissionless social protocol built on bitcoin. Posts are embedded directly in bitcoin transactions using `OP_RETURN` outputs, signed with Schnorr keys (BIP-340). No servers, no accounts, no tokens - just bitcoin transactions.

ORS is heavily inspired by [Nostr](https://github.com/nostr-protocol). The core difference is the transport layer - Nostr uses relays (servers), ORS uses the bitcoin blockchain directly.

Each post consists of:
- A 32-byte x-only Schnorr public key (the author identity)
- A 64-byte Schnorr signature over the content
- A 1-byte kind field (what type of post this is)
- Kind-specific data (the content)

## Wire Formats

ORS defines two wire formats for embedding posts in bitcoin transactions:

- **[wire-formats/v0.md](wire-formats/v0.md)** - Single `OP_RETURN` output
- **[wire-formats/v1.md](wire-formats/v1.md)** - Chunked across multiple 80-byte `OP_RETURN` outputs

## Kind Specs

Content kinds (what a post means) are specified separately in the **[ORSK repo](https://github.com/opreturnsocial/orsk)**. The kind registry lists all defined kinds and links to their specs.

## Post Identity

A post's canonical identifier is the bitcoin txid:
- For v0: the txid of the single transaction containing the `OP_RETURN`
- For v1: the txid of the chunk 0 (root) transaction

## Author Identity

Authors are identified by their 32-byte x-only Schnorr public key. There is no registration step - any key pair is a valid ORS identity.

## Key Decoupling

The ORS identity key is intentionally decoupled from the bitcoin transaction signing key. The public key embedded in an ORS payload is a social identity key - it has no required relationship to the key that signs or funds the bitcoin transaction carrying it.

This is a deliberate design choice with several consequences:

- **No on-chain bitcoin required to post.** A third party (e.g. a Lightning node or a relay service) can broadcast the transaction on behalf of the author. The author signs the ORS payload with their identity key; someone else pays the miner fee.
- **Protocol-agnostic transaction structure.** ORS makes no assumptions about the transaction's input scripts. The carrier transaction can be P2WPKH, P2TR (taproot), P2SH, or any other type - ORS parsers only inspect the `OP_RETURN` output.
- **Separation of concerns.** Losing access to a transaction-signing key (e.g. a hot wallet compromise) does not compromise the author's social identity, and vice versa.
