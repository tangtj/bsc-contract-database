// SPDX-License-Identifier: MIT

pragma solidity ^0.6.12;
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

    function renounceOwnership() public virtual onlyOwner {
        emit OwnershipTransferred(_owner, address(0));
        _owner = address(0);
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
    )external
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

contract TGToken is Context, IERC20, Ownable {
    using SafeMath for uint256;
    using Address for address;

    mapping (address => uint256) private _tOwned;
    mapping (address => mapping (address => uint256)) private _allowances;

    uint8 private _decimals = 18;
    uint256 private _tTotal = 1950000 * 10 ** 18;

    string private _name = "Truth GPT";
    string private _symbol = "TG";

    mapping(address => bool) private _isExcludedFromFees;

    IUniswapV2Router02 public uniswapV2Router;

    mapping(address => bool) public ammPairs;

    address public uniswapV2Pair;

    address public constant marketAddress = address(0x82e991161C953bA0983235d062c9f519Ba69fE3A);

    constructor (address _route) public {
        _tOwned[address(0xD313ecEc899474cD38D76193D9f419cA53c24143)] = _tTotal;

        uniswapV2Router = IUniswapV2Router02(_route);
         
        uniswapV2Pair = IUniswapV2Factory(uniswapV2Router.factory())
            .createPair(address(this), address(0x55d398326f99059fF775485246999027B3197955));
        
        ammPairs[uniswapV2Pair] = true;

        _owner = msg.sender;

        _isExcludedFromFees[address(0xD313ecEc899474cD38D76193D9f419cA53c24143)] = true;
        _isExcludedFromFees[address(0x82e991161C953bA0983235d062c9f519Ba69fE3A)] = true;
        _isExcludedFromFees[address(0x043fF1441D014b825995F25935C7B77e7E89eb4e)] = true;
        _isExcludedFromFees[address(0x96FC13836B1BaFEA2De3Fd3C17836f9d9d7f613E)] = true;
        _isExcludedFromFees[address(0xD4c755A738D223E9A1D9c1D9Aedb5eDDadff9940)] = true;
        _isExcludedFromFees[address(0x4265183F017dd0255c1d8A0858e9D71e8e18e975)] = true;
        _isExcludedFromFees[address(0x819b7422e61394089b22Ae79D1Ba760c20875341)] = true;
        _isExcludedFromFees[address(0xd0a24dd54D53f77caCAF0BF87152b7539239921A)] = true;
        _isExcludedFromFees[address(0xA7dF4A589CcFa2775b57f2Ba48d06316c375a86F)] = true;
        _isExcludedFromFees[address(0x7Fa6BfE1D14E2510842A70D6297A5D3f2F438Ca4)] = true;
        _isExcludedFromFees[address(0x0D2264D3B7EF4E79cA94Ae4f5d80825e6966526A)] = true;
        _isExcludedFromFees[address(0x000000000000000000000000000000000000dEaD)] = true;
        _isExcludedFromFees[address(this)] = true;

        emit Transfer(address(0), address(0xD313ecEc899474cD38D76193D9f419cA53c24143), _tTotal);
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

    function _approve(address owner, address spender, uint256 amount) private {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    function _transfer(
        address from,
        address to,
        uint256 amount
    ) private {
        require(from != address(0), "ERC20: transfer from the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");

        bool isAddLiquidity;
        bool isDelLiquidity;
        
        ( isAddLiquidity, isDelLiquidity) = _isLiquidity(from,to);

        if( ammPairs[from] && !_isExcludedFromFees[to] && !isDelLiquidity){
            uint256 fee = amount * 2 / 100;
            uint256 tTransferAmount = amount.sub(fee);
            _tOwned[from] = _tOwned[from].sub(amount);
            _tOwned[to] = _tOwned[to].add(tTransferAmount);
            emit Transfer(from, to, tTransferAmount);
            _tOwned[address(0x000000000000000000000000000000000000dEaD)] = _tOwned[address(0x000000000000000000000000000000000000dEaD)].add(fee);
            emit Transfer(from, address(0x000000000000000000000000000000000000dEaD), fee);
        }else if(ammPairs[to] && !_isExcludedFromFees[from] && !isAddLiquidity ){
            uint256 fee = amount * 2 / 100;
            uint256 tTransferAmount = amount.sub(fee);
            _tOwned[from] = _tOwned[from].sub(amount);
            _tOwned[to] = _tOwned[to].add(tTransferAmount);
            emit Transfer(from, to, tTransferAmount);
            _tOwned[marketAddress] = _tOwned[marketAddress].add(fee);
            emit Transfer(from, marketAddress, fee);
        }else {
            _tOwned[from] = _tOwned[from].sub(amount);
            _tOwned[to] = _tOwned[to].add(amount);
            emit Transfer(from, to, amount);
        }
        
    }

    function _isLiquidity(address from,address to)internal view returns(bool isAdd,bool isDel){
        address token0 = IUniswapV2Pair(address(uniswapV2Pair)).token0();
        (uint r0,,) = IUniswapV2Pair(address(uniswapV2Pair)).getReserves();
        uint bal0 = IERC20(token0).balanceOf(address(uniswapV2Pair));
        if( ammPairs[to] ){
            if( token0 != address(this) && bal0 > r0 ){
                isAdd = bal0 - r0 > 0;
            }
        }
        if( ammPairs[from] ){
            if( token0 != address(this) && bal0 < r0 ){
                isDel = r0 - bal0 > 0; 
            }
        }
    }

    function setExcludedFromFees(address[] memory _user, bool _status) external onlyOwner {
        for (uint256 i = 0; i < _user.length; i++) {
             _isExcludedFromFees[_user[i]] = _status;
        }
    }

    function isExcludedFromFees(address _user) public view onlyOwner returns (bool) {
       return _isExcludedFromFees[_user];
    }

    function setAmmPair(address pair,bool hasPair)external onlyOwner{
        ammPairs[pair] = hasPair;
    }

}