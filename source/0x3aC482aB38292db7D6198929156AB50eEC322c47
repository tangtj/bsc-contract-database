// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract QUICO{
    string public name = "QUICO";
    string public symbol = "QCO";
    uint8 public decimals = 18;
    uint256 public totalSupply = 28000000 * (10 ** uint256(decimals));
    address public owner;
    address public marketingWallet;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    
    uint256 public buyTaxPercentage;
    uint256 public sellTaxPercentage;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event BuyTaxPercentageUpdated(uint256 newPercentage);
    event SellTaxPercentageUpdated(uint256 newPercentage);

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    constructor(uint256 _buyTaxPercentage, uint256 _sellTaxPercentage, address _marketingWallet) {
        owner = msg.sender;
        marketingWallet = _marketingWallet;
        buyTaxPercentage = _buyTaxPercentage;
        sellTaxPercentage = _sellTaxPercentage;
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }

    function approve(address spender, uint256 value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transfer(address to, uint256 value) public returns (bool) {
        require(to != address(0), "Invalid address");
        uint256 taxedAmount = applyTax(msg.sender, value);
        balanceOf[msg.sender] -= taxedAmount;
        balanceOf[to] += taxedAmount;
        emit Transfer(msg.sender, to, taxedAmount);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) public returns (bool) {
        require(to != address(0), "Invalid address");
        require(value <= balanceOf[from], "Insufficient balance");
        require(value <= allowance[from][msg.sender], "Allowance exceeded");
        
        uint256 taxedAmount = applyTax(from, value);
        
        balanceOf[from] -= taxedAmount;
        balanceOf[to] += taxedAmount;
        allowance[from][msg.sender] -= value;
        emit Transfer(from, to, taxedAmount);
        return true;
    }

    function setBuyTaxPercentage(uint256 _buyTaxPercentage) public onlyOwner {
        buyTaxPercentage = _buyTaxPercentage;
        emit BuyTaxPercentageUpdated(_buyTaxPercentage);
    }

    function setSellTaxPercentage(uint256 _sellTaxPercentage) public onlyOwner {
        sellTaxPercentage = _sellTaxPercentage;
        emit SellTaxPercentageUpdated(_sellTaxPercentage);
    }

    function applyTax(address sender, uint256 amount) internal returns (uint256) {
        uint256 taxPercentage = sender == owner ? buyTaxPercentage : sellTaxPercentage;
        if (taxPercentage == 0) {
            return amount; // No tax
        }
        uint256 taxAmount = (amount * taxPercentage) / 100;
        uint256 taxedAmount = amount - taxAmount;

        // Transfer tax to the marketing wallet
        balanceOf[marketingWallet] += taxAmount;
        emit Transfer(sender, marketingWallet, taxAmount);

        return taxedAmount;
    }
}