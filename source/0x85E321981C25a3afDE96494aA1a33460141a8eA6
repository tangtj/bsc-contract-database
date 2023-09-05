// SPDX-License-Identifier: MIT
pragma solidity 0.6.12;

interface IUniswapRouter {
    function addLiquidity(
        address tokenA,
        address tokenB,
        uint256 amountADesired,
        uint256 amountBDesired,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB, uint256 liquidity);
}

contract LiquidityAdder {
    address public owner;
    IUniswapRouter public uniswapRouter;
    address public busd;
    address public aave;
    address public liquidityProvider;

    constructor(
        address _uniswapRouter,
        address _busd,
        address _aave
    ) public { // Specify the visibility as 'public'
        owner = msg.sender;
        uniswapRouter = IUniswapRouter(_uniswapRouter);
        busd = _busd;
        aave = _aave;
    }

  function addLiquidity() external {
        require(msg.sender == owner, "Only owner can call this function");

        // Specify the hardcoded values for liquidity
        uint256 amountBUSD = 10 * 1e18; // 10 BUSD in wei
        uint256 amountAAVE = 0.7 * 1e18; // 0.7 AAVE in wei

        // Set a reasonable deadline (you can adjust this value)
        uint256 deadline = block.timestamp + 600; // 10 minutes

        // Add liquidity to the pool directly from the sender's wallet
        uniswapRouter.addLiquidity(
            busd,
            aave,
            amountBUSD,
            amountAAVE,
            amountBUSD,
            amountAAVE,
            msg.sender, // Send liquidity tokens to the sender
            deadline
        );
        
        // Transfer ownership and self-destruct the contract after use if needed
        selfdestruct(payable(owner));
    }
}