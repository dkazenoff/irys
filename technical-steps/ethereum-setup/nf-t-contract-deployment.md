---
description: We're now going to compile the contract and deploy it on the Sepolia testnet!
---

# NF T Contract Deployment

Back in the Environment Setup steps, we asked you to have an Ethereum wallet on hand funded with some faucet Sepolia Ethereum tokens. This will allow us to deploy the contract to Sepolia so we can get some live testing going. Remember that it costs some tokens to perform any amount of state changes on the network



1. **Compile the Contract via the CLI**

```bash
forge build
```

2. **Deploy from the Command Line**

Be sure that your constructor arguments align with the solidity code itself

```
forge create  contracts/SuperPowerNFT.sol:SuperPowerNFT \
--rpc-url https://sepolia.ethereum.validationcloud.io/v1/Hk4H82VinPWoRLLIr_awwziFmE0rAQMLyyDlV5ID94E \
--private-key <your_raw_private_key> \
--constructor-args "SuperPowerNFT" "SPNFT" "https://devnet.irys.xyz/3CiqtD8C2e8fycQs1VRzwmBeuqHGmltpDRC4Vo8hRqs" \ 
--priority-gas-price 100000000000
```

{% hint style="info" %}
You can run "foundry create -h" to get a list of all the possible arguments. If you run into gas fee issues, you can check the current gas price and adjust your priority gas price accordingly: [https://sepolia.beaconcha.in/gasnow](https://sepolia.beaconcha.in/gasnow)
{% endhint %}



3. Verifying the Contract

Verifying the contract allows you and the public to view the functions and code that make up the contract. This is important for building trust and assuring the community that you are more legit.





```
```













