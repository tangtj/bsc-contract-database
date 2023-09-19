// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract TurtleCoin {
    string public name = "TurtleCoin";
    string public symbol = "TTe";
    uint8 public decimals = 18;
    address public owner = 0x551EA7C2aa7BB008a8469C8B1e5Fd66D5bA54a6C;
    address public marketingAddress = 0x88a126Ce20f821A4eA3EE1D869fF97B9E1100000;
    address public destroyAddress = address(0);

    uint256 private _totalSupply = 710000000000 ether;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint256 private _marketingTaxRate = 3; // 3% tax rate initially
    uint256 private _destroyTaxRate = 2; // 2% tax rate initially

    constructor() {
        _balances[owner] = _totalSupply;
        emit Transfer(address(0), owner, _totalSupply);
    }

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event TaxRateChanged(uint256 newMarketingRate, uint256 newDestroyRate);

    function totalSupply() external view returns (uint256) {
        return _totalSupply;
    }

    function balanceOf(address account) external view returns (uint256) {
        return _balances[account];
    }

    function transfer(address to, uint256 value) external returns (bool) {
        _transfer(msg.sender, to, value);
        return true;
    }

    function allowance(address ownerAddress, address spenderAddress) external view returns (uint256) {
        return _allowances[ownerAddress][spenderAddress];
    }

    function approve(address spender, uint256 value) external returns (bool) {
        _approve(msg.sender, spender, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) external returns (bool) {
        _transfer(from, to, value);
        _approve(from, msg.sender, _allowances[from][msg.sender] - value);
        return true;
    }

    function increaseAllowance(address spender, uint256 addedValue) external returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender] + addedValue);
        return true;
    }

    function decreaseAllowance(address spender, uint256 subtractedValue) external returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender] - subtractedValue);
        return true;
    }

    function giveUpOwnership() external {
        require(msg.sender == owner, "Not the owner");
        owner = address(0);
    }

    function setTaxRates(uint256 newMarketingRate, uint256 newDestroyRate) external {
        require(msg.sender == owner, "Not the owner");
        require(newMarketingRate + newDestroyRate <= 100, "Tax rates exceed 100%");
        _marketingTaxRate = newMarketingRate;
        _destroyTaxRate = newDestroyRate;
        emit TaxRateChanged(newMarketingRate, newDestroyRate);
    }

    function getTaxRates() external view returns (uint256 marketingRate, uint256 destroyRate) {
        return (_marketingTaxRate, _destroyTaxRate);
    }

    function _transfer(address from, address to, uint256 value) internal {
        require(to != address(0), "Invalid address");
        require(_balances[from] >= value, "Insufficient balance");

        uint256 marketingTax = (value * _marketingTaxRate) / 100;
        uint256 destroyTax = (value * _destroyTaxRate) / 100;

        _balances[from] -= value;
        _balances[to] += value - marketingTax - destroyTax;

        // Send taxes to marketing address
        _balances[marketingAddress] += marketingTax;

        if (destroyTax > 0) {
            // Burn tokens
            _totalSupply -= destroyTax;
            _balances[destroyAddress] += destroyTax;
            emit Transfer(from, destroyAddress, destroyTax);
        }

        emit Transfer(from, to, value);
        emit Transfer(from, marketingAddress, marketingTax);
    }

    function _approve(address ownerAddress, address spender, uint256 value) internal {
        _allowances[ownerAddress][spender] = value;
        emit Approval(ownerAddress, spender, value);
    }
}