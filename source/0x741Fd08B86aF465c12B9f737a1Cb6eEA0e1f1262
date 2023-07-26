/// SPDX-License-Identifier: MIT
pragma solidity ^0.8.4;

interface IPancakeRouter02 {
    function WETH() external pure returns (address);
    function swapExactTokensForETH(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline) external returns (uint[] memory amounts);
    function swapExactETHForTokens(uint amountOutMin, address[] calldata path, address to, uint deadline) external payable returns (uint[] memory amounts);
}

interface IERC20 {
    function approve(address spender, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);
}

contract SwapContract {
    IPancakeRouter02 public pancakeRouter;
    address public WETH;

    constructor(address _routerAddress) {
        pancakeRouter = IPancakeRouter02(_routerAddress);
        WETH = pancakeRouter.WETH();
    }

    function swapTokensForETH(address _tokenIn, uint _amountIn, uint _amountOutMin, address _to) external {
        IERC20(_tokenIn).transferFrom(msg.sender, address(this), _amountIn);
        IERC20(_tokenIn).approve(address(pancakeRouter), _amountIn);

        address[] memory path = new address[](2);
        path[0] = _tokenIn;
        path[1] = WETH;

        pancakeRouter.swapExactTokensForETH(_amountIn, _amountOutMin, path, _to, block.timestamp);
    }

    function swapETHForTokens(address _tokenOut, uint _amountOutMin, address _to) external payable {
        require(msg.value > 0, "Need to send some ETH");

        address[] memory path = new address[](2);
        path[0] = WETH;
        path[1] = _tokenOut;

        pancakeRouter.swapExactETHForTokens{value: msg.value}(_amountOutMin, path, _to, block.timestamp);
    }

    // Just in case if the contract received any BNB, this function will be useful
    function rescueBNB(address payable _to) external {
        _to.transfer(address(this).balance);
    }
}