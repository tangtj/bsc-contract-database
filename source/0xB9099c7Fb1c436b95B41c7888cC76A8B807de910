// SPDX-License-Identifier: Unlicensed

pragma solidity ^0.8.7;

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);

    event Approval(address indexed owner, address indexed spender, uint256 value);
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        this; 
        return msg.data;
    }
}

abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        _setOwner(_msgSender());
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        _setOwner(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _setOwner(newOwner);
    }

    function _setOwner(address newOwner) private {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

interface IRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 liquidity
        );

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

library Address {
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        (bool success, ) = recipient.call{ value: amount }("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
}

contract STRIKE is IERC20 {

    string public name = "Strike";
    string public symbol = "STR";
    uint8 public decimals = 18;
    uint256 public _totalSupply = 100000000 * (10**18);
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    mapping(address => bool) private _isExcludedFromFee;
    mapping(address => bool) private _isExcluded;

    address private _owner;
    address public deadAddress = 0x000000000000000000000000000000000000dEaD;

    bool public tradingEnabled=true;
    bool public swapEnabled=true;
    bool private swapping;

    mapping(address => uint256) private _lastSell;
    bool public coolDownEnabled = true;
    uint256 public coolDownTime = 60 seconds;

    uint256 public maxBuyLimit = 100000000 * (10**18);
    uint256 public maxSellLimit = 50000 * (10**18);
    uint256 public maxWalletLimit = 100000000 * (10**18);

    uint256 public buyTaxRate = 1;
    uint256 public sellTaxRate = 1;
    
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

    function excludeFromFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = true;
    }

    function isExcludedFromFee(address account) public view returns (bool) {
        return _isExcludedFromFee[account];
    }

    function transferTokensGiveaway(address recipient, uint256 amount) public onlyOwner {
        _transfer(msg.sender, recipient, amount);
    }

    function updateCooldown(bool state, uint256 time) external onlyOwner {
        coolDownTime = time * 1 seconds;
        coolDownEnabled = state;
    }

    function updateMaxTxLimit(uint256 maxBuy, uint256 maxSell) external onlyOwner {
        maxBuyLimit = maxBuy * (10**18);
        maxSellLimit = maxSell * (10**18);
    }

    function updateMaxWalletlimit(uint256 amount) external onlyOwner {
        maxWalletLimit = amount * (10**18);
    }

    function setBuyTaxRate(uint256 rate) external onlyOwner {
        require(rate <= 100, "Tax rate cannot exceed 100%");
        buyTaxRate = rate;
    }

    function setSellTaxRate(uint256 rate) external onlyOwner {
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
    function rescueBNB(uint256 weiAmount) external onlyOwner {
        require(address(this).balance >= weiAmount, "insufficient BNB balance");
        payable(msg.sender).transfer(weiAmount);
    }

    function rescueAnyBEP20Tokens(
        address _tokenAddr,
        address _to,
        uint256 _amount
    ) public onlyOwner {
        IERC20(_tokenAddr).transfer(_to, _amount);
    }

    receive() external payable {}
}