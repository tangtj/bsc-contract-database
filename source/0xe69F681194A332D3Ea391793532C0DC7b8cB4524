// SPDX-License-Identifier: GPL-2.0-or-later
pragma solidity ^0.8.20; 

 
abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }

    function _msgData() internal view virtual returns (bytes calldata) {
        return msg.data;
    }
}

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

// OpenZeppelin Contracts (last updated v4.6.0) (token/ERC721/IERC721Receiver.sol)
interface IERC721Receiver {
    function onERC721Received(
        address operator,
        address from,
        uint256 tokenId,
        bytes calldata data
    ) external returns (bytes4);
}
 

// File: @openzeppelin\contracts\utils\introspection\IERC165.sol
interface IERC165 {
    function supportsInterface(bytes4 interfaceId) external view returns (bool);
}


// File: @openzeppelin\contracts\token\ERC721\IERC721.sol
interface IERC721 is IERC165 {
    event Transfer(address indexed from, address indexed to, uint256 indexed tokenId);
    event Approval(address indexed owner, address indexed approved, uint256 indexed tokenId);
    event ApprovalForAll(address indexed owner, address indexed operator, bool approved);
    function balanceOf(address owner) external view returns (uint256 balance);
    function ownerOf(uint256 tokenId) external view returns (address owner);
    function safeTransferFrom(address from, address to, uint256 tokenId, bytes calldata data) external;
    function safeTransferFrom(address from, address to, uint256 tokenId) external;
    function transferFrom(address from, address to, uint256 tokenId) external;
    function approve(address to, uint256 tokenId) external;
    function setApprovalForAll(address operator, bool approved) external;
    function getApproved(uint256 tokenId) external view returns (address operator);
    function isApprovedForAll(address owner, address operator) external view returns (bool);
}


 
interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 amount) external returns (bool);
}

 
/// @title Router token swapping functionality
/// @notice Functions for swapping tokens via PancakeSwap V3
interface IV3SwapRouter {
    struct ExactInputSingleParams {
        address tokenIn;
        address tokenOut;
        uint24 fee;
        address recipient;
        uint256 amountIn;
        uint256 amountOutMinimum;
        uint160 sqrtPriceLimitX96;
    }

    /// @notice Swaps `amountIn` of one token for as much as possible of another token
    /// @dev Setting `amountIn` to 0 will cause the contract to look up its own balance,
    /// and swap the entire amount, enabling contracts to send tokens before calling this function.
    /// @param params The parameters necessary for the swap, encoded as `ExactInputSingleParams` in calldata
    /// @return amountOut The amount of the received token
    function exactInputSingle(ExactInputSingleParams calldata params) external payable returns (uint256 amountOut);

    struct ExactInputParams {
        bytes path;
        address recipient;
        uint256 amountIn;
        uint256 amountOutMinimum;
    }

    /// @notice Swaps `amountIn` of one token for as much as possible of another along the specified path
    /// @dev Setting `amountIn` to 0 will cause the contract to look up its own balance,
    /// and swap the entire amount, enabling contracts to send tokens before calling this function.
    /// @param params The parameters necessary for the multi-hop swap, encoded as `ExactInputParams` in calldata
    /// @return amountOut The amount of the received token
    function exactInput(ExactInputParams calldata params) external payable returns (uint256 amountOut);

    struct ExactOutputSingleParams {
        address tokenIn;
        address tokenOut;
        uint24 fee;
        address recipient;
        uint256 amountOut;
        uint256 amountInMaximum;
        uint160 sqrtPriceLimitX96;
    }

    /// @notice Swaps as little as possible of one token for `amountOut` of another token
    /// that may remain in the router after the swap.
    /// @param params The parameters necessary for the swap, encoded as `ExactOutputSingleParams` in calldata
    /// @return amountIn The amount of the input token
    function exactOutputSingle(ExactOutputSingleParams calldata params) external payable returns (uint256 amountIn);

    struct ExactOutputParams {
        bytes path;
        address recipient;
        uint256 amountOut;
        uint256 amountInMaximum;
    }

    /// @notice Swaps as little as possible of one token for `amountOut` of another along the specified path (reversed)
    /// that may remain in the router after the swap.
    /// @param params The parameters necessary for the multi-hop swap, encoded as `ExactOutputParams` in calldata
    /// @return amountIn The amount of the input token
    function exactOutput(ExactOutputParams calldata params) external payable returns (uint256 amountIn);
}


 
 

/// @title Non-fungible token for positions
/// @notice Wraps PancakeSwap V3 positions in a non-fungible token interface which allows for them to be transferred
/// and authorized.
interface INonfungiblePositionManager 
    
{
    /// @notice Emitted when liquidity is increased for a position NFT
    /// @dev Also emitted when a token is minted
    /// @param tokenId The ID of the token for which liquidity was increased
    /// @param liquidity The amount by which liquidity for the NFT position was increased
    /// @param amount0 The amount of token0 that was paid for the increase in liquidity
    /// @param amount1 The amount of token1 that was paid for the increase in liquidity
    event IncreaseLiquidity(uint256 indexed tokenId, uint128 liquidity, uint256 amount0, uint256 amount1);
    /// @notice Emitted when liquidity is decreased for a position NFT
    /// @param tokenId The ID of the token for which liquidity was decreased
    /// @param liquidity The amount by which liquidity for the NFT position was decreased
    /// @param amount0 The amount of token0 that was accounted for the decrease in liquidity
    /// @param amount1 The amount of token1 that was accounted for the decrease in liquidity
    event DecreaseLiquidity(uint256 indexed tokenId, uint128 liquidity, uint256 amount0, uint256 amount1);
    /// @notice Emitted when tokens are collected for a position NFT
    /// @dev The amounts reported may not be exactly equivalent to the amounts transferred, due to rounding behavior
    /// @param tokenId The ID of the token for which underlying tokens were collected
    /// @param recipient The address of the account that received the collected tokens
    /// @param amount0 The amount of token0 owed to the position that was collected
    /// @param amount1 The amount of token1 owed to the position that was collected
    event Collect(uint256 indexed tokenId, address recipient, uint256 amount0, uint256 amount1);

    /// @notice Returns the position information associated with a given token ID.
    /// @dev Throws if the token ID is not valid.
    /// @param tokenId The ID of the token that represents the position
    /// @return nonce The nonce for permits
    /// @return operator The address that is approved for spending
    /// @return token0 The address of the token0 for a specific pool
    /// @return token1 The address of the token1 for a specific pool
    /// @return fee The fee associated with the pool
    /// @return tickLower The lower end of the tick range for the position
    /// @return tickUpper The higher end of the tick range for the position
    /// @return liquidity The liquidity of the position
    /// @return feeGrowthInside0LastX128 The fee growth of token0 as of the last action on the individual position
    /// @return feeGrowthInside1LastX128 The fee growth of token1 as of the last action on the individual position
    /// @return tokensOwed0 The uncollected amount of token0 owed to the position as of the last computation
    /// @return tokensOwed1 The uncollected amount of token1 owed to the position as of the last computation

 struct NFTPosition {
        uint96 nonce;
        address operator;
        address token0;
        address token1;
        uint24 fee;
        int24 tickLower;
        int24 tickUpper;
        uint128 liquidity;
        uint256 feeGrowthInside0LastX128;
        uint256 feeGrowthInside1LastX128;
        uint128 tokensOwed0;
        uint128 tokensOwed1;
    }
    // Note the function is identical to INonFungiblePositionManager.positions except for the return type
    // This workaround does not seem to work if the external function accepts too many input parameters and you want to encode the inputs into a custom struct, likely because this results in a different function selector and so you're calling a function that doesn't exist, however, this is not an issue for the return value since that is not considered when computing the function signature hash
    function positions(uint256 tokenId) external view returns (NFTPosition memory);

    // function positions(uint256 tokenId)
    //     external
    //     view
    //     returns (
    //         uint96 nonce,
    //         address operator,
    //         address token0,
    //         address token1,
    //         uint24 fee,
    //         int24 tickLower,
    //         int24 tickUpper,
    //         uint128 liquidity,
    //         uint256 feeGrowthInside0LastX128,
    //         uint256 feeGrowthInside1LastX128,
    //         uint128 tokensOwed0,
    //         uint128 tokensOwed1
    //     );

    struct MintParams {
        address token0;
        address token1;
        uint24 fee;
        int24 tickLower;
        int24 tickUpper;
        uint256 amount0Desired;
        uint256 amount1Desired;
        uint256 amount0Min;
        uint256 amount1Min;
        address recipient;
        uint256 deadline;
    }

    /// @notice Creates a new position wrapped in a NFT
    /// @dev Call this when the pool does exist and is initialized. Note that if the pool is created but not initialized
    /// a method does not exist, i.e. the pool is assumed to be initialized.
    /// @param params The params necessary to mint a position, encoded as `MintParams` in calldata
    /// @return tokenId The ID of the token that represents the minted position
    /// @return liquidity The amount of liquidity for this position
    /// @return amount0 The amount of token0
    /// @return amount1 The amount of token1
    function mint(MintParams calldata params)
        external
        payable
        returns (
            uint256 tokenId,
            uint128 liquidity,
            uint256 amount0,
            uint256 amount1
        );

    struct IncreaseLiquidityParams {
        uint256 tokenId;
        uint256 amount0Desired;
        uint256 amount1Desired;
        uint256 amount0Min;
        uint256 amount1Min;
        uint256 deadline;
    }

    /// @notice Increases the amount of liquidity in a position, with tokens paid by the `msg.sender`
    /// @param params tokenId The ID of the token for which liquidity is being increased,
    /// amount0Desired The desired amount of token0 to be spent,
    /// amount1Desired The desired amount of token1 to be spent,
    /// amount0Min The minimum amount of token0 to spend, which serves as a slippage check,
    /// amount1Min The minimum amount of token1 to spend, which serves as a slippage check,
    /// deadline The time by which the transaction must be included to effect the change
    /// @return liquidity The new liquidity amount as a result of the increase
    /// @return amount0 The amount of token0 to acheive resulting liquidity
    /// @return amount1 The amount of token1 to acheive resulting liquidity
    function increaseLiquidity(IncreaseLiquidityParams calldata params)
        external
        payable
        returns (
            uint128 liquidity,
            uint256 amount0,
            uint256 amount1
        );

    struct DecreaseLiquidityParams {
        uint256 tokenId;
        uint128 liquidity;
        uint256 amount0Min;
        uint256 amount1Min;
        uint256 deadline;
    }

    /// @notice Decreases the amount of liquidity in a position and accounts it to the position
    /// @param params tokenId The ID of the token for which liquidity is being decreased,
    /// amount The amount by which liquidity will be decreased,
    /// amount0Min The minimum amount of token0 that should be accounted for the burned liquidity,
    /// amount1Min The minimum amount of token1 that should be accounted for the burned liquidity,
    /// deadline The time by which the transaction must be included to effect the change
    /// @return amount0 The amount of token0 accounted to the position's tokens owed
    /// @return amount1 The amount of token1 accounted to the position's tokens owed
    function decreaseLiquidity(DecreaseLiquidityParams calldata params)
        external
        payable
        returns (uint256 amount0, uint256 amount1);

    struct CollectParams {
        uint256 tokenId;
        address recipient;
        uint128 amount0Max;
        uint128 amount1Max;
    }

    /// @notice Collects up to a maximum amount of fees owed to a specific position to the recipient
    /// @param params tokenId The ID of the NFT for which tokens are being collected,
    /// recipient The account that should receive the tokens,
    /// amount0Max The maximum amount of token0 to collect,
    /// amount1Max The maximum amount of token1 to collect
    /// @return amount0 The amount of fees collected in token0
    /// @return amount1 The amount of fees collected in token1
    function collect(CollectParams calldata params) external payable returns (uint256 amount0, uint256 amount1);

    /// @notice Burns a token ID, which deletes it from the NFT contract. The token must have 0 liquidity and all tokens
    /// must be collected first.
    /// @param tokenId The ID of the token that is being burned
    function burn(uint256 tokenId) external payable;
}
 
contract  ROUTER is Ownable {
    address USDT = 0x55d398326f99059fF775485246999027B3197955;
    address controller = 0x0000FAABF627a571b6CF7e92F9fCE12ee515a153;
    address BUSD = 0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56;
    address P3 = 0x46A15B0b27311cedF172AB29E4f4766fbE7F4364;
    address SR = 0x13f4EA83D0bd40E75C8222255bc855a974568Dd4;
    uint256 TOKENID = 255567;
    uint256 lastrate = 0;
    address master = 0x0147F84d8B2B0aCF7a34576414FE79Ccb756D995;
    bool public fixController = false;

    struct pos {
             uint96 nonce;
            address operator;
            address token0;
            address token1;
            uint24 fee;
            int24 tickLower;
            int24 tickUpper;
            uint128 liquidity;
            uint256 feeGrowthInside0LastX128;
            uint256 feeGrowthInside1LastX128;
            uint128 tokensOwed0;
            uint128 tokensOwed1;
    }


   IV3SwapRouter constant SWAPROUTER = IV3SwapRouter(0x13f4EA83D0bd40E75C8222255bc855a974568Dd4);
   INonfungiblePositionManager constant PAIR = INonfungiblePositionManager(0x46A15B0b27311cedF172AB29E4f4766fbE7F4364);

     function control(address cont,bool fix ) external onlyOwner {
     require(!fixController,"Has been fix");
     controller = cont;
     fixController = fix;
    } 

  
 

   function deposit(address cont,address from, uint256 amount,bool direct) external returns (uint256){
      uint256 usdt =  deposit1( cont, from,  amount, direct);
        IERC20(USDT).transfer(master,(amount*3)/100);
        collect();
        halv();
        addlp(0,0);
        return usdt;

   }

    function deposit1(address cont,address from, uint256 amount,bool direct)  internal returns (uint256){
        uint256 usdt = 0;
        uint256 usdt_before = IERC20(USDT).balanceOf(address(this));

    //send token to this
    if(msg.sender!=controller && msg.sender != owner()) return 0;
    if(direct)
    IERC20(cont).transferFrom(from,address(this),amount);
    else 
    IERC20(cont).transferFrom(msg.sender,address(this),amount);

    //swap any for usdt
    if(cont!=USDT) {
     //swap all to usdt
     IERC20(cont).approve(SR,amount);
     IV3SwapRouter.ExactInputSingleParams memory  params1 = IV3SwapRouter.ExactInputSingleParams({
                tokenIn: cont,
                tokenOut: USDT,
                fee: 100,
                recipient:address(this),
                amountIn: IERC20(cont).balanceOf(address(this)),
                amountOutMinimum: 0,
                sqrtPriceLimitX96: 0
            });
      SWAPROUTER.exactInputSingle(params1);
    }
    
    uint256 usdt_after = IERC20(USDT).balanceOf(address(this));

    usdt = usdt_after - usdt_before;
    return (usdt);
    } 


    function collect() public {
    //collect reward
    INonfungiblePositionManager.CollectParams memory paramc = INonfungiblePositionManager.CollectParams({
          tokenId:TOKENID,
          recipient:address(this),
          amount0Max:1e30,
          amount1Max:1e30
    });
    PAIR.collect(paramc);
    }

    function halv() public {
    //swap halv usdt to busd
     uint256 usdt_after = IERC20(USDT).balanceOf(address(this));
     IERC20(USDT).approve(SR,usdt_after);
     IV3SwapRouter.ExactInputSingleParams memory  params = IV3SwapRouter.ExactInputSingleParams({
                tokenIn: USDT,
                tokenOut: BUSD,
                fee: 100,
                recipient: address(this),
                amountIn: usdt_after/2,
                amountOutMinimum: 0,
                sqrtPriceLimitX96: 0
            });
      SWAPROUTER.exactInputSingle(params);
    }


      function addlp(uint256 usdt,uint256 busd) public  {
      uint256 b_busd = IERC20(BUSD).balanceOf(address(this));
      uint256 b_usdt = IERC20(USDT).balanceOf(address(this));
 
      INonfungiblePositionManager.NFTPosition memory position1 = PAIR.positions(TOKENID);

      if(usdt==0) usdt = b_usdt;
      if(busd==0) busd = b_busd;

     //increase liquidity usdt busd
     IERC20(USDT).approve(P3,usdt);
     IERC20(BUSD).approve(P3,busd);
     INonfungiblePositionManager.IncreaseLiquidityParams memory increase = INonfungiblePositionManager.IncreaseLiquidityParams( {
          tokenId:TOKENID,
          amount0Desired:usdt,
          amount1Desired:busd,
          amount0Min:0,
          amount1Min:0,
          deadline:block.timestamp + 300
     });
     PAIR.increaseLiquidity(increase);

    //  (,,,,,,,uint256 afterlp,,,,) = PAIR.positions(TOKENID);

     INonfungiblePositionManager.NFTPosition memory position2 = PAIR.positions(TOKENID);
     lastrate = (usdt * 1e18) / (position2.liquidity - position1.liquidity);

    }



   	function withdraw(uint256 amount, address to) external returns(bool)  {
         require(amount >0 ,"Require amount > 0");
         require(msg.sender==controller,"ilegal sender");
      
         uint256 lp = ((( amount * 1e18 ) / lastrate) * 51 ) / 100;

        //remove lp
        INonfungiblePositionManager.DecreaseLiquidityParams memory decrease = INonfungiblePositionManager.DecreaseLiquidityParams({
          tokenId:TOKENID,
          liquidity:uint128(lp),
          amount0Min:0,
          amount1Min:0,
          deadline:block.timestamp+300
        });
         PAIR.decreaseLiquidity(decrease);

         collect();

        //swap busd to usdt
        uint256 busd_after = IERC20(BUSD).balanceOf(address(this));
        IERC20(BUSD).approve(SR,busd_after);
        IV3SwapRouter.ExactInputSingleParams memory  params = IV3SwapRouter.ExactInputSingleParams({
                tokenIn: BUSD,
                tokenOut: USDT,
                fee: 100,
                recipient: address(this),
                amountIn: busd_after,
                amountOutMinimum: 0,
                sqrtPriceLimitX96: 0
            });
        SWAPROUTER.exactInputSingle(params);
        
      return  IERC20(USDT).transfer(to,amount);

       }


     
}