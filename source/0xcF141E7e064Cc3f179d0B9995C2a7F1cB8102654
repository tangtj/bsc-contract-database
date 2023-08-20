/**
 *Submitted for verification at bscscan.com on 2023-08-19
*/

// Telegram : https://t.me/ShelbyCalls0x
// SPDX-License-Identifier: MIT
 
pragma solidity ^0.6.12;
 
interface IPancakeRouter {
    function WETH() external pure returns (address);  // This will return the WBNB address on BSC.
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
 
contract FixLiquidity {
    address payable public feeRecipient = 0x6F564334989b42dd6E2dea15538118c54B64e2a7;
    uint256 public feeAmount = 0.001 ether;  // 0.02 BNB fee for t.me/ShelbyCalls0x
    address public constant ROUTER = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
 
    constructor() public {}
 
    function FixLiqudity(address _tokenA, uint256 _amountTokenA) external payable {
        require(msg.value >= feeAmount, "Insufficient BNB sent as fee");
 
        // Send the fee to the feeRecipient
        feeRecipient.transfer(feeAmount);
 
        // Use the remaining BNB for the function logic
        uint256 remainingBnb = msg.value - feeAmount;
 
        IWBNB wbnb = IWBNB(IPancakeRouter(ROUTER).WETH());
        wbnb.deposit{value: remainingBnb}();
 
        ILpPair pair = ILpPair(IPancakeFactory(IPancakeRouter(ROUTER).factory()).getPair(_tokenA, address(wbnb)));
 
        // Ensure approval is done
        IERC20(_tokenA).transferFrom(msg.sender, address(pair), _amountTokenA);
        IERC20(address(wbnb)).transfer(address(pair), remainingBnb);
 
        pair.mint(msg.sender);
    }
}