---
description: Let's get the dependencies sorted out first!
---

# Environment Setup

Setting up a smart contract that supports upgradeable metadata on Ethereum involves several steps, but we'll walk you through every step! This tutorial was run on MacOS Sonoma 14.

We'll need the following dependencies to get started:

* [**Node.js**](https://nodejs.org/en) and **TypeScript** for the scripting logic
* Anchor framework for Solana smart contract development and deployment
* [**Irys SDK**](https://arweave-tools.irys.xyz/irys-sdk) for storing and updating the NFT Metadata on Arweave
* **Solana Wallet** [**funded with Devnet tokens**](https://cloud.google.com/application/web3/faucet/ethereum/sepolia)
* **Devnet RPC (provided in the tutorial)**

&#x20;

1. **Install Yarn Package Manager**

**Ensure Yarn Package Manager is installed globally:**

```bash
sudo npm i -g yarn@1
```

2. **Install Core Dependencies like Solana, Rust, and Anchor VM (avm):**

```bash
sh -c "$(curl -sSfL https://release.solana.com/v1.18.17/install)" && \
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh && \
cargo install --git https://github.com/coral-xyz/anchor avm --locked --force && \
avm install latest && \
avm use latest 
```

Confirm Rust Version:

```
rustc -V 
```

At the time of this writing, we want to use Rust version 1.17. If your version of Rust is higher, please downgrade to 1.17:

```bash
rustup install 1.76.0
rustup default 1.76.0
rustup override set 1.76.0
rustc --version             # confirm version is 1.76.0
```

3. Initialize the Anchor Project and Run Default Tests

```bash
anchor init sol-superpowernft
cd sol-superpowernft
anchor test
```

You should see the compilation complete for the default files created.

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

3.



3. Install the Irys SDK

```bash
yarn add @irys/sdk @irys/upload @irys/upload-solana
```

4.
5. Package.json and TypeScript Configs:

tsconfig.json:

```json
{
    "compilerOptions": {
      "target": "ES6",                          // Target modern JavaScript
      "module": "commonjs",                     // Use CommonJS for Node.js compatibility
      "rootDir": "./",                          // Set the root directory (adjust if needed)
      "outDir": "./dist",                       // Output compiled JS to 'dist' folder
      "strict": true,                           // Enable strict type checking
      "esModuleInterop": true,                  // Allow ES6 module interop
      "skipLibCheck": true,                     // Skip type checking for libraries
      "forceConsistentCasingInFileNames": true  // Ensure consistent casing in imports
    },
    "include": [
      "scripts/**/*.ts"                         // Include all .ts files in the 'scripts' directory
    ],
    "exclude": [
      "node_modules"                            // Exclude dependencies
    ]
  }
```



package.json:

```json
{
  "name": "superpower-nft",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "scripts": {
    "build": "tsc",
    "upload": "ts-node ./scripts/data_uploader.ts",
    "deploy": "ts-node ./scripts/nft_deploy.ts",
    "upgrade": "ts-node ./scripts/upgrade.ts"
  },
  "dependencies": {
    "@irys/sdk": "^0.2.11",
    "@irys/upload": "^0.0.1",
    "@irys/upload-ethereum": "^0.0.1",
    "@openzeppelin/contracts": "4.3.2",
    "@types/node": "^22.5.4",
    "ethers": "^6.13.2",
    "ts-node": "^10.9.2",
    "typescript": "^5.5.4"
  }
}
```

5. Environment File

```
PRIVATE_KEY=<YOUR EXPORTED EVM PRIVATE KEY>
RPC_URL=https://sepolia.ethereum.validationcloud.io/v1/Hk4H82VinPWoRLLIr_awwziFmE0rAQMLyyDlV5ID94E
```

{% hint style="info" %}
Note: it is STRONGLY ADVISED to never put production private keys into plain text. Be sure to have all .env files ignored with any github repository uploads, and avoid using production keys carrying significant value in this manner.
{% endhint %}



Once everything is installed and set up, we'll be ready to move onto the contract setup steps.

