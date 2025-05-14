---
title: Hardhat
---

[Hardhat](https://hardhat.org/) is the most widely used development environment for Ethereum smart contracts.

If you want to deploy your smart contracts to the Vaulta EVM using hardhat, update your `hardhat.config.js` file 
with the following network configuration:

```javascript
// mainnet
const config: HardhatUserConfig = {
    // ...

    networks: {
        vaulta_evm: {
            url: "https://api.evm.eosnetwork.com",
            accounts:[process.env.PRIVATE_KEY],
        },
        vaulta_evm_testnet: {
            url: "https://api.testnet.evm.eosnetwork.com",
            accounts:[process.env.PRIVATE_KEY],
        }
    }
};
```

Now you can deploy your contract to either the mainnet or the testnet using:

```bash
npx hardhat run scripts/deploy.js --network vaulta_evm

// or for testnet
npx hardhat run scripts/deploy.js --network vaulta_evm_testnet
```
