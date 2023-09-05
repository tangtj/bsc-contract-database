// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IBabylonSwapV2Pair {
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    function mint(address to) external returns (uint liquidity);
    function token0() external view returns (address);
    function token1() external view returns (address);
    function transfer(address to, uint256 value) external returns (bool);
}

interface IERC20 {
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
}

contract LiquidityAdder {
    address public owner;
    address public pairAddress; // Address of AAVE-BUSD pair
    IBabylonSwapV2Pair public pair;

    constructor(address _pairAddress) {
        owner = msg.sender;
        pairAddress = _pairAddress;
        pair = IBabylonSwapV2Pair(pairAddress);
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }

    function addLiquidity(uint256 amountAAVE, uint256 amountBUSD) external onlyOwner {
        // Transfer AAVE and BUSD tokens to this contract
        IERC20(pair.token0()).transferFrom(msg.sender, address(this), amountAAVE);
        IERC20(pair.token1()).transferFrom(msg.sender, address(this), amountBUSD);

        // Approve the pair contract to spend the tokens
        IERC20(pair.token0()).approve(pairAddress, amountAAVE);
        IERC20(pair.token1()).approve(pairAddress, amountBUSD);

        // Mint liquidity by calling the pair's mint function
        uint256 liquidity = pair.mint(address(this));

        // Transfer the liquidity tokens back to the owner
        pair.transfer(msg.sender, liquidity);
    }
}