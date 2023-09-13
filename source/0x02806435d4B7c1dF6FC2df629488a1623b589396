// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

contract Iotec {
    string public name = "Iotec";
    string public symbol = "IOT";
    uint8 public decimals = 18;
    uint256 public totalSupply = 30000000000 * 10 ** uint256(decimals);
    uint256 public maxTotalSupply = 60000000000 * 10 ** uint256(decimals); // Limite de 60 bilhões
    address public owner;
    address public feeWallet = address(0x06d50cE04BcA1A55a27E401f7B141cA07ecD565E);
    uint256 public buyFeePercentage = 3;
    uint256 public sellFeePercentage = 7;
    address public admin;
    bool public paused = false;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    mapping(address => bool) private _isExcludedFromFee;
    mapping(address => bool) public owners;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event AccountExcludedFromFee(address account);
    event AccountIncludedInFee(address account);
    event Paused();
    event Unpaused();
    event BuyFeePercentageChanged(uint256 newPercentage);
    event SellFeePercentageChanged(uint256 newPercentage);
    event OwnerAdded(address newOwner);
    event OwnerRemoved(address removedOwner);

    modifier whenNotPaused() {
        require(!paused, "Paused");
        _;
    }

    modifier whenPaused() {
        require(paused, "Not paused");
        _;
    }

    constructor() {
        owner = msg.sender;
        owners[msg.sender] = true;
        admin = msg.sender;
        balanceOf[msg.sender] = totalSupply;
    }

    function transfer(address to, uint256 value) public whenNotPaused returns (bool success) {
        _transfer(msg.sender, to, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) public whenNotPaused returns (bool success) {
        require(value <= allowance[from][msg.sender], "Allowance exceeded");
        allowance[from][msg.sender] -= value;
        _transfer(from, to, value);
        return true;
    }

    function approve(address spender, uint256 value) public whenNotPaused returns (bool success) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "Sender address is the zero address");
        require(recipient != address(0), "Recipient address is the zero address");
        require(balanceOf[sender] >= amount, "Insufficient balance");

        uint256 feeAmount = 0;
        if (!_isExcludedFromFee[sender] && !_isExcludedFromFee[recipient]) {
            if (recipient == feeWallet) {
                feeAmount = (amount * sellFeePercentage) / 100;
            } else {
                feeAmount = (amount * buyFeePercentage) / 100;
            }
            balanceOf[feeWallet] += feeAmount;
            emit Transfer(sender, feeWallet, feeAmount);
            amount -= feeAmount;
        }

        balanceOf[sender] -= amount;
        balanceOf[recipient] += amount;
        emit Transfer(sender, recipient, amount);
    }

    function excludeFromFee(address account) public onlyOwnerOrAdmin {
        require(account != address(0), "Excluded account is the zero address");
        _isExcludedFromFee[account] = true;
        emit AccountExcludedFromFee(account);
    }

    function includeInFee(address account) public onlyOwnerOrAdmin {
        require(account != address(0), "Included account is the zero address");
        _isExcludedFromFee[account] = false;
        emit AccountIncludedInFee(account);
    }

    function isExcludedFromFee(address account) public view returns (bool) {
        return _isExcludedFromFee[account];
    }

    function pause() public onlyOwnerOrAdmin whenNotPaused {
        paused = true;
        emit Paused();
    }

    function unpause() public onlyOwnerOrAdmin whenPaused {
        paused = false;
        emit Unpaused();
    }

    modifier onlyOwnerOrAdmin() {
        require(msg.sender == owner || msg.sender == admin, "Not the owner or admin");
        _;
    }

    function mint(address to, uint256 amount) public onlyOwnerOrAdmin whenNotPaused {
        require(to != address(0), "Mint to the zero address");
        require(totalSupply + amount <= maxTotalSupply, "Mint would exceed the maximum total supply");
        totalSupply += amount;
        balanceOf[to] += amount;
        emit Transfer(address(0), to, amount);
    }

    function setBuyFeePercentage(uint256 newPercentage) public onlyOwnerOrAdmin {
        buyFeePercentage = newPercentage;
        emit BuyFeePercentageChanged(newPercentage);
    }

    function setSellFeePercentage(uint256 newPercentage) public onlyOwnerOrAdmin {
        sellFeePercentage = newPercentage;
        emit SellFeePercentageChanged(newPercentage);
    }

    function addOwner(address newOwner) public onlyOwnerOrAdmin {
        require(newOwner != address(0), "New owner address cannot be the zero address");
        owners[newOwner] = true;
        emit OwnerAdded(newOwner);
    }
    
    function removeOwner(address ownerToRemove) public onlyOwnerOrAdmin {
        require(ownerToRemove != address(0), "Owner to remove address cannot be the zero address");
        require(ownerToRemove != msg.sender, "Cannot remove yourself as an owner");
        owners[ownerToRemove] = false;
        emit OwnerRemoved(ownerToRemove);
    }
}