// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Two {
    string public name = "2066";
    string public symbol = "2066";
    uint8 public decimals = 18;
    uint256 public totalSupply = 1000000 * 10 ** uint256(decimals);

    address public burnAddress = 0x0000000000000000000000000000000000000000;
    uint256 public taxRateSell = 2; // 2% de impuesto para ventas
    uint256 public taxRateBuy = 2;  // 2% de impuesto para compras
    bool public sellingEnabled = true;

    address public owner;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Tax(address indexed from, address indexed to, uint256 value);

    constructor() {
        owner = msg.sender;
        balanceOf[msg.sender] = totalSupply;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function setBurnAddress(address _newBurnAddress) external onlyOwner {
        burnAddress = _newBurnAddress;
    }

    function setTaxRates(uint256 _sellRate, uint256 _buyRate) external onlyOwner {
        require(_sellRate <= 100 && _buyRate <= 100, "Tax rate must be less than or equal to 100%");
        taxRateSell = _sellRate;
        taxRateBuy = _buyRate;
    }

    function toggleSelling(bool _enabled) external onlyOwner {
        sellingEnabled = _enabled;
    }

    function transfer(address recipient, uint256 amount) external returns (bool) {
        require(balanceOf[msg.sender] >= amount, "Not enough tokens to transfer");
        uint256 taxAmount = (amount * taxRateSell) / 100;
        balanceOf[msg.sender] -= amount;
        balanceOf[burnAddress] += taxAmount;
        balanceOf[recipient] += amount - taxAmount;
        emit Transfer(msg.sender, recipient, amount);
        emit Tax(msg.sender, burnAddress, taxAmount);
        return true;
    }

    function approve(address spender, uint256 amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool) {
        require(balanceOf[sender] >= amount, "Not enough tokens to transfer");
        require(allowance[sender][msg.sender] >= amount, "Allowance exceeded");
        uint256 taxAmount = (amount * taxRateSell) / 100;
        balanceOf[sender] -= amount;
        balanceOf[burnAddress] += taxAmount;
        balanceOf[recipient] += amount - taxAmount;
        allowance[sender][msg.sender] -= amount;
        emit Transfer(sender, recipient, amount);
        emit Tax(sender, burnAddress, taxAmount);
        return true;
    }
    
    function burn(uint256 amount) external {
        require(balanceOf[msg.sender] >= amount, "Not enough tokens to burn");
        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;
        emit Transfer(msg.sender, burnAddress, amount);
    }
}