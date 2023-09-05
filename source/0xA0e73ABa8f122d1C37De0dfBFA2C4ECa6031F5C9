// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

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
    uint256 constant public liquidityAmountBUSD = 10 ether; // 10 BUSD
    uint256 constant public liquidityAmountAAVE = 0.7 ether; // 0.7 AAVE

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }

    constructor(
        address _uniswapRouter,
        address _busd,
        address _aave
    ) {
        owner = msg.sender;
        uniswapRouter = IUniswapRouter(_uniswapRouter);
        busd = _busd;
        aave = _aave;
    }

    function addLiquidity() external onlyOwner {
        // Set a reasonable deadline (you can adjust this value)
        uint256 deadline = block.timestamp + 600; // 10 minutes

        // Add liquidity to the pool using the hardcoded amounts
        uniswapRouter.addLiquidity(
            busd,
            aave,
            liquidityAmountBUSD,
            liquidityAmountAAVE,
            0, // Min amounts can be set to 0 if you want to provide the specified liquidity
            0,
            owner, // Send liquidity tokens to the owner
            deadline
        );
    }
}