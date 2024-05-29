# FAE_ETH_AVAX_CODES
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Reyes_DigitalWallet {
    address public walletOwner;
    mapping(address => uint256) public userBalances;

    event FundsDeposited(address indexed user, uint256 amount);
    event PaymentProcessed(address indexed user, uint256 amount);

    constructor() {
        walletOwner = msg.sender;
    }

    function depositFunds() external payable {
        require(msg.value > 0, "Deposit amount must be greater than zero");

        userBalances[msg.sender] += msg.value;
        emit FundsDeposited(msg.sender, msg.value);
    }

    function getBalance() external view returns (uint256) {
        return userBalances[msg.sender];
    }

    function processPayment(uint256 paymentAmount) external {
        require(paymentAmount > 0, "Payment amount must be greater than zero");
        require(userBalances[msg.sender] >= paymentAmount, "Insufficient balance for payment");

        userBalances[msg.sender] -= paymentAmount;
        emit PaymentProcessed(msg.sender, paymentAmount);
    }

    receive() external payable {
        revert("Digital wallet contract does not accept direct payments");
    }
}
