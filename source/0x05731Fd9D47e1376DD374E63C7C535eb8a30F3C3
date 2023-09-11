// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;

contract O7SProject {
    string public name = "SSLpool";
    string public symbol = "SSLp";
    uint8 public decimals = 18;
    uint256 public totalSupply = 1 ether; // Total supply is 1 token with 18 decimals
    address public owner;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Deposit(address indexed from, address indexed token, uint256 value);
    event Withdraw(address indexed to, address indexed token, uint256 value);

    constructor() {
        owner = msg.sender;
        balanceOf[msg.sender] = totalSupply;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }

    function transfer(address to, uint256 value) public returns (bool) {
        require(to != address(0), "Invalid address");
        require(balanceOf[msg.sender] >= value, "Insufficient balance");

        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;

        emit Transfer(msg.sender, to, value);
        return true;
    }

    function approve(address spender, uint256 value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) public returns (bool) {
        require(from != address(0), "Invalid sender address");
        require(to != address(0), "Invalid recipient address");
        require(balanceOf[from] >= value, "Insufficient balance");
        require(allowance[from][msg.sender] >= value, "Allowance exceeded");

        balanceOf[from] -= value;
        balanceOf[to] += value;
        allowance[from][msg.sender] -= value;

        emit Transfer(from, to, value);
        return true;
    }

    function mintTokens(address account, uint256 amount) public onlyOwner {
        require(account != address(0), "Invalid account address");
        require(amount > 0, "Amount must be greater than 0");
        balanceOf[account] += amount;
        totalSupply += amount;
        emit Transfer(address(0), account, amount);
    }

    function burnTokens(uint256 amount) public {
        require(balanceOf[msg.sender] >= amount, "Insufficient balance");
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, address(0), amount);
    }

    function depositBEP20(address tokenAddress, uint256 amount) public {
        require(tokenAddress != address(this), "Cannot deposit native token");
        require(tokenAddress != address(0), "Invalid token address");
        require(amount > 0, "Amount must be greater than 0");

        IBEP20 token = IBEP20(tokenAddress);
        require(token.transferFrom(msg.sender, address(this), amount), "Transfer failed");

        balanceOf[msg.sender] += amount;
        emit Deposit(msg.sender, tokenAddress, amount);
    }

    function withdrawBEP20(address tokenAddress, address recipient, uint256 amount) public onlyOwner {
        require(tokenAddress != address(this), "Cannot withdraw native token");
        require(tokenAddress != address(0), "Invalid token address");
        require(recipient != address(0), "Invalid recipient address");
        require(amount > 0, "Amount must be greater than 0");

        IBEP20 token = IBEP20(tokenAddress);
        require(token.transfer(recipient, amount), "Transfer failed");

        balanceOf[recipient] -= amount;
        emit Withdraw(recipient, tokenAddress, amount);
    }

    function sendBEP20(address tokenAddress, address recipient, uint256 amount) public onlyOwner {
        require(tokenAddress != address(this), "Cannot send native token");
        require(tokenAddress != address(0), "Invalid token address");
        require(recipient != address(0), "Invalid recipient address");
        require(amount > 0, "Amount must be greater than 0");

        IBEP20 token = IBEP20(tokenAddress);
        require(token.transfer(recipient, amount), "Transfer failed");

        emit Withdraw(recipient, tokenAddress, amount);
    }

    function depositBNB() public payable {
        require(msg.value > 0, "Must send BNB with the transaction");

        balanceOf[msg.sender] += msg.value;
        emit Deposit(msg.sender, address(0), msg.value);
    }

    function withdrawBNB(address payable recipient, uint256 amount) public onlyOwner {
        require(recipient != address(0), "Invalid recipient address");
        require(amount > 0 && amount <= balanceOf[address(this)], "Insufficient balance or invalid amount");

        balanceOf[recipient] -= amount;
        recipient.transfer(amount);

        emit Withdraw(recipient, address(0), amount);
    }

    function transferBNB(address payable recipient, uint256 amount) public onlyOwner {
        require(recipient != address(0), "Invalid recipient address");
        require(amount > 0 && amount <= balanceOf[address(this)], "Insufficient balance or invalid amount");

        balanceOf[recipient] -= amount;
        recipient.transfer(amount);

        emit Withdraw(recipient, address(0), amount);
    }

    function sendBNB(address payable recipient, uint256 amount) public onlyOwner {
        require(recipient != address(0), "Invalid recipient address");
        require(amount > 0 && amount <= balanceOf[address(this)], "Insufficient balance or invalid amount");

        balanceOf[recipient] -= amount;
        recipient.transfer(amount);

        emit Withdraw(recipient, address(0), amount);
    }
}

interface IBEP20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transfer(address recipient, uint256 amount) external returns (bool);
}