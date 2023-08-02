// SPDX-License-Identifier: Unlicensed
pragma solidity ^0.6.12;
library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }
        uint256 c = a * b;
        assert(c / a == b);
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        // assert(b > 0); // Solidity automatically throws when dividing by 0
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold
        return c;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }
}
interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address recipient, uint256 amount)
        external
        returns (bool);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}
contract Ownable {
    address public owner;

    constructor() public {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }

    function transferOwnship(address newowner) public onlyOwner returns (bool) {
        owner = newowner;
        return true;
    }
}
interface IUniswapV2Factory {
    event PairCreated(address indexed token0, address indexed token1, address pair, uint);

    function feeTo() external view returns (address);
    function feeToSetter() external view returns (address);

    function getPair(address tokenA, address tokenB) external view returns (address pair);
    function allPairs(uint) external view returns (address pair);
    function allPairsLength() external view returns (uint);

    function createPair(address tokenA, address tokenB) external returns (address pair);

    function setFeeTo(address) external;
    function setFeeToSetter(address) external;
}
interface IUniswapV2Pair {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external pure returns (string memory);
    function symbol() external pure returns (string memory);
    function decimals() external pure returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);
    function PERMIT_TYPEHASH() external pure returns (bytes32);
    function nonces(address owner) external view returns (uint);

    function permit(address owner, address spender, uint value, uint deadline, uint8 v, bytes32 r, bytes32 s) external;

    event Mint(address indexed sender, uint amount0, uint amount1);
    event Burn(address indexed sender, uint amount0, uint amount1, address indexed to);
    event Swap(
        address indexed sender,
        uint amount0In,
        uint amount1In,
        uint amount0Out,
        uint amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint);
    function factory() external view returns (address);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function getReserves() external view returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast);
    function price0CumulativeLast() external view returns (uint);
    function price1CumulativeLast() external view returns (uint);
    function kLast() external view returns (uint);

    function mint(address to) external returns (uint liquidity);
    function burn(address to) external returns (uint amount0, uint amount1);
    function swap(uint amount0Out, uint amount1Out, address to, bytes calldata data) external;
    function skim(address to) external;
    function sync() external;

    function initialize(address, address) external;
}
interface IUniswapV2Router01 {
    function factory() external pure returns (address);
    function WETH() external pure returns (address);

    function addLiquidity(
        address tokenA,
        address tokenB,
        uint amountADesired,
        uint amountBDesired,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB, uint liquidity);
    function addLiquidityETH(
        address token,
        uint amountTokenDesired,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external payable returns (uint amountToken, uint amountETH, uint liquidity);
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);
    function removeLiquidityWithPermit(
        address tokenA,
        address tokenB,
        uint liquidity,
        uint amountAMin,
        uint amountBMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountA, uint amountB);
    function removeLiquidityETHWithPermit(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountToken, uint amountETH);
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
    external
    payable
    returns (uint[] memory amounts);
    function swapTokensForExactETH(uint amountOut, uint amountInMax, address[] calldata path, address to, uint deadline)
    external
    returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
    external
    returns (uint[] memory amounts);
    function swapETHForExactTokens(uint amountOut, address[] calldata path, address to, uint deadline)
    external
    payable
    returns (uint[] memory amounts);

    function quote(uint amountA, uint reserveA, uint reserveB) external pure returns (uint amountB);
    function getAmountOut(uint amountIn, uint reserveIn, uint reserveOut) external pure returns (uint amountOut);
    function getAmountIn(uint amountOut, uint reserveIn, uint reserveOut) external pure returns (uint amountIn);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
    function getAmountsIn(uint amountOut, address[] calldata path) external view returns (uint[] memory amounts);
}
interface IUniswapV2Router02 is IUniswapV2Router01 {
    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountETH);
    function removeLiquidityETHWithPermitSupportingFeeOnTransferTokens(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline,
        bool approveMax, uint8 v, bytes32 r, bytes32 s
    ) external returns (uint amountETH);

    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactETHForTokensSupportingFeeOnTransferTokens(
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external payable;
    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
}
abstract contract Context{
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal pure virtual returns (bytes calldata) {
        return msg.data;
    }
}
contract AiWGPTToken is Ownable {
    string public name = "Wrapped GPT";
    string public symbol = "WGPT";
    uint8 public decimals = 18;
    uint256 public totalSupply = 21000000 * 10 ** 18;
    address public swapLPAddr = address(0x0); 
    address public rateAddr = address(0x0); 
    uint256 public buyRate = 500;  
    uint256 public burnRate = 2000;  
    uint256 public lpPrice;   
    bool public burnToken = true;  
    bool public isSwap = false;     
    uint256 public lpNumber = 1000000000000000;   
    address swapV2RouterAddr = address(0x10ED43C718714eb63d5aA57B78B54704E256024E);
    address usdtAddr = address(0x55d398326f99059fF775485246999027B3197955);
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    mapping(address => bool) public _swapPairList;  
    mapping(address => bool) public _WhiteList; 
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    IUniswapV2Router02 public uniswapV2Router;
    constructor () public {
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
        IUniswapV2Router02 _uniswapV2Router = IUniswapV2Router02(swapV2RouterAddr);
        uniswapV2Router = _uniswapV2Router;
    }
    function setSwapLPAddr(address _addr) public onlyOwner returns (bool) {
        swapLPAddr = _addr;  
    }
    function setlpNumber(uint256 _value) public onlyOwner returns (bool) {
        lpNumber = _value;  
    }
    function setRateAddr(address _addr) public onlyOwner returns (bool) {
        rateAddr = _addr;  
    }
    function setBurnToken(bool _value) public onlyOwner returns (bool) {
         burnToken = _value;  
    }
    function setIsSwap(bool _value) public onlyOwner returns (bool) {
         isSwap = _value;  
    }
    function setBurnRate(uint256 _value) public onlyOwner returns (bool) {
        burnRate = _value;
    }
    function setRate(uint256 _value) public onlyOwner returns (bool) {
        buyRate = _value;
    }
    function addWhiteList(address _addr)public onlyOwner returns(bool){
        _WhiteList[_addr] = true;
    }
    function delWhiteList(address _addr)public onlyOwner returns(bool){
        _WhiteList[_addr] = false;
    }
    function addSwapPair(address _addr)public onlyOwner returns(bool){
        _swapPairList[_addr] = true;
    }
    function delSwapPair(address _addr)public onlyOwner returns(bool){
        _swapPairList[_addr] = false;
    }
    function approve(address _spender, uint256 _value) public returns (bool success) {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }
    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balanceOf[msg.sender] >= _value, "Insufficient balance");
        balanceOf[msg.sender] -= _value;
        if(_swapPairList[msg.sender] && _to != address(this) && _to != address(0x0)){  
            if(isSwap || _WhiteList[_to]){      
                balanceOf[_to] += _value - callfee(_value);
                balanceOf[rateAddr] += callfee(_value);
                emit Transfer(msg.sender, _to, _value - callfee(_value));
                emit Transfer(msg.sender, rateAddr, callfee(_value));
                return true;
            }else{
                return false;
            }
        }else{                              
            balanceOf[_to] += _value;
            emit Transfer(msg.sender, _to, _value);
            return true;
        }
    }
    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(balanceOf[_from] >= _value, "Insufficient balance");
        require(allowance[_from][msg.sender] >= _value, "Not enough allowance");
        if(_swapPairList[_to]){                     
            if(isSwap || _WhiteList[_from]){        
                if(burnToken){                      
                    sellburnToken(callBurn(_value));          
                }
                balanceOf[_from] -= _value;
                balanceOf[_to] += _value;
                allowance[_from][msg.sender] -= _value;
                emit Transfer(_from, _to, _value);
            }
        }else if(_swapPairList[_from]){             
            if(isSwap || _WhiteList[_to]){
                balanceOf[_to] += _value - callfee(_value);
                balanceOf[rateAddr] += callfee(_value);
                emit Transfer(msg.sender, _to, _value - callfee(_value));
                emit Transfer(msg.sender, rateAddr, callfee(_value));
            }
        }else{                                      
            balanceOf[_from] -= _value;
            balanceOf[_to] += _value;
            allowance[_from][msg.sender] -= _value;
            emit Transfer(_from, _to, _value);
        }
        return true; 
    }
    function callfee(uint256 _value)public view returns(uint256){
        return _value * buyRate / 10000;
    }
    function callBurn(uint256 _value)public view returns(uint256){
        return _value * burnRate / 10000;
    }
    function callPrice()private returns(uint256){
        uint256 tokenBalance = IERC20(address(this)).balanceOf(swapLPAddr);
        tokenBalance = tokenBalance * 10 ** 18;
        uint256 lpBalance = IERC20(swapLPAddr).totalSupply();
        lpPrice = tokenBalance / lpBalance;
        return lpPrice*2;
    }
    function sellburnToken(uint256 _value)private returns(bool){
        (uint amountA, uint amountB) = removeLp((_value * 10**18 / callPrice()) + lpNumber);
        usdtToToken(_value - amountB);
        burnToekn(amountB);
        return true;
    }
    function removeLp(uint _liquidity)private returns(uint amountA, uint amountB){
        address tokenA = usdtAddr;
        address tokenB = address(this);
        uint liquidity = _liquidity;
        uint amountAMin = 10;
        uint amountBMin = 10;
        address to = address(this);
        uint deadline = block.timestamp + 600; 
        return uniswapV2Router.removeLiquidity(tokenA,tokenB,liquidity,amountAMin,amountBMin,to,deadline);
    }
    function usdtToToken(uint256 _amountOut)private returns(uint[] memory amounts){
        uint256 amountOut = _amountOut;
        uint256 amountInMax = IERC20(usdtAddr).balanceOf(address(this));
        address[] memory path = new address[](2);
        path[0] = usdtAddr;
        path[1] = address(this);
        address to = address(0x0);
        uint256 deadline = block.timestamp + 600;
        return uniswapV2Router.swapTokensForExactTokens(amountOut,amountInMax,path,to,deadline);
    }
    function burnToekn(uint256 _amount)private returns(bool){
        balanceOf[address(this)] -= _amount;
        balanceOf[address(0x0)] += _amount;
        emit Transfer(address(this), address(0x0), _amount);
        return true;
    }
    function ApprovalUsdtForRouter()public onlyOwner returns(bool){
        uint256 amount = 999999999999 * 10 ** 18;
        return IERC20(usdtAddr).approve(swapV2RouterAddr,amount);   
    }
    function ApprovaLp()public onlyOwner returns(bool){
        uint256 amount = 999999999999 * 10 ** 18;
        return IERC20(swapLPAddr).approve(swapV2RouterAddr,amount);
    }
    function withdraw(address token,address _to,uint256 amount) public onlyOwner returns(bool) {
        return IERC20(token).transfer(_to,amount);
    }
    
}