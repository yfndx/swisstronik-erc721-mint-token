
# Swisstronik ERC721 Mint Token

## Overview

This repository contains a Hardhat project for deploying and managing an ERC721 Mintable Token on the Ethereum network. Below is an explanation of the key components and how to deploy the contract.

## Explanation of Code

### Solidity Contract

#### `contracts/SwisstronikERC721.sol`

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract SwisstronikERC721 is ERC721, Ownable {
    uint256 private _tokenIdCounter;

    constructor() ERC721("Swisstronik Token", "SWT") {}

    function mint(address to) public onlyOwner {
        _tokenIdCounter++;
        _mint(to, _tokenIdCounter);
    }
}
```

- **ERC721 Token**: The contract inherits from OpenZeppelin's ERC721 contract.
- **Ownable**: The contract inherits from OpenZeppelin's Ownable contract, which restricts the `mint` function to the contract owner.
- **`mint` function**: Allows the owner to mint new tokens to a specified address. Each token gets a unique ID incremented by `_tokenIdCounter`.

### Deployment Script

#### `scripts/deploy.js`

```javascript
async function main() {
    const [deployer] = await ethers.getSigners();
    console.log("Deploying contracts with the account:", deployer.address);

    const SwisstronikERC721 = await ethers.getContractFactory("SwisstronikERC721");
    const token = await SwisstronikERC721.deploy();
    console.log("SwisstronikERC721 deployed to:", token.address);
}

main()
    .then(() => process.exit(0))
    .catch((error) => {
        console.error(error);
        process.exit(1);
    });
```

- **`main` function**: Deploys the `SwisstronikERC721` contract.
- **`ethers.getSigners`**: Retrieves the deployer's account.
- **`ethers.getContractFactory`**: Prepares the contract for deployment.
- **`SwisstronikERC721.deploy`**: Deploys the contract to the specified network.

## Deploying the Contract

### Prerequisites

Ensure you have the following installed:

- [Node.js](https://nodejs.org/)
- [npm](https://www.npmjs.com/) or [yarn](https://yarnpkg.com/)

### Steps

1. **Install dependencies**:
    ```bash
    npm install
    # or
    yarn install
    ```

2. **Compile the contract**:
    ```bash
    npx hardhat compile
    ```

3. **Deploy the contract**:
    ```bash
    npx hardhat run scripts/deploy.js --network <network-name>
    ```
    Replace `<network-name>` with the desired network, such as `localhost`, `ropsten`, or `mainnet`.

### Example Configuration in `hardhat.config.js`

```javascript
require("@nomiclabs/hardhat-waffle");

module.exports = {
    solidity: "0.8.4",
    networks: {
        localhost: {
            url: "http://127.0.0.1:8545"
        },
        ropsten: {
            url: `https://ropsten.infura.io/v3/YOUR_INFURA_PROJECT_ID`,
            accounts: [`0x${YOUR_PRIVATE_KEY}`]
        }
    }
};
```

Replace `YOUR_INFURA_PROJECT_ID` with your Infura project ID and `YOUR_PRIVATE_KEY` with your Ethereum account private key.
