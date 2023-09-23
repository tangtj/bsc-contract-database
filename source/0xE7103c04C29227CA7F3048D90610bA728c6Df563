// SPDX-License-Identifier: MIT
pragma solidity ^0.8.17;
pragma experimental ABIEncoderV2;
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
library SafeMath {
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;  

        return c;
    }
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");
        return c;
    }
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b > 0, errorMessage);
        uint256 c = a / b;
        return c;
    }
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}
library Address {
    function isContract(address account) internal view returns (bool) {
        uint256 size;
        // solhint-disable-next-line no-inline-assembly
        assembly {size := extcodesize(account)}
        return size > 0;
    }
    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");
        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success,) = recipient.call{value : amount}("");
        require(success, "Address: unable to send value, recipient may have reverted");
    }
    function functionCall(address target, bytes memory data) internal returns (bytes memory) {
        return functionCall(target, data, "Address: low-level call failed");
    }
    function functionCall(address target, bytes memory data, string memory errorMessage) internal returns (bytes memory) {
        return _functionCallWithValue(target, data, 0, errorMessage);
    }
    function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns (bytes memory) {
        return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
    }
    function functionCallWithValue(address target, bytes memory data, uint256 value, string memory errorMessage) internal returns (bytes memory) {
        require(address(this).balance >= value, "Address: insufficient balance for call");
        return _functionCallWithValue(target, data, value, errorMessage);
    }
    function _functionCallWithValue(address target, bytes memory data, uint256 weiValue, string memory errorMessage) private returns (bytes memory) {
        require(isContract(target), "Address: call to non-contract");
        (bool success, bytes memory returndata) = target.call{value : weiValue}(data);
        if (success) {
            return returndata;
        } else {
            if (returndata.length > 0) {
                assembly {
                    let returndata_size := mload(returndata)
                    revert(add(32, returndata), returndata_size)
                }
            } else {
                revert(errorMessage);
            }
        }
    }
}
contract Ownable is Context {
    address internal _owner;
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
    function owner() public view returns (address) {
        return _owner;
    }
    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
        _;
    }
    function transferOwnership(address newOwner) public virtual onlyOwner {
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}
interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB)
    external
    returns (address pair);
}
interface IUniswapV2Router02 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external
    returns (
        uint256 amountA,
        uint256 amountB,
        uint256 liquidity
    );
}
interface IUniswapV2Pair {
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function sync() external;
}
contract CRD is Context, IERC20, Ownable {
    using SafeMath for uint256;
    using Address for address;
    mapping(address => uint256) private _balances;
    mapping(address => mapping(address => uint256)) private _allowances;
    uint8 private _decimals;
    uint256 private _totalSupply;
    string private _name;
    string private _symbol;
    IUniswapV2Router02 public uniswapV2Router;
    mapping(address => bool) public ammPairs;
    address public uniswapV2Pair;
    address public token;
    address  holder;
    
    struct Interest {
        IERC20 token;
    }
    Interest internal lpInterest;
    uint public addPriceTokenAmount = 1e14;
    address private _route;

    address private LPDivvy_ad;
    address private delDivvy_ad;

    uint256 public _burnFee;
    uint256 public _buyFee;
    uint256 public _sellFee;
    uint256 public _delFee;

    bool private buylock;
    bool private selllock;

    mapping(address=>bool) private whitelist;
    
    constructor (address holderAdr,address LPAdr,address delAdr) {
        _decimals = 18;
        _totalSupply = 16721319000000000000000000;
        _name = "CRD";
        _symbol = "CRD";

        token = 0x55d398326f99059fF775485246999027B3197955;
        _route = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

         _owner = msg.sender;
        holder = holderAdr;
        LPDivvy_ad = LPAdr;
        delDivvy_ad = delAdr; 

        uniswapV2Router = IUniswapV2Router02(_route);  
        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory()).createPair(address(this), token);
        ammPairs[uniswapV2Pair] = true;
        lpInterest.token = IERC20(uniswapV2Pair);

        _balances[holder] = 16721319000000000000000000;
        emit Transfer(address(0), holder, 16721319000000000000000000);
        buylock = true;
        selllock = false;
        _burnFee = 10;
        _buyFee = 20;
        _sellFee = 20;
        _delFee = 60;
    }

    function setwhitelist(address[] memory accounts) public onlyOwner {
        for( uint i = 0; i < accounts.length; i++ ){
            whitelist[accounts[i]] = true;
        }
    }
    
    function cancelwhitelist(address account) public onlyOwner {
        whitelist[account] = false;
    }

    function iswhitelist(address account) public view returns(bool) {
        return whitelist[account];
    }
    function setLock(bool _buylock,bool _selllock) public onlyOwner{
        buylock = _buylock;
        selllock = _selllock;
    }

    function setFee(uint256 burnFee,uint256 buyFee,uint256 sellFee,uint256 delFee) public onlyOwner{
        _burnFee = burnFee;
        _buyFee = buyFee;
        _sellFee = sellFee;
        _delFee = delFee;
    }
    
    function name() public view returns (string memory) {
        return _name;
    }
    function symbol() public view returns (string memory) {
        return _symbol;
    }
    function decimals() public view returns (uint8) {
        return _decimals;
    }
    function totalSupply() public view override returns (uint256) {
        return _totalSupply;
    }
    function balanceOf(address account) public view override returns (uint256) {
        return _balances[account];
    }
    function _mint(address account, uint256 amount) internal {
        require(account != address(0), "BEP20: mint to the zero address");
        _totalSupply = _totalSupply.add(amount);
        _balances[account] = _balances[account].add(amount);
        emit Transfer(address(0), account, amount);
    }
    function burn(uint256 amount) public returns (bool) {
        _burn(_msgSender(), amount);
        return true;
    }
    function _burn(address account, uint256 amount) internal {
        require(account != address(0), "ERC20: burn from the zero address");
        _balances[account] = _balances[account].sub(amount, "ERC20: burn amount exceeds balance");
        _totalSupply = _totalSupply.sub(amount);
        emit Transfer(account, address(0), amount);
    }
    function transfer(address recipient, uint256 amount) public override returns (bool) {
        _transfer(_msgSender(), recipient, amount);
        return true;
    }
    function allowance(address owner, address spender) public view override returns (uint256) {
        return _allowances[owner][spender];
    }
    function approve(address spender, uint256 amount) public override returns (bool) {
        _approve(_msgSender(), spender, amount);
        return true;
    }
    function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, _msgSender(), _allowances[sender][_msgSender()].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }
    function increaseAllowance(address spender, uint256 addedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].add(addedValue));
        return true;
    }
    function decreaseAllowance(address spender, uint256 subtractedValue) public virtual returns (bool) {
        _approve(_msgSender(), spender, _allowances[_msgSender()][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }
    receive() external payable {}
    
    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");
        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    event test1(address, uint, uint);
    function _isLiquidity(address from, address to) internal returns (bool isAdd, bool isDel, bool isSell, bool isBuy){
        address token0 = IUniswapV2Pair(address(uniswapV2Pair)).token0();
        (uint r0,,) = IUniswapV2Pair(address(uniswapV2Pair)).getReserves();
        uint bal0 = IERC20(token0).balanceOf(address(uniswapV2Pair));

        emit test1(token0, r0, bal0);
        if (ammPairs[to]) {
            if (token0 != address(this) && bal0 > r0) {
                isAdd = bal0 - r0 > addPriceTokenAmount;
            }
            if (!isAdd) {
                isSell = true;
            }
        }
        if (ammPairs[from]) {
            if (token0 != address(this) && bal0 < r0) {
                isDel = r0 - bal0 > 0;
            }
            if (!isDel) {
                isBuy = true;
            }
        }
    }

    event test(bool, bool, bool, bool);
    function _transfer(address from, address to, uint256 amount) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        bool isAdd;
        bool isDel;
        bool isSell;
        bool isBuy;
        (isAdd, isDel, isSell, isBuy) = _isLiquidity(from, to);
        emit test(isAdd, isDel, isSell, isBuy);

        bool takeFee;
        uint256 fee;
        if(isBuy){
            if(buylock){
                require(whitelist[to]==true,"buy lock");
            }
            takeFee = true;
            fee = _buyFee;
        }else if(isSell){
            if(selllock){
                require(whitelist[from]==true,"sell lock");
            }
            takeFee = true;
            fee = _sellFee;
        }else if(isDel){
            takeFee = true;
            fee = _delFee;
        }else{
            takeFee = false;
        }
        if(_totalSupply.sub(_balances[address(0)]) <= 200000000000000000000000){
            takeFee = false;
        }

        _tokenTransfer(from, to, amount,takeFee,isDel,fee);
    }
    function _tokenTransfer(address sender, address recipient, uint256 tAmount,bool takeFee,bool isDel,uint256 fee) private {
        
        _balances[sender] = _balances[sender].sub(tAmount);
        if(takeFee){
            uint256 freeAmount;
            uint256 amount;
            address lpAdr = delDivvy_ad;

            freeAmount = tAmount.mul( fee ).div(1000);
            amount =  tAmount.sub(freeAmount);
            if(!isDel){
                lpAdr = LPDivvy_ad;
                uint256 burnAmount = tAmount.mul( _burnFee ).div(1000);
                amount =  amount.sub(burnAmount);
                _balances[address(0)] = _balances[address(0)].add(burnAmount);
                emit Transfer(sender, address(0), burnAmount);
            }

            _balances[recipient] = _balances[recipient].add(amount);
            emit Transfer(sender, recipient, amount);
            _balances[lpAdr] = _balances[lpAdr].add(freeAmount);
            emit Transfer(sender, lpAdr, freeAmount);
            
        }else{
            _balances[recipient] = _balances[recipient].add(tAmount);
            emit Transfer(sender, recipient, tAmount);
        }
    }

    function gettoken01() public view returns (address, address, bool){
        return (IUniswapV2Pair(address(uniswapV2Pair)).token0(), address(this), IUniswapV2Pair(address(uniswapV2Pair)).token0() < address(this));
    }
    
    function getLock() public view returns(bool,bool){
        return (buylock, selllock);
    }
    
    function getFee() public view returns(uint256,uint256,uint256,uint256){
        return (_burnFee,_buyFee,_sellFee,_delFee);
    }
}