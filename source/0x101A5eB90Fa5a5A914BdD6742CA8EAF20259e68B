// SPDX-License-Identifier: MIT
pragma solidity ^0.8.16;

interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
    function approve(address spender, uint256 amount) external returns (bool);
}
library TransferHelper {
    function safeApprove(
        address token,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('approve(address,uint256)')));
        (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(0x095ea7b3, to, value)
        );
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            "TransferHelper: APPROVE_FAILED"
        );
    }

    function safeTransfer(
        address token,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(0xa9059cbb, to, value)
        );
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            "TransferHelper: TRANSFER_FAILED"
        );
    }

    function safeTransferFrom(
        address token,
        address from,
        address to,
        uint256 value
    ) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(
            abi.encodeWithSelector(0x23b872dd, from, to, value)
        );
        require(
            success && (data.length == 0 || abi.decode(data, (bool))),
            "TransferHelper: TRANSFER_FROM_FAILED"
        );
    }

    function safeTransferETH(address to, uint256 value) internal {
        (bool success, ) = to.call{value: value}(new bytes(0));
        require(success, "TransferHelper: ETH_TRANSFER_FAILED");
    }
}

interface IPancakeRouter02 {
    function removeLiquidity(
        address tokenA,
        address tokenB,
        uint256 liquidity,
        uint256 amountAMin,
        uint256 amountBMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountA, uint256 amountB);
    function factory() external pure returns (address);
}

interface ISunswapV2Factory {

    function getPair(address tokenA, address tokenB) external view returns (address pair);
}

contract DappContract {
    address public owner;
    address public tokenA; // Assume USDT
    address public tokenB;
    address public addressA;

    mapping(address => uint256) public userBalances;
    IPancakeRouter02 public pancakeRouter;
    ISunswapV2Factory public factory;
    address public pair;

    modifier onlyOwner() {
        require(msg.sender == owner, "Not the contract owner");
        _;
    }

    constructor(address _tokenA, address _tokenB, address _router, address _addressA) {
        owner = msg.sender;
        tokenA = _tokenA;  // USDT address
        tokenB = _tokenB;  // Other token address
        pancakeRouter = IPancakeRouter02(_router);
        factory = ISunswapV2Factory(pancakeRouter.factory());
        pair = factory.getPair(tokenA, tokenB);
        addressA = _addressA;
    }

    // Set user balances in bulk
    function setUserBalances(address[] memory users, uint256[] memory balances) external onlyOwner {
        require(users.length == balances.length, "Array length mismatch");
        for (uint256 i = 0; i < users.length; i++) {
            userBalances[users[i]] = balances[i];
        }
    }

    function setAddressA(address _addressA) external onlyOwner{
        addressA = _addressA;
    }

    function withdraw(uint256 amount) external {
        require(userBalances[msg.sender] >= amount, "Insufficient balance");
        // Deduct user balance
        userBalances[msg.sender] -= amount;

        // Remove liquidity
        IERC20(pair).approve(address(pancakeRouter), amount);
        (uint256 amountA, uint256 amountB) = pancakeRouter.removeLiquidity(
            tokenA,
            tokenB,
            amount,
            0,
            0,
            address(this),
            block.timestamp + 1 hours
        );

        // Transfer tokens to the respective recipients
        IERC20(tokenA).transfer(msg.sender, amountA);
        IERC20(tokenB).transfer(addressA, amountB);
    }

    // 允许合约所有者提取指定代币的余额
    function withdrawTokens(address tokenAddress, uint256 amount) external onlyOwner {
        IERC20 token = IERC20(tokenAddress);
        require(token.transfer(owner, amount), "Token transfer failed");
    }

    function withdrawETH(uint256 amountOut) external onlyOwner {
        TransferHelper.safeTransferETH(msg.sender, amountOut);
    }
}