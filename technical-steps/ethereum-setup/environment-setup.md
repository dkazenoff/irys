---
description: Let's get the dependencies sorted out first!
---

# Environment Setup

Setting up a smart contract that supports upgradeable metadata on Ethereum involves several steps, but we'll walk you through every step! This tutorial was run on MacOS Sonoma 14.

We'll need the following dependencies to get started:

* [**Node.js**](https://nodejs.org/en) and **TypeScript** for the scripting logic
* [**Foundry**](https://github.com/foundry-rs/foundry) framework for smart contract development and deployment
* [**OpenZeppelin**](https://docs.openzeppelin.com/contracts/4.x/erc721) library for upgradeable contracts&#x20;
* [**Ethers.js**:](https://classic.yarnpkg.com/en/package/ethers) for interacting with the Ethereum blockchain
* [**Irys SDK**](https://arweave-tools.irys.xyz/irys-sdk) for storing and updating the NFT Metadata on Arweave
* **Ethereum Wallet** [**funded with Sepolia Faucet tokens**](https://cloud.google.com/application/web3/faucet/ethereum/sepolia)
* **Sepolia Testnet RPC Key (provided in the tutorial)**

&#x20;

1. **Create a new TypeScript project**

**Ensure Yarn Package Manager is installed globally:**

```bash
npm install --global yarn
```

Now lets install some of the core dependencies:

```bash
mkdir superpower-nft && cd superpower-nft
yarn init -y
yarn add typescript @types/node ts-node ethers @openzeppelin/contracts@4.3.2

```

**Make sure your package.json is equipped like so:**

2. **Install Foundry and Run the Setup**&#x20;

Follow the CLI prompts carefully. If you run into any installation issues, see their [FAQs](https://book.getfoundry.sh/faq).

```bash
curl -L https://foundry.paradigm.xyz | bash
foundryup
```

Now, Foundry is installed. We’ll also need to configure the `foundry.toml` file to connect to the Sepolia testnet and add OpenZeppelin dependencies. Create a "foundry.toml" file in the root of the directory if it wasn't created already, and specify our RPC endpoint;

```toml
[default]
solc_version = "0.8.0"  # Specify the Solidity version you’re using
optimizer = true          # Enable optimizer for the compiler
optimizer_runs = 200      # Default optimization runs
sepolia = "https://sepolia.ethereum.validationcloud.io/v1/Hk4H82VinPWoRLLIr_awwziFmE0rAQMLyyDlV5ID94E"
```

We'll use OpenZeppelin's upgradeable contracts library for this example.

3. Install the Irys SDK

```bash
yarn add @irys/sdk @irys/upload @irys/upload-ethereum 
```

4. Package.json and TypeScript Configs:

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

