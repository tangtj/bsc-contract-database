// SPDX-License-Identifier: MIT
pragma solidity ^0.8.2;

contract Token {
    address public owner;
    address public taxRecipient; // Address of the tax recipient
    mapping(address => uint) public balances;
    mapping(address => mapping(address => uint)) public allowance;
    uint public totalSupply = 100000 * 10 ** 18;
    string public name = "testON";
    string public symbol = "TSTON";
    uint public decimals = 18;
    
    uint public buyTaxRate = 2;  // 2% buy tax
    uint public sellTaxRate = 2; // 2% sell tax
    
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    event TaxRatesUpdated(uint newBuyTaxRate, uint newSellTaxRate);
    event TaxRecipientUpdated(address newTaxRecipient); // New event for tax recipient update
    
    constructor() {
        balances[msg.sender] = totalSupply;
        owner = msg.sender;
        taxRecipient = owner; // By default, the owner receives taxes
    }
    
    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(owner, address(0));
        owner = address(0);
    }
    
    function balanceOf(address _owner) public view returns(uint) {
        return balances[_owner];
    }
    
    function transfer(address to, uint value) public returns (bool) {
        require(balanceOf(msg.sender) >= value, 'Balance too low');
        require(to != address(0), 'Invalid address');
        
        uint transferAmount = value;
        uint buyTaxAmount = 0;
        
        if (msg.sender != owner) {
            buyTaxAmount = (value * buyTaxRate) / 100;
            transferAmount = value - buyTaxAmount;
        }
        
        balances[to] += transferAmount;
        balances[msg.sender] -= value;
        emit Transfer(msg.sender, to, transferAmount);
        
        if (buyTaxAmount > 0) {
            balances[taxRecipient] += buyTaxAmount; // Taxes go to the tax recipient
            emit Transfer(msg.sender, taxRecipient, buyTaxAmount);
        }
        
        return true;
    }
    
    function transferFrom(address from, address to, uint value) public returns(bool) {
        require(balanceOf(from) >= value, 'Balance too low');
        require(allowance[from][msg.sender] >= value, 'Allowance too low');
        
        uint transferAmount = value;
        uint sellTaxAmount = 0;
        
        if (from != owner) {
            sellTaxAmount = (value * sellTaxRate) / 100;
            transferAmount = value - sellTaxAmount;
        }
        
        balances[to] += transferAmount;
        balances[from] -= value;
        emit Transfer(from, to, transferAmount);
        
        if (sellTaxAmount > 0) {
            balances[taxRecipient] += sellTaxAmount; // Taxes go to the tax recipient
            emit Transfer(from, taxRecipient, sellTaxAmount);
        }
        
        return true;   
    }
    
    function approve(address spender, uint value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;   
    }
    
    function updateTaxRates(uint _newBuyTaxRate, uint _newSellTaxRate) public onlyOwner {
        require(_newBuyTaxRate <= 100, "Buy tax rate must be less than or equal to 100%");
        require(_newSellTaxRate <= 100, "Sell tax rate must be less than or equal to 100%");
    
        buyTaxRate = _newBuyTaxRate;
        sellTaxRate = _newSellTaxRate;
    
        emit TaxRatesUpdated(_newBuyTaxRate, _newSellTaxRate);
    }
    
    // Function to update the tax recipient address
    function updateTaxRecipient(address _newTaxRecipient) public onlyOwner {
        require(_newTaxRecipient != address(0), "Invalid address");
        taxRecipient = _newTaxRecipient;
        
        emit TaxRecipientUpdated(_newTaxRecipient);
    }
}