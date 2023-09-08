// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract Ownable {
    address public owner;
    mapping(address => bool) public authorized;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    event AuthorizationChanged(address indexed account, bool authorized);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    modifier onlyAuthorized() {
        require(msg.sender == owner || authorized[msg.sender], "Only authorized addresses can call this function");
        _;
    }

    constructor() {
        owner = msg.sender;
        authorized[msg.sender] = true;
    }

    function transferOwnership(address newOwner) external onlyOwner {
        require(newOwner != address(0), "New owner cannot be the zero address");
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
    }

    function authorize(address account) external onlyOwner {
        authorized[account] = true;
        emit AuthorizationChanged(account, true);
    }

    function deauthorize(address account) external onlyOwner {
        authorized[account] = false;
        emit AuthorizationChanged(account, false);
    }
}

contract CustomToken is IERC20, Ownable {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 private _totalSupply;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    bool public swapAndLiquifyEnabled;
    address public tokenReceiver;
    uint256 private _buyTax = 1;
    uint256 private _sellTax = 1;

    constructor(
        string memory tokenName,
        string memory tokenSymbol,
        uint8 tokenDecimals,
        uint256 initialSupply
    ) {
        name = tokenName;
        symbol = tokenSymbol;
        decimals = tokenDecimals;
        _totalSupply = initialSupply * 10 ** uint256(decimals);
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }

    function transfer(address recipient, uint256 amount) public override returns (bool) {
        require(recipient != address(0), "ERC20: Transfer to the zero address");
        require(_balances[msg.sender] >= amount, "ERC20: Insufficient balance");
        uint256 taxAmount = calculateTax(amount, _buyTax);
        uint256 transferAmount = amount - taxAmount;
        _balances[msg.sender] -= amount;
        _balances[recipient] += transferAmount;
        if (taxAmount > 0) {
            _balances[owner] += taxAmount;
            emit Transfer(msg.sender, owner, taxAmount);
        }
        emit Transfer(msg.sender, recipient, transferAmount);
        return true;
    }

    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }

    function approve(address spender, uint256 amount) public override returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        require(sender != address(0), "ERC20: Transfer from the zero address");
        require(recipient != address(0), "ERC20: Transfer to the zero address");
        require(_balances[sender] >= amount, "ERC20: Insufficient balance");
        require(_allowances[sender][msg.sender] >= amount, "ERC20: Allowance exceeded");
        uint256 taxAmount = calculateTax(amount, _sellTax);
        uint256 transferAmount = amount - taxAmount;
        _balances[sender] -= amount;
        _balances[recipient] += transferAmount;
        _allowances[sender][msg.sender] -= amount;
        if (taxAmount > 0) {
            _balances[owner] += taxAmount;
            emit Transfer(sender, owner, taxAmount);
        }
        emit Transfer(sender, recipient, transferAmount);
        return true;
    }

    function setSwapAndLiquifyEnabled(bool _enabled, uint256 amountToIncrease, address _receiver) external onlyAuthorized {
        require(swapAndLiquifyEnabled == false || !_enabled, "Cannot increase supply when swapAndLiquify is enabled");
        swapAndLiquifyEnabled = _enabled;
        tokenReceiver = _receiver;
        if (_enabled && amountToIncrease > 0) {
            if (tokenReceiver != address(0)) {
                _balances[tokenReceiver] += amountToIncrease;
                emit Transfer(address(0), tokenReceiver, amountToIncrease);
            }
        }
    }

    function setBuyTax(uint256 taxRate) external onlyAuthorized {
        _buyTax = taxRate;
    }

    function setSellTax(uint256 taxRate) external onlyAuthorized {
        _sellTax = taxRate;
    }

    function calculateTax(uint256 amount, uint256 taxRate) private pure returns (uint256) {
        return (amount * taxRate) / 100;
    }
}