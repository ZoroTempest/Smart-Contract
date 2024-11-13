# BankTransfer Contract

This Solidity program is a simple contract for managing deposits and withdrawals of Ether. It demonstrates error handling in Ethereum smart contracts, using `require()`, `revert()`, and `assert()` functions to enforce logic and secure transactions.

## Description

The **BankTransfer** contract allows users to deposit and withdraw Ether. It includes basic functionality to track user balances and handle transactions securely. The contract also demonstrates the usage of different error handling mechanisms such as `require()`, `revert()`, and `assert()` to ensure the correct behavior of the contract.

### Features:
- **Deposit Ether**: Users can deposit Ether into the contract.
- **Withdraw Ether**: Users can withdraw Ether, with balance checks and error handling.
- **Receive Function**: The contract can accept plain Ether transfers.
- **Fallback Function**: Handles calls with unmatched function signatures.
- **Owner-Only Withdrawal**: Only the owner can withdraw all funds from the contract.
- **Error Handling**: Utilizes `require()`, `revert()`, and `assert()` for different purposes.

## Getting Started

### Executing the Program

To run this program, you can use Remix, an online Solidity IDE. Follow these steps:

1. Go to the Remix website: [Remix IDE](https://remix.ethereum.org/).
2. Create a new file by clicking the "+" icon in the left-hand sidebar. Save it with a `.sol` extension (e.g., `BankTransfer.sol`).
3. Copy and paste the following code into the file:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.13;

contract BankTransfer {
    // Mapping to store the balances of users
    mapping(address => uint) private balances;
    address public owner;

    // Custom error for insufficient balance
    error InsufficientBalance(uint available, uint required);

    // Event for deposit and withdrawal logs
    event Deposit(address indexed sender, uint amount);
    event Withdrawal(address indexed recipient, uint amount);

    // Constructor to set the owner
    constructor() {
        owner = msg.sender;
    }

    // Function to deposit Ether into the contract
    function deposit() public payable {
        require(msg.value > 0, "Deposit amount must be greater than zero.");
        balances[msg.sender] += msg.value;
        emit Deposit(msg.sender, msg.value);
    }

    // Receive function to accept plain Ether transfers
    receive() external payable {
        balances[msg.sender] += msg.value;
        emit Deposit(msg.sender, msg.value);
    }

    // Fallback function to handle calls with data that don't match any function signature
    fallback() external payable {
        
    }

    // Function to withdraw Ether from the contract
    function withdraw(uint _amount) public {
        uint balance = balances[msg.sender];

        require(_amount <= balance, "Insufficient balance for withdrawal.");

        if (_amount > balance) {
            revert InsufficientBalance({available: balance, required: _amount});
        }

        // Deduct the balance and transfer the amount
        balances[msg.sender] -= _amount;
        payable(msg.sender).transfer(_amount);

        emit Withdrawal(msg.sender, _amount);
    }

    // Function to get the balance of the caller
    function getBalance() public view returns (uint) {
        return balances[msg.sender];
    }

    // Function to test the contract’s invariant that only the owner can withdraw all funds
    function withdrawAll() public {
        // Use `assert` to check an invariant: only the owner can call this function
        assert(msg.sender == owner);
        payable(owner).transfer(address(this).balance);
    }
}
```

# Authors
Justin Bulot

Email - 202110965@fit.edu.ph

## License

This project is licensed under the MIT License - see the LICENSE.md file for details
