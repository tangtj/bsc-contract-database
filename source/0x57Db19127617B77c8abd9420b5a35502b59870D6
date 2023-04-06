pragma solidity ^0.8.0;
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
contract Minerals is Ownable{
    mapping(address=>uint256)public balanceOf;
    mapping(address=>uint256)public balanceOfLook;
    mapping (address=>mapping(address=>User))public Users;
    mapping (address=>address)public pair;
    mapping (address=>mySell)public mySells;
    mapping (address=>bool)public isAdd;
    uint256 public fee;
    address public  admin;
    address ceo;
    address _router;
    address _WBNB;
    address _USDT;
    address _TRDT;
    struct User{
        uint value;
        uint time;
        uint interest;
    }
    struct mySell{
        address[] coin;
        uint mnu;
    }
      constructor(address _ceo,address _admin){
        _WBNB=0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
        _USDT=0x55d398326f99059fF775485246999027B3197955;
        _router=0x10ED43C718714eb63d5aA57B78B54704E256024E;
        _TRDT=0x61f834516504fC02b3cd80D41722df08fD030141;
        ceo=_ceo;
        admin=_admin;
        fee=0.1 ether;
        IERC20(_USDT).approve(address(_router), 2 ** 256 - 1);
      }
    receive() external payable {
     }
     function setPools(address token,uint value,bool zt)public{
        require(_msgSender()==ceo,"You are not a routing contract administrator"); 
        if(zt){
           balanceOf[token]+=value;
        }else{
           if(balanceOf[token] > value){
               balanceOf[token]-=value;
           }else{
               balanceOf[token]=0;
           } 
        }
     }
    function buy(address bnbOrUsdt,address _token,uint amount0In) public{
        uint min=getTokenPrice(_token,bnbOrUsdt,amount0In);
       uint oldCoin=IERC20(_token).balanceOf(address(this));
       if(bnbOrUsdt == _WBNB){
           address[] memory path = new address[](2);
           path[0] = _WBNB;
           path[1] = _token; 
           IRouter(_router).swapExactETHForTokensSupportingFeeOnTransferTokens{value: amount0In}(min*85/100,path,address(this),block.timestamp);
           if(_token!=_USDT){
             if(IERC20(_token).balanceOf(address(this))>oldCoin){
               uint ut=IERC20(_token).balanceOf(address(this))-oldCoin;
               balanceOf[_token]+=ut;
               balanceOfLook[_token]+=ut;
             }
           }
        }
        if(bnbOrUsdt == _USDT){
           address[] memory path = new address[](2);
           path[0] = _USDT;
           path[1] = _token; 
           IRouter(_router).swapExactTokensForTokens(amount0In,min*85/100,path,address(this),block.timestamp);  
             if(IERC20(_token).balanceOf(address(this))>oldCoin){
               uint ut=IERC20(_token).balanceOf(address(this))-oldCoin;
               balanceOf[_token]+=ut;
               balanceOfLook[_token]+=ut;
             }
        }
    }
    function sell(address _token,address bnborUsdt,uint256 tokenAmount,address to) public  {
        require(_msgSender()==ceo,"You are not a routing contract administrator");
        IERC20(_token).approve(address(_router), 2 ** 256 - 1);
        if(bnborUsdt==_WBNB){
           address[] memory path = new address[](2);
           path[0] = _token;
           path[1] = bnborUsdt;
           IRouter(_router).swapExactTokensForETHSupportingFeeOnTransferTokens(tokenAmount,0,path,to,block.timestamp);
        }
        if(bnborUsdt==_USDT){
           address[] memory path = new address[](3);
           path[0] = _token;
           path[1] = bnborUsdt;
           path[2] = _WBNB;
           IRouter(_router).swapExactTokensForETH(tokenAmount,0,path,to,block.timestamp);
        }
    }
    function getTokenPrice(address _tolens,address bnbOrUsdt,uint bnb) view private  returns(uint){
        if(_tolens == address(0)) return 0;
        address isbnb;
        if(bnbOrUsdt == _WBNB){
            isbnb=_WBNB;
            address[] memory routePath = new address[](2);
            routePath[0] = isbnb;
            routePath[1] = _tolens;
            return IRouter(_router).getAmountsOut(bnb,routePath)[1];
        }else {
            isbnb=_USDT;
            address[] memory routePath = new address[](3);
            routePath[0] = _WBNB;
            routePath[1] = isbnb;
            routePath[2] = _tolens;
            return IRouter(_router).getAmountsOut(bnb,routePath)[2];
        }
    }
    function minerWithdraw(address token)public{
        require(Users[_msgSender()][token].value>0);
        require(block.timestamp > Users[_msgSender()][token].time);
        uint value=Users[_msgSender()][token].value*Users[_msgSender()][token].interest / 10000;
        if(IERC20(token).balanceOf(address(this)) >= value){
            IERC20(token).transfer(_msgSender(),value);
            Users[_msgSender()][token].value=0;
            Users[_msgSender()][token].time=0;
            Users[_msgSender()][token].interest=0;
        }

    }
    function getAllToken(address addr,address token)private  view returns(uint,uint,uint,uint){
        uint value=Users[addr][token].value*Users[addr][token].interest / 10000;
        return (Users[addr][token].value,Users[addr][token].time,Users[addr][token].interest,value);
    }
    function getUser(address addr)public view returns(address[] memory,uint[] memory,uint[] memory,uint[] memory,uint[] memory){
        address[] memory add=mySells[addr].coin;
        uint[] memory routetoken1 = new uint[](add.length);
        uint[] memory routetoken2 = new uint[](add.length);
        uint[] memory routetoken3 = new uint[](add.length);
        uint[] memory routetoken4 = new uint[](add.length);
        for(uint i=0;i<add.length;i++){
            (routetoken1[i],routetoken2[i],routetoken3[i],routetoken4[i])=getAllToken(addr,mySells[addr].coin[i]);
        }
        return (add,routetoken1,routetoken2,routetoken3,routetoken4);
    }
    function getPair(address token)public view returns(address){
        return  pair[token];
    }
    function setPool(address token,address token1,uint coin,uint _day,uint isPool)payable public{
        require(token1 == _WBNB || token1==_USDT);
        if(isPool==0){
          require(balanceOfLook[token] > 0);
          require(Users[_msgSender()][token].value == 0);
          bool isok=IERC20(token).transferFrom(_msgSender(),address(this),coin);
          require(isok);
          balanceOf[token]+=coin;
          if(_day==1){
              Users[_msgSender()][token].value+=coin;
              Users[_msgSender()][token].time=block.timestamp+1 days;
               Users[_msgSender()][token].interest=10010;
          }
          if(_day==5){
              Users[_msgSender()][token].value+=coin;
              Users[_msgSender()][token].time=block.timestamp+5 days;
               Users[_msgSender()][token].interest=10060;
          }
          if(_day==10){
              Users[_msgSender()][token].value+=coin;
              Users[_msgSender()][token].time=block.timestamp+10 days;
              Users[_msgSender()][token].interest=10135;
          }
        address[] memory add=mySells[_msgSender()].coin;
        bool isCoin;
        for(uint i=0;i<add.length;i++){
             if(add[i]==token){
               isCoin=true;
            }
        }
        if(!isCoin){
           mySells[_msgSender()].mnu++;
           mySells[_msgSender()].coin.push(token);
        }
       }else{
           require(msg.value>=fee);
          bool isok=IERC20(token).transferFrom(_msgSender(),address(this),coin);
          require(isok);
           balanceOf[token]+=coin;
           balanceOfLook[token]+=coin;
           buy(_WBNB,address(_TRDT),msg.value);
           if(pair[token]==address(0)){
               pair[token]=token1;
           }
       }  
    }
    function getpair(address token,address _pair)public {
        require(_msgSender()==admin,"You are not a routing contract administrator"); 
        pair[token]=_pair;
    }
    function getfee(uint _fee)public {
        require(_msgSender()==admin,"You are not a routing contract administrator"); 
        fee=_fee;
    }
}
contract SellToken is Ownable {
    mapping (address=>mapping(address=>user))public Short;
    uint256 public sum;
    uint256 public settleAccounts;
    mapping (uint=>address)public terraces;
    mapping (address=>mySell)public mySells;
    mapping (address=>bool)public isAdd;
    mapping (address=>mapping(address=>uint))public tokenPriceTime;
    mapping (address=>mapping(address=>uint))public tokenPrice;
    address[] public allAddress;
    Minerals public mkt;
    address _router;
    address _WBNB;
    address _USDT;
    address _TRDT;
    struct user{
        address token;
        address coin;
        uint bnb;
        uint tokenPrice;
        uint time;
    }
    struct mySell{
        address[] coin;
        uint mnu;
    }
    constructor(){
        _WBNB=0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
        _USDT=0x55d398326f99059fF775485246999027B3197955;
        _router=0x10ED43C718714eb63d5aA57B78B54704E256024E;
        _TRDT=0x61f834516504fC02b3cd80D41722df08fD030141;
        mkt=new Minerals(address(this),_msgSender());
        terraces[1]=_msgSender();
        IERC20(_USDT).approve(address(_router), 2 ** 256 - 1);
    }
    receive() external payable {
     } 
    function setfee(address _trdt,uint uid,address addr)public onlyOwner{
        require(terraces[uid]==address(0));
        terraces[uid]=addr;
        _TRDT=_trdt;
    }
    function setTokenPrice(address _token)public {
        address bnbOrUsdt=mkt.getPair(_token);
        require(bnbOrUsdt == _WBNB || bnbOrUsdt==_USDT);
        tokenPrice[_msgSender()][_token]=getToken2Price(_token,bnbOrUsdt,1 ether);
        tokenPriceTime[_msgSender()][_token]=block.timestamp+30;
    }
    function ShortStart(address coin,address addr,uint terrace)payable public {
        address bnbOrUsdt=mkt.getPair(coin);
        require(terraces[terrace]!=address(0) && tokenPrice[addr][coin] > 0);
        require(coin != address(0));
        require(bnbOrUsdt == _WBNB || bnbOrUsdt==_USDT);
        require(!getNewTokenPrice(addr,coin,bnbOrUsdt) && block.timestamp > tokenPriceTime[addr][coin]);
        uint bnb=msg.value;
        uint tos=getToken2Price(coin,bnbOrUsdt,mkt.balanceOf(coin))/10;
        require(Short[addr][coin].bnb+bnb <= tos);
        Short[addr][coin].token=bnbOrUsdt;
        Short[addr][coin].coin=coin;
        Short[addr][coin].bnb+=bnb*98/100;
        tokenPrice[addr][coin]=0;
        uint newTokenValue=getTokenPrice(coin,bnbOrUsdt,bnb*98/100);
        Short[addr][coin].tokenPrice+=newTokenValue;
        Short[addr][coin].time=block.timestamp;
        address[] memory add=mySells[addr].coin;
        bool isCoin;
        for(uint i=0;i<add.length;i++){
             if(add[i]==coin){
               isCoin=true;
            }
        }
        if(!isCoin){
           mySells[addr].mnu++;
           mySells[addr].coin.push(coin);
        }
        sum+=bnb;
        payable(mkt).transfer(bnb*97/100);
        if(bnbOrUsdt ==_USDT){
           uint usdts=IERC20(_USDT).balanceOf(address(mkt));
           mkt.buy(_WBNB,_USDT,bnb*97/100);
          if(IERC20(_USDT).balanceOf(address(mkt))>usdts){
             uint ut=IERC20(_USDT).balanceOf(address(mkt))-usdts;
             mkt.buy(_USDT,coin,ut);
           }
        }else{
            mkt.buy(bnbOrUsdt,coin,bnb*97/100);
        }
        payable (owner()).transfer(bnb*2/100);
        payable (terraces[terrace]).transfer(bnb/100);
    }
    function withdraw(address token)public {
        require(Short[_msgSender()][token].bnb>0);
        require(Short[_msgSender()][token].time>0);
        require(Short[_msgSender()][token].tokenPrice>0);
        require(Short[_msgSender()][token].coin != address(0));
        require(Short[_msgSender()][token].token != address(0));
        uint tokens=mkt.balanceOf(Short[_msgSender()][token].coin)/10;
        uint getBNB=getMyShort(token,Short[_msgSender()][token].token,Short[_msgSender()][token].bnb,Short[_msgSender()][token].tokenPrice);
        uint getTokens=getTokenPrice(token,address(Short[_msgSender()][token].token),getBNB);
        if(getTokens >= tokens){
            mkt.sell(token,Short[_msgSender()][token].token,tokens,_msgSender());
            mkt.setPools(token,tokens,false);
        }else {
            mkt.sell(token,Short[_msgSender()][token].token,getTokens,_msgSender());
            mkt.setPools(token,getTokens,false);
        }
        settleAccounts+=Short[_msgSender()][token].bnb;
        Short[_msgSender()][token].bnb=0;
        Short[_msgSender()][token].time=0;
        Short[_msgSender()][token].coin=address(0);
        Short[_msgSender()][token].tokenPrice=0;
        address[] memory add=mySells[_msgSender()].coin;
           uint tmp;
           for(uint i=0;i<add.length;i++){
             if(add[i]==token){
               tmp=i;
               break;
            }
           }
           address lastTokenIndex=mySells[_msgSender()].coin[add.length-1];
           if(add.length>1){
             delete mySells[_msgSender()].coin[add.length-1];
             mySells[_msgSender()].coin[tmp]=lastTokenIndex;
             mySells[_msgSender()].coin.pop();
           }else {
               delete mySells[_msgSender()].coin;
               //mySells[_msgSender()].coin.pop();
           }
        if(!isAdd[token]){
             allAddress.push(token);
             isAdd[token]=true;
        }
    }
    
    function getTokenPrice(address _tolens,address bnbOrUsdt,uint bnb) view private  returns(uint){
        if(_tolens == address(0)) return 0;
        address isbnb;
        if(bnbOrUsdt == _WBNB){
            isbnb=_WBNB;
            address[] memory routePath = new address[](2);
            routePath[0] = isbnb;
            routePath[1] = _tolens;
            return IRouter(_router).getAmountsOut(bnb,routePath)[1];
        }else {
            isbnb=_USDT;
            address[] memory routePath = new address[](3);
            routePath[0] = _WBNB;
            routePath[1] = isbnb;
            routePath[2] = _tolens;
            return IRouter(_router).getAmountsOut(bnb,routePath)[2];
        }
    }
    function getToken2Price(address token,address bnbOrUsdt,uint bnb) view public returns(uint){
        if(token == address(0) || bnbOrUsdt == address(0)) return 0;
        address isbnb;
        if(bnbOrUsdt == _WBNB){
            isbnb=_WBNB;
            address[] memory routePath = new address[](2);
            routePath[0] = token;
            routePath[1] = isbnb;
            return IRouter(_router).getAmountsOut(bnb,routePath)[1];
        }else {
            isbnb=_USDT;
            address[] memory routePath = new address[](3);
            routePath[0] = token;
            routePath[1] = isbnb;
            routePath[2] = _WBNB;
            return IRouter(_router).getAmountsOut(bnb,routePath)[2];
        }
        
    }
    function getToken3Price(address token,address bnbOrUsdt) view public returns(uint){
        if(token == address(0) || bnbOrUsdt == address(0)) return 0;
        address isbnb;
        if(bnbOrUsdt == _WBNB){
            isbnb=_WBNB;
            address[] memory routePath = new address[](3);
            routePath[0] = token;
            routePath[1] = isbnb;
            routePath[2] = _USDT;
            return IRouter(_router).getAmountsOut(1 ether,routePath)[2];
        }else {
            isbnb=_USDT;
            address[] memory routePath = new address[](2);
            routePath[0] = token;
            routePath[1] = _USDT;
            return IRouter(_router).getAmountsOut(1 ether,routePath)[1];
        }
        
    }
    function getMyShort(address _tokens,address bnbOrUsdt,uint bnb,uint oldPrice) view private  returns(uint){
        uint nowPrice=getTokenPrice(_tokens,bnbOrUsdt,bnb);
        uint zt=nowPrice * 1 ether / oldPrice;
        return bnb*zt/1 ether;
    }
    function getMyPrice(address token) view public returns(uint,uint){
        if(Short[_msgSender()][token].bnb==0){
            return (0,mySells[_msgSender()].mnu);
        }
        uint nowPrice=getTokenPrice(token,Short[_msgSender()][token].token,Short[_msgSender()][token].bnb);
        uint zt;
        if(nowPrice < Short[_msgSender()][token].tokenPrice){
          zt=nowPrice * 1 ether / Short[_msgSender()][token].tokenPrice;
        }else {
          zt=nowPrice * 1 ether / Short[_msgSender()][token].tokenPrice;
        }
        return (Short[_msgSender()][token].bnb*zt/1 ether,mySells[_msgSender()].mnu);
    }
    function getMyPriceSell() view public returns(address[] memory add,uint[] memory,uint[] memory){
        add=mySells[_msgSender()].coin;
        uint[] memory routePath = new uint[](add.length);
        uint[] memory routetoken = new uint[](add.length);
        //getToken3Price
        if(add.length == 0){
           return (add,routePath,routetoken); 
        }
        for(uint i=0;i< add.length;i++){
            if(Short[_msgSender()][add[i]].bnb > 0){
               routePath[i]=Short[_msgSender()][add[i]].bnb;
               (,,uint _bbnb)=getShortPrice(_msgSender(),add[i]);
               routetoken[i]=_bbnb;  
            }    
        }
        return (add,routePath,routetoken); 
    }
    function getShortPrice(address my,address token0) private view returns (uint,uint,uint) {
        uint nowPrice=getTokenPrice(token0,Short[my][token0].token,Short[my][token0].bnb);
        uint zt=nowPrice * 1 ether / Short[my][token0].tokenPrice;
        uint istoken=Short[my][token0].bnb*zt/1 ether;
        return (nowPrice,zt,istoken);
    }
    function getShorts(address token) public view returns (uint,uint,uint,uint,uint) {
        if(mkt.balanceOf(token) <=0){
         return (sum,settleAccounts,mkt.balanceOf(token),mkt.balanceOfLook(token),0); 
        }
        uint tos=getToken2Price(token,address(Short[_msgSender()][token].token),mkt.balanceOf(token));
        return (sum,settleAccounts,mkt.balanceOf(token),mkt.balanceOfLook(token),tos); 
    }
    function getShortsMoV(address token,address token1) public view returns (uint){
        if(mkt.balanceOf(token)==0) return 0;
        uint isV=getToken2Price(token,token1,mkt.balanceOf(token))/10;
        if(isV>Short[msg.sender][token].bnb){
            return isV-Short[msg.sender][token].bnb;
        }else{
            return 0;
        }
    }
    function getNewTokenPrice(address my,address token,address token1) view private  returns(bool){
        uint oldOrice=tokenPrice[my][token];
        uint newPrice=getToken2Price(token,token1,1 ether);
        if(newPrice > oldOrice * 105 /100){
           return true;
        }
        if(oldOrice > newPrice * 105 /100){
           return true;
        }
    }
    function getTokenName(address token)public view returns(string memory,string memory,uint){
        string memory pair;
        if(mkt.getPair(token)!=address(0)){
            if(mkt.getPair(token)==_USDT){
                pair="USDT";
            }else{
               pair="BNB"; 
            }

        }else {
            pair="No pair";
        }
        return (IERC20(token).symbol(),pair,IERC20(token).balanceOf(msg.sender));
    }
}