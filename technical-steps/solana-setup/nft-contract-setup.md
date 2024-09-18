---
description: >-
  Now, we’ll create an ERC721 smart contract that supports upgradeable metadata
  for NFTs. Do NOT deploy the contract just yet!
---

# NFT Contract Setup

We're going to make an ERC721 NFT that contains special superpowers that can be upgraded over time.&#x20;



**1. Create the Contract**

We’ll use OpenZeppelin’s **upgradeable contracts** to make this flexible for future updates.

**Steps for the tutorial**:

1. Create a folder called `contracts/` in your root project directory.
2. Add a new file called `SuperPowerNFT.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
import "./../node_modules/@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "./../node_modules/@openzeppelin/contracts/access/Ownable.sol";
import "./../node_modules/@openzeppelin/contracts/token/ERC721/extensions/ERC721URIStorage.sol";

/**
 * @title SuperPowerNFT
 * @dev This contract is a simple ERC721 token that allows creating NFTs with metadata stored externally.
 * The first NFT is minted in the constructor with a predefined token URI.
 */
contract SuperPowerNFT is ERC721URIStorage, Ownable {
    uint256 public tokenCounter;
    
    /**
     * @dev Constructor that mints the first NFT with a predefined tokenURI.
     * The constructor also sets the token counter to 1.
     * @param tokenURI The URI pointing to the metadata of the initial NFT (e.g., a JSON file).
     */
     constructor(string memory name, string memory symbol, string memory tokenURI) 
        ERC721(name, symbol)
        Ownable()
    {
        
        tokenCounter = 1; // Start token counter at 1
        // Mint the first NFT to the owner (deployer) of the contract
        _safeMint(msg.sender, tokenCounter);
        _setTokenURI(tokenCounter, tokenURI);
    }

    /**
     * @dev Creates a new NFT with a given tokenURI.
     * @return The ID of the newly minted token.
     */
    function createSuperPower() public onlyOwner returns (uint256) {
        tokenCounter += 1; // Increment token counter
        uint256 newItemId = tokenCounter;

        // Mint the new token to the owner's address
        _safeMint(msg.sender, newItemId);

        return newItemId;
    }

    /**
     * @dev Updates the tokenURI for a specific token ID.
     * Only the owner can call this function.
     * @param tokenId The ID of the token whose URI is being updated.
     * @param newTokenURI The new URI pointing to the updated metadata.
     */
    function updateTokenURI(uint256 tokenId, string memory newTokenURI) public onlyOwner {
        require(_exists(tokenId), "ERC721URIStorage: URI set of nonexistent token");
        _setTokenURI(tokenId, newTokenURI); // Update the token's metadata URI
    }

}
```



**Let's wait to deploy this contract...**&#x20;

**Key Elements:**

* **Minting:** When you mint a new NFT with the `createSuperPower()` function, you provide the `tokenURI` (which points to the metadata on Arweave).
* **ERC721URIStorageUpgradeable**: This is the upgradeable version of ERC721 with URI storage for metadata.
* **Initializable**: Instead of using constructors, this contract uses an `initialize()` function due to the upgradeable pattern.
* **createSuperPower**: This function allows the owner to mint new NFTs with metadata representing a superpower.
* **upgradeSuperPower**: This function allows the owner to update the metadata (or "upgrade" the superpower traits) by changing the URI.
