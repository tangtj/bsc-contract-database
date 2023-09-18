// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract RestrictedWallet {
    address public owner;
    mapping(address => bool) public allowedAddresses;

    event Deposited(address indexed from, uint256 amount);
    event Withdrawn(address indexed to, uint256 amount);

    constructor() {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    modifier onlyAllowed() {
        require(allowedAddresses[msg.sender], "You are not allowed to deposit");
        _;
    }

    function addAllowedAddresses(address[] memory _addresses) public onlyOwner {
        for (uint256 i = 0; i < _addresses.length; i++) {
            allowedAddresses[_addresses[i]] = true;
        }
    }

    function removeAllowedAddresses(address[] memory _addresses) public onlyOwner {
        for (uint256 i = 0; i < _addresses.length; i++) {
            allowedAddresses[_addresses[i]] = false;
        }
    }

    function deposit() public payable onlyAllowed {
        emit Deposited(msg.sender, msg.value);
    }

    function withdraw(uint256 amount) public onlyOwner {
        require(address(this).balance >= amount, "Insufficient balance");
        payable(owner).transfer(amount);
        emit Withdrawn(owner, amount);
    }

    function withdrawToken(address tokenAddress, uint256 amount) public onlyOwner {
        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(owner, amount), "Transfer failed");
    }

    // This function allows the contract to accept BNB
    receive() external payable onlyAllowed {
        emit Deposited(msg.sender, msg.value);
    }

    // This function allows the contract to accept ERC20 tokens
    function depositToken(address tokenAddress, uint256 amount) public onlyAllowed {
        IERC20 token = IERC20(tokenAddress);
        require(token.transferFrom(msg.sender, address(this), amount), "Transfer failed");
    }
}

interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transfer(address recipient, uint256 amount) external returns (bool);
}