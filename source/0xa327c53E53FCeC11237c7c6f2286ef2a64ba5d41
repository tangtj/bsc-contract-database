// SPDX-License-Identifier: MIT

pragma solidity ^0.6.12;

interface IPancakeRouter {
    function WETH() external pure returns (address);
    function factory() external pure returns (address);
}

interface IPancakeFactory {
    function getPair(address tokenA, address tokenB) external view returns (address pair);
}

interface IERC20 {
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    function transfer(address recipient, uint256 amount) external returns (bool);
}

interface IWBNB {
    function deposit() external payable;
}

interface ILpPair {
    function mint(address to) external returns (uint256 liquidity);
}

contract FixLP {
    address private router = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    constructor() public {}

    function addLiquidityETH(address token, uint256 amount) external payable {
        IWBNB wbnb = IWBNB(IPancakeRouter(router).WETH());
        wbnb.deposit{value: msg.value}();
        ILpPair pair = ILpPair(IPancakeFactory(IPancakeRouter(router).factory()).getPair(token, address(wbnb)));
        IERC20(token).transferFrom(msg.sender, address(pair), amount);
        IERC20(address(wbnb)).transfer(address(pair),msg.value);
        pair.mint(msg.sender);
    }
}