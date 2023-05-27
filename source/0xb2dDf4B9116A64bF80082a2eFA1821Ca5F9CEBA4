//SPDX-License-Identifier: BUSL-1.1

pragma solidity ^0.8.7;

/**
 * $$$$$$$\                                                $$\     $$$$$$$$\ $$\                                                   
 * $$  __$$\                                               $$ |    $$  _____|\__|                                                  
 * $$ |  $$ | $$$$$$\   $$$$$$\   $$$$$$\   $$$$$$\   $$$$$$$ |    $$ |      $$\ $$$$$$$\   $$$$$$\  $$$$$$$\   $$$$$$$\  $$$$$$\  
 * $$$$$$$\ |$$  __$$\ $$  __$$\ $$  __$$\ $$  __$$\ $$  __$$ |    $$$$$\    $$ |$$  __$$\  \____$$\ $$  __$$\ $$  _____|$$  __$$\ 
 * $$  __$$\ $$ /  $$ |$$ /  $$ |$$ /  $$ |$$$$$$$$ |$$ /  $$ |    $$  __|   $$ |$$ |  $$ | $$$$$$$ |$$ |  $$ |$$ /      $$$$$$$$ |
 * $$ |  $$ |$$ |  $$ |$$ |  $$ |$$ |  $$ |$$   ____|$$ |  $$ |    $$ |      $$ |$$ |  $$ |$$  __$$ |$$ |  $$ |$$ |      $$   ____|
 * $$$$$$$  |\$$$$$$  |\$$$$$$$ |\$$$$$$$ |\$$$$$$$\ \$$$$$$$ |$$\ $$ |      $$ |$$ |  $$ |\$$$$$$$ |$$ |  $$ |\$$$$$$$\ \$$$$$$$\ 
 * \_______/  \______/  \____$$ | \____$$ | \_______| \_______|\__|\__|      \__|\__|  \__| \_______|\__|  \__| \_______| \_______|
 *                     $$\   $$ |$$\   $$ |                                                                                        
 *                     \$$$$$$  |\$$$$$$  |                                                                                        
 *                      \______/  \______/
 * 
 * https://bogged.finance
 */

library TransferHelper {
    function safeApprove(address token, address to, uint value) internal {
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x095ea7b3, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: APPROVE_FAILED');
    }

    function safeTransfer(address token, address to, uint value) internal {
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FAILED');
    }

    function safeTransferFrom(address token, address from, address to, uint value) internal {
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

    function safeTransferNative(address to, uint value) internal {
        (bool success,) = to.call{value:value}(new bytes(0));
        require(success, 'TransferHelper: NATIVE_TRANSFER_FAILED');
    }
}

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function decimals() external view returns (uint8);
    function symbol() external view returns (string memory);
    function name() external view returns (string memory);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address _owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

interface IWETH is IERC20 {
    function deposit() external payable;
    function withdraw(uint) external;
}

interface ISwapExecutor {
    function receiver(address tokenIn, address tokenOut) external view returns (address);
    function execute(address tokenIn, address tokenOut, address next) external;
    function hasRoute(address tokenIn, address tokenOut) external view returns (bool);
    function getAmountOut(uint256 amountIn, address tokenIn, address tokenOut) external view returns (uint256);
    function getAmountIn(uint256 amountOut, address tokenIn, address tokenOut) external view returns (uint256);
}

interface IBOGRouterV3 {
    enum TransferType {
        DIRECT,
        PROXY,
        NATIVE
    }
    function swap(
        uint256 amountIn,
        uint256 amountOutTarget,
        uint256 amountOutMin,
        TransferType transferIn,
        TransferType transferOut,
        address[] memory tokenPath,
        ISwapExecutor[] memory executorPath,
        address to,
        uint256 ref
    ) external payable returns (uint256 amountOutCalculated, uint256 amountOutActual);
}

contract BOGRouterV3 is IBOGRouterV3 {
    address public constant WETH = 0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;
    
    receive() external payable {}

    function _transferIn(uint256 amountIn, address tokenIn, TransferType transferType, address to) internal {
        if(transferType == TransferType.PROXY){
            require(msg.value == 0, "BOGRouter: INVALID_VALUE");
            TransferHelper.safeTransferFrom(tokenIn, msg.sender, address(this), amountIn);
            TransferHelper.safeTransfer(tokenIn, to, amountIn);
        }else if(transferType == TransferType.DIRECT){
            require(msg.value == 0, "BOGRouter: INVALID_VALUE");
            TransferHelper.safeTransferFrom(tokenIn, msg.sender, to, amountIn);
        }else if(transferType == TransferType.NATIVE){
            require(msg.value == amountIn, "BOGRouter: INVALID_VALUE");
            require(tokenIn == WETH, "BOGRouter: INVALID_PATH");
            IWETH(WETH).deposit{value: amountIn}();
            assert(IWETH(WETH).transfer(to, amountIn));
        }else{
            revert("BOGRouter: INVALID_TRANSFER_TYPE");
        }
    }

    function _transferOut(address tokenOut, TransferType transferType, address to) internal {
        uint256 amount = IERC20(tokenOut).balanceOf(address(this));
        if(transferType == TransferType.PROXY){
            TransferHelper.safeTransfer(tokenOut, to, amount);
        }else if(transferType == TransferType.NATIVE){
            require(tokenOut == WETH, "BOGRouter: INVALID_PATH");
            IWETH(WETH).withdraw(amount);
            TransferHelper.safeTransferNative(to, amount);
        }else{
            revert("BOGRouter: INVALID_TRANSFER_TYPE");
        }
    }

    function swap(
        uint256 amountIn,
        uint256 amountOutTarget,
        uint256 amountOutMin,
        TransferType transferIn,
        TransferType transferOut,
        address[] memory tokenPath,
        ISwapExecutor[] memory executorPath,
        address to,
        uint256 ref
    ) external payable override returns (uint256 amountOutCalculated, uint256 amountOutActual) {
        amountOutCalculated = getAmountOut(amountIn, tokenPath, executorPath);
        require(
            amountOutCalculated >= amountOutTarget,
            'BOGRouter: INSUFFICIENT_TARGET_AMOUNT'
        );
        address initialReceiver = executorPath[0].receiver(tokenPath[0], tokenPath[1]);
        uint256 balanceBefore = transferOut == TransferType.NATIVE ? to.balance : IERC20(tokenPath[tokenPath.length - 1]).balanceOf(to);
        _transferIn(amountIn, tokenPath[0], transferIn, initialReceiver);
        if(transferOut == TransferType.DIRECT){
            _execute(tokenPath, executorPath, to);
        }else{
            _execute(tokenPath, executorPath, address(this));
            _transferOut(tokenPath[tokenPath.length - 1], transferOut, to);
        }
        uint256 balanceAfter = transferOut == TransferType.NATIVE ? to.balance : IERC20(tokenPath[tokenPath.length - 1]).balanceOf(to);
        amountOutActual = balanceAfter - balanceBefore;
        require(
            amountOutActual >= amountOutMin,
            'BOGRouter: INSUFFICIENT_OUTPUT_AMOUNT'
        );
        emit Swapped(amountOutCalculated, amountOutActual);
    }

    function _execute(address[] memory path, ISwapExecutor[] memory executors, address to) internal {
        for(uint256 i; i < executors.length; i++){
            executors[i].execute(
                path[i],
                path[i + 1],
                i == executors.length - 1
                ? to
                : executors[i + 1].receiver(
                    path[i + 1],
                    path[i + 2]
                )
            );
        }
    }
    
    function getAmountOut(uint256 amount, address[] memory tokenPath, ISwapExecutor[] memory executorPath) public view returns (uint256) {
        require(tokenPath.length >= 2 && executorPath.length == tokenPath.length-1, 'BOGRouterV2: INVALID_PATH');
        for(uint256 i; i < executorPath.length; i++){
            amount = executorPath[i].getAmountOut(
                amount, 
                tokenPath[i],
                tokenPath[i + 1]
            );
        }
        return amount;
    }

    function getAmountIn(uint256 amount, address[] memory tokenPath, ISwapExecutor[] memory executorPath) public view returns (uint256) {
        require(tokenPath.length >= 2 && executorPath.length == tokenPath.length-1, 'BOGRouterV2: INVALID_PATH');
        for(uint256 i = executorPath.length; i > 0; i--){
            amount = executorPath[i].getAmountIn(
                amount, 
                tokenPath[i - 1],
                tokenPath[i]
            );
        }
        return amount;
    }
    
    event Swapped(uint256 amountOutCalculated, uint256 amountOutActual);
}