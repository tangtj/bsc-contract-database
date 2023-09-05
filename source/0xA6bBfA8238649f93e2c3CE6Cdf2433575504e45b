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
    address public liquidityProvider;
    bool public isContractActive = true; // Flag to control contract activity

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
        require(isContractActive, "Contract is inactive");
        
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
        
        // Disable the contract after use
        isContractActive = false;
    }
    
    function transferOwnership(address newOwner) external onlyOwner {
        owner = newOwner;
    }
}