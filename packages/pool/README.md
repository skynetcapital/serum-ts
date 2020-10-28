# Serum Pools JS Library

JavaScript client library for interacting with Project Serum Pools.

## Installation

Using npm:

```
npm install @solana/web3.js @project-serum/pool
```

Using yarn:

```
yarn add @solana/web3.js @project-serum/pool
```

## Usage

### Create pool tokens

```js
import { Account, Connection, PublicKey } from '@solana/web3.js';
import { loadPoolInfo } from '@project-serum/pool';
import { PoolTransactions } from './transactions';
import BN from 'bn.js';

let connection = new Connection('...');
let poolAddress = new PublicKey('...'); // Address of the pool.
let payer = new Account('...'); // Account to pay for solana fees.

let poolInfo = await loadPoolInfo(connection, poolAddress);
let { transaction, signers } = PoolTransactions.execute(
  poolInfo,
  {
    // Number of tokens to create.
    create: new BN(100),
  },
  {
    // Spl-token account to send the created tokens.
    poolTokenAccount: new PublicKey('...'),
    // Spl-token accounts to pull funds from.
    assetAccounts: [new PublicKey('...'), new Public('...')],
    // Owner of poolTokenAccount and assetAccounts.
    owner: payer.publicKey,
  },
  // Expected creation cost.
  [new BN(10), new BN(10)],
);
await connection.sendTransaction(transaction, [payer, ...signers]);
```

### Redeem pool tokens

```js
import { Account, Connection, PublicKey } from '@solana/web3.js';
import { loadPoolInfo } from '@project-serum/pool';
import { PoolTransactions } from './transactions';
import BN from 'bn.js';

let connection = new Connection('...');
let poolAddress = new PublicKey('...'); // Address of the pool.
let payer = new Account('...'); // Account to pay for solana fees.

let poolInfo = await loadPoolInfo(connection, poolAddress);
let { transaction, signers } = PoolTransactions.execute(
  poolInfo,
  {
    // Number of tokens to redeem.
    redeem: new BN(100),
  },
  {
    // Spl-token account to pull the pool tokens to redeem from.
    poolTokenAccount: new PublicKey('...'),
    // Spl-token accounts to send the redemption proceeds.
    assetAccounts: [new PublicKey('...'), new Public('...')],
    // Owner of poolTokenAccount and assetAccounts.
    owner: payer.publicKey,
  },
  // Expected redemption proceeds.
  [new BN(10), new BN(10)],
);
await connection.sendTransaction(transaction, [payer, ...signers]);
```

## API Reference

[API Reference](https://project-serum.github.io/serum-ts/pool/modules/_index_.html)