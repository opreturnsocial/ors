# ORS - OP_RETURN SOCIAL Protocol

Core protocol specifications for ORS, a permissionless social protocol built on bitcoin.

## Documents

- **[ORS-01.md](ORS-01.md)** - Base protocol specification: what ORS is, identity, post structure
- **[wire-formats/v0.md](wire-formats/v0.md)** - Wire format v0: single OP_RETURN
- **[wire-formats/v1.md](wire-formats/v1.md)** - Wire format v1: chunked 80-byte

## Kind Specs

Content kind specifications live in the **[ORSK repo](https://github.com/opreturnsocial/orsk)**. The ORSK repo defines what each kind value means (text note, profile update, reply, repost, etc.).

## Overview

ORS posts are bitcoin transactions with `OP_RETURN` outputs. Each post is signed with a Schnorr key pair - no accounts, no servers, no tokens. The bitcoin blockchain is the data layer.

See [ORS-01.md](ORS-01.md) to get started.
