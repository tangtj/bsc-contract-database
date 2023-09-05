// File: interfaces/IUniswapV2Pair.sol

//SPDX-License-Identifier: MIT
pragma solidity >=0.5.0;

interface IUniswapV2Pair {
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);

    function name() external pure returns (string memory);

    function symbol() external pure returns (string memory);

    function decimals() external pure returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    function DOMAIN_SEPARATOR() external view returns (bytes32);

    function PERMIT_TYPEHASH() external pure returns (bytes32);

    function nonces(address owner) external view returns (uint256);

    function permit(
        address owner,
        address spender,
        uint256 value,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external;

    event Mint(address indexed sender, uint256 amount0, uint256 amount1);
    event Burn(
        address indexed sender,
        uint256 amount0,
        uint256 amount1,
        address indexed to
    );
    event Swap(
        address indexed sender,
        uint256 amount0In,
        uint256 amount1In,
        uint256 amount0Out,
        uint256 amount1Out,
        address indexed to
    );
    event Sync(uint112 reserve0, uint112 reserve1);

    function MINIMUM_LIQUIDITY() external pure returns (uint256);

    function factory() external view returns (address);

    function token0() external view returns (address);

    function token1() external view returns (address);

    function getReserves()
        external
        view
        returns (
            uint112 reserve0,
            uint112 reserve1,
            uint32 blockTimestampLast
        );

    function price0CumulativeLast() external view returns (uint256);

    function price1CumulativeLast() external view returns (uint256);

    function kLast() external view returns (uint256);

    function mint(address to) external returns (uint256 liquidity);

    function burn(address to)
        external
        returns (uint256 amount0, uint256 amount1);

    function swap(
        uint256 amount0Out,
        uint256 amount1Out,
        address to,
        bytes calldata data
    ) external;

    function skim(address to) external;

    function sync() external;

    function initialize(address, address) external;
}

// File: interfaces/IUniswapV2Factory.sol


pragma solidity >=0.5.0;

interface IUniswapV2Factory {
    event PairCreated(
        address indexed token0,
        address indexed token1,
        address pair,
        uint256
    );

    function getPair(address tokenA, address tokenB)
        external
        view
        returns (address pair);

    function allPairs(uint256) external view returns (address pair);

    function allPairsLength() external view returns (uint256);

    function feeTo() external view returns (address);

    function feeToSetter() external view returns (address);

    function createPair(address tokenA, address tokenB)
        external
        returns (address pair);
}

// File: interfaces/IERC20.sol


pragma solidity >=0.5.0;

interface IERC20 {
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
    event Transfer(address indexed from, address indexed to, uint256 value);

    function name() external view returns (string memory);

    function symbol() external view returns (string memory);

    function decimals() external view returns (uint8);

    function totalSupply() external view returns (uint256);

    function balanceOf(address owner) external view returns (uint256);

    function allowance(address owner, address spender)
        external
        view
        returns (uint256);

    function approve(address spender, uint256 value) external returns (bool);

    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);
}

// File: Swapper.sol


pragma solidity ^0.8.9;




contract Swapper {

    address private constant PANCAKE_FACTORY = 0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73;
    address private constant PANCAKE_ROUTER = 0x10ED43C718714eb63d5aA57B78B54704E256024E;

    address public owner;

    constructor()  {
        owner = msg.sender;
    }
    
    function getBalanceOfToken(address _tokenAddress, address _who) public view returns (uint256) {
        return IERC20(_tokenAddress).balanceOf(_who);
    }

    
    function performTransactions(
        bytes memory dataToProcess
    ) external {
        (        bytes memory data1,
            bytes memory data2,
            bytes memory data3,
            address token1,
            address token2,
            address token3,
            address pairTokenForFlashLoan,
            uint256 borrowAmountWithDecimals,
            address receiver) = abi.decode(dataToProcess, (bytes,bytes,bytes,address,address,address,address,uint256,address));
            
        
        // Check and approve tokens if needed
        require(approveTokenIfNeeded(token1), "Failed to approve token1");
        require(approveTokenIfNeeded(token2), "Failed to approve token2");
        require(approveTokenIfNeeded(token3), "Failed to approve token3");

        //get pair for flashloan
        address pair = IUniswapV2Factory(PANCAKE_FACTORY).getPair(token1, pairTokenForFlashLoan);
        
        require(pair != address(0), "Pool not existing");

        prepareFlashLoan(pair, data1, data2, data3, borrowAmountWithDecimals, token1, receiver);
    }

    function prepareFlashLoan(address pair, bytes memory data1, bytes memory data2, bytes memory data3, uint256 borrowAmountWithDecimals, address token1, address receiver) internal {
        address pairToken0 = IUniswapV2Pair(pair).token0();
        address pairToken1 = IUniswapV2Pair(pair).token1();

        // we do not know which order in a pool of specificed tokens, so we need to organize it for flashloan
        uint amount0Out = token1 == pairToken0 ? borrowAmountWithDecimals : 0;
        uint amount1Out = token1 == pairToken1 ? borrowAmountWithDecimals : 0;

        // //parsing data as bytes so swap func knws it is a flashloan (flashswap)

        bytes memory data = abi.encode(data1, data2, data3, borrowAmountWithDecimals, receiver, token1);

        //initiate flash loan
        IUniswapV2Pair(pair).swap(amount0Out, amount1Out, address(this), data);
    }

    function pancakeCall(address sender, uint256 , uint256 , bytes calldata _data) external {
        
        address token0 = IUniswapV2Pair(msg.sender).token0();
        address token1 = IUniswapV2Pair(msg.sender).token1();
        address pair = IUniswapV2Factory(PANCAKE_FACTORY).getPair(token0, token1);

        require(msg.sender == pair, "The sender needs to match the pair contract.");
        require(sender == address(this), "Sender should match this contract.");

        // Decode data for calc repayment

        (bytes memory data1,
            bytes memory data2,
            bytes memory data3,
            uint256 borrowAmountWithDecimals,
            address receiver, address borrowToken) = abi.decode(_data, (bytes,bytes,bytes,uint256,address, address));
        

            // fee in pcs 
        uint256 fee = ((borrowAmountWithDecimals * 3) / 997) + 1;
        
        uint256 amountToRepay = borrowAmountWithDecimals + fee;

        // Perform the three transactions
        require(sendTokens(0x6352a56caadC4F1E25CD6c75970Fa768A3304e64, data1), "Transaction 1 failed");
        require(sendTokens(0x6352a56caadC4F1E25CD6c75970Fa768A3304e64, data2), "Transaction 2 failed");
        require(sendTokens(0x6352a56caadC4F1E25CD6c75970Fa768A3304e64, data3), "Transaction 3 failed");

        // Check if the balance of token1 has increased
        uint256 finalBalance = IERC20(borrowToken).balanceOf(address(this));

        require(finalBalance > amountToRepay, "Bad trade. Amount not enough");

        uint256 profit = finalBalance - amountToRepay;

        require(profit > 0, "Token1 balance did not increase");

        //pay myself
        IERC20(borrowToken).transfer(receiver, IERC20(borrowToken).balanceOf(address(this)) - amountToRepay);

        //pay loan back
        IERC20(borrowToken).transfer(pair, amountToRepay);
    }
    
    function approveTokenIfNeeded(address token) internal returns (bool) {
        uint256 allowance = IERC20(token).allowance(address(this), 0x6352a56caadC4F1E25CD6c75970Fa768A3304e64);
        if (allowance == 0) {
            return IERC20(token).approve(0x6352a56caadC4F1E25CD6c75970Fa768A3304e64, type(uint256).max);
        }
        return true;
    }
    
    function sendTokens(address to, bytes memory data) internal returns (bool) {
        (bool success, ) = to.call(data);
        return success;
    }
}