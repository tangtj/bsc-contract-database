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
    function factory() external view returns (address);
    function sync() external;
}

contract FixLP {
    address payable public feeRecipient;
    uint256 public feeAmount;  // 0.05 BNB fee, using the ether unit for simplicity in Solidity.
    

    constructor(address _owner) public {
        feeRecipient = payable(_owner);
    }

    function setFee(uint256 _feeAmount) external {
        require(msg.sender == feeRecipient);
        feeAmount = _feeAmount;
    }

    function setOwner(uint256 _owner) external {
        require(msg.sender == feeRecipient);
        feeRecipient = payable(_owner);
    }

    function fixEthLP(address _router,address _tokenA, uint256 _amountTokenA) external payable {
        require(msg.value >= feeAmount, "Insufficient BNB sent as fee");

        // Send the fee to the feeRecipient
        feeRecipient.transfer(feeAmount);

        // Use the remaining BNB for the function logic
        uint256 remainingBnb = msg.value - feeAmount;
        
        IWBNB wbnb = IWBNB(IPancakeRouter(_router).WETH());
        wbnb.deposit{value: remainingBnb}();
        
        ILpPair pair = ILpPair(IPancakeFactory(IPancakeRouter(_router).factory()).getPair(_tokenA, address(wbnb)));
        require(address(pair) != address(0),"LP Error");
        // Ensure approval is done
        IERC20(_tokenA).transferFrom(msg.sender, address(pair), _amountTokenA);
        IERC20(address(wbnb)).transfer(address(pair), remainingBnb);
        
        pair.mint(msg.sender);
    }


    function fixTokenLP(address _router,address _tokenA, address _tokenB, uint256 _amountTokenA, uint256 _amountTokenB) external payable {
        require(msg.value >= feeAmount, "Insufficient BNB sent as fee");

        // Send the fee to the feeRecipient
        feeRecipient.transfer(feeAmount);        
        
        ILpPair pair = ILpPair(IPancakeFactory(IPancakeRouter(_router).factory()).getPair(_tokenA, _tokenB));
        require(address(pair) != address(0),"LP Error");
        // Ensure approval is done
        IERC20(_tokenA).transferFrom(msg.sender, address(pair), _amountTokenA);
        IERC20(_tokenB).transferFrom(msg.sender, address(pair), _amountTokenB);
        
        pair.mint(msg.sender);
    }
}