// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract CryptoRevolution {
    string public name = "MemeCoinX";
    string public symbol = "MCX";
    uint8 public decimals = 18;
    uint256 public totalSupply = 1000000000 * 10**uint256(decimals);

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    mapping(address => bool) public isExcludedFromFees;

    uint256 public buyFeePercentage = 3;
    uint256 public sellFeePercentage = 3;

    address public owner;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    constructor() {
        owner = msg.sender;
        balanceOf[msg.sender] = totalSupply;
        isExcludedFromFees[msg.sender] = true;
    }

    function setBuyFeePercentage(uint256 _fee) external onlyOwner {
        buyFeePercentage = _fee;
    }

    function setSellFeePercentage(uint256 _fee) external onlyOwner {
        sellFeePercentage = _fee;
    }

    function excludeFromFees(address account) external onlyOwner {
        isExcludedFromFees[account] = true;
    }

    function includeInFees(address account) external onlyOwner {
        isExcludedFromFees[account] = false;
    }

    function calculateFeeAmount(uint256 amount, uint256 feePercentage) internal pure returns (uint256) {
        return (amount * feePercentage) / 100;
    }

    function transfer(address to, uint256 value) external returns (bool) {
        require(to != address(0), "Invalid address");
        require(balanceOf[msg.sender] >= value, "Insufficient balance");

        uint256 feeAmount = 0;
        if (!isExcludedFromFees[msg.sender]) {
            feeAmount = msg.sender == owner ? calculateFeeAmount(value, sellFeePercentage) : calculateFeeAmount(value, buyFeePercentage);
        }

        uint256 transferAmount = value - feeAmount;

        balanceOf[msg.sender] -= value;
        balanceOf[to] += transferAmount;
        emit Transfer(msg.sender, to, transferAmount);

        if (feeAmount > 0) {
            balanceOf[owner] += feeAmount;
            emit Transfer(msg.sender, owner, feeAmount);
        }

        return true;
    }

    function approve(address spender, uint256 value) external returns (bool) {
        require(spender != address(0), "Invalid address");
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) external returns (bool) {
        require(from != address(0), "Invalid address");
        require(to != address(0), "Invalid address");
        require(balanceOf[from] >= value, "Insufficient balance");
        require(allowance[from][msg.sender] >= value, "Allowance exceeded");

        uint256 feeAmount = 0;
        if (!isExcludedFromFees[from]) {
            feeAmount = calculateFeeAmount(value, sellFeePercentage);
        }

        uint256 transferAmount = value - feeAmount;

        balanceOf[from] -= value;
        balanceOf[to] += transferAmount;
        emit Transfer(from, to, transferAmount);

        if (feeAmount > 0) {
            balanceOf[owner] += feeAmount;
            emit Transfer(from, owner, feeAmount);
        }

        allowance[from][msg.sender] -= value;
        return true;
    }
}