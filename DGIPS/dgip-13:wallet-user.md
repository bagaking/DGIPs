---
dgip: 13
title: WalletUser
author: bagaking <kinghand@foxmail.com>
status: Draft
type: Role
created: 2019-08-07
---

## Abstract

Wallet user corresponds to a user in real life. It can be identified by the `user-id (dgid)` in the dgcenter.  
Each wallet user will have a set of wallets that record the real-world token address for each of each user’s token.

### User-ID (DGID)

User-ID in dgcenter are defined as dgid (Decentralized Game's Internal iDentifier) in the dgcenter.

See [dgid](./dgip-20:dgid.md)

### Wallets

Wallets are a group of user's wallet entry, which **SHOULD** these three fields.

- dgid: id of the user
- sym: symbol of the token
- address: the related address
