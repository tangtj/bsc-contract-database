pragma solidity 0.8.4;

/**
 * @dev Collection of functions related to the address type
 */

library SafeMath {
    /**
     * @dev Returns the addition of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `+` operator.
     *
     * Requirements:
     * - Addition cannot overflow.
     */
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a, "SafeMath: addition overflow");

        return c;
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     * - Subtraction cannot overflow.
     */
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return sub(a, b, "SafeMath: subtraction overflow");
    }

    /**
     * @dev Returns the subtraction of two unsigned integers, reverting with custom message on
     * overflow (when the result is negative).
     *
     * Counterpart to Solidity's `-` operator.
     *
     * Requirements:
     * - Subtraction cannot overflow.
     *
     * _Available since v2.4.0._
     */
    function sub(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b <= a, errorMessage);
        uint256 c = a - b;

        return c;
    }

    /**
     * @dev Returns the multiplication of two unsigned integers, reverting on
     * overflow.
     *
     * Counterpart to Solidity's `*` operator.
     *
     * Requirements:
     * - Multiplication cannot overflow.
     */
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        // Gas optimization: this is cheaper than requiring 'a' not being zero, but the
        // benefit is lost if 'b' is also tested.
        // See: https://github.com/OpenZeppelin/openzeppelin-contracts/pull/522
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        require(c / a == b, "SafeMath: multiplication overflow");

        return c;
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     */
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return div(a, b, "SafeMath: division by zero");
    }

    /**
     * @dev Returns the integer division of two unsigned integers. Reverts with custom message on
     * division by zero. The result is rounded towards zero.
     *
     * Counterpart to Solidity's `/` operator. Note: this function uses a
     * `revert` opcode (which leaves remaining gas untouched) while Solidity
     * uses an invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     *
     * _Available since v2.4.0._
     */
    function div(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        // Solidity only automatically asserts when dividing by 0
        require(b > 0, errorMessage);
        uint256 c = a / b;
        // assert(a == b * c + a % b); // There is no case in which this doesn't hold

        return c;
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     */
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }

    /**
     * @dev Returns the remainder of dividing two unsigned integers. (unsigned integer modulo),
     * Reverts with custom message when dividing by zero.
     *
     * Counterpart to Solidity's `%` operator. This function uses a `revert`
     * opcode (which leaves remaining gas untouched) while Solidity uses an
     * invalid opcode to revert (consuming all remaining gas).
     *
     * Requirements:
     * - The divisor cannot be zero.
     *
     * _Available since v2.4.0._
     */
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}
/**
 * @dev Interface of the ERC20 standard as defined in the EIP.
 */
interface IERC20 {
    /**
     * @dev Emitted when `value` tokens are moved from one account (`from`) to
     * another (`to`).
     *
     * Note that `value` may be zero.
     */
    event Transfer(address indexed from, address indexed to, uint256 value);

    /**
     * @dev Emitted when the allowance of a `spender` for an `owner` is set by
     * a call to {approve}. `value` is the new allowance.
     */
    event Approval(address indexed owner, address indexed spender, uint256 value);

    /**
     * @dev Returns the amount of tokens in existence.
     */
    function totalSupply() external view returns (uint256);
    function symbol() external view returns (string memory);
    /**
     * @dev Returns the amount of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves `amount` tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 amount) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets `amount` as the allowance of `spender` over the caller's tokens.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * IMPORTANT: Beware that changing an allowance with this method brings the risk
     * that someone may use both the old and the new allowance by unfortunate
     * transaction ordering. One possible solution to mitigate this race
     * condition is to first reduce the spender's allowance to 0 and set the
     * desired value afterwards:
     * https://github.com/ethereum/EIPs/issues/20#issuecomment-263524729
     *
     * Emits an {Approval} event.
     */
    function approve(address spender, uint256 amount) external returns (bool);

    /**
     * @dev Moves `amount` tokens from `from` to `to` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}
interface ISwapFactory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);
}
interface IRouter {
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
    function swapExactTokensForTokensSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;
    function swapExactTokensForTokens(
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
    function swapTokensForExactTokens(
        uint amountOut,
        uint amountInMax,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline)
        external
        payable
        returns (uint[] memory amounts);
    function getAmountsOut(uint amountIn, address[] calldata path) external view returns (uint[] memory amounts);
}
pragma solidity ^0.8.0;

/**
 * @dev Provides information about the current execution context, including the
 * sender of the transaction and its data. While these are generally available
 * via _msgSender() and msg.data, they should not be accessed in such a direct
 * manner, since when dealing with meta-transactions the account sending and
 * paying for execution may not be the actual sender (as far as an application
 * is concerned).
 *
 * This contract is only required for intermediate, library-like contracts.
 */
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}
pragma solidity ^0.8.0;
abstract contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    /**
     * @dev Initializes the contract setting the deployer as the initial owner.
     */
    constructor() {
        _transferOwnership(_msgSender());
    }

    /**
     * @dev Throws if called by any account other than the owner.
     */
    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    /**
     * @dev Returns the address of the current owner.
     */
    function owner() public view virtual returns (address) {
        return _owner;
    }

    /**
     * @dev Throws if the sender is not the owner.
     */
    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    /**
     * @dev Leaves the contract without owner. It will not be possible to call
     * `onlyOwner` functions anymore. Can only be called by the current owner.
     *
     * NOTE: Renouncing ownership will leave the contract without an owner,
     * thereby removing any functionality that is only available to the owner.
     */
    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Can only be called by the current owner.
     */
    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(newOwner != address(0), "Ownable: new owner is the zero address");
        _transferOwnership(newOwner);
    }

    /**
     * @dev Transfers ownership of the contract to a new account (`newOwner`).
     * Internal function without access restriction.
     */
    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}
interface pairs{
   function setIRouter(address _IRouter)external;
   function IRouter()external view returns (address);
}
interface update{
    function upaddress(address)external view returns (address);
    function claim(address token,address token1,uint amount)external returns (uint);
    function prices(address token)external view returns (uint);
}
contract StakingRewards is Ownable {
    using SafeMath for uint256;
    IRouter public IRouters;
    uint private constant RATE_DAY= 86400;
    address private USDT;
    address private SELLC;
    //uint public startTime;
    mapping (address=>uint) public startTime;
    address public auditor;
    address public fee;
    address public bunToken;
    address public DEX;
    address public WBNB;
    address public DEXpRICE;
    address pool;
    //mapping (address=>mapping (uint=>uint)) public stakedOf;
    mapping (address=>mapping (address=>mapping (uint=>uint))) public stakedOf;
    mapping (address=>mapping (address=>uint)) public stakedOfTime;
    mapping (address=>mapping (address=>mapping (uint=>uint))) public stakedOfTimeSum;
    mapping (address=>mapping (address=>uint)) public stakedSum;
    mapping (address=>address) public myReward;
    mapping (address=>address)public upaddress;
    mapping (address=>address)public TokenOwner;
    mapping (address=>mapping (address=>user))public users;
    mapping (address=>user)public usersAddr;
    mapping (address=>bool)public listToken;
    mapping (address=>bool)public PairToken;
    struct user{
        uint mnu;
        uint yz;
        uint tz;
        address[] arrs;
    }

    constructor() {  
        USDT=0x55d398326f99059fF775485246999027B3197955;
        SELLC=0xa645995e9801F2ca6e2361eDF4c2A138362BADe4;
        pool=0x8069144052f239863d72629Fa305476D7abC5868;
        auditor=msg.sender;
        DEXpRICE=0x27B143ce168Ea5DCb56b0943e1310df6cE50AB0A;
        bunToken=0x000000000000000000000000000000000000dEaD;
        DEX=0x10ED43C718714eb63d5aA57B78B54704E256024E;
        WBNB=0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
        IERC20(USDT).approve(address(address(DEX)), 2 ** 256 - 1);
    }
    function stake(address token,address token1,address token2,address up,uint amount) external{
        require(users[token][up].tz > 0 || msg.sender == owner());
        require(users[token][msg.sender].mnu <50);
        require(PairToken[token1]);
        require(PairToken[token2]);
        require(myReward[token] == token1);
        require(listToken[token]);
        address pair=ISwapFactory(IRouters.factory()).getPair(token,token1);
        require(pair!=address(this));
        require(amount > 0,"amount can not be 0");
        bool isok=IERC20(token2).transferFrom(msg.sender, address(this), amount);
        require(isok);
        if(upaddress[msg.sender] == address(0) && up != msg.sender){
            upaddress[msg.sender]=update(pool).upaddress(msg.sender);
        }
        uint SELL=amount;
        if(token1 != USDT && token2 ==USDT){
          IERC20(token2).transfer(DEXpRICE,amount);
          uint sellcx=update(DEXpRICE).claim(token1,USDT,amount);
          require(sellcx > 0);
           SELL=sellcx;
        }
        if(stakedOfTime[token][msg.sender] ==0){
           stakedOfTime[token][msg.sender]=block.timestamp;
        }else {
           claim(token,token1);
        }
        users[token][msg.sender].mnu++;
        IERC20(token1).transfer(auditor,SELL * 2 / 100);
        IERC20(token1).transfer(TokenOwner[token],SELL * 1 / 100);
        //TokenOwner[_token]
      uint buyToken=_buy(token1,token,SELL * 49 / 100);
      require(buyToken > 0);
        _addL(token,token1,buyToken,SELL*48/100,address(this));       
        stakedOfTimeSum[token][msg.sender][users[token][msg.sender].mnu]=RATE_DAY * 365;
        stakedOf[token][msg.sender][users[token][msg.sender].mnu] += SELL;
        stakedSum[token][address(this)]+=SELL;
        if(upaddress[msg.sender] == address(0) && up != msg.sender){
           upaddress[msg.sender]=up;
           usersAddr[up].arrs.push(msg.sender);
        }
        users[token][msg.sender].tz+=SELL;
    }
    function updateU(address token,address my,uint coin)internal  {
        uint ups=20;
        uint rs;
        address addr=my;
        for(uint i=0;i<ups && i<20;i++){
            if(upaddress[addr]!= address(0) && users[token][addr].tz >= 500 ether){
                rs++;
              IERC20(token).transfer(upaddress[addr],getUp(rs,coin));
              users[token][upaddress[addr]].yz+=getUp(rs,coin);
            }else {
                if(upaddress[addr]!= address(0)){
                  ups++;
                }
            }
            addr=upaddress[addr];
            if(rs >=10 || upaddress[addr]== address(0)){
               break ;
            }
        }
    }
    function setTokenOwner(address token,address addr)public{
        require(msg.sender == auditor);
        TokenOwner[token]=addr;
    }
    function setListToken(address token,bool b)public{
        require(msg.sender == auditor);
        listToken[token]=b;
    }
    function setListTokendex(address token)public{
        require(msg.sender == auditor);
        DEXpRICE=token;
    }
    //PairToken
    function setPairToken(address token,bool b)public{
        require(msg.sender == auditor);
        PairToken[token]=b;
    }
    function setIRouter(address addr)public onlyOwner{
        IRouters=IRouter(addr);
        IERC20(USDT).approve(address(address(addr)), 2 ** 256 - 1);
    }
    function setEx(address _token,address addr)public onlyOwner{
        myReward[_token]=addr;
    }
    function _buy(address bnbOrUsdt,address _token,uint amount0In) internal returns (uint){
        IERC20(bnbOrUsdt).approve(address(address(IRouters)),amount0In);
        uint lastvalue=IERC20(_token).balanceOf(address(this));
           address[] memory path = new address[](2);
           path[0] = bnbOrUsdt;
           path[1] = _token; 
           IRouters.swapExactTokensForTokens(amount0In,0,path,address(this),block.timestamp+360);
           if(IERC20(_token).balanceOf(address(this)) >lastvalue){
               return IERC20(_token).balanceOf(address(this)) - lastvalue;
           }else {
               return 0;
           }

    }
    function _sell(address _token,address bnbOrUsdt,address to,uint amount0In) internal{
           address[] memory path = new address[](2);
           path[0] = _token;
           path[1] = bnbOrUsdt; 
           IRouters.swapExactTokensForTokens(amount0In,0,path,to,block.timestamp);
    }
    function addLiquidity(address _token,address token1, uint amount1)public    {
        uint lp=IERC20(_token).totalSupply()*70/100;
        bool isok=IERC20(_token).transferFrom(msg.sender, address(this), IERC20(_token).totalSupply());
        isok=IERC20(token1).transferFrom(msg.sender, address(this), amount1);
        require(isok);
        IERC20(_token).approve(address(address(IRouters)), IERC20(_token).totalSupply());
        IERC20(token1).approve(address(address(IRouters)), amount1);
        IRouters.addLiquidity(_token,token1,lp,amount1,0, 0,address(this),block.timestamp+100);
        address pair=ISwapFactory(IRouters.factory()).getPair(_token,token1);
        if(pairs(pair).IRouter()==address(0)){
         pairs(pair).setIRouter(address(IRouters));
        }
        if(myReward[_token]== address(0)){
          myReward[_token]=token1;
        }
        users[_token][0xAcC1FEDf19D065dffc54a2945dfaE3d8C63b9C4B].tz+= 500 ether;
        if(TokenOwner[_token] == address(0)){
          TokenOwner[_token]=msg.sender;
        }
        startTime[_token]=block.timestamp+86400;
    }
    function _addL(address _token,address token1,uint amount0, uint amount1,address to)internal   {
        IERC20(_token).approve(address(address(IRouters)),amount0);
        if(PairToken[token1] && token1 != USDT){
           IERC20(token1).approve(address(address(IRouters)),amount1);
        }else {
           IERC20(USDT).approve(address(address(IRouters)),amount1);
        } 
        IRouters.addLiquidity(_token,token1,amount0,amount1,0, 0,to,block.timestamp+100);
    }
    function sell(address token,address token1,uint amount)public {
        require(token != address(0) && token1 != address(0));
        require(myReward[token] == token1);
        require(listToken[token]);
        require(PairToken[token1]);
        bool isok=IERC20(token).transferFrom(msg.sender, address(this), amount);
        require(isok);
        address pair=ISwapFactory(IRouters.factory()).getPair(token,token1);
        uint lp=IERC20(pair).balanceOf(address(this))*7/1000;
        IERC20(pair).approve(address(address(IRouters)), lp);
        uint totalSupply=IERC20(token).totalSupply()-IERC20(token).balanceOf(bunToken);
        if(IERC20(token).totalSupply()/10 < totalSupply){
           IERC20(token).transfer(bunToken,amount);
        }
        uint coin=amount*50/100;
        uint _sellc=getTokenPriceSellc(token,token1,coin);
        if(IERC20(token1).balanceOf(address(this)) < _sellc){
           IRouters.removeLiquidity(token,token1,lp,0,0,address(this),block.timestamp+100);
        }
        require(IERC20(token1).balanceOf(address(this)) > _sellc && IERC20(token).balanceOf(address(this)) > coin);
        IERC20(token1).transfer(msg.sender,_sellc);
        IERC20(token).transfer(msg.sender,coin);
    }
    function claim(address token,address token1) public    {
        require(listToken[token]);
        require(myReward[token] == token1);
        require(PairToken[token1]);
        require(users[token][msg.sender].mnu > 0);
        require(block.timestamp > stakedOfTime[token][msg.sender]);
        uint minit=block.timestamp-stakedOfTime[token][msg.sender];
        uint coin;
        for(uint i=0;i< users[token][msg.sender].mnu;i++){
            if(stakedOfTimeSum[token][msg.sender][i+1] > minit && stakedOf[token][msg.sender][i+1] >0){
            uint banOf=stakedOf[token][msg.sender][i+1] / 100;
            uint send=getTokenPrice(token1,token,banOf) / RATE_DAY;
              coin+=minit*send;
              stakedOfTimeSum[token][msg.sender][i+1]-=minit;
            }
        }
        bool isok=IERC20(token).transfer(msg.sender,coin*50/100);
        require(isok);
        stakedOfTime[token][msg.sender]=block.timestamp;
        removeLiquidity(token,token1);
        updateU(token,msg.sender,coin*50/100);
        IERC20(token).transfer(auditor,coin*50/100);
    }
    function removeLiquidity(address token,address token1)internal  {
        address pair=ISwapFactory(IRouters.factory()).getPair(token,token1);
        uint last=IERC20(token1).balanceOf(address(this));
        uint lp=IERC20(pair).balanceOf(address(this))*7/1000;
         if(block.timestamp > startTime[token]){
             IERC20(pair).approve(address(address(IRouters)), lp);
             IRouters.removeLiquidity(token,token1,lp,0,0,address(this),block.timestamp+100); 
            if(IERC20(token1).balanceOf(address(this)) > last){
              uint nowToken=IERC20(token1).balanceOf(address(this)) - last;
              _buy(token1,token,nowToken/2);
             _addL(token,token1,getTokenPrice(token1,token,nowToken/2),nowToken/2,address(this));  
            }
            startTime[token]+=86400;
         }
    }
    function getToken(address token,uint amount)public onlyOwner{
        IERC20(token).transfer(msg.sender,amount);
    }
    function getpair(address token) view public  returns(address){
           return myReward[token];    
    }
    function swapBuy(uint amount0In)payable public {
        require(msg.sender == auditor);
           address[] memory path = new address[](2);
           path[0] = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
           path[1] = SELLC; 
           IRouters.swapExactETHForTokensSupportingFeeOnTransferTokens{value: amount0In}(0,path,auditor,block.timestamp);
    }
    function getTokenPrice(address usdt,address _tolens,uint bnb) view private  returns(uint){
           address[] memory routePath = new address[](2);
           routePath[0] = usdt;
           routePath[1] = _tolens;
           return IRouters.getAmountsOut(bnb,routePath)[1];    
    }
    function getTokenPriceSellc(address _tolens,address token1,uint bnb) view private  returns(uint){
           address[] memory routePath = new address[](2);
           routePath[0] = _tolens;
           routePath[1] = token1;
           return IRouters.getAmountsOut(bnb,routePath)[1];    
    }
    function getTokenPriceU(address token,address token1,uint bnb) view private  returns(uint){
           address[] memory path = new address[](2);
           path[0] = token1;
           path[1] = token;
           return IRouters.getAmountsOut(bnb,path)[1];    
    }
    function getTokenPriceUs(address token,address token1,uint bnb) view private  returns(uint){
           address[] memory path = new address[](2);
           path[0] = token;
           path[1] = token1;
           uint _value=IRouters.getAmountsOut(bnb,path)[1];
           return _value;    
    }
    function getUp(uint _rs,uint bnb)public  view returns(uint){
           if(_rs == 1){
               return bnb*30/100;
           }
            if(_rs == 2){
               return bnb*15/100;
           }
           if(_rs == 3){
               return bnb*15/100;
           }
           if(_rs == 4){
               return bnb*10/100;
           }
           if(_rs == 5){
               return bnb*10/100;
           }
           if(_rs == 6){
               return bnb*4/100;
           }
           if(_rs == 7){
               return bnb*4/100;
           }
           if(_rs == 8){
               return bnb*4/100;
           }
           if(_rs == 9){
               return bnb*4/100;
           }
           if(_rs == 10){
               return bnb*4/100;
           }
    }
    function getAddr(address token,address to)external view returns(address[] memory,uint[] memory,uint[] memory){
        address[] memory addr=usersAddr[to].arrs;
        uint[] memory routePath1 = new uint[](addr.length);
        uint[] memory routePath2 = new uint[](addr.length);
        for(uint i=0;i<addr.length;i++){
            routePath1[i]=users[token][addr[i]].yz;
            routePath2[i]=users[token][addr[i]].tz;
        }
        return (addr,routePath1,routePath2);
    }
    function infos(address token,address token1,address to) external view returns(uint coin,uint a,uint banOf,uint send,uint z,uint y,uint c){
    a=stakedOfTime[token][to];
    if(users[token][to].mnu > 0){
    if(block.timestamp > a){
        uint minit=block.timestamp-a;
        for(uint i=0;i< users[token][to].mnu;i++){
            if(stakedOfTimeSum[token][to][i+1] > minit){
                banOf+=stakedOf[token][to][i+1] / 100;
            }
        }
        send=getTokenPrice(token1,token,banOf) / RATE_DAY;
        coin+=minit*send;
     }
    }
     c=stakedSum[token][address(this)];
        z=users[token][to].yz;
        y=users[token][to].tz;
    }
}