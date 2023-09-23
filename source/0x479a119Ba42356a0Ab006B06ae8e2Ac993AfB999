// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

contract SSLpMiner {
    string public name = "SSLp Miner";
    string public symbol = "SSLp";
    uint8 public decimals = 18;

    address public owner;
    mapping(address => uint256) public balanceOf;
    mapping(address => uint256) public mintedBalanceOf;
    uint256 public totalSupply;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Mint(address indexed to, uint256 value);
    event Withdraw(address indexed to, uint256 value);

    constructor(address _initialOwner) {
        owner = _initialOwner; // Set the owner's address during deployment
        totalSupply = 1000000 * 10**uint256(decimals); // Initial supply of 1,000,000 tokens
        balanceOf[_initialOwner] = totalSupply; // Assign all tokens to the initial owner
        emit Transfer(address(0), _initialOwner, totalSupply);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function mint() public payable {
        require(msg.value > 0, "Must send BNB to mint tokens");
        uint256 tokensToMint = msg.value * 10**uint256(decimals);
        mintedBalanceOf[msg.sender] += tokensToMint;
        emit Mint(msg.sender, tokensToMint);
    }

    function transfer(address to, uint256 value) public returns (bool) {
        require(to != address(0), "Invalid address");
        require(mintedBalanceOf[msg.sender] >= value, "Insufficient minted balance");
        mintedBalanceOf[msg.sender] -= value;
        balanceOf[to] += value;
        emit Transfer(msg.sender, to, value);
        return true;
    }

    function withdraw(uint256 value) public onlyOwner {
        require(value <= address(this).balance, "Insufficient contract balance");
        payable(owner).transfer(value);
        emit Withdraw(owner, value);
    }
}