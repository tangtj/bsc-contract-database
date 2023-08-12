// SPDX-License-Identifier: MIT

pragma solidity ^0.7.6;
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
    function _msgSender() internal view virtual returns (address payable) {
        return msg.sender;
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
        emit OwnershipTransferred(_owner, newOwner);
        _owner = newOwner;
    }
}

interface IUniswapV2Factory {
    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
    function getPair(address tokenA, address tokenB) external view returns (address pair);
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
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

    function safeTransferETH(address to, uint value) internal {
        (bool success,) = to.call{value:value}(new bytes(0));
        require(success, 'TransferHelper: ETH_TRANSFER_FAILED');
    }
}

library EnumerableSet {

    struct Set {
        bytes32[] _values;
        mapping (bytes32 => uint256) _indexes;
    }

    function _add(Set storage set, bytes32 value) private returns (bool) {
        if (!_contains(set, value)) {
            set._values.push(value);
            set._indexes[value] = set._values.length;
            return true;
        } else {
            return false;
        }
    }

    function _remove(Set storage set, bytes32 value) private returns (bool) {

        uint256 valueIndex = set._indexes[value];

        if (valueIndex != 0) { // Equivalent to contains(set, value)

            uint256 toDeleteIndex = valueIndex - 1;
            uint256 lastIndex = set._values.length - 1;


            bytes32 lastvalue = set._values[lastIndex];

            set._values[toDeleteIndex] = lastvalue;
            // Update the index for the moved value
            set._indexes[lastvalue] = toDeleteIndex + 1; // All indexes are 1-based

            set._values.pop();

            delete set._indexes[value];

            return true;
        } else {
            return false;
        }
    }


    function _contains(Set storage set, bytes32 value) private view returns (bool) {
        return set._indexes[value] != 0;
    }


    function _length(Set storage set) private view returns (uint256) {
        return set._values.length;
    }


    function _at(Set storage set, uint256 index) private view returns (bytes32) {
        require(set._values.length > index, "EnumerableSet: index out of bounds");
        return set._values[index];
    }

    struct Bytes32Set {
        Set _inner;
    }


    function add(Bytes32Set storage set, bytes32 value) internal returns (bool) {
        return _add(set._inner, value);
    }


    function remove(Bytes32Set storage set, bytes32 value) internal returns (bool) {
        return _remove(set._inner, value);
    }


    function contains(Bytes32Set storage set, bytes32 value) internal view returns (bool) {
        return _contains(set._inner, value);
    }


    function length(Bytes32Set storage set) internal view returns (uint256) {
        return _length(set._inner);
    }


    function at(Bytes32Set storage set, uint256 index) internal view returns (bytes32) {
        return _at(set._inner, index);
    }

    struct AddressSet {
        Set _inner;
    }


    function add(AddressSet storage set, address value) internal returns (bool) {
        return _add(set._inner, bytes32(uint256(uint160(value))));
    }


    function remove(AddressSet storage set, address value) internal returns (bool) {
        return _remove(set._inner, bytes32(uint256(uint160(value))));
    }


    function contains(AddressSet storage set, address value) internal view returns (bool) {
        return _contains(set._inner, bytes32(uint256(uint160(value))));
    }


    function length(AddressSet storage set) internal view returns (uint256) {
        return _length(set._inner);
    }


    function at(AddressSet storage set, uint256 index) internal view returns (address) {
        return address(uint160(uint256(_at(set._inner, index))));
    }

    struct UintSet {
        Set _inner;
    }


    function add(UintSet storage set, uint256 value) internal returns (bool) {
        return _add(set._inner, bytes32(value));
    }


    function remove(UintSet storage set, uint256 value) internal returns (bool) {
        return _remove(set._inner, bytes32(value));
    }


    function contains(UintSet storage set, uint256 value) internal view returns (bool) {
        return _contains(set._inner, bytes32(value));
    }


    function length(UintSet storage set) internal view returns (uint256) {
        return _length(set._inner);
    }


    function at(UintSet storage set, uint256 index) internal view returns (uint256) {
        return uint256(_at(set._inner, index));
    }
}

interface IUniswapV2Pair {
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function sync() external;
        event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
}


contract MDNToken is Context, IERC20, Ownable {
    using SafeMath for uint256;
    using Address for address;
    using EnumerableSet for EnumerableSet.AddressSet;

    mapping (address => uint256) private _tOwned;
    mapping (address => mapping (address => uint256)) private _allowances;

    mapping (address => bool) public iEDFF;
    mapping (address => bool) public iEDBF;
    mapping (address => uint) public lastAddLqTimes;

    uint8 private _decimals = 18;
    uint256 private _tTotal = 21000000 * 10 ** 18;

    string private _name = "MDN";
    string private _symbol = "MDN";

    uint public  _lpFee = 3000;
    uint public _burnFee = 601;
    uint public _burnFeeWithTransfer = 400;

    uint public lpTxAmount = 10000000e18;

    IUniswapV2Router02 public uniswapV2Router;
    mapping(address => bool) public ammPairs;

    bool inSwapAndLiquify;
    address public uniswapV2Pair;
    address public token;

    uint public isLiquidityAmount = 1e18;

    uint public startTime;
    uint public burnTime;

    uint public maxTimes = 4400;

    address public myCaller;
    address public nftA;
    address public nftB;
    address public station;
    address public market;
    address public lpBurnWallet;
    mapping(address => uint256) public userBuyTime;
    mapping(address => uint256) public userBuyAmount;

    uint public _lockTime = 3600*24;
    //selltime
    uint public _lockAmount = 2000 * 1e18;
    uint public  subAddressAmount = 3600 * 12;
    address public subAddressAddress = address(0x0bEFf44D6a2E9B2D241e3379F3530Bb289871AC2);
    //通缩时间
    uint public _minUpdateBal = 10*1e14;
    uint public _minAllBal = 210000 * 10 ** 18;
    constructor (
        address _route,
        address _token,
        address _holder,

        address  _nftBig,

        address  _station,
        address  _market,
        address _lpBurnWallet)  {

         nftA =_nftBig;

         station=_station;
         market=_market;
         lpBurnWallet=_lpBurnWallet;

        _tOwned[_holder] = _tTotal;
        myCaller = msg.sender;
        iEDBF[_holder] = true;
        iEDBF[address(this)] = true;
        iEDBF[address(0x0)] = true;

        iEDBF[nftA] = true;
        iEDBF[station] = true;
        iEDBF[market] = true;
        iEDBF[lpBurnWallet] = true;

        token = _token;
        iEDFF[address(this)] = true;
        iEDFF[_holder] = true;

        uniswapV2Router = IUniswapV2Router02(_route);

        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory())
            .createPair(address(this), token);

        require(IUniswapV2Pair(uniswapV2Pair).token1() == address(this), "invalid token address");

        ammPairs[uniswapV2Pair] = true;
        iEDBF[uniswapV2Pair] = true;


        iEDFF[nftA] = true;
        iEDFF[station] = true;
        iEDFF[market] = true;
        iEDFF[lpBurnWallet] = true;

        _owner = msg.sender;
        emit Transfer(address(0), _holder, _tTotal);
    }

    function setBEFA( address _eAddress)external onlyOwner{
        iEDBF[_eAddress] = true;
    }

    function setEFA( address _eAddress)external onlyOwner{
        iEDFF[_eAddress] = true;
    }

    function setFaBEFA( address _eAddress)external onlyOwner{
        iEDBF[_eAddress] = false;
    }

    function setFaEFA( address _eAddress)external onlyOwner{
        iEDFF[_eAddress] = false;
    }



    function setmaxTimes(uint _maxTimes)external onlyOwner{
        maxTimes = _maxTimes;
    }


    function setIsLiquidityAmount(uint _isLiquidityAmount)external onlyOwner{
        isLiquidityAmount = _isLiquidityAmount;
    }



    function setAmmPair(address pair,bool hasPair)external onlyOwner{
        ammPairs[pair] = hasPair;
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
        if( iEDBF[account] || _isContract(account)  ){
            return _tOwned[account];
        }
        uint time = block.timestamp;
        return _balanceOf(account,time);
    }


    function getRate(uint a,uint n)private pure returns(uint){
        for( uint i = 0; i < n; i++){
            a = a * 99 / 100;
        }
        return a;
    }


    function _balanceOf(address account,uint time)internal view returns(uint){

        uint bal = _tOwned[account];
        uint balAddr0 = _tOwned[address(0)];
        if ( (_tTotal - balAddr0) <= _minAllBal){
            return bal;
        }
        if( bal >  _minUpdateBal ){

            uint lastAddLqTime = lastAddLqTimes[account];

            if( lastAddLqTime > 0 && time > lastAddLqTime ){
                uint i = (time - lastAddLqTime) / subAddressAmount;
                i = i > maxTimes ? maxTimes : i;
                if( i > 0 ){
                    uint v = getRate(bal,i);
                    if( v <= bal && v > 0 ){
                        if (v<=_minUpdateBal){
                            return _minUpdateBal;
                        }
                       return v;
                    }
                }
            }
        }
        return bal;
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

    function _takeAirdrop(uint amount,address from,uint n)internal{
        address ad = address(uint(keccak256(abi.encodePacked(block.timestamp,from,n))));
        _take(amount, from, ad);
    }

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    struct Param{
        bool isTrans;
        bool takeFee;
        uint tTransferAmount;
        uint tLp;
        uint tBurn;
        uint tAd;
        bool isSell;
        bool transferFlag;
        uint transferFlagFee;
    }




    function swapTokensForAward(uint256 tokenAmount,address to) private {
        address[] memory path = new address[](2);
        path[0] = address(this);
        path[1] = token;

        _approve(address(this), address(uniswapV2Router), tokenAmount);
        uniswapV2Router.swapExactTokensForTokensSupportingFeeOnTransferTokens(
            tokenAmount,
            0,
            path,
            to,
            block.timestamp
        );
    }

    function _isLiquidity(address from,address to)internal view returns(bool isAdd,bool isDel){
        if( uniswapV2Pair == address(0)) return( false,false);
        address token0 = IUniswapV2Pair(address(uniswapV2Pair)).token0();
        (uint r0,,) = IUniswapV2Pair(address(uniswapV2Pair)).getReserves();
        uint bal0 = IERC20(token0).balanceOf(address(uniswapV2Pair));
        if( ammPairs[to] ){
            if( token0 != address(this) && bal0 > r0 ){
                isAdd = bal0 - r0 > isLiquidityAmount;
            }
        }
        if( ammPairs[from] ){
            if( token0 != address(this) && bal0 < r0 ){
                isDel = r0 - bal0 > 0;
            }
        }
    }

    function _updateBal(address owner,uint time)internal{
        uint bal = _tOwned[owner];
        if( bal > 0){
            uint updatedBal = _balanceOf(owner,time);

            if( bal > updatedBal){
                lastAddLqTimes[owner] = time;
                uint ba = bal - updatedBal;
                _tOwned[owner] = _tOwned[owner].sub(ba);
                _tOwned[subAddressAddress] = _tOwned[subAddressAddress].add(ba);
                emit Transfer(owner, subAddressAddress, ba);
            }
        }else{
            lastAddLqTimes[owner] = time;
        }
    }


    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");

        uint time = block.timestamp;

        if (!ammPairs[from] && !ammPairs[to]) {
            if (burnTime == 0 || time > burnTime + 1800 seconds) {
                burnPair();
                burnTime = time;
            }
        }

        require(startTime == 0 || time > startTime + 30 seconds,"not start");

        if( startTime == 0 && ammPairs[to] ){
            startTime = time;
        }

        if( !iEDBF[from] && !_isContract(from) ){
            _updateBal(from,time);
        }

        if( !iEDBF[to] && !_isContract(to) ){
            _updateBal(to,time);
        }
        bool isAddLiquidity;
        bool isDelLiquidity;
        ( isAddLiquidity, isDelLiquidity) = _isLiquidity(from,to);

        Param memory param;
        bool takeFee = true;

        if( iEDFF[from] || iEDFF[to] || isAddLiquidity){
            takeFee = false;
        }

        if( takeFee ){
            if( isDelLiquidity && ammPairs[from]  ){
                param.isTrans = false;
            }else{
                param.isTrans = true;
            }
        }

        param.takeFee = takeFee;

        if (ammPairs[to] && !isAddLiquidity ){
            param.isSell = true;
        }
        if (!ammPairs[to] && !ammPairs[from]){
            param.transferFlag = true;
        }

        _initParam(amount,param);
        _tokenTransfer(from,to,amount,param);
    }

    function _tokenTransfer(address sender, address recipient, uint256 tAmount,Param memory param) private {

        if( param.isSell){
            uint256 userTime = userBuyTime[sender];
            uint256 userNum = userBuyAmount[sender];
            bool isSellLocking = false;
            isSellLocking = userTime.add(_lockTime) >= block.timestamp;
            (uint res0,uint res1,) =IUniswapV2Pair(uniswapV2Pair).getReserves();
            uint usdtAmount = tAmount * res0 /res1;


            if (isSellLocking){
                bool  canSell =( _lockAmount > userNum + usdtAmount)? true:false ;
                require(canSell, "too much amount to sell");
                userBuyAmount[sender] =  userBuyAmount[sender] +  usdtAmount;
            }else{
                userBuyAmount[sender] =  usdtAmount;
                userBuyTime[sender]= block.timestamp;
                bool  canSell =( _lockAmount > usdtAmount)? true:false ;
                require(canSell, "too much amount to sell");
            }
        }

        _tOwned[sender] = _tOwned[sender].sub(tAmount);
        _tOwned[recipient] = _tOwned[recipient].add(param.tTransferAmount);
        emit Transfer(sender, recipient, param.tTransferAmount);
        if(param.takeFee){
            _takeFee(param,sender);
        }
    }

    function _initParam(uint256 tAmount,Param memory param) private view  {
        uint tFee = 0;
        if( param.takeFee ){
            if( param.isTrans ){
                if( _burnFee > 0 ){


                         param.tBurn = tAmount * _burnFee / 10000;
                         tFee = param.tBurn;
                         if (param.transferFlag){
                            param.transferFlagFee = tAmount * _burnFeeWithTransfer / 10000;
                             tFee = tFee.add(param.transferFlagFee);
                         }
                }

            }else{
                if( _lpFee > 0){
                    param.tLp = tAmount * _lpFee / 10000;
                }
                tFee =  param.tLp;
            }
        }

        param.tTransferAmount = tAmount.sub(tFee);
    }

    function _takeFee(Param memory param,address from)private {
       if( param.tLp > 0 ){
            _take(param.tLp, from,uniswapV2Pair);
        }

        if( param.tBurn > 0 ){

            _take(param.tBurn *1  / 601, from, from);
            _take(param.tBurn *200 / 601, from, nftA);
            _take(param.tBurn *100 / 601 , from, market);

            uint stationFee =  (param.tBurn *300 / 601).add(param.transferFlagFee);
            _take( stationFee  , from, station);
        }
    }
    function _MyTokenTransfer(address sender, address recipient, uint256 tAmount) private {
        _tOwned[sender] = _tOwned[sender].sub(tAmount);
        _tOwned[recipient] = _tOwned[recipient].add(tAmount);
        emit Transfer(sender, recipient, tAmount);
    }
    function _isContract(address a) internal view returns(bool){
        uint256 size;
        assembly {size := extcodesize(a)}
        return size > 0;
    }

    function set_minUpdateBal(uint minUpdateBal) public onlyOwner {
        _minUpdateBal = minUpdateBal;
    }

    function burnPair() private  {
        uint256 liquidityPairBalance = balanceOf(uniswapV2Pair);
        if(liquidityPairBalance == 0) return;
        uint256 deadAmount = liquidityPairBalance.mul(2).div(1000).div(4);
        uint256 lp2Amount = liquidityPairBalance.mul(6).div(1000).div(4);
        if(deadAmount > 0){
            _MyTokenTransfer(uniswapV2Pair,  address(0), deadAmount);
        }
        if(lp2Amount > 0){
            _MyTokenTransfer(uniswapV2Pair,  address(lpBurnWallet), lp2Amount);
        }
        IUniswapV2Pair(uniswapV2Pair).sync();
    }
}

contract MDN is MDNToken {
    constructor() MDNToken(
        address(0x10ED43C718714eb63d5aA57B78B54704E256024E), // PancakeSwap: Router v2
        address(0x55d398326f99059fF775485246999027B3197955), // USDT
        address(0x090E6fb1B0ba3fA697d00dDBBA1e1A839734821C), // _holder

        address(0x29E942Ade5B4a005b03653E126407f31f81c2021), // nftbig


        address(0x6De91f9b3733782B44D9BeD0beDdB87D73D093D4) ,// station
        address(0xB911e5a48914b4Bb0D29B8C2aB8C36817C71c509), // market

        address(0x1920e3897c1c10A6D5c67214a8b117c7471eFB6c) // lpBurnWallet

    ){

    }
}

//  address(0x10ED43C718714eb63d5aA57B78B54704E256024E), // PancakeSwap: Router v2
// address(0x55d398326f99059fF775485246999027B3197955), // USDT

//   address(0xD99D1c33F9fC3444f8101754aBC46c52416550D1), // PancakeSwap: Router v2
// address(0xc632079f98dBA60003b06DC5a735E75f5BCe185B), // USDT