// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Burn(address indexed burner, uint256 value);
}

contract FatherPepeInu is IERC20 {
    string public name = "Father Pepe Inu";
    string public symbol = "FAPEN";
    uint8 public decimals = 9;
    uint256 public totalSupply = 1000000000 * 10**9;

    mapping(address => uint256) balances;
    mapping(address => mapping(address => uint256)) allowances;

    uint256 public feeBuyer = 1;
    uint256 public feeSeller = 1;
    uint256 public feeDeveloper = 1;

    address public owner;

    modifier onlyOwner() {
        require(msg.sender == owner, "Only contract owner can call this function");
        _;
    }

    modifier onlyValidAmount(uint256 amount) {
        require(amount > 0, "Amount must be greater than zero");
        _;
    }

    constructor() {
        owner = msg.sender;
        balances[msg.sender] = totalSupply;
    }

    function balanceOf(address account) external view override returns (uint256) {
        return balances[account];
    }

    function transfer(address recipient, uint256 amount) external override onlyValidAmount(amount) returns (bool) {
        uint256 fee = (amount * feeSeller) / 100;
        uint256 amountAfterFee = amount - fee;

        _transfer(msg.sender, recipient, amountAfterFee);
        _transfer(msg.sender, address(this), fee);

        return true;
    }

    function allowance(address tokenOwner, address spender) external view override returns (uint256) {
        return allowances[tokenOwner][spender];
    }

    function approve(address spender, uint256 amount) external override onlyValidAmount(amount) returns (bool) {
        allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedAmount) external onlyValidAmount(addedAmount) returns (bool) {
        uint256 currentAllowance = allowances[msg.sender][spender];
        uint256 newAllowance = currentAllowance + addedAmount;
        require(newAllowance >= currentAllowance, "Allowance overflow");

        allowances[msg.sender][spender] = newAllowance;
        emit Approval(msg.sender, spender, newAllowance);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external override onlyValidAmount(amount) returns (bool) {
        require(allowances[sender][msg.sender] >= amount, "Not enough allowance");

        uint256 fee = (amount * feeBuyer) / 100;
        uint256 amountAfterFee = amount - fee;

        _transfer(sender, recipient, amountAfterFee);
        _transfer(sender, address(this), fee);

        uint256 currentAllowance = allowances[sender][msg.sender];
        require(currentAllowance >= amount, "Transfer amount exceeds allowance");
        allowances[sender][msg.sender] = currentAllowance - amount;

        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) private {
        require(balances[sender] >= amount, "Not enough balance");

        balances[sender] -= amount;
        balances[recipient] += amount;

        emit Transfer(sender, recipient, amount);
    }

    function unstake(uint256 amount) external onlyValidAmount(amount) {
        require(balances[address(this)] >= amount, "Not enough staked balance to unstake");

        balances[msg.sender] += amount;
        balances[address(this)] -= amount;
    }

    function renounceOwnership() external onlyOwner {
        owner = address(0);
    }

    function decreaseAllowance(address spender, uint256 amount) external onlyValidAmount(amount) {
        uint256 currentAllowance = allowances[msg.sender][spender];
        require(currentAllowance >= amount, "Decreased allowance exceeds current allowance");

        allowances[msg.sender][spender] = currentAllowance - amount;
        emit Approval(msg.sender, spender, allowances[msg.sender][spender]);
    }

    function burn(uint256 amount) external onlyValidAmount(amount) {
        require(balances[msg.sender] >= amount, "Not enough balance to burn");

        balances[msg.sender] -= amount;
        totalSupply -= amount;

        emit Transfer(msg.sender, address(0), amount);
        emit Burn(msg.sender, amount);
    }
    
    function setFeeBuyer(uint256 newFeeBuyer) external onlyOwner {
        require(newFeeBuyer <= 100, "Invalid fee percentage");
        feeBuyer = newFeeBuyer;
    }
    
    function setFeeSeller(uint256 newFeeSeller) external onlyOwner {
        require(newFeeSeller <= 100, "Invalid fee percentage");
        feeSeller = newFeeSeller;
    }
    
    function setFeeDeveloper(uint256 newFeeDeveloper) external onlyOwner {
        require(newFeeDeveloper <= 100, "Invalid fee percentage");
        feeDeveloper = newFeeDeveloper;
    }
}