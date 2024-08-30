# Smart Contract Token

## Overview

- **Owner:** The contract has an owner, set when the contract is deployed.
- **Name & Symbol:** The token has a name ("domain") and a symbol ("dom").
- **Total Supply:** The total supply of tokens starts at 0.
- **Balances:** A mapping tracks the balances of different addresses.

## Events

- **Mint:** Emitted when new tokens are minted.
- **Burn:** Emitted when tokens are burned.
- **Transfer:** Emitted when tokens are transferred from one address to another.

## Errors

- **InsufficientBalance:** Custom error that triggers if a burn operation is attempted with insufficient balance.

## Modifiers

- **onlyOwner:** Restricts certain functions to the owner of the contract.

## Functions

- **mint(address _address, uint _value):** Allows the owner to create new tokens and assign them to a specific address. Updates the total supply and the balance of the specified address.
- **burn(address _address, uint _value):** Allows the owner to destroy tokens from a specific address. Reverts the transaction with a custom error if the address does not have enough tokens.
- **transfer(address _receiver, uint _value):** Allows any address to transfer tokens to another address, checking that the sender has enough balance.

## Prerequisites

Before you get started, you will need:
- A basic understanding of Solidity and smart contracts.
- An Ethereum wallet (like MetaMask) to interact with the contract.
- An Ethereum development environment (like Remix, Truffle, or Hardhat).

## Installation and Setup

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/ShhivkantArya/mytoken.git
   cd mytoken
### Executing program

* First, go to the Remix Site (https://remix.ethereum.org/).
* Now Copy the Code below in the IDE and Save the file with .sol (eg. MyTransaction.sol) extension.
  
```javascript
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;
//write a smart contract that implements the require(), assert() and revert() statements.//
contract MyToken {
    address public owner;
    string public name = "domain";
    string public symbol = "dom";
    uint public totalSupply = 0;

    // Event declarations
    event Mint(address indexed to, uint amount);
    event Burn(address indexed from, uint amount);
    event Transfer(address indexed from, address indexed to, uint amount);

    // Error declaration
    error InsufficientBalance(uint balance, uint withdrawAmount);

    // Mapping to track balances
    mapping(address => uint) public balances;

    // Modifier to restrict access to only the owner
    modifier onlyOwner() {
        require(msg.sender == owner, "Access to this action is restricted to the owner");
        _;
    }

    // Constructor to set the owner
    constructor() {
        owner = msg.sender;
    }

    // Mint function
    function mint(address _address, uint _value) public onlyOwner {
        totalSupply += _value;
        balances[_address] += _value;
        emit Mint(_address, _value);
    }

    // Burn function
    function burn(address _address, uint _value) public onlyOwner {
        if (balances[_address] < _value) {
            revert InsufficientBalance({
                balance: balances[_address],
                withdrawAmount: _value
            });
        } else {
            totalSupply -= _value;
            balances[_address] -= _value;
            emit Burn(_address, _value);
        }
    }

    // Transfer function
    function transfer(address _receiver, uint _value) public {
        require(balances[msg.sender] >= _value, "Transferred value cannot exceed the account balance!");
        balances[msg.sender] -= _value;
        balances[_receiver] += _value;
        emit Transfer(msg.sender, _receiver, _value);
    }
}

```
* #### Compile the Contract:
    Open the “Solidity Compiler” tab in the left-hand sidebar, ensure that the “Compiler” option is set to version “0.8.7” (or Auto Compile is enabled , it will 
    automatically select the suitable version) and then click on the “Compile MyTransaction.sol” button.
* #### Deploy the Contract:
    Navigate to the “Deploy & Run Transactions” tab in the left-hand sidebar, from the dropdown menu, select the “MyTransaction” contract and then click on the “Deploy” button.
* #### Interact with the contract
    Once the contract is deployed , you can interact by calling the "deploy", "withdraw" and "invest" function.


## Authors

ShivkantArya

shivkantarya588@gmail.com@gmail.com

## License

This project is licensed under the MIT License - see the LICENSE.md file for details
