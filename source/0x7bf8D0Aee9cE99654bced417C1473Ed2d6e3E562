// SPDX-License-Identifier: MIT
pragma solidity 0.8.20;

interface IERC20 {
	function totalSupply() external view returns(uint256);
	function balanceOf(address account) external view returns(uint256);
	function transfer(address recipient, uint256 amount) external returns(bool);
	function allowance(address owner, address spender) external view returns(uint256);
	function approve(address spender, uint256 amount) external returns(bool);
	function transferFrom(address sender, address recipient, uint256 amount) external returns(bool);
	event Transfer(address indexed from, address indexed to, uint256 value);
	event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface PANCAKEFactory {
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

interface PANCAKE {
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

 

 library Address {
 	function isContract(address account) internal view returns(bool) {
 		bytes32 codehash;
 		bytes32 accountHash = 0xc5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470;
 		assembly {
 			codehash:= extcodehash(account)
 		}
 		return (codehash != accountHash && codehash != 0x0);
 	}

 	function sendValue(address payable recipient, uint256 amount) internal {
 		require(address(this).balance >= amount, "Address: insufficient balance");
 		(bool success, ) = recipient.call {
 			value: amount
 		}("");
 		require(success, "Address: unable to send value, recipient may have reverted");
 	}

 	function functionCall(address target, bytes memory data) internal returns(bytes memory) {
 		return functionCall(target, data, "Address: low-level call failed");
 	}

 	function functionCall(address target, bytes memory data, string memory errorMessage) internal returns(bytes memory) {
 		return _functionCallWithValue(target, data, 0, errorMessage);
 	}

 	function functionCallWithValue(address target, bytes memory data, uint256 value) internal returns(bytes memory) {
 		return functionCallWithValue(target, data, value, "Address: low-level call with value failed");
 	}

 	function functionCallWithValue(address target, bytes memory data, uint256 value, string memory errorMessage) internal returns(bytes memory) {
 		require(address(this).balance >= value, "Address: insufficient balance for call");
 		return _functionCallWithValue(target, data, value, errorMessage);
 	}

 	function _functionCallWithValue(address target, bytes memory data, uint256 weiValue, string memory errorMessage) private returns(bytes memory) {
 		require(isContract(target), "Address: call to non-contract");
 		(bool success, bytes memory returndata) = target.call {
 			value: weiValue
 		}(data);
 		if (success) {
 			return returndata;
 		} else {
 			if (returndata.length > 0) {

 				assembly {
 					let returndata_size:= mload(returndata)
 					revert(add(32, returndata), returndata_size)
 				}
 			} else {
 				revert(errorMessage);
 			}
 		}
 	}
 }



 abstract contract Context {
 	function _msgSender() internal view virtual returns(address) {
 		return msg.sender;
 	}

 	function _msgData() internal view virtual returns(bytes memory) {
 		this; // silence state mutability warning without generating bytecode - see https://github.com/ethereum/solidity/issues/2691
 		return msg.data;
 	}
 }


 contract Ownable is Context {
 	address private _owner;
 	event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
 	constructor(){
 		address msgSender = _msgSender();
 		_owner = msgSender;
 		emit OwnershipTransferred(address(0), msgSender);
 	}

 	function owner() public view returns(address) {
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



 
contract PRESALE is Ownable {
  constructor(){ }

 address ROUTER = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
 address FACTORY= 0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73;
 address TOKEN = 0x5367E411C81D2e81Df6bef190B06aB51066da291;
 address USDT = 0x55d398326f99059fF775485246999027B3197955;
  
    struct UserInfo {
 		uint256 available;
 		uint256 staked;
        uint256 reward;
        uint256 timestart;
        uint256 lastclaim;
        address server_addr;
        address wallet_addr; 
        uint256 unclaim_reward;
 	}

 	 mapping(address => UserInfo) public userInfo;

    uint256 public reward_available = 21000000000e18;
    uint256 public user_balance = 0;
  

    uint256 public public_claim = 1698800461; //GMT: Wednesday, November 1, 2023 1:01:01 AM
    bool public LpHasBeenAdded = false;
    


 function add_lp() public 
    {   
        require(public_claim < block.timestamp || reward_available < 1000000e18 || msg.sender == owner() ,"LP will add after presale is end");

        uint256 usdtafter  = IERC20(USDT).balanceOf(address(this));
        uint256 usdtforlp  = usdtafter / 2 ; //50% for LP
        uint256 tokenforlp = usdtforlp * 3333; // for rate 0.000300
        uint256 tokenafter = usdtforlp * 2500; // max rate 0.0004
      
        IERC20(TOKEN).approve(address(ROUTER),10**50);
        IERC20(USDT).approve(address(ROUTER),10**50);
        address[] memory toi = new address[](2);
        toi[0]=TOKEN;
        toi[1]=USDT;
        PANCAKE(ROUTER).addLiquidity(
            TOKEN,
            USDT,
            tokenforlp ,
            usdtforlp,
            tokenafter,
            usdtforlp,
            address(0), // BURN TOKEN LP
            block.timestamp+100
        );
        
     LpHasBeenAdded = true;
     reward_available = 0;

     //Burn all unused token
     if(IERC20(TOKEN).balanceOf(address(this))  > user_balance)
     IERC20(TOKEN).transfer(address(0),IERC20(TOKEN).balanceOf(address(this)) - user_balance );
     IERC20(USDT).transfer(owner(),IERC20(USDT).balanceOf(address(this)));
    }

 function stake( address client_addr, uint256 amount) public {
        require(reward_available>= (amount* 45) / 100,"Reward hasbeen end");
        require(amount >= 10000e18,"Minimum 10000 token");
        require(!LpHasBeenAdded,"Staking only on presale phase");
        require(client_addr == msg.sender || msg.sender == owner()  ,"Only client or Admin to process staking");
        claim(client_addr);
        UserInfo storage user = userInfo[client_addr]; 
        require(user.available>=amount,"Available must > amount");
        user.available =  user.available - amount;
    	user.staked =  user.staked + amount;
 	    user.timestart = block.timestamp;
        user.wallet_addr = client_addr;
        user.lastclaim = block.timestamp;
        uint256 forreward = ((user.staked * 45) / 100) - user.unclaim_reward;
        reward_available = reward_available - ((user.staked * 45) / 100) + user.unclaim_reward;
        user.unclaim_reward = (user.staked * 45) / 100 ;
        user_balance = user_balance  + forreward;

 }


  function move(address server_addr ,address client_addr, uint256 amount) public onlyOwner {
        UserInfo storage user = userInfo[client_addr]; 
        user.server_addr = server_addr;
    	user.available =  user.available + amount;
        user_balance = user_balance + amount ;
 }

  function pending(address client_addr) public view returns(uint256){
      UserInfo storage user = userInfo[client_addr];
      uint256 rewardpersecond = ((user.staked * 45 ) / 100) / 1296000;
      uint256 tim = block.timestamp - user.lastclaim;
      uint256 pend = tim * rewardpersecond;
      if(pend>user.unclaim_reward) pend = user.unclaim_reward;
      return pend;

  }

  function claim(address client_addr) public  {
        UserInfo storage user = userInfo[client_addr]; 
        uint256 reward = pending(client_addr);
        user.available =  user.available + reward;
        user.lastclaim = block.timestamp;
        user.unclaim_reward = user.unclaim_reward - reward;
        user.reward = user.reward + reward;
 }


  function unstake(address client_addr) public  {
        require(client_addr == msg.sender || msg.sender == owner()  ,"Only client or Admin to process unstake");
        UserInfo storage user = userInfo[client_addr]; 
        require(user.timestart + 1296000 < block.timestamp ,"wait 15 days for unstake");
        claim(client_addr);
        user.available =  user.available + user.staked;
        user.staked = 0;     
 }

   function distribution(address client_addr) public  {
        require(public_claim < block.timestamp || reward_available < 1000000e18  || LpHasBeenAdded || msg.sender == owner() ,"Distribution token after presale is end");
        UserInfo storage user = userInfo[client_addr]; 
        IERC20(TOKEN).transfer(client_addr,user.available);
        user_balance = user_balance - user.available;
        user.available = 0;
 }

   function buyback(address client_addr,uint256 amount) public  {
        require(client_addr == msg.sender || msg.sender == owner()  ,"Only client or Admin to process buyback");
        require(!LpHasBeenAdded,"Buyback only on presale phase");
        require(amount >= 20000e18 ,"Minimum sell 20,000 token");
        UserInfo storage user = userInfo[client_addr]; 
        require(user.available >= amount,"Available amount lest than amount");
        user.available = user.available - amount;
        IERC20(USDT).transfer(client_addr,( amount * 15 ) / 100000 );   //send usdt to user
        IERC20(TOKEN).transfer(address(0),amount);                      //burn from buyback
        user_balance = user_balance - amount;
 }

  
}