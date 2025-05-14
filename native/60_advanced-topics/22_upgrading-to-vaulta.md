---
title: Upgrading to Vaulta from EOS
sidebar_label: Upgrading to Vaulta
---

The EOS Network has rebranded to Vaulta.
There are some basic changes to the network that you need to be aware of when upgrading your applications to Vaulta.

## Basic information

- Network name: **Vaulta**
- System contract: `core.vaulta`
- Token symbol: **A**
- Token precision: **4**
- Token contract: `core.vaulta`
- Token swap contract: `core.vaulta`
- Token swap ratio: **1:1**

## The system contract

The new system contract is `core.vaulta` which serves three purposes:
- It is a token contract for the new token `A`
- It is a token swap contract
- It is a system contract that wraps all the functionality of the old system contract (`eosio`)

The new system contract is a drop-in replacement for the old system contract.
You can use the same user-facing actions that you would with the `eosio` contract, except that the token symbol is
now `A` instead of `EOS` for all actions that have an `asset` that expects `EOS` as the symbol.

## The token

The new `A` token is a drop-in replacement for `EOS`, has the same precision (4), and max supply (2.1 billion).

### Integrating the new token

You will need to add new handlers for catching token transfers to your smart contracts if you want to support the
`A` token.

```cpp
[[eosio::on_notify("core.vaulta::transfer")]]
void on_transfer(name from, name to, asset quantity, string memo) {
    // Fail out on self transfers and transfers not to this contract
    if (from == get_self() || to != get_self()) return;
    
    // ... do your logic here
}
```

If you want to simply make sure that your `EOS` supporting contract isn't allowing `A` transfers, you can
prevent the action from being executed by checking the symbol of the asset or contract.

```cpp
// Deny transfers from all tokens except EOS/eosio.token
[[eosio::on_notify("*::transfer")]]
void on_transfer(name from, name to, asset quantity, string memo) {
    if (from == get_self() || to != get_self()) return;
    
    // The contract that initiated the transfer
    auto contract = get_first_receiver();
    if (contract != name("eosio.token")) {
        check(false, "This contract only supports EOS transfers");
    }
}

// Or explicitly denying A transfers
[[eosio::on_notify("core.vaulta::transfer")]]
void on_vaulta(name from, name to, asset quantity, string memo) {
    // Keep in mind you still want these checks, or you will not be able to 
    // transfer A from the smart contract.
    if (from == get_self() || to != get_self()) return;
    check(false, "This contract does not support Vaulta (A) transfers");
}
```

### Transferring Vaulta tokens

Transferring Vaulta tokens is the same as transferring EOS tokens.

Here is an example using [Wharfkit](https://wharfkit.com/) to transfer `A` tokens:

```javascript
import { Session, Chains } from "@wharfkit/session"
// See here for more wallet plugins: https://wharfkit.com/plugins?tag=wallet-plugin
import { WalletPluginPrivateKey } from "@wharfkit/wallet-plugin-privatekey"

const session = new Session({
    chain: Chains.Vaulta,
    actor: "youraccount",
    permission: "active",

    // Never use this plugin in the frontend!
    walletPlugin: new WalletPluginPrivateKey(
        process.env.YOUR_PRIVATE_KEY
    ),
}, {});

const result = await session.transact({
    action: {
        account: "core.vaulta",
        name: "transfer",
        authorization: [session.permissionLevel],
        data: {
            from: session.actor, // this is your account
            to: "change.me", // the recipient's account
            quantity: "100.0000 A",
            memo: "your optional extra data", // optional, can be blank
        },
    },
})
```



### Getting a balance

You can get a balance in the same way that you would with the `eosio.token` contract.

Here is an example using [Wharfkit](https://wharfkit.com/) to get a balance:

```javascript
import { APIClient } from "@wharfkit/antelope"
const client = new APIClient({ url: "https://vaulta.greymass.com" });
// or session.client
const response = await client.v1.chain.get_table_rows(
    {
        code: "core.vaulta",
        scope: "some.account",
        table: "accounts",
        json: true
    }
);
console.log(
    JSON.stringify(response, null, 4)
)
```

## Swapping

If you have EOS in your account, you will need to swap it to `A`.
The swap is a 1:1 swap, so you will get the same amount of `A` as you had EOS.

### RAM considerations

The swap contract will sponsor the RAM required for the swap, so you do not need to worry about RAM costs
when swapping, however you will need 241 bytes of RAM available in your account on your first transfer.

### Swapping from EOS to `A`

Swapping from EOS to `A` is as simple as sending your EOS tokens to the new system contract (`core.vaulta`).
You can do this using any wallet that supports the EOS network, or using javascript like below.

Here is an example using [Wharfkit](https://wharfkit.com/) to swap from EOS to `A`:

```javascript
await api.transact({
    actions: [{
        // using the EOS token contract
        account: "eosio.token",
        name: "transfer",
        authorization: [session.permissionLevel],
        data: {
            from: session.actor,
            to: "core.vaulta",
            quantity: "100.0000 EOS",
            memo: ""
        }
    }]
}, {
    blocksBehind: 3,
    expireSeconds: 30,
});

```

In the transfer above, the sender account will get back the same amount of `A` as the amount of EOS
they sent to the `core.vaulta` contract, in this case `100.0000 A`.

Here is an example of using cleos to swap from EOS to `A`:

```bash
cleos push action core.vaulta transfer '["youraccount", "change.me", "100.0000 A", "your optional memo"]' -p 
youraccount
```

> ⚠ **Take precautions**
>
> Make sure to double check the recipient address before sending your tokens. If you send to the wrong address,
> you will not be able to get your tokens back. You should also try to use a small amount first to test the
> transaction before sending a large amount.

### Swapping to another account

You can swap tokens from your account to another account using the `swapto` action.

```cpp
swapto(
    const name& from, 
    const name& to, 
    const asset& quantity, 
    const std::string& memo
)
```

```javascript
await session.transact({
    action: {
        account: "core.vaulta",
        name: "swapto",
        authorization: [session.permissionLevel],
        data: {
            from: session.actor,
            to: "some.account",
            quantity: "100.0000 EOS",
            memo: "",
        },
    },
})
```

This will swap 100 EOS in your account to 100 `A` and send it to the `some.account` account.

> ⚠ **Important**
>
> You do **NOT** need to catch the `swapto` action in your smart contract. This is a special action that
> wraps the `transfer` action and is only used for the swap. You will still be able to catch the underlying
> `transfer` action in your contract.

#### Blocking this to your account

You might not want your account to be able to be a recipient of the `swapto` action.
For instance if you are a centralized exchange and you want to prevent your users from 
swapping to your account and possibly sending you untracked tokens.

You can prevent this by adding your own account to a blocklist.

```cpp
ACTION blockswapto(name account, bool blocked);
```

Here is and example using [Wharfkit](https://wharfkit.com/) to block the `swapto` action:

```javascript
await session.transact({
    action: {
        account: "core.vaulta",
        name: "blockswapto",
        authorization: [session.permissionLevel],
        data: {
            account: session.actor,
            blocked: true
        },
    },
})
```

Example using cleos:

```bash
cleos push action core.vaulta blockswapto '["youraccount", true]' -p youraccount
```

## System contract actions

The new system contract has the same actions as the old system contract, except that the token symbol is now `A`.
With actions that would normally expect EOS, you will need to use `A` instead. 

For example, if you wanted to buy RAM using Vaulta tokens, you would do it like this:

```javascript
await session.transact({
    action: {
        account: "core.vaulta",
        name: "buyram",
        authorization: [session.permissionLevel],
        data: {
            payer: session.actor,
            receiver: session.actor,
            quant: "1.0000 A"
        },
    },
})
```

You will also get `A` tokens back if you sell RAM.

```javascript
await session.transact({
    action: {
        account: "core.vaulta",
        name: "sellram",
        authorization: [session.permissionLevel],
        data: {
            account: session.actor,
            bytes: 1024
        },
    },
})
```


## Q&A

- **What happens to staked eos (in REX or staked to an account)?**
    - All staked EOS will be automatically swapped when withdrawing from the new system contract. You do not need to do anything to swap your staked EOS.
- **What if I don't swap in time?**
    - You will always be able to move from EOS to `A`
- **Can I swap back from `A` to EOS?**
    - This is a period where a bidirectional swap is supported. You can send your Vaulta tokens to the `core.vaulta` contract and get back EOS tokens. 
    - This is aimed at making sure that contract developers and users have time to upgrade their contracts and wallets to support the new token.


