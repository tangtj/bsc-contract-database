// SPDX-License-Identifier: MIT

pragma solidity ^0.8.18;
interface IERC20 {
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
    function _msgSender() internal view virtual returns (address payable) {
        return payable(msg.sender);
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}

library Address {

    function isContract(address account) internal view returns (bool) {
        uint256 size;
        // solhint-disable-next-line no-inline-assembly
        assembly { size := extcodesize(account) }
        return size > 0;
    }

    function sendValue(address payable recipient, uint256 amount) internal {
        require(address(this).balance >= amount, "Address: insufficient balance");

        // solhint-disable-next-line avoid-low-level-calls, avoid-call-value
        (bool success, ) = recipient.call{ value: amount }("");
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

        // solhint-disable-next-line avoid-low-level-calls
        (bool success, bytes memory returndata) = target.call{ value: weiValue }(data);
        if (success) {
            return returndata;
        } else {
            // Look for revert reason and bubble it up if present
            if (returndata.length > 0) {
                // The easiest way to bubble the revert reason is using memory via assembly

                // solhint-disable-next-line no-inline-assembly
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
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

interface IUniswapV2Pair{
    function token0() external view returns (address);
    function token1() external view returns (address); 
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function factory() external view returns (address);
    function sync() external;
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
    )external
        returns (
            uint256 amountA,
            uint256 amountB,
            uint256 liquidity
        );
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] memory path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] memory path) external view returns (uint[] memory amounts);        
}


library UniswapV2Library {
    using SafeMath for uint;

    // returns sorted token addresses, used to handle return values from pairs sorted in this order
    function sortTokens(address tokenA, address tokenB) internal pure returns (address token0, address token1) {
        require(tokenA != tokenB, 'UniswapV2Library: IDENTICAL_ADDRESSES');
        (token0, token1) = tokenA < tokenB ? (tokenA, tokenB) : (tokenB, tokenA);
        require(token0 != address(0), 'UniswapV2Library: ZERO_ADDRESS');
    }

    // calculates the CREATE2 address for a pair without making any external calls
    function pairFor(address factory, address tokenA, address tokenB) internal pure returns (address pair) {
        (address token0, address token1) = sortTokens(tokenA, tokenB);
        pair = address(uint160(uint256(keccak256(abi.encodePacked(
                hex'ff',
                factory,
                keccak256(abi.encodePacked(token0, token1)),
                hex'2bbf561059bfad76dd3e7f5e7e45d68e76db16d20d00dc76889202628a6417d9' // init code hash
            )))));
    }

    // fetches and sorts the reserves for a pair
    function getReserves(address factory, address tokenA, address tokenB) internal view returns (uint reserveA, uint reserveB) {
        (address token0,) = sortTokens(tokenA, tokenB);
        (uint reserve0, uint reserve1,) = IUniswapV2Pair(pairFor(factory, tokenA, tokenB)).getReserves();
        (reserveA, reserveB) = tokenA == token0 ? (reserve0, reserve1) : (reserve1, reserve0);
    }

    // given some amount of an asset and pair reserves, returns an equivalent amount of the other asset
    function quote(uint amountA, uint reserveA, uint reserveB) internal pure returns (uint amountB) {
        require(amountA > 0, 'UniswapV2Library: INSUFFICIENT_AMOUNT');
        require(reserveA > 0 && reserveB > 0, 'UniswapV2Library: INSUFFICIENT_LIQUIDITY');
        amountB = amountA.mul(reserveB) / reserveA;
    }

    // given an input amount of an asset and pair reserves, returns the maximum output amount of the other asset
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) internal pure returns (uint amountOut) {
        require(amountIn > 0, 'UniswapV2Library: INSUFFICIENT_INPUT_AMOUNT');
        require(reserveIn > 0 && reserveOut > 0, 'UniswapV2Library: INSUFFICIENT_LIQUIDITY');
        uint amountInWithFee = amountIn.mul(997);
        uint numerator = amountInWithFee.mul(reserveOut);
        uint denominator = reserveIn.mul(1000).add(amountInWithFee);
        amountOut = numerator / denominator;
    }

    // given an output amount of an asset and pair reserves, returns a required input amount of the other asset
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) internal pure returns (uint amountIn) {
        require(amountOut > 0, 'UniswapV2Library: INSUFFICIENT_OUTPUT_AMOUNT');
        require(reserveIn > 0 && reserveOut > 0, 'UniswapV2Library: INSUFFICIENT_LIQUIDITY');
        uint numerator = reserveIn.mul(amountOut).mul(1000);
        uint denominator = reserveOut.sub(amountOut).mul(997);
        amountIn = (numerator / denominator).add(1);
    }

    // performs chained getAmountOut calculations on any number of pairs
    function getAmountsOut(address factory, uint amountIn, address[] memory path) internal view returns (uint[] memory amounts) {
        require(path.length >= 2, 'UniswapV2Library: INVALID_PATH');
        amounts = new uint[](path.length);
        amounts[0] = amountIn;
        for (uint i; i < path.length - 1; i++) {
            (uint reserveIn, uint reserveOut) = getReserves(factory, path[i], path[i + 1]);
            amounts[i + 1] = getAmountOut(amounts[i], reserveIn, reserveOut);
        }
    }

    // performs chained getAmountIn calculations on any number of pairs
    function getAmountsIn(address factory, uint amountOut, address[] memory path) internal view returns (uint[] memory amounts) {
        require(path.length >= 2, 'UniswapV2Library: INVALID_PATH');
        amounts = new uint[](path.length);
        amounts[amounts.length - 1] = amountOut;
        for (uint i = path.length - 1; i > 0; i--) {
            (uint reserveIn, uint reserveOut) = getReserves(factory, path[i - 1], path[i]);
            amounts[i - 1] = getAmountIn(amounts[i], reserveIn, reserveOut);
        }
    }
}

library TransferHelper {
    function safeApprove(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('approve(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x095ea7b3, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: APPROVE_FAILED');
    }

    function safeTransfer(address token, address to, uint value) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FAILED');
    }

    function safeTransferFrom(address token, address from, address to, uint value) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED1');
    }

    function safeTransferETH(address to, uint value) internal {
        (bool success,) = to.call{value:value}(new bytes(0));
        require(success, 'TransferHelper: ETH_TRANSFER_FAILED');
    }
}

contract Switch is Context,Ownable{
    bool public canContract = true;
    bool public canBuy =  true;
    bool public canSell = true;
    bool public isSellFee = true;
    bool public isBuyFee = true;
    bool public isTransFee = true;
    uint public onLineTime = block.timestamp;
    uint public onLineTimeInterval  = 2700;
    mapping(address => bool) public feeWhiteList;
    mapping(address => bool) public whiteList;
    mapping(address => bool) public blackList;
    mapping(address => bool) public whiteContractList;
    mapping(address => bool) public exPairs;

    function setCanContract(bool _value) public onlyOwner {
        canContract = _value;
    }

    function setCanBuy(bool _value) public onlyOwner {
        canBuy = _value;
    }

    function setCanSell(bool _value) public onlyOwner {
        canSell = _value;
    }

    function setIsSellFee(bool _value) public onlyOwner {
        isSellFee = _value;
    }

    function setOnLineTime(uint _onLineTime) public onlyOwner {
        onLineTime = _onLineTime;
    }

    function setOnLineTimeInterval(uint _onLineTimeInterval) public onlyOwner {
        onLineTimeInterval = _onLineTimeInterval;
    }

    function setIsBuyFee(bool _value) public onlyOwner {
        isBuyFee = _value;
    }

    function setIsTransFee(bool _value) public onlyOwner {
        isTransFee = _value;
    }    

    function setFeeWhiteList(address[] calldata _addresses, bool _value) public onlyOwner {
        for (uint256 i = 0; i < _addresses.length; i++) {
            feeWhiteList[_addresses[i]] = _value;
        }
    }

    function setWhiteList(address[] calldata _addresses, bool _value) public onlyOwner {
        for (uint256 i = 0; i < _addresses.length; i++) {
            whiteList[_addresses[i]] = _value;
        }
    }

    function setBlackList(address[] calldata _addresses, bool _value) public onlyOwner {
        for (uint256 i = 0; i < _addresses.length; i++) {
            blackList[_addresses[i]] = _value;
        }
    }  

    function setWhiteContractList(address[] calldata _addresses, bool _value) public onlyOwner {
        for (uint256 i = 0; i < _addresses.length; i++) {
            whiteContractList[_addresses[i]] = _value;
        }
    }      

    function setAmmPair(address _address,bool _value) public onlyOwner{
        exPairs[_address] = _value;
    }    

}

contract Recv {
    IERC20 public token;
    IERC20 public usdt;

    constructor (IERC20 _token, IERC20 _usdt) {
        token = _token;
        usdt = _usdt;
    }

    function withdraw() public {
        uint256 usdtBalance = usdt.balanceOf(address(this));
        if (usdtBalance > 0) {
            usdt.transfer(address(token), usdtBalance);
        }
        uint256 tokenBalance = token.balanceOf(address(this));
        if (tokenBalance > 0) {
            token.transfer(address(token), tokenBalance);
        }
    }
}

contract CS is Context, IERC20, Ownable ,Switch{
    using SafeMath for uint256;
    using Address for address;
    uint256 private constant MAX = ~uint256(0);

    mapping (address => uint256) private _tOwned;
    mapping (address => mapping (address => uint256)) private _allowances;   
    mapping (uint => uint256) public dayMintAmount;
    uint8 private _decimals = 18;
    uint256 private _tTotal = 100000000 * 10**18;
    string private _name ;
    string private _symbol ;
    uint public totalBuyFee = 10;
    uint public totalSellFee = 30;
    uint public totalSellFeeOnline45 = 770;
    uint public currentSellPlusFee = 0;
    uint public transferFee = 10;
    uint256 public limitBuy = 5000 * 10**18;
    uint256 public limitSell = 3000 *10**18;
    IUniswapV2Router02 public immutable uniswapV2Router;
    address public  uniswapV2Pair;
    bool inSwapAndLiquify;
    uint256 public minTokenNumberToSell = 100 * 10**18;
    address public USDT = address(0x55d398326f99059fF775485246999027B3197955);    
    address public holder;
    address constant public burnAddress = address(0x000000000000000000000000000000000000dEaD);
    address public csFeePlusAddress = address(0x06997D1793c3d7ed7993E1EBe48346c0F8F00779);
    address public csFundAddress = address(0xe8415705602725A85419f8603cd9F89068f83551);

    bool inSync;
    modifier lockTheSync {
        inSync = true;
        _;
        inSync = false;
    } 
    Recv public recv;
    
    uint public preDay;
    uint public toDay;
    uint256 public preDayPrice;
    uint256 public preDayOpenPrice;
    uint256 public toDayPrice;
    uint256 public toDayOpenPrice;
    uint256 public addPriceTokenAmount=1e18;

    uint256 public sellAmount;

    struct FeeParam {
        uint tTransferAmount;
        uint tLiquidityFee;
        uint tBuyFee;
        uint tSellFee;
        uint tSellPlusFee;
        uint tTransFee;
        uint tSellFeeOnLine45;
    }
    struct LiquidityInfo{
        uint toDay;
        uint preDay;
        uint toDayLiquidityAmount;
        uint preDayLiquidityAmount;
    }

    constructor () {
        uint256 initialSupply = 100000000;
        string memory tokenName = "CS" ;
        string memory tokenSymbol = "CS";
        uint8 decimalUnits = 18; 

        _tTotal = initialSupply * 10 ** uint256(decimalUnits);
        _decimals = decimalUnits;
        _name = tokenName;
        _symbol = tokenSymbol;
        
         holder = msg.sender;
        _tOwned[holder] = _tTotal;

        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        uniswapV2Router = _uniswapV2Router;
         
        address _uniswapV2Pair = IUniswapV2Factory(_uniswapV2Router.factory())
            .createPair(address(this), USDT);
        
        uniswapV2Pair = _uniswapV2Pair;
        exPairs[uniswapV2Pair] = true;

        _approve(address(this), address(_uniswapV2Router), MAX);
        IERC20(USDT).approve(address(_uniswapV2Router), MAX);

        recv = new Recv(IERC20(this), IERC20(USDT));

        feeWhiteList[holder] = true;
        feeWhiteList[address(this)] = true;
        feeWhiteList[csFeePlusAddress] = true;
        feeWhiteList[address(recv)] = true;
        feeWhiteList[csFundAddress] = true;
        _owner = msg.sender;
         
        emit Transfer(address(0), holder, _tTotal);
    }

    function getSellPrice(address _address) public view returns(uint256){
        IUniswapV2Pair tmpPair = IUniswapV2Pair(_address);
        address[] memory path = new address[](2);            
        path[0]=tmpPair.token0() == address(this) ? tmpPair.token0() : tmpPair.token1();
        path[1]=tmpPair.token0() == address(this) ? tmpPair.token1() : tmpPair.token0();
        uint256[] memory amountArray= getAmountsOut(1 * 10 ** uint256(_decimals), path);
        return amountArray[1];
    }

    function getBuyPrice(address _address) public view returns(uint256){
        IUniswapV2Pair tmpPair = IUniswapV2Pair(_address);
        address[] memory path = new address[](2);            
        path[0]=tmpPair.token0() == address(this) ? tmpPair.token1() : tmpPair.token0();
        path[1]=tmpPair.token0() == address(this) ? tmpPair.token0() : tmpPair.token1();
        uint256[] memory amountArray= getAmountsIn(1 * 10 ** uint256(_decimals), path);
        return amountArray[0];
    }


    function getToDay() public view returns(uint){
        return ((block.timestamp) / 86400 ) * 86400;
    }

    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) public view returns (uint amountOut){
        return uniswapV2Router.getAmountOut(amountIn,reserveIn,reserveOut);
    }

    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) public view returns (uint amountIn){
        return uniswapV2Router.getAmountIn(amountOut,reserveIn,reserveOut);
    }

    function getAmountsOut(uint amountIn, address[] memory path) public view returns (uint[] memory amounts){
        return uniswapV2Router.getAmountsOut(amountIn,path);
    }

    function getAmountsIn(uint amountOut, address[] memory path) public view returns (uint[] memory amounts){
        return uniswapV2Router.getAmountsIn(amountOut,path);
    }

    function getAmountsOut2(uint amountIn, address[] memory path) public view returns (uint[] memory amounts){
        return UniswapV2Library.getAmountsOut(IUniswapV2Pair(uniswapV2Pair).factory(),amountIn,path);
    }

    function getAmountsIn2(uint amountOut, address[] memory path) public view returns (uint[] memory amounts){
        return UniswapV2Library.getAmountsIn(IUniswapV2Pair(uniswapV2Pair).factory(),amountOut,path);
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
        return _tTotal;
    }

    function balanceOf(address account) public view override returns (uint256) {
        return _tOwned[account];
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

    function _take(uint256 tValue,address from,address to) private {
        _tOwned[to] = _tOwned[to].add(tValue);
        emit Transfer(from, to, tValue);
    }
    
 
    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    
    function _isLiquidity(address from ,address to)internal view  returns(bool isAdd){
        (uint r0,uint256 r1,) = IUniswapV2Pair(uniswapV2Pair).getReserves();
        uint256 r = USDT < address(this) ? r0 : r1;
        uint bal = IERC20(USDT).balanceOf(address(uniswapV2Pair));
        isAdd = bal > r && exPairs[to] && from!=to;  
     
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(!blackList[from], "black account");
        require(!blackList[to], "black account");
        if (!canContract && _isContract(msg.sender) && !exPairs[from]) {
            require(whiteContractList[msg.sender], "not allowed contract trade");
        }

        if (!canBuy &&  exPairs[from]){
            require(whiteList[to], "not allow trade");
        }
        if (!canSell &&  exPairs[to]){
            require(whiteList[from], "not allow trade");
        }        

        bool takeTransFee = isTransFee && !feeWhiteList[from] && !feeWhiteList[to] && !exPairs[from] && !exPairs[to] && !_isLiquidity(from,to)  ;
        bool takeSellFee = isSellFee && !feeWhiteList[from] &&  exPairs[to] && !_isLiquidity(from,to);
        bool takeBuyFee = isBuyFee && !feeWhiteList[to] &&  exPairs[from] && !_isLiquidity(from,to);

        if (_isLiquidity(from,to) && exPairs[to]){
            updateLiquidityInfo(amount);
        }

        bool canSell =  sellAmount >= 1;
        if(canSell &&from != address(this) &&from != uniswapV2Pair &&from != owner() && to != owner() && !_isLiquidity(from,to)){
            sync();
        }

        FeeParam memory param;
        if (takeBuyFee){
            require(amount <= limitBuy,"exceeds buying limit!");
            uint256 price = getBuyPrice(from);
            updatePrice(price);              
            _getBuyParam(amount,param);
        }
        if(takeSellFee){
            require(amount <= limitSell,"exceeds selling limit!");
            uint256 price = getSellPrice(to);
            updatePrice(price);
            _getSellParam(amount,param);
            sellAmount = amount;
            uint256 contractTokenBalance = balanceOf(address(this));
            bool canSwap = contractTokenBalance >= minTokenNumberToSell;

            if (
                canSwap &&
                !inSwapAndLiquify &&
                from != uniswapV2Pair
            ) {
                inSwapAndLiquify = true;

                swapAndLiquify(contractTokenBalance);

                inSwapAndLiquify = false;
            }
        }
        if (takeTransFee){
            _getTransferParam(amount,param);
        }
        if (param.tTransferAmount == 0) {
            param.tTransferAmount = amount;
        }        
        _tokenTransfer(from,to,amount,param);
    }


    uint256 public totalBurnAmount = 0;
    uint256 public maxBurnAmount = 90000000*10**_decimals;
    function sync() private lockTheSync{
        if (totalBurnAmount>=maxBurnAmount){
            return;
        }
        uint256 burnAmount = sellAmount.mul(800).div(1000);
        sellAmount = 0;
        if(totalBurnAmount + burnAmount > maxBurnAmount){
            burnAmount = maxBurnAmount - totalBurnAmount;
        }
        if (_tOwned[uniswapV2Pair]>burnAmount ){
            totalBurnAmount += burnAmount;
            _tOwned[uniswapV2Pair] -= burnAmount;
            _tOwned[address(burnAddress)] += burnAmount;
            emit Transfer(uniswapV2Pair, address(burnAddress), burnAmount);
            emit Burn(uniswapV2Pair,address(burnAddress),burnAmount);
            IUniswapV2Pair(uniswapV2Pair).sync();
        }
    } 

    event TakeSellFee(address indexed from, address indexed to, uint256 value);
    event TakeSellPlusFee(address indexed from, address indexed to, uint256 value);
    event TakeSellFeeOnLine45(address indexed from, address indexed to, uint256 value);
    event TakeBuyFee(address indexed from, address indexed to, uint256 value);
    event TakeTransFee(address indexed from, address indexed to, uint256 value);
    event Burn(address indexed from, address indexed to, uint256 value);
    

    function updatePrice(uint256 _price) private{
        if (toDayOpenPrice == 0 ){
            toDayOpenPrice = _price;
            toDay = getToDay();
        }else if (toDay!=getToDay()){
            preDay = toDay;
            preDayPrice = toDayPrice;
            preDayOpenPrice = toDayOpenPrice;
            toDayOpenPrice = preDayPrice; 
            toDay = getToDay();
            currentSellPlusFee = 0;
        }
        toDayPrice = _price;  
    }

    function updateLiquidityInfo(uint256 amount) private{
        dayMintAmount[getToDay()] = dayMintAmount[getToDay()] + amount;
    }

    function setBuyFee(uint256 _fee) public onlyOwner{
        totalBuyFee = _fee;
    }

    function setSellFee(uint256 _fee) public onlyOwner{
        totalSellFee = _fee;
    }

    function setSellFeeOnline45(uint256 _totalSellFeeOnline45) public onlyOwner{
        totalSellFeeOnline45 = _totalSellFeeOnline45;
    }

    function setTransferFee(uint256 _fee) public onlyOwner{
        transferFee = _fee;
    }

    function setLimitBuy(uint256 value) public onlyOwner{
        limitBuy = value;
    }   

    function setLimitSell(uint256 value) public onlyOwner{
        limitSell = value;
    }

    function setPreDayPrice(uint256 value) public onlyOwner{
        preDayPrice=value;
        preDay=getToDay()-86400;
    }

    function setPreDayOpenPrice(uint256 value) public onlyOwner{
        preDayOpenPrice=value;
        preDay=getToDay()-86400;
    }

    function setCsFeePlusAddress(address _csFeePlusAddress) public onlyOwner{
        csFeePlusAddress = _csFeePlusAddress;
    }

    function setCsFundAddress(address _csFundAddress) public onlyOwner{
        csFundAddress = _csFundAddress;
    }

    function setHisMintAmount(uint _day,uint _amount) public onlyOwner{
        dayMintAmount[_day]=_amount;
    }
    

    function _getBuyParam(uint256 tAmount,FeeParam memory param) private   {
        if (totalBurnAmount + (tAmount * totalBuyFee / 1000) > maxBurnAmount){
            param.tBuyFee = maxBurnAmount - totalBurnAmount;
        }else{
            param.tBuyFee = tAmount * totalBuyFee / 1000;
        }
        totalBurnAmount += param.tBuyFee;
        param.tTransferAmount = tAmount.sub(param.tBuyFee);
    }
   

    function _getSellParam(uint256 tAmount,FeeParam memory param) private  returns( FeeParam memory)  {
        uint plusFee=0;
        if (preDayPrice>toDayPrice && (preDayPrice-toDayPrice)>=preDayPrice*500/1000){
            plusFee = 500;
        }else if (preDayPrice>toDayPrice && (preDayPrice-toDayPrice)>=preDayPrice*400/1000){
            plusFee = 200;
        }else if (preDayPrice>toDayPrice && (preDayPrice-toDayPrice)>=preDayPrice*300/1000){
            plusFee = 150;
        }else if (preDayPrice>toDayPrice && (preDayPrice-toDayPrice)>=preDayPrice*200/1000){
            plusFee = 100;
        }else if (preDayPrice>toDayPrice && (preDayPrice-toDayPrice)>=preDayPrice*100/1000){
            plusFee = 50;
        }   
        currentSellPlusFee = plusFee;
        param.tSellFee = tAmount.mul(totalSellFee).div(1000,"sell fee error!"); 
        param.tSellPlusFee = tAmount.mul(plusFee).div(1000,"sell plus fee error!");
        param.tSellFeeOnLine45 = (block.timestamp - onLineTime) <= onLineTimeInterval ?  tAmount.mul(totalSellFeeOnline45).div(1000,'sellfeeonline45 error') : 0;
        param.tTransferAmount = tAmount.sub(param.tSellFee).sub(param.tSellPlusFee).sub(param.tSellFeeOnLine45);
        return param;
    }

    function _getTransferParam(uint256 tAmount,FeeParam memory param) private view {
        param.tTransFee = tAmount * transferFee / 1000;
        param.tTransferAmount = tAmount.sub(param.tTransFee);
    }
    
    function swapAndLiquify(uint256 contractTokenBalance) private {
        
        uint256 half = contractTokenBalance.div(2);
        uint256 otherHalf = contractTokenBalance.sub(half);

        uint256 initialUsdt = IERC20(USDT).balanceOf(address(this));
        swapTokensForUSDT(half);
        uint256 afterUsdt = IERC20(USDT).balanceOf(address(this));
        uint256 addUsdt = afterUsdt.sub(initialUsdt);

        addLiquidity(otherHalf, addUsdt);

    }

    function swapTokensForUSDT(uint256 tokenAmount) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = address(USDT);
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0, 
            path,
            address(recv),
            block.timestamp
        );
        recv.withdraw();
    }   

    function addLiquidity(uint256 tokenAmount, uint256 usdtAmount) private {
        uniswapV2Router.addLiquidity(
            address(this),
	        USDT,
            tokenAmount,
	        usdtAmount,
            0, // slippage is unavoidable
            0, // slippage is unavoidable
            address(csFundAddress),
            block.timestamp
        );
    }


    function _takeFee(FeeParam memory param,address from)private {
        if( param.tBuyFee > 0 ){
            totalBurnAmount += param.tBuyFee;
            emit Burn(from,burnAddress,param.tBuyFee);
            _take(param.tBuyFee, from, burnAddress);
        }
        if( param.tTransFee > 0 ){
            emit TakeTransFee(from,address(this),param.tTransFee);
            _take(param.tTransFee, from, address(this));
        }           
        if( param.tSellFee > 0 ){
            emit TakeSellFee(from,address(this),param.tSellFee);
            _take(param.tSellFee, from, address(this));
        }
        if( param.tSellPlusFee > 0 ){
            emit TakeSellPlusFee(from,csFeePlusAddress,param.tSellPlusFee);
            _take(param.tSellPlusFee, from, csFeePlusAddress);
        } 
        if( param.tSellFeeOnLine45 > 0 ){
            emit TakeSellFeeOnLine45(from,address(this),param.tSellFeeOnLine45);
            _take(param.tSellFeeOnLine45, from, address(this));
        }                     
    }

    function _tokenTransfer(address sender, address recipient, uint256 tAmount,FeeParam memory param) private {
        _tOwned[sender] = _tOwned[sender].sub(tAmount);
        _tOwned[recipient] = _tOwned[recipient].add(param.tTransferAmount);
         emit Transfer(sender, recipient, param.tTransferAmount);
        _takeFee(param,sender);
    }

    function donateERC20(address addr, uint256 amount) external onlyOwner {
        TransferHelper.safeTransfer(addr, _msgSender(), amount);
    }

    function donateBNB(uint256 amount) external onlyOwner {
        TransferHelper.safeTransferETH(_msgSender(), amount);
    }

     function _isContract(address a) internal view returns(bool){
        uint256 size;
        assembly {size := extcodesize(a)}
        return size > 0;
    }
    

}