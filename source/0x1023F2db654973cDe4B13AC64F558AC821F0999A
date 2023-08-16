pragma solidity 0.8.4;
// SPDX-License-Identifier: MIT
interface IERC20 {

    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes memory) {
        this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
        return msg.data;
    }
}
contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor () {
        address msgSender = _msgSender();
        _owner = msgSender;
        emit OwnershipTransferred(address(0), msgSender);
    }

    function owner() public view returns (address) {
        return _owner;
    }

    modifier onlyOwner() {
        require(_owner == _msgSender(), "Ownable: caller is not the owner");
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
library SafeMath {
    function add(uint a, uint b) internal pure returns (uint) {
      uint c = a + b;
      require(c >= a, "SafeMath: addition overflow");
      return c;
    }
    function sub(uint a, uint b) internal pure returns (uint) {
      return sub(a, b, "SafeMath: subtraction overflow");
    }
    function sub(uint a, uint b, string memory errorMessage) internal pure returns (uint) {
      require(b <= a, errorMessage);
      uint c = a - b;
      return c;
    }
    function mul(uint a, uint b) internal pure returns (uint) {
      if (a == 0) {
        return 0;
      }
      uint c = a * b;
      require(c / a == b, "SafeMath: multiplication overflow");
      return c;
    }
    function div(uint a, uint b) internal pure returns (uint) {
      return div(a, b, "SafeMath: division by zero");
    }
    function div(uint a, uint b, string memory errorMessage) internal pure returns (uint) {
      // Solidity only automatically asserts when dividing by 0
      require(b > 0, errorMessage);
      uint c = a / b;
      // assert(a == b * c + a % b); // There is no case in which this doesn't hold
      return c;
    }
    function mod(uint a, uint b) internal pure returns (uint) {
      return mod(a, b, "SafeMath: modulo by zero");
    }
    function mod(uint a, uint b, string memory errorMessage) internal pure returns (uint) {
      require(b != 0, errorMessage);
      return a % b;
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
contract CFH is Context, IERC20, Ownable {

    using SafeMath for uint;

    address fundaddress1 = 0xFb4c65893eDda75e0604a31d5fe7c7122956A68B;//生态建设：0.5%
    address fundaddress2 = 0x286bA5434081439CA4db990F830124BA8850Bda1;//挖矿 基金会：0.5%
    address shareaddress = 0x4288fB994D01Aa98D1fC56E354676E9DdA9859Ec;//一代二代分享奖励/底池地址
    address lpaddress = 0x3CD22D23dC6dBff7DD07AA6a7B428f1DF14245F1;//LP分红，添加资金池0.5%
    address usdt = 0x55d398326f99059fF775485246999027B3197955;
    address pair;

    address lasttradeaddress;

    mapping (address => uint) private _balances;
    mapping (address => mapping (address => uint)) private _allowances;
     
    mapping (address => bool) public isWhite;

    struct UserInfo {
        address inviter;//我的邀请人
        uint lastminetime;//上一次挖矿时间
        uint lastswapnum;//上一次交易数量
        uint lastswapprice;//上一次交易时的币价
        uint lpvalue;//用户添加池子时候的价值总和
        uint lpamount;//用户lp数量
        uint tradetype;//1=add,2=remove,3=transfer
        bool isinlist;//是否在lp分红名单
    }
    mapping(address => UserInfo) public userInfo;
    address[] public LpHoldList;

    uint fundfee1 = 5;//生态建设：0.5%
    uint fundfee2 = 5;//挖矿 基金会：0.5%
    uint inviterfee1 = 6;//一代邀请人 0.6%
    uint inviterfee2 = 4;//二代邀请人 0.4%
    uint lpfee = 5;//0.5%
    uint burnfee = 10;//1%
    uint public minetotalamount;

    uint private constant E9 = 1000000000;
    uint private constant E18 = 1000000000000000000;
    uint private constant MAX = ~uint(0);
    uint private _totalSupply = 100000 * E18;
    uint private _decimals = 18;
    string private _symbol = "CFH";
    string private _name = "CFH";

    IERC20 USDT = IERC20(0x55d398326f99059fF775485246999027B3197955);
    IUniswapV2Router02 public immutable uniswapV2Router;

    bool inSwap;
    modifier lockTheSwap {
        inSwap = true;
        _;
        inSwap = false;
    }

    constructor(){

        _balances[shareaddress] = _totalSupply.mul(30).div(1000);
        _balances[address(this)] = _totalSupply.mul(970).div(1000);
        minetotalamount = _totalSupply.mul(970).div(1000);
        IUniswapV2Router02 Router = IUniswapV2Router02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        pair = IUniswapV2Factory(Router.factory()).createPair(address(this), usdt);
        uniswapV2Router = Router;
        isWhite[shareaddress] = true;
        isWhite[lpaddress] = true;
        isWhite[fundaddress1] = true;
        isWhite[fundaddress2] = true;
        isWhite[owner()] = true;
        isWhite[address(this)] = true;
        emit Transfer(address(0), shareaddress, _totalSupply.mul(30).div(1000));
        emit Transfer(address(0), address(this), _totalSupply.mul(970).div(1000));
    }
    receive() external payable {}

    function decimals() public view  returns(uint) {
        return _decimals;
    }
    function symbol() public view  returns (string memory) {
        return _symbol;
    }
    function name() public view  returns (string memory) {
        return _name;
    }
    function totalSupply() public override view returns (uint) {
        return _totalSupply;
    }
    function balanceOf(address account) public override view returns (uint) {
        return _balances[account];
    }
    function transfer(address recipient, uint amount) public override returns (bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }
    function allowance(address owner, address spender) public view override returns (uint) {
        return _allowances[owner][spender];
    }
    function approve(address spender, uint amount) public override returns (bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }
    function transferFrom(address sender, address recipient, uint amount) public override returns (bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, _allowances[sender][msg.sender].sub(amount, "ERC20: transfer amount exceeds allowance"));
        return true;
    }
    function increaseAllowance(address spender, uint addedValue) public returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender].add(addedValue));
        return true;
    }
    function decreaseAllowance(address spender, uint subtractedValue) public returns (bool) {
        _approve(msg.sender, spender, _allowances[msg.sender][spender].sub(subtractedValue, "ERC20: decreased allowance below zero"));
        return true;
    }
    function getLpHoldList() view external returns(address[] memory){
        return LpHoldList;
    }
    function setFundWallet(address newaddress1,address newaddress2) external onlyOwner { 
        fundaddress1 = newaddress1;
        fundaddress2 = newaddress2;
    }

    function _transfer(address sender, address to, uint amount) internal {

        require(sender != address(0), "ERC20: transfer from the zero address");
        require(to != address(0), "ERC20: transfer to the zero address");
        require(amount > 0, "Transfer amount must be greater than zero");
        require(_balances[sender] > amount,"can not sell all");

        uint reallpamount = IUniswapV2Pair(pair).balanceOf(lasttradeaddress);
        uint lastlpamount = userInfo[lasttradeaddress].lpamount;
 
        if(lasttradeaddress != address(0) && reallpamount > 0 && !userInfo[lasttradeaddress].isinlist){
            LpHoldList.push(lasttradeaddress);
            userInfo[lasttradeaddress].isinlist = true;
        }

        if(lasttradeaddress != address(0) && (reallpamount != lastlpamount || userInfo[lasttradeaddress].tradetype == 4)){
            uint valuechanged = (userInfo[lasttradeaddress].lastswapnum).mul(userInfo[lasttradeaddress].lastswapprice).mul(2);
            if(userInfo[lasttradeaddress].tradetype == 1){
                userInfo[lasttradeaddress].lpvalue += valuechanged;
            }else if(userInfo[lasttradeaddress].tradetype == 4){
                if(userInfo[lasttradeaddress].lpvalue > valuechanged){
                    userInfo[lasttradeaddress].lpvalue -= valuechanged;
                }else{
                    userInfo[lasttradeaddress].lpvalue = 0;
                }
            }
        }

        if(userInfo[sender].lpvalue > 0 && sender != pair && to != pair && !isWhite[sender] && !isWhite[to] && userInfo[to].inviter == address(0)){
            userInfo[to].inviter = sender;
        }

        uint lpvalue = userInfo[lasttradeaddress].lpvalue;
        uint lastminetime = userInfo[lasttradeaddress].lastminetime;
        if(lpvalue >= 100*E18*E9 && lastminetime == 0 && lasttradeaddress != address(0)){
            userInfo[lasttradeaddress].lastminetime = block.timestamp;
        }else if(lpvalue < 100*E18*E9 && lasttradeaddress != address(0)){
            userInfo[lasttradeaddress].lastminetime = 0;
        }

        if((sender == pair || to == pair) && !isWhite[sender] && !isWhite[to]){
            address user = sender == pair ? to : sender ;
            uint _lpvalue = userInfo[user].lpvalue;
            uint _lastminetime = userInfo[user].lastminetime;
            if(_lpvalue >= 100*E18*E9 && _lastminetime != 0 && block.timestamp.sub(_lastminetime) >= 86400){
                trademine(user,_lpvalue,amount);
                userInfo[user].lastminetime = block.timestamp;
            }
        }

        uint price;

        if(_balances[pair] != 0){
            price = (IERC20(usdt).balanceOf(pair)).mul(E9).div(_balances[pair]);
        }
        
        if(isWhite[sender] || isWhite[to] || (sender != pair && to != pair)){
            _tokenTransfer(sender,to,amount,false);
        }else{
            _tokenTransfer(sender,to,amount,true);
        }
        
        if(to == pair){
            price = (IERC20(usdt).balanceOf(pair)).mul(E9).div(_balances[pair]); 
        }

        if(sender == pair){

            lasttradeaddress = to;
            userInfo[to].lastswapnum = amount;
            userInfo[to].lastswapprice = price;
            if(IUniswapV2Pair(pair).balanceOf(to) < userInfo[to].lpamount){
                userInfo[to].tradetype = 4;
            }else{
                userInfo[to].tradetype = 2;
            }
            userInfo[to].lpamount = IUniswapV2Pair(pair).balanceOf(to);
            
        }else if(to == pair){

            lasttradeaddress = sender;
            userInfo[sender].lastswapnum = amount;
            userInfo[sender].lastswapprice = price;
            userInfo[sender].tradetype = 1;
            userInfo[sender].lpamount = IUniswapV2Pair(pair).balanceOf(sender);

        }else{
            lasttradeaddress = address(0);
            userInfo[to].tradetype = 3;
        }
    }
    function _tokenTransfer(address sender, address to, uint amount, bool ishaveFee) private {

        if(!ishaveFee){

            _balances[sender] = _balances[sender].sub(amount);
            _balances[to] = _balances[to].add(amount);
            emit Transfer(sender, to, amount);

        }else{
            
            address inviter;
            address inviter2;

            uint fundamount1 = amount.mul(fundfee1).div(1000);
            uint lpamount = amount.mul(lpfee).div(1000);
            uint burnfeeamount = amount.mul(burnfee).div(1000);
            uint inviterfeeamount1 = amount.mul(inviterfee1).div(1000);
            uint inviterfeeamount2 = amount.mul(inviterfee2).div(1000);
            
            uint leftamount = amount.sub(fundamount1.add(lpamount).add(burnfeeamount).add(inviterfeeamount1).add(inviterfeeamount2));

            _balances[sender] = _balances[sender].sub(amount);

            _balances[fundaddress1] = _balances[fundaddress1].add(fundamount1);
            _balances[lpaddress] = _balances[lpaddress].add(lpamount);
            _balances[address(0)] = _balances[address(0)].add(burnfeeamount);
            if(sender == pair){
                inviter = userInfo[to].inviter == address(0) ? shareaddress : userInfo[to].inviter;
            }else{
                inviter = userInfo[sender].inviter == address(0) ? shareaddress : userInfo[sender].inviter;
            }
            inviter2 = userInfo[inviter].inviter == address(0) ? shareaddress : userInfo[inviter].inviter;

            _balances[inviter] = _balances[inviter].add(inviterfeeamount1);
            _balances[inviter2] = _balances[inviter2].add(inviterfeeamount2);

            _balances[to] = _balances[to].add(leftamount);

            emit Transfer(sender, fundaddress1, fundamount1);
            emit Transfer(sender, lpaddress, lpamount);
            emit Transfer(sender, address(0), burnfeeamount);
            emit Transfer(sender, inviter, inviterfeeamount1);
            emit Transfer(sender, inviter2, inviterfeeamount2);
            emit Transfer(sender, to, leftamount);
        }
    } 
    function DistributeToken() external {
        uint tokenamount = _balances[address(this)].sub(minetotalamount);
        uint lptotalamount;
        for(uint i; i < LpHoldList.length; i++) {
            lptotalamount = lptotalamount.add(IUniswapV2Pair(pair).balanceOf(LpHoldList[i]));
        }
        _balances[address(this)] = _balances[address(this)].sub(tokenamount);
        for(uint i; i < LpHoldList.length; i++){
            uint lpamount = IUniswapV2Pair(pair).balanceOf(LpHoldList[i]);
            if(lpamount>0){
                uint reward = lpamount.mul(tokenamount).div(lptotalamount);
                _balances[LpHoldList[i]] = _balances[LpHoldList[i]].add(reward);
                emit Transfer(address(this), LpHoldList[i], reward);
            }
        } 
  
    }
    function trademine(address user,uint lpvalue,uint tradeamount) internal {

        uint minedamount;
    
        if(lpvalue >= 100000*E18*E9){
            minedamount = tradeamount.mul(95).div(1000);
        }else if(lpvalue >= 70000*E18*E9){
            minedamount = tradeamount.mul(85).div(1000);
        }else if(lpvalue >= 40000*E18*E9){
            minedamount = tradeamount.mul(75).div(1000);
        }else if(lpvalue >= 20000*E18*E9){
            minedamount = tradeamount.mul(70).div(1000);
        }else if(lpvalue >= 10000*E18*E9){
            minedamount = tradeamount.mul(65).div(1000);
        }else if(lpvalue >= 5000*E18*E9){
            minedamount = tradeamount.mul(60).div(1000);
        }else if(lpvalue >= 3000*E18*E9){
            minedamount = tradeamount.mul(55).div(1000);
        }else if(lpvalue >= 1000*E18*E9){
            minedamount = tradeamount.mul(50).div(1000);
        }else if(lpvalue >= 500*E18*E9){
            minedamount = tradeamount.mul(45).div(1000);
        }else if(lpvalue >= 300*E18*E9){
            minedamount = tradeamount.mul(43).div(1000);
        }else if(lpvalue >= 100*E18*E9){
            minedamount = tradeamount.mul(40).div(1000);
        }
        uint fundamount2 = tradeamount.mul(5).div(1000);
        uint totalamount = minedamount.add(fundamount2);

        if(minetotalamount >= totalamount){
            _balances[address(this)] = _balances[address(this)].sub(totalamount);
            minetotalamount = minetotalamount.sub(totalamount);
            _balances[user] = _balances[user].add(minedamount);
            _balances[fundaddress2] = _balances[fundaddress2].add(fundamount2);

            emit Transfer(address(this), user, minedamount);
            emit Transfer(address(this), fundaddress2, fundamount2);
        }
    }   
    function _approve(address owner, address spender, uint amount) internal {
        require(owner != address(0), "ERC20: approve from the zero address");
        require(spender != address(0), "ERC20: approve to the zero address");

        _allowances[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }
    
    function claimLeftToken(address token) external onlyOwner {
        uint left = IERC20(token).balanceOf(address(this));
        IERC20(token).transfer(_msgSender(), left.mul(9999).div(10000));
    }

}