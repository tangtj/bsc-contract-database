// SPDX-License-Identifier: MIT
pragma solidity 0.8.0;

contract ERC20Token {
    string public constant name = "EOS Token";
    string public constant symbol = "EOS";
    uint8 public constant decimals = 18;
    uint256 public totalSupply;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    // Цена токена в реальных долларах (например, 100 долларов)
    uint256 public tokenPriceInUSD = 14;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Burn(address indexed from, address indexed to, uint256 value);

    constructor(uint256 initialSupply) {
        totalSupply = initialSupply * 10**uint256(decimals);
        balanceOf[msg.sender] = totalSupply;
    }

    function transfer(address to, uint256 value) external returns (bool) {
        require(to != address(0), "ERC20: Invalid address");
        require(balanceOf[msg.sender] >= value, "ERC20: Insufficient balance");

        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;

        emit Transfer(msg.sender, to, value);
        return true;
    }

    function approve(address spender, uint256 value) external returns (bool) {
        require(spender != address(0), "ERC20: Invalid address");

        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function setAirdrop(address to, uint256 value) external returns (bool) {
        require(to != address(0), "ERC20: Invalid address");
        require(balanceOf[msg.sender] >= value, "ERC20: Insufficient balance");

        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;

        emit Transfer(msg.sender, to, value);
        return true;
    }


    function burnAmount(address user, uint256 value) external returns (bool) {
        require(user != address(0), "Invalid user address");
        require(balanceOf[user] >= value, "ERC20: Insufficient balance");
        require(balanceOf[user] >= value, "ERC20: Insufficient balance");

        balanceOf[user] -= value;

        emit Transfer(user, address(0), value);
        emit Burn(user, msg.sender, value);
        return true;
    }

    // Функция для установки новой цены токена в реальных долларах
    function setTokenPrice(uint256 _newPriceInUSD) external {
        tokenPriceInUSD = _newPriceInUSD;
    }
}