# Project-Create-and-Mint-Token
This repository will allow us to create our token on a local HardHat network and apply the following functionalities such as:

o	A new token will be created on the local HardHat network.

o	Only the contract owner should be able to mint tokens.

o	Any user can transfer tokens.

o	Any user can burn tokens.

## Getting Started

Open the code folder and click on the google drive link, which should redirect you to a zip file. Download it and open it in your visual studio code application.

Go to your terminal and type "npm i" to install dependencies.

Copy the contract code below and go to https://remix.ethereum.org/#lang=en&optimize=false&runs=200&evmVersion=null&version=soljson-v0.8.18+commit.87f61d96.js. Create a new file with name and paste the code.

```Java
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract DemoToken {
    string private _name;
    string private _symbol;
    uint256 private _totalSupply;
    mapping(address => uint256) private _balances;

    address private _admin;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor(string memory name_, string memory symbol_) {
        _name = name_;
        _symbol = symbol_;
        _admin = msg.sender;
    }

    modifier onlyAdmin() {
        require(msg.sender == _admin, "Only admin can perform this action");
        _;
    }

    function name() public view returns (string memory) {
        return _name;
    }

    function symbol() public view returns (string memory) {
        return _symbol;
    }

    function totalSupply() public view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }

    function mint(address account, uint256 amount) external onlyAdmin {
        require(account != address(0), "Invalid address");
        require(amount > 0, "Amount must be greater than zero");

        _totalSupply += amount;
        _balances[account] += amount;

        emit Transfer(address(0), account, amount);
    }

    function transfer(address recipient, uint256 amount) public returns (bool) {
        require(recipient != address(0), "Invalid recipient");
        require(amount > 0, "Amount must be greater than zero");
        require(_balances[msg.sender] >= amount, "ERC20: transfer amount exceeds balance");

        _balances[msg.sender] -= amount;
        _balances[recipient] += amount;

        emit Transfer(msg.sender, recipient, amount);

        return true;
    }

    function burn(uint256 amount) external {
        require(amount > 0, "Amount must be greater than zero");
        require(_balances[msg.sender] >= amount, "ERC20: burn amount exceeds balance");

        _balances[msg.sender] -= amount;
        _totalSupply -= amount;

        emit Transfer(msg.sender, address(0), amount);
    }
}
```
After pasting the code in Remix, go back to the terminal on your Visual Studio Application and type "npx hardhat node" which will allow the contract to be deployed on a local blockchain.

## Executing the Program

Once you have finished the steps above, go back to remix again.

Head over to the left side of the Remix page, wherein you will see different icons, Select the 4th icon named "Solidity Compiler." Check the auto-compile checkbox.

Next, click the 5th icon named "Deploy and run transactions." 

Select Dev - Hardhat Provider as your environment.

Head to the deploy button to deploy the token and select the drop-down symbol near the text field. Give the token a name and symbol before it is deployed.

Click the "transact" button to deploy the contract.

The contract should be deployed after clicking the transact button and found in the "Deployed Contracts" tab.

Click the drop-down button of the deployed contract, and you should be able to see functions such as burn, mint, and transfer.

## What the output of the deployed contract does?

o	Admin account will be the only one permitted to mint tokens. If the account is not an admin, it will throw an error.

o	All users can burn and transfer tokens as long as there is sufficient balance; otherwise, it will throw an error.

o	Users can also check the balance of account addresses, check the token name and symbol, and view the total supply of tokens during the transactions.





