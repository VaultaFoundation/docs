---
title: Token Standard
---

A token standard is a set of rules that all tokens on a blockchain must follow.
This allows for interoperability between different tokens and applications.

> **Note**
> 
> Vaulta tokens support more than one token per contract, however in practice not 
> many contracts will do this.

## Actions 

### create

```cpp
[[eosio::action]]
void create(const name& issuer, const asset& maximum_supply);
```

Creates a new token with a maximum supply limit, and sets the issuer account.
The `symbol` of the `asset` defined the precision (decimals) and the token ticker.
For instance, a maximum supply of `1000.0000 XYZ` means that the token has a precision of 4 decimals,
a ticker of `XYZ`, and a maximum supply of `1000`.

**Parameters:**
- `issuer` - The account that creates the token
- `maximum_supply` - The maximum supply set for the token created

**Preconditions:**
- Token symbol must not already exist

### issue

```cpp
[[eosio::action]]
void issue(const name& to, const asset& quantity, const string& memo);
```

Issues a specific quantity of tokens to an account.

**Preconditions:**
- Must be the issuer of the token

**Parameters:**
- `to` - The account to issue tokens to (must be the same as the issuer)
- `quantity` - The amount of tokens to be issued
- `memo` - The memo string that accompanies the token issue transaction

### retire

```cpp
[[eosio::action]]
void retire(const asset& quantity, const string& memo);
```

Effectively burns the specified quantity of tokens, removing them from circulation.
Only the token issuer can retire tokens. Another way to burn tokens would be to 
send them to a blackhole account (e.g. `null.vaulta`).

**Parameters:**
- `quantity` - The quantity of tokens to retire
- `memo` - The memo string to accompany the transaction

### transfer

```cpp
[[eosio::action]]
void transfer(const name& from, const name& to, const asset& quantity, const string& memo);
```

Transfers a specified quantity of tokens from one account to another.

**Parameters:**
- `from` - The account to transfer from
- `to` - The account to be transferred to
- `quantity` - The quantity of tokens to be transferred
- `memo` - The memo string to accompany the transaction

### open

```cpp
[[eosio::action]]
void open(const name& owner, const symbol& symbol, const name& ram_payer);
```

Each account's balance is a row in a table, which costs 240 bytes of RAM.
This action creates a row in the table for the owner account and token symbol.
If this is not done, then the first sender of a token to an account that does 
not have tokens will pay the RAM cost of creating the row.

**Parameters:**
- `owner` - The account to be created
- `symbol` - The token to be paid with by ram_payer
- `ram_payer` - The account that supports the cost of this action

Additional information can be found in [issue #62](https://github.com/EOSIO/eosio.contracts/issues/62) and [issue #61](https://github.com/EOSIO/eosio.contracts/issues/61).

### close

```cpp
[[eosio::action]]
void close(const name& owner, const symbol& symbol);
```

This action is the opposite of open. 
It closes the row for an account for a specific token symbol and reclaims the RAM.

**Parameters:**
- `owner` - The owner account to execute the close action for
- `symbol` - The symbol of the token to execute the close action for

**Preconditions:**
- The pair of owner plus symbol must exist, otherwise no action is executed
- If the pair of owner plus symbol exists, the balance must be zero


## Tables

### Account data structure

```cpp
struct [[eosio::table]] account {
    asset    balance;
    uint64_t primary_key() const { return balance.symbol.code().raw(); }
};
```

The `account` struct represents an individual token account and stores the balance for a specific token symbol.


```cpp
typedef eosio::multi_index<"accounts"_n, account> accounts;
```

The `accounts` table stores token balances for all accounts.

- **Table Name:** `accounts`
- **Index Type:** Primary index on the token symbol code (ticker)
- **Scope:** The scope is the account name, which is the owner of the token balance

**Usage:**
- Stores balance information for each account and token combination
- Used during transfers to check balances and update them
- Queried when retrieving an account's balance for a specific token

### Fetching balances using [Wharfkit](https://wharfkit.com/)

```typescript
import {APIClient} from "@wharfkit/session"

const client = new APIClient({ url: Chains.Vaulta.url });

const result = await client.v1.chain.get_table_rows({
    json: true,
    code: 'core.vaulta',
    scope: 'SOME_ACCOUNT_HERE',
    table: 'accounts',
});

/*
{
  rows: [
    {
      balance: "100.0000 A",
    }
  ],
  more: false,
}
 */
```

### currency_stats

```cpp
struct [[eosio::table]] currency_stats {
    asset    supply;
    asset    max_supply;
    name     issuer;
    uint64_t primary_key() const { return supply.symbol.code().raw(); }
};
```

The `currency_stats` struct stores information about a token.

**Fields:**
- `supply` - The current supply of the token in circulation
- `max_supply` - The maximum possible supply of the token
- `issuer` - The account name of the token issuer who has authority to issue new tokens

```cpp
typedef eosio::multi_index<"stat"_n, currency_stats> stats;
```

The `stats` table stores information about each token type.

- **Table Name:** `stat`
- **Index Type:** Primary index on the token symbol code (ticker)
- **Scope:** The scope is the token symbol (ticker)

**Usage:**
- Stores supply, maximum supply, and issuer information for each token
- Checked during token operations to validate permissions and limits
- Used to enforce rules like maximum supply constraints
- Queried to get current supply and other token information

### Fetching stats using [Wharfkit](https://wharfkit.com/)

```typescript
import {APIClient} from "@wharfkit/session"

const client = new APIClient({ url: Chains.Vaulta.url });

const result = await client.v1.chain.get_table_rows({
    json: true,
    code: 'core.vaulta',
    scope: 'A',
    table: 'stat',
});

/*
{
  rows: [
    {
      supply: "2100000000.0000 A",
      max_supply: "2100000000.0000 A",
      issuer: "core.vaulta",
    }
  ],
  more: false,
}

 */
```

