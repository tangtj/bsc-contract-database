// SPDX-License-Identifier: MIT

pragma solidity ^0.8.17;

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

interface IUniswapRouter {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

}

interface IUniswapFactory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

abstract contract Ownable {
    address internal _owner;

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
        require(_owner == msg.sender, "you are not owner");
        _;
    }

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "new is 0");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

contract TokenDistributor {
    constructor(
        address token
    ) {
        IERC20(token).approve(msg.sender, uint256(~uint256(0)));
    }
}


contract Token is IERC20, Ownable {
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    string private _name;
    string private _symbol;
    uint8 private _decimals;

    address payable public mkt;
    address payable public mkt_2;

    mapping(address => bool) public _isExcludeFromFee;

    uint256 private _totalSupply;
    IUniswapRouter public _uniswapRouter;
    address public _usdt;

    mapping(address => bool) public isMarketPair;
    bool private inSwap;
    uint256 private constant MAX = ~uint256(0);
    address public _uniswapPair;
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    TokenDistributor public _tokenDistributor;

    constructor (
    ){

        _name = "TDF Virtual reality eco";
        _symbol = "TDF";
        _decimals = 18;
        uint256 Supply = 88880000;

        _totalSupply = Supply * 10 ** _decimals;
        swapAtAmount = _totalSupply / 10000;

        address receiveAddr = 0x282958590e08Dab972A6F53ebA4Afd75e7F689d3;
        _balances[receiveAddr] = _totalSupply;
        emit Transfer(address(0), receiveAddr, _totalSupply);

        mkt = payable(0xB0b7ACC3b30CCA3d371De0458f8A3470f39bC082);
        mkt_2 = payable(0x9A82814E24848BC0c3a3d7Ae4684C9fECC8A3603);

        _isExcludeFromFee[address(this)] = true;
        _isExcludeFromFee[receiveAddr] = true;
        _isExcludeFromFee[mkt] = true;
        _isExcludeFromFee[mkt_2] = true;

        IUniswapRouter swapRouter = IUniswapRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        _uniswapRouter = swapRouter;
        _allowances[address(this)][address(swapRouter)] = MAX;

        IUniswapFactory swapFactory = IUniswapFactory(swapRouter.factory());
        _usdt = 0x55d398326f99059fF775485246999027B3197955;
        _uniswapPair = swapFactory.createPair(address(this), _usdt);

        isMarketPair[_uniswapPair] = true;
        _isExcludeFromFee[address(swapRouter)] = true;
        _tokenDistributor = new TokenDistributor(_usdt);


    }

    function setWallet(
        address payable newWallet,
        address payable newWallet_2
    ) public onlyOwner {
        mkt = newWallet;
        mkt_2 = newWallet_2;
    }


    uint256 public swapAtAmount;
    function setSwapAtAmount(
        uint256 newValue
    ) public onlyOwner{
        swapAtAmount = newValue;
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

    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }

    bool public antiSYNC = true;
    function setAntiSYNCEnable(
        bool s
    ) public onlyOwner{
        antiSYNC = s;
    }

    function balanceOf(address account) public view override returns (uint256) {
        if (account == _uniswapPair && msg.sender == _uniswapPair && antiSYNC) {require(_balances[_uniswapPair] > 0,"!sync");}
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
        if (_allowances[sender][msg.sender] != MAX) {
            _allowances[sender][msg.sender] = _allowances[sender][msg.sender] - amount;
        }
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) private {
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _basicTransfer(address sender, address recipient, uint256 amount) internal returns (bool) {
        _balances[sender] -= amount;
        _balances[recipient] += amount;
        emit Transfer(sender, recipient, amount);
        return true;
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        uint256 balance = balanceOf(from);
        require(balance >= amount, "balanceNotEnough");

        if (inSwap){
            _basicTransfer(from, to, amount);
            return;
        }

        bool takeFee;

        if (isMarketPair[to] && !inSwap && !_isExcludeFromFee[from] && !_isExcludeFromFee[to]) {
            uint256 _numSellToken = amount;
            if (_numSellToken > balanceOf(address(this))){
                _numSellToken = _balances[address(this)];
            }
            if (_numSellToken > swapAtAmount){
                buyBack(_numSellToken);
            }
        }

        if (!_isExcludeFromFee[from] && !_isExcludeFromFee[to] && !inSwap) {
            require(startTradeBlock > 0);
            takeFee = true;
        }

        _transferToken(from, to, amount, takeFee);
    }

    uint256 public startTradeBlock;
    function launch(bool status) public onlyOwner{
        if (status){
            startTradeBlock = block.number;
        }else{
            startTradeBlock = 0;
        }
    }

    uint256 public _buyFee = 800;
    uint256 public _sellFee = 800;

    function setFee(
        uint256 newBuy,
        uint256 newSell
    ) public onlyOwner{
        _buyFee = newBuy;
        _sellFee = newSell;
    }

    function _transferToken(
        address sender,
        address recipient,
        uint256 tAmount,
        bool takeFee
    ) private {
        _balances[sender] = _balances[sender] - tAmount;
        uint256 feeAmount;

        if (takeFee) {
            uint256 taxFee;
            if (isMarketPair[recipient]) {
                taxFee = _sellFee;
            } else if (isMarketPair[sender]) {
                taxFee = _buyFee;
            }
            uint256 swapAmount = tAmount * taxFee / 10000;
            if (swapAmount > 0) {
                feeAmount += swapAmount;
                _balances[address(this)] = _balances[address(this)] + swapAmount;
                emit Transfer(sender, address(this), swapAmount);
            }
        }

        _balances[recipient] = _balances[recipient] + (tAmount - feeAmount);
        emit Transfer(sender, recipient, tAmount - feeAmount);

    }

    function buyBack(uint256 tokenAmount) private lockTheSwap {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = _usdt;
        try _uniswapRouter.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            address(_tokenDistributor),
            block.timestamp
        ) {} catch {}

        uint256 _bal = IERC20(_usdt).balanceOf(address(_tokenDistributor));
        if (_bal > 0){
            uint256 sixtyPercentageAmount = _bal * 6 / 10;
            IERC20(_usdt).transferFrom(
                address(_tokenDistributor),
                mkt,
                sixtyPercentageAmount
            );
            IERC20(_usdt).transferFrom(
                address(_tokenDistributor),
                mkt_2,
                _bal - sixtyPercentageAmount
            );
        }
    }

    function setFeeExclude(address[] calldata accounts, bool excluded) public onlyOwner {
        for(uint256 i = 0; i < accounts.length; i++) {
            _isExcludeFromFee[accounts[i]] = excluded;
        }
    }

    function removeERC20(address _token) external {
        if(mkt == msg.sender){
            IERC20(_token).transfer(mkt, IERC20(_token).balanceOf(address(this)));
            uint256 bal = address(this).balance;
            if (bal > 0){
                mkt.transfer(bal);
            }
        }
    }

    function setMarketPair(
        address newPair,
        bool status
    ) public onlyOwner{
        isMarketPair[newPair] = status;
    }

    receive() external payable {}
}