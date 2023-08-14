// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

contract pepeBITCOIN {
    string public name = "pepeBITCOIN";
    string public symbol = "pepeBTC";
    uint8 public decimals = 18; // Assuming 18 decimals for consistency
    uint256 public totalSupplyCap = 21000000 * 10**uint256(decimals); // Total supply capped at 21 million tokens
    uint256 public initialSupply = 5000000 * 10**uint256(decimals); // Initial supply of 5 million tokens
    uint256 public halvingInterval = 2100; // Halving every 2100 blocks
    uint256 public nextHalvingBlock;
    address public owner;
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    constructor() {
        owner = msg.sender;
        balanceOf[owner] = initialSupply;
        nextHalvingBlock = block.number + halvingInterval;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can perform this action");
        _;
    }

    function _mint(address account, uint256 amount) internal {
        require(totalSupply() + amount <= totalSupplyCap, "Total supply cap reached");
        balanceOf[account] += amount;
        emit Transfer(address(0), account, amount);
    }

    function totalSupply() public view returns (uint256) {
        return balanceOf[address(this)];
    }

    function transfer(address recipient, uint256 amount) public returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "Transfer from the zero address");
        require(recipient != address(0), "Transfer to the zero address");
        require(amount <= balanceOf[sender], "Insufficient balance");
        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }

    function approve(address spender, uint256 amount) public returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public returns (bool) {
        require(amount <= allowance[sender][msg.sender], "Allowance exceeded");
        allowance[sender][msg.sender] -= amount;
        _transfer(sender, recipient, amount);
        return true;
    }

    function mine() external onlyOwner {
        require(block.number >= nextHalvingBlock, "Halving not reached yet");
        _mint(owner, initialSupply / 2); // Halving: add half of the initial supply
        nextHalvingBlock += halvingInterval;
    }
}