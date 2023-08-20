// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract OZONToken {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;

    mapping(address => uint256) private balances;
    mapping(address => mapping(address => uint256)) private allowances;
    mapping(address => bool) private isWhitelisted;
    address private uniswapV2Pair;
    address private owner;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event SetUniswapV2Pair(address indexed pair);
    event UpdateWhitelist(address indexed account, bool isWhitelisted);
    event UpdateOwner(address indexed newOwner);

    constructor(
        string memory _name,
        string memory _symbol,
        uint8 _decimals,
        uint256 _initialSupply
    ) {
        name = _name;
        symbol = _symbol;
        decimals = _decimals;
        totalSupply = _initialSupply * 10**uint256(_decimals);
        owner = msg.sender;
        balances[owner] = totalSupply;
        emit Transfer(address(0), owner, totalSupply);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Caller is not the owner");
        _;
    }

    function setUniswapV2Pair(address pair) external onlyOwner {
        require(pair != address(0), "Invalid UniswapV2Pair address");
        uniswapV2Pair = pair;
        emit SetUniswapV2Pair(pair);
    }

    function updateWhitelist(address account, bool isWhitelisted_) external onlyOwner {
        isWhitelisted[account] = isWhitelisted_;
        emit UpdateWhitelist(account, isWhitelisted_);
    }

    function updateOwner(address newOwner) external onlyOwner {
        require(newOwner != address(0), "Invalid new owner address");
        owner = newOwner;
        emit UpdateOwner(newOwner);
    }

    function transfer(address recipient, uint256 amount) external returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool) {
        require(allowances[sender][msg.sender] >= amount, "Insufficient allowance");
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, allowances[sender][msg.sender] - amount);
        return true;
    }

    function _transfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal {
        require(sender != address(0), "Invalid sender address");
        require(recipient != address(0), "Invalid recipient address");
        require(amount > 0, "Invalid transfer amount");

        if (uniswapV2Pair != address(0) && recipient == uniswapV2Pair) {
            if (!isWhitelisted[sender]) {
                uint256 fee = (amount * 5) / 100;
                uint256 transferAmount = amount - fee;
                require(amount == transferAmount + fee, "Invalid transfer amount");
                balances[sender] -= amount;
                balances[recipient] += transferAmount;
                balances[address(0)] += fee;
                emit Transfer(sender, recipient, transferAmount);
                emit Transfer(sender, address(0), fee);
                return;
            }
        }

        balances[sender] -= amount;
        balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }

    function approve(address spender, uint256 amount) external returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal {
        require(owner != address(0), "Invalid owner address");
        require(spender != address(0), "Invalid spender address");
        allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function balanceOf(address account) external view returns (uint256) {
        require(account != address(0), "Invalid account address");
        return balances[account];
    }

    function allowance(address owner, address spender) external view returns (uint256) {
        require(owner != address(0), "Invalid owner address");
        require(spender != address(0), "Invalid spender address");
        return allowances[owner][spender];
    }
}