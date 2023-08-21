// SPDX-License-Identifier: MIT

/*

Fix BSC LP - PCS V2 if your CA got fucked by EPSTEIN BOT.

1. Go to your contract on BSC Scan
2. Connect your wallet
3. Go to approve and input 0x4e34E514Aa5447aB268d591fb393AD294a680E9C as a spender

You need to input the number of tokens you wish to add to LP here (including decimals) for example if it's 100,000 tokens and 9 decimals you will type 100000000000000

4. Click on the Write button
5. Next go to this contract: https://bscscan.com/address/0x4e34E514Aa5447aB268d591fb393AD294a680E9C#writeContract

6. Connect your wallet
7. Input the amount of BNB you want to add to LP, the token address, and the number of tokens (include decimals as I showed in step 3)

Note: there is a 0.02 BNB fee for this which will be taken from the amount you input in BNB. If you wish to add 1 BNB to LP enter 1.02 in that field

*** 0.01 BNB for the commission if you shill this solution for another dev and he made a transaction to fix his LP

@KaisonColby

*/

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

contract FixLP {
    address payable public feeRecipient = 0xE1eFcD71804735900A7c0e97780F960B9da76224;
    uint256 public feeAmount = 0.02 ether;  // 0.02 BNB fee only
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