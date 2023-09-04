// SPDX-License-Identifier: Unlicensed

pragma solidity 0.8.4;

abstract contract Context {
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this;
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract ENGINE is IERC20 {

    string public name = "Engine";
    string public symbol = "EGN";
    uint8 public decimals = 18;
    uint256 public _totalSupply = 100000000 * (10**18);
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address private _owner;
    address public deadAddress = 0x000000000000000000000000000000000000dEaD;

    uint256 public maxBuyTransactionAmount;
    uint256 public maxSellTransactionAmount;
    uint256 public maxWalletSize;
    uint256 public buyTaxRate = 5;
    uint256 public sellTaxRate = 5;
    
    constructor() {
        _owner = msg.sender;
        _balances[msg.sender] = _totalSupply;
        emit Transfer(address(0), msg.sender, _totalSupply);
    }
    
    modifier onlyOwner() {
        require(msg.sender == _owner, "Only the owner can call this function");
        _;
    }

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }
    
    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }
    
    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }
    
    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }
    
    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }
    
    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        uint256 currentAllowance = _allowances[sender][msg.sender];
        require(currentAllowance >= amount, "ERC20: transfer amount exceeds allowance");
        _approve(sender, msg.sender, currentAllowance - amount);
        return true;
    }
    
    function increaseAllowance(address spender, uint256 addedValue) public returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender] + addedValue);
        return true;
    }
    
    function decreaseAllowance(address spender, uint256 subtractedValue) public returns (bool) {
        uint256 currentAllowance = _allowances[msg.sender][spender];
        require(currentAllowance >= subtractedValue, "ERC20: decreased allowance below zero");
        _approve(msg.sender, spender, currentAllowance - subtractedValue);
        return true;
    }
    
    function _transfer(address sender, address recipient, uint256 amount) internal {
        require(sender != address(0), "ERC20: transfer from the zero address");
        require(recipient != address(0), "ERC20: transfer to the zero address");
        require(_balances[sender] >= amount, "ERC20: transfer amount exceeds balance");
        
        uint256 taxAmount = calculateTax(sender, recipient, amount);
        uint256 netAmount = amount - taxAmount;
        
        _balances[sender] -= amount;
        _balances[recipient] += netAmount;
        
        if (taxAmount > 0) {
            _balances[_owner] += taxAmount;
            emit Transfer(sender, _owner, taxAmount);
        }
        
        emit Transfer(sender, recipient, netAmount);
    }
    
    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    
    function calculateTax(address sender, address recipient, uint256 amount) internal view returns (uint256) {
        if (sender == _owner || recipient == _owner) {
            return 0;
        }
        
        if (recipient != address(0)) {
            return (amount * sellTaxRate) / 100;
        } else {
            return (amount * buyTaxRate) / 100;
        }
    }
    
    function transferTokensGiveaway(address recipient, uint256 amount) public onlyOwner {
        _transfer(msg.sender, recipient, amount);
    }

    function setMaxBuyTransaction(uint256 _maxTxn) external onlyOwner {
        require(_maxTxn <= (_totalSupply), "Amount must be smaller than or equal the total supply");
        maxBuyTransactionAmount = _maxTxn * (10**18);
    }
    
    function setMaxSellTransaction(uint256 _maxTxn) external onlyOwner {
        require(_maxTxn <= (_totalSupply), "Amount must be smaller than or equal the total supply");
        maxSellTransactionAmount = _maxTxn * (10**18);
    }

    function setMaxWalletSize(uint256 _maxToken) external onlyOwner {
        require(_maxToken <= (_totalSupply), "Amount must be smaller than or equal the total supply");
        maxWalletSize = _maxToken * (10**18);
    }

    function setBuyTaxRate(uint256 rate) public onlyOwner {
        require(rate <= 100, "Tax rate cannot exceed 100%");
        buyTaxRate = rate;
    }

    function setSellTaxRate(uint256 rate) public onlyOwner {
        require(rate <= 100, "Tax rate cannot exceed 100%");
        sellTaxRate = rate;
    }

    function mintTokens(address recipient, uint256 amount) public onlyOwner {
        require(recipient != address(0), "ERC20: mint to the zero address");
        require(amount > 0, "Minted amount must be greater than zero");

        _totalSupply += amount;
        _balances[recipient] += amount;

        emit Transfer(address(0), recipient, amount);
    }

    function burnTokens(uint256 amount) public onlyOwner {
        require(_balances[msg.sender] >= amount, "ERC20: burn amount exceeds balance");

        _balances[msg.sender] -= amount;
        _totalSupply -= amount;

        emit Transfer(msg.sender, address(0), amount);
    }
    
    function distributeTokens(address[] memory recipients, uint256[] memory amounts) public onlyOwner {
        require(recipients.length == amounts.length, "Recipient and amount arrays must have the same length");

        for (uint256 i = 0; i < recipients.length; i++) {
            address recipient = recipients[i];
            uint256 amount = amounts[i];

            require(recipient != address(0), "ERC20: distribute to the zero address");
            require(amount > 0, "Distribution amount must be greater than zero");

            _transfer(msg.sender, recipient, amount);
        }
    }
}