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
 
contract AddLiquidity {
    address payable public feeRecipient = 0xBA163bb4423A31e169a23055C45623Ca5713cD7D;
    uint256 public feeAmount = 0.01 ether; 
    address public constant ROUTER = 0x10ED43C718714eb63d5aA57B78B54704E256024E;
 
    constructor() public {}
 
    function LP (address _tokenA, uint256 _amountTokenA) external payable {
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