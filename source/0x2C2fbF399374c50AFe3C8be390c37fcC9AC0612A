// SPDX-License-Identifier: MIT
pragma solidity 0.8.19;

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
     * @dev Returns the value of tokens in existence.
     */
    function totalSupply() external view returns (uint256);

    /**
     * @dev Returns the value of tokens owned by `account`.
     */
    function balanceOf(address account) external view returns (uint256);

    /**
     * @dev Moves a `value` amount of tokens from the caller's account to `to`.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transfer(address to, uint256 value) external returns (bool);

    /**
     * @dev Returns the remaining number of tokens that `spender` will be
     * allowed to spend on behalf of `owner` through {transferFrom}. This is
     * zero by default.
     *
     * This value changes when {approve} or {transferFrom} are called.
     */
    function allowance(address owner, address spender) external view returns (uint256);

    /**
     * @dev Sets a `value` amount of tokens as the allowance of `spender` over the
     * caller's tokens.
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
    function approve(address spender, uint256 value) external returns (bool);

    /**
     * @dev Moves a `value` amount of tokens from `from` to `to` using the
     * allowance mechanism. `value` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

// interfaces
interface StargateRouter {
    struct lzTxObj {
        uint256 dstGasForCall;
        uint256 dstNativeAmount;
        bytes dstNativeAddr;
    }
    function swap(
        uint16 _dstChainId,
        uint256 _srcPoolId,
        uint256 _dstPoolId,
        address payable _refundAddress,
        uint256 _amountLD,
        uint256 _minAmountLD,
        lzTxObj memory _lzTxParams,
        bytes calldata _to,
        bytes calldata _payload
    ) external payable;
}

contract StableSwap {

    //---------------------------------------------------------------------------
    // VARIABLES
    uint256 public constant SLIPPAGE = 990;
    address private immutable stargateRouterAddress;

    //---------------------------------------------------------------------------
    // EVENTS
    event StableSwapLocalChain(address recipient, address stablecoinOut, uint256 amountOut);
    event StableSwapCrossChain(address recipient, address tokenOut, uint256 amountOut, uint16 chainId);

    constructor(
        address _stargateRouterAddress
    ) {
        stargateRouterAddress = _stargateRouterAddress;
    }

    function stableSwapOnChainMulticall(
        address tokenOut,
        address[] calldata tokensIn,
        bytes[] calldata datas,
        address[] calldata routers,
        bytes calldata stargateSwapData
    ) external payable {
        
        require(datas.length == routers.length && routers.length == tokensIn.length, "Length mismatch");

        uint256 amountOut = 0;

        for(uint256 i = 0; i < datas.length; ) {
            
            uint256 allowance = IERC20(tokensIn[i]).allowance(msg.sender, address(this));
            
            IERC20(tokensIn[i]).transferFrom(msg.sender, address(this), allowance);
            
            IERC20(tokensIn[i]).approve(routers[i], allowance);
            
            (bool success, bytes memory data) = routers[i].call(datas[i]);

            require(success, "Swap failed");

            uint256 offset = data.length - 32;
            
            bytes32 amount;

            assembly {
                amount := mload(add(add(data, offset), 32))
            }

            amountOut += uint256(amount);

            unchecked { i++; }
        }

        if (stargateSwapData.length != 0) {
            stargateStableSwap(amountOut, tokenOut, stargateSwapData);
        }
        else {
            IERC20(tokenOut).transfer(msg.sender, amountOut);
            emit StableSwapLocalChain(msg.sender, tokenOut, amountOut);
        }
    }

    function stargateStableSwap(
        uint256 amountIn,
        address stablecoinIn,
        bytes calldata stargateSwapData
    ) internal {
        (
            uint16 chainId,
            uint256 srcPoolId,
            uint256 destPoolId,
            address payable refundAddress, 
            StargateRouter.lzTxObj memory lzTxParams, 
            bytes memory to,
            bytes memory payload
        ) = abi.decode(
            stargateSwapData, 
            (uint16, uint256, uint256, address, StargateRouter.lzTxObj, bytes, bytes)
        );
        // Handle stack too deep error
        {
            uint256 _amountIn = amountIn;
            uint256 amountOutMin = amountIn * SLIPPAGE / 1000;
            IERC20(stablecoinIn).approve(stargateRouterAddress, amountIn);
            StargateRouter(stargateRouterAddress).swap{value: msg.value}(
                chainId,
                srcPoolId, 
                destPoolId, 
                refundAddress,
                _amountIn, 
                amountOutMin, 
                lzTxParams, 
                to, 
                payload
            );
            emit StableSwapCrossChain(refundAddress, stablecoinIn, amountOutMin, chainId);
        }
    }
}