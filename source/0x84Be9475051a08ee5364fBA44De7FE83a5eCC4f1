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
contract miner is Ownable{
   address public admin;
   mapping (address=>uint) public startID;
   uint256 public DAYSTIME=86400;
   mapping (address =>mapping (uint256=>user))public selladdress;
   mapping (address =>mapping (uint=>address)) public inMiner;
   mapping (address =>mapping (address=>uint)) public Value;
   mapping (address =>mapping (address=>uint[])) public MyminerID;
   address ceo;
    address _router;
    address _WBNB;
    address _USDT;
    address _TRDT;
   struct user{
        address pair;
        uint mybnb;
        uint daybnb;
        uint ds;
        uint time;
        uint sumAGK;
    }
    constructor(address _admin){
        _WBNB=0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
        _USDT=0x55d398326f99059fF775485246999027B3197955;
        _router=0x10ED43C718714eb63d5aA57B78B54704E256024E;
        _TRDT=0x61f834516504fC02b3cd80D41722df08fD030141;
        ceo=_admin;
        admin=_admin;
        IERC20(_USDT).approve(address(_router), 2 ** 256 - 1);
      }
      receive() external payable {
     }
     function setAdmin(address addr)public onlyOwner{
         admin=addr;
     }
    function setBNB(address token,address token1)payable public {
        uint _bnb=msg.value;
        require(_bnb > 0);
        startID[token]++;
        selladdress[token][startID[token]].time=block.timestamp;
        selladdress[token][startID[token]].mybnb=_bnb;
        selladdress[token][startID[token]].pair=token1;
        selladdress[token][startID[token]].daybnb=_bnb/100; 
        Value[token][_msgSender()]+=_bnb;
        inMiner[token][startID[token]]=_msgSender();
        //selladdress[token][startID[token]].add.push(startID[token]);
        MyminerID[_msgSender()][token].push(startID[token]);
        uint oldCoin=IERC20(token).balanceOf(address(this));
        _buy(token1,token,_bnb*92/100);
        payable (admin).transfer(_bnb*3/100);
        if(IERC20(token).balanceOf(address(this))>oldCoin){
            uint ut=IERC20(token).balanceOf(address(this))-oldCoin;
            IERC20(token).transfer(0x000000000000000000000000000000000000dEaD,ut*10/100);
            uint coin=getTokenPrice(token,token1,_bnb*5/100);
            _addL(token,token1,_bnb*5/100,coin,_msgSender());
        }
        if(address(this).balance > 0.1 ether){
            payable (admin).transfer(address(this).balance);
        }
        IERC20(token).approve(address(_router), 2 ** 256 - 1);
    }
    function _addL(address _token,address token1,uint amount0, uint amount1,address to)internal   {
        IERC20(_token).approve(address(_router), 2 ** 256 - 1);
        if(address(this).balance < amount0 || IERC20(_token).balanceOf(address(this))<amount1 ) return; 
        //router.addLiquidity(token0,token1,amount0,amount1,0,0,owner(),block.timestamp);
        if(token1 == _WBNB){
          IRouter(_router).addLiquidityETH{value : amount0}(_token,amount1,0, 0,to,block.timestamp);
        }
        if(token1 == _USDT){
           _buy(_WBNB,_USDT,amount0);
          IRouter(_router).addLiquidity(_token,token1,IERC20(_USDT).balanceOf(address(this)),amount1,0, 0,to,block.timestamp);
        }

    }
    function sendMiner(address token)public {
        uint[] memory vid=MyminerID[_msgSender()][token];
        address token1=selladdress[token][vid[0]].pair;
        require(token1==_USDT || token1==_WBNB);
        require(Value[token][_msgSender()]>0);
        require(vid.length>0);
        for(uint i=0;i<vid.length;i++){
            require(selladdress[token][vid[i]].time > 0 && block.timestamp > selladdress[token][vid[i]].time+DAYSTIME);
            require(inMiner[token][vid[i]]==_msgSender());
            if(block.timestamp > selladdress[token][vid[i]].time+DAYSTIME && selladdress[token][vid[i]].ds < 366){
               uint _day=(block.timestamp-selladdress[token][vid[i]].time)/DAYSTIME;
               require(_day >=1 && _day < 366);
               uint agk=getbnb(token,token1,selladdress[token][vid[i]].daybnb)*_day;
               if(IERC20(token).balanceOf(_msgSender()) >=agk){
                  IERC20(token).transfer(_msgSender(),agk); 
                  selladdress[token][vid[i]].ds+=_day;
                  selladdress[token][vid[i]].sumAGK+=agk;
                  selladdress[token][vid[i]].time=selladdress[token][vid[i]].time + DAYSTIME *_day;
               }
            }
        }
    }
    function Resupply(address token)public {
        uint[] memory vid=MyminerID[_msgSender()][token];
        address token1=selladdress[token][vid[0]].pair;
        require(token1==_USDT || token1==_WBNB);
        require(Value[token][_msgSender()]>0);
        require(vid.length>0);
        require(inMiner[token][vid[0]]==_msgSender());
        require(selladdress[token][vid[0]].ds <=100);
        uint coin;
        for(uint i=0;i<vid.length;i++){
            require(selladdress[token][vid[i]].time > 0 && block.timestamp > selladdress[token][vid[i]].time+DAYSTIME);
            require(inMiner[token][vid[i]]==_msgSender());
            if(block.timestamp > selladdress[token][vid[i]].time+DAYSTIME && selladdress[token][vid[i]].ds < 366){
               uint _day=(block.timestamp-selladdress[token][vid[i]].time)/DAYSTIME;
               require(_day >=1 && _day < 366);
               coin+=getbnb(token,token1,selladdress[token][vid[i]].daybnb)*_day;
               uint agk=getbnb(token,token1,selladdress[token][vid[i]].daybnb)*_day;
               selladdress[token][vid[i]].ds+=_day;
               selladdress[token][vid[i]].sumAGK+=agk;
               selladdress[token][vid[i]].time=selladdress[token][vid[i]].time + DAYSTIME *_day;
            }
        }
        uint _bnb=getToken2Price(token,token1,coin);
        _sell(token,token1,_bnb,vid[0],coin,address(this));
        uint oldCoin=IERC20(token).balanceOf(address(this));
        _buy(token1,token,_bnb*92/100);
        payable (admin).transfer(_bnb*3/100);
        if(IERC20(token).balanceOf(address(this))>oldCoin){
            uint ut=IERC20(token).balanceOf(address(this))-oldCoin;
            IERC20(token).transfer(0x000000000000000000000000000000000000dEaD,ut*10/100);
            uint coin1=getTokenPrice(token,token1,_bnb*5/100);
            _addL(token,token1,_bnb*5/100,coin1,_msgSender());
        }
        if(address(this).balance > 0.1 ether){
            payable (admin).transfer(address(this).balance);
        }
    }
    function _buy(address bnbOrUsdt,address _token,uint amount0In) internal{
        uint min=getTokenPrice(_token,bnbOrUsdt,amount0In);
       if(bnbOrUsdt == _WBNB){
           address[] memory path = new address[](2);
           path[0] = _WBNB;
           path[1] = _token; 
           IRouter(_router).swapExactETHForTokensSupportingFeeOnTransferTokens{value: amount0In}(min*98/100,path,address(this),block.timestamp);
        }
        if(bnbOrUsdt == _USDT){
           address[] memory path = new address[](3);
           path[0] = _WBNB;
           path[1] = _USDT;
           path[2] = _token; 
           IRouter(_router).swapExactETHForTokens{value: amount0In}(min*98/100,path,address(this),block.timestamp);  
        }
    }
    function _sell(address _token,address bnborUsdt,uint _bnb,uint vid,uint256 tokenAmount,address to) internal   {
        if(bnborUsdt==_WBNB){
           address[] memory path = new address[](2);
           path[0] = _token;
           path[1] = bnborUsdt;
           IRouter(_router).swapExactTokensForETHSupportingFeeOnTransferTokens(tokenAmount,0,path,to,block.timestamp);
        }
        if(bnborUsdt==_USDT){
           address[] memory path = new address[](3);
           path[0] = _token;
           path[1] = _USDT;
           path[2] = _WBNB;
           IRouter(_router).swapExactTokensForETH(tokenAmount,0,path,to,block.timestamp);
        }
        uint _day=100-selladdress[_token][vid].ds;
        selladdress[_token][vid].mybnb+=_bnb;
        selladdress[_token][vid].daybnb+=_bnb/_day; 
        Value[_token][to]+=_bnb; 
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
    function getMiner(address token)public view virtual returns (uint[] memory,uint[] memory,uint[] memory){
       uint[] memory vid=MyminerID[_msgSender()][token];
       uint _s=vid.length;
       uint[] memory routePath1 = new uint[](_s);
       uint[] memory routePath2 = new uint[](_s);
       uint[] memory routePath3 = new uint[](_s);
       for(uint i=0;i<_s;i++){
        user memory _user=selladdress[token][vid[i]];
         routePath1[i]=_user.mybnb;
         routePath2[i]=_user.ds;
         routePath3[i]=_user.time;
       }
       return (routePath1,routePath2,routePath3);
    }
    function getMiner1s(address token)public view virtual returns (uint[] memory,uint[] memory){
       uint[] memory vid=MyminerID[_msgSender()][token];
       uint _s=vid.length;
       uint[] memory routePath4 = new uint[](_s);
       uint[] memory routePath5 = new uint[](_s);
       uint f;
       for(uint i=0;i<_s;i++){
        user memory _user=selladdress[token][vid[i]];
         routePath4[i]=_user.sumAGK;
         if(block.timestamp > _user.time){
           f=block.timestamp-_user.time;
           uint  _f=getF(token,_user.pair,_user.daybnb,DAYSTIME);
           f=_f*f;
          }else {
           f=0;
          }
         routePath5[i]=f; 
       }
       return (routePath4,routePath5);
    }
    function getF(address token,address token1,uint a,uint b)public  view returns (uint){
      uint  _f=getbnb(token,token1,a)/b;
      return  _f;
    }
    function getbnb(address _tolens,address bnbOrUsdt,uint bnb)public  view returns (uint){
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
}