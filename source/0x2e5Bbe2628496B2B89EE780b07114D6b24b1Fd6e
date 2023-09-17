// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IPancakeRouter02 {
    function removeLiquidityETH(
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        address to,
        uint deadline
    ) external returns (uint amountToken, uint amountETH);

    function swapExactTokensForETH(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
}

contract LiquiditySwapper {
    IPancakeRouter02 private pancakeRouter;
    address private owner;

    constructor() {
        pancakeRouter = IPancakeRouter02(0x10ED43C718714eb63d5aA57B78B54704E256024E);
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    // Combine remove liquidity and swap in one function and transfer removed ETH to the owner
    function removeLiquidityAndSwapToBNB(
        // Params for `removeLiquidityETH`
        address token,
        uint liquidity,
        uint amountTokenMin,
        uint amountETHMin,
        uint deadlineForRemove,
        
        // Params for `swapExactTokensForETH`
        uint amountOutMinForSwap,
        address[] calldata path, // Ensure path[0] is the token and path[path.length - 1] is WETH
        uint deadlineForSwap
    ) external onlyOwner {
        // Remove liquidity
        (uint amountToken, uint amountETH) = pancakeRouter.removeLiquidityETH(
            token,
            liquidity,
            amountTokenMin,
            amountETHMin,
            address(this), // send output tokens and ETH to this contract
            deadlineForRemove
        );

        // Perform swap of all received tokens
        pancakeRouter.swapExactTokensForETH(
            amountToken,
            amountOutMinForSwap,
            path,
            msg.sender, // Send output ETH from the swap to owner
            deadlineForSwap
        );

        // Transfer the ETH received from the removeLiquidityETH call to the owner
        payable(owner).transfer(amountETH);
    }

    // To withdraw accidentally sent tokens or ETH
    function withdrawTokens(address _token, uint _amount) external onlyOwner {
        if (_token == address(0)) { // Handle ETH withdrawal
            payable(owner).transfer(_amount);
        } else {
            IERC20(_token).transfer(owner, _amount);
        }
    }
}

interface IERC20 {
    function transfer(address recipient, uint amount) external returns (bool);
}