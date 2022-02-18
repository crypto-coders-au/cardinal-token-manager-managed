# Cardinal

[![License](https://img.shields.io/badge/license-AGPL%203.0-blue)](https://github.com/cardinal-labs/cardinal-token-manager/blob/master/LICENSE)
[![Release](https://github.com/cardinal-labs/cardinal-token-manager/actions/workflows/release.yml/badge.svg?branch=v0.0.27)](https://github.com/cardinal-labs/cardinal-token-manager/actions/workflows/release.yml)

<p align="center">
    <img src="./images/banner.png" />
</p>

<p align="center">
    An open protocol for issuing managed tokens on Solana.
</p>

## Background

Cardinal is a composable protocol for issuing conditional NFTs that are managed by the protocol. Using the invalidators and approvers in various ways allows for building rentals, expiring in-game items, subscriptions, permits, tickets, passes and more.

Carinal protocol provides a token-manager implementation as well as basic plugins for paid claim, permissioned transfer, and time invalidation. These plugins can be extended to support various use cases or similar ones built with entirely new logic for token handling the token invalidation.

## Packages

| Package                        | Description                                         | Version                                                                                                                             | Docs                                                                                                               |
| :----------------------------- | :-------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------- |
| `cardinal-token-manager`       | Manages conditionally owned tokens                  | [![Crates.io](https://img.shields.io/crates/v/cardinal-token-manager)](https://crates.io/crates/cardinal-token-manager)             | [![Docs.rs](https://docs.rs/cardinal-token-manager/badge.svg)](https://docs.rs/cardinal-token-manager)             |
| `cardinal-paid-claim-approver` | Approves users to claim tokens from a token-manager | [![Crates.io](https://img.shields.io/crates/v/cardinal-paid-claim-approver)](https://crates.io/crates/cardinal-paid-claim-approver) | [![Docs.rs](https://docs.rs/cardinal-paid-claim-approver/badge.svg)](https://docs.rs/cardinal-paid-claim-approver) |
| `cardinal-time-invalidator`    | Invalidator for time-based token-managers           | [![Crates.io](https://img.shields.io/crates/v/cardinal-time-invalidator)](https://crates.io/crates/cardinal-time-invalidator)       | [![Docs.rs](https://docs.rs/cardinal-time-invalidator/badge.svg)](https://docs.rs/cardinal-time-invalidator)       |
| `cardinal-use-invalidator`     | Invalidator for use-based token-managers            | [![Crates.io](https://img.shields.io/crates/v/cardinal-use-invalidator)](https://crates.io/crates/cardinal-use-invalidator)         | [![Docs.rs](https://docs.rs/cardinal-use-invalidator/badge.svg)](https://docs.rs/cardinal-use-invalidator)         |
| `@cardinal/token-manager`      | TypeScript SDK for token-manager                    | [![npm](https://img.shields.io/npm/v/@cardinal/token-manager.svg)](https://www.npmjs.com/package/@cardinal/token-manager)           | [![Docs](https://img.shields.io/badge/docs-typedoc-blue)](https://cardinal-labs.github.io/cardinal-token-manager/) |

## Addresses

Program addresses are the same on devnet, testnet, and mainnet-beta.

- TokenManager: [`mgr99QFMYByTqGPWmNqunV7vBLmWWXdSrHUfV8Jf3JM`](https://explorer.solana.com/address/mgr99QFMYByTqGPWmNqunV7vBLmWWXdSrHUfV8Jf3JM)
- PaidClaimApprover: [`pcaBwhJ1YHp7UDA7HASpQsRUmUNwzgYaLQto2kSj1fR`](https://explorer.solana.com/address/pcaBwhJ1YHp7UDA7HASpQsRUmUNwzgYaLQto2kSj1fR)
- TimeInvalidator: [`tmeEDp1RgoDtZFtx6qod3HkbQmv9LMe36uqKVvsLTDE`](https://explorer.solana.com/address/tmeEDp1RgoDtZFtx6qod3HkbQmv9LMe36uqKVvsLTDE)
- UseInvalidator: [`useZ65tbyvWpdYCLDJaegGK34Lnsi8S3jZdwx8122qp`](https://explorer.solana.com/address/useZ65tbyvWpdYCLDJaegGK34Lnsi8S3jZdwx8122qp)

## Plugins

Cardinal token-manager is made to be composable. It allows for plugins for

1. Claim approvers
2. Transfer authorities
3. Invalidators

When instantiating a token-manager, the issuer can set a claim approver, transfer authority and invalidators that can control the claim, transfer and invalidate mechanisms. These are all plugins that can be pointed to any program-derived account or user owned account. Out of the box, there are basic plugins to power use and time rentals and subscriptions.

## Documentation

Documentation is a work in progress. For now, one should read [the tests](/tests/issueUnissue.spec.ts).

We soon plan on releasing a React library to make it easy to integrate Cardinal ui components with your frontend.

## Installation

#### Javascript create rental example

```
npm i @cardinal/token-manager
```

```javascript
import { Connection } from "@solana/web3.js";

const issueTokenParameters = {
  paymentAmount: new BN(10),
  paymentMint: new PublicKey('...'),
  expiration: (Date.now() / 1000) + 86400,
  usages: 1,
  mint: new PublicKey('...'),
  amount: new BN(1), // default 1
  issuerTokenAccountId: new PublicKey('3c5mtZ9PpGu3hj1W1a13Hie1CAXKnRyj2xruNxwWcWTz'),
  visibility: "public" // default public
  kind: TokenManagerKind.Edition, // used for metaplex master / editions,
  invalidationType: InvalidationType.Return // indicates this token will be returned when invalidated
};

try {
    const [transaction] = await issueToken(issueTokenParameters);
    transaction.feePayer = wallet.publicKey;
    transaction.recentBlockhash = (
    await connection.getRecentBlockhash("max")
    ).blockhash;
    transaction.sign(wallet, masterEditionMint);
    await sendAndConfirmRawTransaction(connection, transaction.serialize(), {
        commitment: "confirmed",
    });
} catch (exception) {
  // handle exception
}
```

## License

Cardinal Protocol is licensed under the GNU Affero General Public License v3.0.

In short, this means that any changes to this code must be made open source and available under the AGPL-v3.0 license, even if only used privately.
