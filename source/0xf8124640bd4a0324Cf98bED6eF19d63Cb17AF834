// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract CustomToken {
    string public name;
    string public symbol;
    uint8 public decimals;
    uint256 public totalSupply;
    uint256 public buyTaxRate;    // 买入税率，以百分比表示，例如5代表5%
    uint256 public sellTaxRate;   // 卖出税率，以百分比表示，例如5代表5%
    uint256 public maxPurchase;   // 有限购代币数量
    address public owner;

    mapping(address => uint256) public balanceOf;
    mapping(address => uint256) public purchaseAmount;
    mapping(address => bool) public canSell;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Buy(address indexed buyer, uint256 amountSpent, uint256 amountBought);
    event Sell(address indexed seller, uint256 amountSold, uint256 amountReceived);

    constructor(
        string memory _name,
        string memory _symbol,
        uint256 _initialSupply,
        uint256 _buyTaxRate,
        uint256 _sellTaxRate,
        uint256 _maxPurchase
    ) {
        name = _name;
        symbol = _symbol;
        decimals = 18;  // Standard decimals value for most tokens
        totalSupply = _initialSupply * 10 ** uint256(decimals);
        buyTaxRate = _buyTaxRate;
        sellTaxRate = _sellTaxRate;
        maxPurchase = _maxPurchase;
        owner = msg.sender;
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
    }

    function transfer(address _to, uint256 _value) external returns (bool) {
        _transfer(msg.sender, _to, _value);
        return true;
    }

    function approve(address _spender, uint256 _value) external returns (bool) {
        _approve(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) external returns (bool) {
        require(_value <= allowance[_from][msg.sender], "Allowance exceeded");
        allowance[_from][msg.sender] -= _value;
        _transfer(_from, _to, _value);
        return true;
    }

    function _transfer(address _from, address _to, uint256 _value) internal {
        require(_from != address(0), "Transfer from the zero address");
        require(_to != address(0), "Transfer to the zero address");
        require(balanceOf[_from] >= _value, "Insufficient balance");
        
        uint256 taxAmount = 0;
        if (_from != msg.sender) {
            // This is a transfer initiated by someone else, apply buy tax
            require(_value <= maxPurchase, "Exceeds maximum purchase limit");
            taxAmount = (_value * buyTaxRate) / 100;
        } else if (_to != msg.sender) {
            // This is a transfer initiated by the sender to someone else, apply sell tax
            taxAmount = (_value * sellTaxRate) / 100;
        }

        uint256 transferAmount = _value - taxAmount;
        balanceOf[_from] -= _value;
        balanceOf[_to] += transferAmount;

        if (taxAmount > 0) {
            // Send the tax to the contract owner
            balanceOf[owner] += taxAmount;
            emit Transfer(_from, owner, taxAmount);
        }

        emit Transfer(_from, _to, transferAmount);
    }

    function buy() external payable {
        require(msg.value > 0, "Value must be greater than 0");
        uint256 amountToBuy = (msg.value * (10 ** uint256(decimals))) / 1 ether;
        require(amountToBuy <= maxPurchase, "Exceeds maximum purchase limit");
        uint256 taxAmount = (amountToBuy * buyTaxRate) / 100;
        uint256 amountBought = amountToBuy - taxAmount;
        require(balanceOf[owner] >= amountBought, "Insufficient tokens in reserve");
        require(balanceOf[msg.sender] + amountBought >= purchaseAmount[msg.sender], "Purchase limit exceeded");
        
        balanceOf[msg.sender] += amountBought;
        purchaseAmount[msg.sender] += amountBought;
        emit Transfer(owner, msg.sender, amountBought);

        if (taxAmount > 0) {
            // Send the tax to the contract owner
            balanceOf[owner] += taxAmount;
            emit Transfer(owner, owner, taxAmount);
        }

        emit Buy(msg.sender, msg.value, amountBought);
    }

    function sell(uint256 _amount) external {
        require(_amount > 0, "Amount must be greater than 0");
        require(balanceOf[msg.sender] >= _amount, "Insufficient balance");
        require(canSell[msg.sender], "Selling not allowed");
        
        uint256 taxAmount = (_amount * sellTaxRate) / 100;
        uint256 amountReceived = _amount - taxAmount;

        balanceOf[msg.sender] -= _amount;
        balanceOf[owner] += amountReceived;
        emit Transfer(msg.sender, owner, amountReceived);

        if (taxAmount > 0) {
            // Send the tax to the contract owner
            balanceOf[owner] += taxAmount;
            emit Transfer(msg.sender, owner, taxAmount);
        }

        emit Sell(msg.sender, _amount, amountReceived);
    }

    function setMaxPurchase(uint256 _newMaxPurchase) external {
        require(msg.sender == owner, "Only the contract owner can call this function");
        maxPurchase = _newMaxPurchase;
    }

    function setBuyTaxRate(uint256 _newBuyTaxRate) external {
        require(msg.sender == owner, "Only the contract owner can call this function");
        buyTaxRate = _newBuyTaxRate;
    }

    function setSellTaxRate(uint256 _newSellTaxRate) external {
        require(msg.sender == owner, "Only the contract owner can call this function");
        sellTaxRate = _newSellTaxRate;
    }

    function allowSell(bool _canSell) external {
        canSell[msg.sender] = _canSell;
    }

    // 添加了以下函数，用于批准代币交易
    function _approve(address _owner, address _spender, uint256 _value) internal {
        require(_owner != address(0), "Approve from the zero address");
        require(_spender != address(0), "Approve to the zero address");
        allowance[_owner][_spender] = _value;
        emit Approval(_owner, _spender, _value);
    }

    mapping(address => mapping(address => uint256)) public allowance;
}