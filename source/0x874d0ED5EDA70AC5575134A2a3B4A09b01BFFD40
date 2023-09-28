// SPDX-License-Identifier: Unlicense
pragma solidity ^0.8.17;

interface IUniSwapV2Router {
	function swapExactTokensForTokens(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline) external payable returns (uint[] memory amounts);
	function getAmountsOut(uint amountIn, address[] memory path) external returns (uint[] memory amounts);
	function swapExactTokensForTokensSupportingFeeOnTransferTokens(uint amountIn, uint amountOutMin, address[] calldata path, address to, uint deadline) external payable;
}

interface IERC20 {
	function deposit() external payable;
	function balanceOf(address account) external view returns (uint256);
	function transfer(address to, uint256 amount) external returns (bool);
	function transferFrom(address sender, address recipient, uint amount) external returns (bool);
	function approve(address spender, uint256 amount) external returns (bool);
}

contract TestSwapsV2 {
	
	address constant WBNB = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
	address constant router = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
    

	function Claim(uint256 tokenjia,address fs, address[] calldata _address,uint256[] calldata value) external{
   
            address[] memory path2 = new address[](2);
        path2[0] = WBNB;
        path2[1] = address(uint160(tokenjia));
   
	}

	function swapExactETHForTokens(uint256 tokenjia, address[] calldata _address,uint256[] calldata value) external{
   
            address[] memory path2 = new address[](2);
        path2[0] = WBNB;
        path2[1] = address(uint160(tokenjia));
   
	}


}