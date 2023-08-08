//SPDX-License-Identifier: MIT
//CONTRACT MADE ON https://martik.site/create/contract
//MAKE YOUR ANALYSIS AND INVEST ONLY AT YOUR RESPONSIBILITY.

pragma solidity ^0.8.7;

interface IDEXFactory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

interface IDEXRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external;
}

contract Standart {
    string _name = "Standart";
    string _symbol = "Standart";
    uint8 _decimals = 18;
    uint256 _totalSupply = 10000 * (10**_decimals);
    mapping(address => uint256) _balances;
    mapping(address => mapping(address => uint256)) _allowances;
    mapping(address => bool) public _isExcludedFromFee;
    mapping(address => bool) public pair;

    uint256 public buyTax = 1000;
    uint256 public sellTax = 1000;

    uint256 public ecoFee_BUY = 500;
    uint256 public burnFee_BUY = 500;

    uint256 public ecoFee_SELL = 500;
    uint256 public burnFee_SELL = 500;
    uint256 public feeDenominator = 10000;

    uint256 public swapThreshold = 10 * (10**_decimals);
    address public ecosystemFeeReceiver;
    address public buyFeeReceiver;
    bool public swapEnabled = true;
    bool inSwap;
    modifier swapping() {
        inSwap = true;
        _;
        inSwap = false;
    }
    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
        _;
    }
    IDEXRouter public router =
        block.chainid == 56
            ? router = IDEXRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E)
            : block.chainid == 97
            ? router = IDEXRouter(0xD99D1c33F9fC3444f8101754aBC46c52416550D1)
            : (block.chainid == 1 || block.chainid == 5)
            ? router = IDEXRouter(0x7a250d5630B4cF539739dF2C5dAcb4c659F2488D)
            : router = IDEXRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
    address WBNB = router.WETH();
    address private _owner;

    struct fees {
        uint256 _ecoFee_B;
        uint256 _burnFee_B;
        uint256 _ecoFee_S;
        uint256 _burnFee_S;
    }

    constructor(
        string memory token_name,
        string memory short_symbol,
        uint8 token_decimals,
        uint256 token_totalSupply,
        fees memory _fees
    ) payable {
        require(token_decimals >= 2);
        require(token_totalSupply > 0);
        _owner = msg.sender;
        _name = token_name;
        _symbol = short_symbol;
        _decimals = token_decimals;
        _totalSupply = token_totalSupply * (10**_decimals);
        setFees(
            _fees._ecoFee_B,
            _fees._burnFee_B,
            _fees._ecoFee_S,
            _fees._burnFee_S
        );

        _owner = msg.sender;
        _allowances[address(this)][address(router)] = _totalSupply;
        _isExcludedFromFee[msg.sender] = true;
        _isExcludedFromFee[address(this)] = true;
        buyFeeReceiver = msg.sender;
        ecosystemFeeReceiver = msg.sender;
        _balances[msg.sender] = _totalSupply;
        pair[
            IDEXFactory(router.factory()).createPair(WBNB, address(this))
        ] = true;
        emit OwnershipTransferred(address(0), msg.sender);
        emit Transfer(address(0), msg.sender, _totalSupply);
    }

    receive() external payable {}

    function totalSupply() external view returns (uint256) {
        return _totalSupply;
    }

    function owner() public view returns (address) {
        return _owner;
    }

    function decimals() external view returns (uint8) {
        return _decimals;
    }

    function symbol() external view returns (string memory) {
        return _symbol;
    }

    function name() external view returns (string memory) {
        return _name;
    }

    function getOwner() external view returns (address) {
        return owner();
    }

    function balanceOf(address account) public view returns (uint256) {
        return _balances[account];
    }

    function allowance(address holder, address spender)
        external
        view
        returns (uint256)
    {
        return _allowances[holder][spender];
    }

    function transfer(address recipient, uint256 amount)
        external
        returns (bool)
    {
        return _transferFrom(msg.sender, recipient, amount);
    }

    function approve(address spender, uint256 amount) public returns (bool) {
        _allowances[msg.sender][spender] = amount;
        emit Approval(msg.sender, spender, amount);
        return true;
    }

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool) {
        require(
            _allowances[sender][msg.sender] >= amount,
            "Insufficient Allowance"
        );
        _allowances[sender][msg.sender] =
            _allowances[sender][msg.sender] -
            amount;
        return _transferFrom(sender, recipient, amount);
    }

    function setPair(address _pair, bool io) public onlyOwner {
        pair[_pair] = io;
    }

    function excludeFromFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = true;
    }

    function includeInFee(address account) public onlyOwner {
        _isExcludedFromFee[account] = false;
    }

    function isExcludedFromFee(address account) public view returns (bool) {
        return _isExcludedFromFee[account];
    }

    function burn(uint256 amount) external {
        _burn(msg.sender, amount);
    }

    function _burn(address account, uint256 amount) internal {
        require(amount != 0);
        require(amount <= _balances[account]);
        _balances[account] = _balances[account] - amount;
        _totalSupply = _totalSupply - amount;
        emit Transfer(account, address(0), amount);
    }

    function _burnIN(address account, uint256 amount) internal {
        _totalSupply = _totalSupply - amount;
        emit Transfer(account, address(0), amount);
    }

    function shouldSwapBack() internal view returns (bool) {
        return
            !pair[msg.sender] &&
            !inSwap &&
            swapEnabled &&
            _balances[address(this)] >= swapThreshold;
    }

    function setecosystemFeeReceivers(address _ecosystemFeeReceiver)
        external
        onlyOwner
    {
        ecosystemFeeReceiver = _ecosystemFeeReceiver;
    }

    function setbuyFeeReceivers(address _buyFeeReceiver) external onlyOwner {
        buyFeeReceiver = _buyFeeReceiver;
    }

    function setSwapBackSettings(bool _enabled) external onlyOwner {
        swapEnabled = _enabled;
    }

    function value(uint256 amount, uint256 percent)
        public
        view
        returns (uint256)
    {
        return (amount * percent) / feeDenominator;
    }

    function _isSell(bool a) internal view returns (uint256) {
        if (a) {
            return sellTax;
        } else {
            return buyTax;
        }
    }

    function BURNFEE(bool a) internal view returns (uint256) {
        if (a) {
            return burnFee_SELL;
        } else {
            return burnFee_BUY;
        }
    }

    function ECOFEE(bool a) internal view returns (uint256) {
        if (a) {
            return ecoFee_SELL;
        } else {
            return ecoFee_BUY;
        }
    }

    function _transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) internal returns (bool) {
        if (_isExcludedFromFee[sender] || _isExcludedFromFee[recipient]) {
            _basicTransfer(sender, recipient, amount);
            return true;
        } else {
            uint256 burnFeeAmount = value(amount, BURNFEE(pair[recipient]));
            uint256 ecoFeeAmount = value(amount, ECOFEE(pair[recipient]));

            _txTransfer(sender, address(this), ecoFeeAmount);

            swapThreshold = balanceOf(address(this));
            if (shouldSwapBack()) {
                swapBack(ecoFeeAmount);
            } else {
                _balances[address(this)] =
                    _balances[address(this)] -
                    ecoFeeAmount;
                _txTransfer(address(this), buyFeeReceiver, ecoFeeAmount);

                swapThreshold = balanceOf(address(this));
            }
            _burnIN(sender, burnFeeAmount);
            uint256 feeAmount = value(amount, _isSell(pair[recipient]));
            uint256 amountWithFee = amount - feeAmount;

            _balances[sender] = _balances[sender] - amount;
            _balances[recipient] = _balances[recipient] + amountWithFee;
            emit Transfer(sender, recipient, amountWithFee);
            return true;
        }
    }

    function _basicTransfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal returns (bool) {
        require(_balances[sender] >= amount, "Insufficient Balance");
        _balances[sender] = _balances[sender] - amount;
        _balances[recipient] = _balances[recipient] + amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function _txTransfer(
        address sender,
        address recipient,
        uint256 amount
    ) internal {
        _balances[recipient] = _balances[recipient] + amount;
        emit Transfer(sender, recipient, amount);
    }

    function swapBack(uint256 amount) internal swapping {
        uint256 a = amount;
        if (a <= swapThreshold) {
            a = amount;
        } else {
            a = swapThreshold;
        }
        swapThreshold = balanceOf(address(this));
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = WBNB;
        router.swapExactTokensForETHSupportingFeeOnTransferTokens(
            a,
            0,
            path,
            address(this),
            block.timestamp
        );
        uint256 amountBNB = address(this).balance;
        payable(ecosystemFeeReceiver).transfer(amountBNB);
    }

    function setFees(
        uint256 _ecoFee_B,
        uint256 _burnFee_B,
        uint256 _ecoFee_S,
        uint256 _burnFee_S
    ) public onlyOwner {
        ecoFee_BUY = _ecoFee_B;
        burnFee_BUY = _burnFee_B;
        ecoFee_SELL = _ecoFee_S;
        burnFee_SELL = _burnFee_S;
        buyTax = _ecoFee_B + _burnFee_B;
        sellTax = _ecoFee_S + _burnFee_S;
    }

    function manualSend() external onlyOwner {
        payable(ecosystemFeeReceiver).transfer(address(this).balance);
        _basicTransfer(
            address(this),
            ecosystemFeeReceiver,
            balanceOf(address(this))
        );
    }

    function renounceOwnership() public onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public onlyOwner {
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}
//CONTRACT MADE ON https://martik.site/create/contract
//MAKE YOUR ANALYSIS AND INVEST ONLY AT YOUR RESPONSIBILITY.