// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract CryptoRevolution {
    string public name = "MemeCoinX";
    string public symbol = "MCX";
    uint8 public decimals = 18; // 18 decimals is the most common standard
    uint256 public totalSupply = 1000000000 * 10**uint256(decimals); // 1,000,000,000 tokens

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    mapping(address => uint256) public taxRate; // Tax rate in basis points (1 basis point = 0.01%)
    mapping(address => bool) public isExcludedFromFee; // Addresses excluded from tax

    uint256 public buyFeePercentage = 500; // 5%
    uint256 public sellFeePercentage = 500; // 5%

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event TaxRateChanged(address indexed account, uint256 newRate);
    event FeeExclusionChanged(address indexed account, bool isExcluded);
    event BuyFeePercentageChanged(uint256 newPercentage);
    event SellFeePercentageChanged(uint256 newPercentage);

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        taxRate[msg.sender] = 0; // Initial tax rate is set to 0 for the contract deployer
    }

    function changeTaxRate(uint256 newRate) external {
        require(newRate <= 10000, "Tax rate can't exceed 100%");
        taxRate[msg.sender] = newRate;
        emit TaxRateChanged(msg.sender, newRate);
    }

    function excludeFromFee(address account, bool exclude) external {
        require(msg.sender == address(this), "Only the contract can modify fee exclusions");
        isExcludedFromFee[account] = exclude;
        emit FeeExclusionChanged(account, exclude);
    }

    function changeBuyFeePercentage(uint256 newPercentage) external {
        require(msg.sender == address(this), "Only the contract can modify the buy fee percentage");
        require(newPercentage <= 10000, "Percentage can't exceed 100%");
        buyFeePercentage = newPercentage;
        emit BuyFeePercentageChanged(newPercentage);
    }

    function changeSellFeePercentage(uint256 newPercentage) external {
        require(msg.sender == address(this), "Only the contract can modify the sell fee percentage");
        require(newPercentage <= 10000, "Percentage can't exceed 100%");
        sellFeePercentage = newPercentage;
        emit SellFeePercentageChanged(newPercentage);
    }

    function transfer(address to, uint256 value) external returns (bool success) {
        require(to != address(0), "Invalid address");
        require(value > 0, "Value must be greater than 0");
        require(balanceOf[msg.sender] >= value, "Insufficient balance");

        uint256 taxAmount = isExcludedFromFee[msg.sender] ? 0 : (value * taxRate[msg.sender]) / 10000;
        uint256 netValue = value - taxAmount;

        balanceOf[msg.sender] -= value;
        balanceOf[to] += netValue;

        emit Transfer(msg.sender, to, netValue);
        if (taxAmount > 0) {
            emit Transfer(msg.sender, address(0), taxAmount); // Transfer tax to address(0)
        }

        return true;
    }

    function approve(address spender, uint256 value) external returns (bool success) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool success) {
        require(from != address(0), "Invalid address");
        require(to != address(0), "Invalid address");
        require(value > 0, "Value must be greater than 0");
        require(balanceOf[from] >= value, "Insufficient balance");
        require(allowance[from][msg.sender] >= value, "Allowance exceeded");

        uint256 taxAmount = isExcludedFromFee[from] ? 0 : (value * taxRate[from]) / 10000;
        uint256 netValue = value - taxAmount;

        balanceOf[from] -= value;
        balanceOf[to] += netValue;
        allowance[from][msg.sender] -= value;

        emit Transfer(from, to, netValue);
        if (taxAmount > 0) {
            emit Transfer(from, address(0), taxAmount); // Transfer tax to address(0)
        }

        return true;
    }

    function buyTokens() external payable {
        uint256 amount = (msg.value * 10**decimals) / getCurrentBuyPrice();
        require(amount > 0, "Amount must be greater than 0");

        uint256 fee = (amount * buyFeePercentage) / 10000;
        uint256 netAmount = amount - fee;

        balanceOf[msg.sender] += netAmount;
        totalSupply += amount;

        emit Transfer(address(0), msg.sender, netAmount);
        emit Transfer(address(0), address(0), fee); // Transfer fee to address(0)
    }

    function sellTokens(uint256 amount) external {
        require(amount > 0, "Amount must be greater than 0");
        require(balanceOf[msg.sender] >= amount, "Insufficient balance");

        uint256 fee = (amount * sellFeePercentage) / 10000;
        uint256 netAmount = amount - fee;

        balanceOf[msg.sender] -= amount;
        totalSupply -= amount;

        (bool sent, ) = payable(msg.sender).call{value: getSellPrice(netAmount)}("");
        require(sent, "Failed to send Ether");

        emit Transfer(msg.sender, address(0), fee); // Transfer fee to address(0)
        emit Transfer(msg.sender, address(0), amount); // Burn the sold tokens
    }

    function getCurrentBuyPrice() public view returns (uint256) {
        return (10**decimals); // 1 Ether for 1 token
    }

    function getSellPrice(uint256 amount) public view returns (uint256) {
        return (amount * getCurrentBuyPrice()) / (10**decimals);
    }
}