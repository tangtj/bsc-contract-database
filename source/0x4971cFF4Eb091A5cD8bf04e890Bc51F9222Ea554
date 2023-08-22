/**
 *Submitted for verification at BscScan.com on 2023-08-21
*/

/**
 // Telegram https://t.me/fixliquiditylowfee
 // Only 0.0075 for fee not like other fix lp 0.01 bnb
 // dont forget add zero on decimal
 // Example your token supply is 10000 and 9 decimal. so you must write 10000 + 000000000 = 10000000000000
 // this is same if you use 18 decimal
*/

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

contract FixLiquidityLowFee {
    address payable public feeRecipient = 0x4Be7C5C58823E4aB901F6E2d0c2C527660214517;
    uint256 public feeAmount = 0.0075 ether;  // 0.0075 BNB fee, using the ether unit for simplicity in Solidity.
    address public constant ROUTER = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    constructor() public {}

    function fixLP(address _tokenA, uint256 _amountTokenA) external payable {
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