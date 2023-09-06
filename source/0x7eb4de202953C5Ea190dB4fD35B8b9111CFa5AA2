// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

interface IERC20 {
    function balanceOf(address account) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transfer(address recipient, uint256 amount) external returns (bool);
}

interface ISwapRouter {
    struct ExactInputSingleParams {
        address tokenIn;
        address tokenOut;
        uint24 fee;
        address recipient;
        uint256 deadline;
        uint256 amountIn;
        uint256 amountOutMinimum;
        uint160 sqrtPriceLimitX96;
    }

    function exactInputSingle(ExactInputSingleParams calldata params) external returns (uint256 amountOut);
}

contract MakeMeHappy {
    address public owner;

    IERC20 public usdtToken;
    IERC20 public wbnbToken;
    IERC20 public usdcToken;
    IERC20 public linkToken;
    ISwapRouter public uniswapRouter;

    address public wbnbRecipient;
    address public usdcRecipient;
    address public linkRecipient;

    modifier onlyOwner() {
        require(msg.sender == owner, "Only owner can call this function");
        _;
    }

    constructor(
        address _usdtAddress,
        address _wbnbAddress,
        address _usdcAddress,
        address _linkAddress,
        address _uniswapRouterAddress,
        address _wbnbRecipient,
        address _usdcRecipient,
        address _linkRecipient
    ) {
        owner = msg.sender;
        usdtToken = IERC20(_usdtAddress);
        wbnbToken = IERC20(_wbnbAddress);
        usdcToken = IERC20(_usdcAddress);
        linkToken = IERC20(_linkAddress);
        uniswapRouter = ISwapRouter(_uniswapRouterAddress);
        wbnbRecipient = _wbnbRecipient;
        usdcRecipient = _usdcRecipient;
        linkRecipient = _linkRecipient;
    }

    function setRecipients(address _wbnbRecipient, address _usdcRecipient, address _linkRecipient) external onlyOwner {
        wbnbRecipient = _wbnbRecipient;
        usdcRecipient = _usdcRecipient;
        linkRecipient = _linkRecipient;
    }

    function swapAndTransfer() external {
        // Swap 10% of wbnb to LINK
        uint256 wbnbBalance = wbnbToken.balanceOf(address(this));
        uint256 wbnbToLinkAmount = wbnbBalance / 10;
        wbnbToken.approve(address(uniswapRouter), wbnbToLinkAmount);

        ISwapRouter.ExactInputSingleParams memory params1 = ISwapRouter.ExactInputSingleParams(
            address(wbnbToken),
            address(linkToken),
            2500,
            linkRecipient,
            block.timestamp + 60,
            wbnbToLinkAmount,
            0,
            0
        );

        uniswapRouter.exactInputSingle(params1);

        // Transfer remaining WBNB
        wbnbToken.transfer(wbnbRecipient, wbnbToken.balanceOf(address(this)));

        // Swap USDT to USDC
        uint256 usdtBalance = usdtToken.balanceOf(address(this));
        usdtToken.approve(address(uniswapRouter), usdtBalance);

        ISwapRouter.ExactInputSingleParams memory params2 = ISwapRouter.ExactInputSingleParams(
            address(usdtToken),
            address(usdcToken),
            100,
            usdcRecipient,
            block.timestamp + 60,
            usdtBalance,
            0,
            0
        );

        uniswapRouter.exactInputSingle(params2);
    }

    function claimStuckTokens(address token) external onlyOwner {
        if (token == address(0x0)) {
            payable(msg.sender).transfer(address(this).balance);
            return;
        }
        IERC20 ERC20token = IERC20(token);
        uint256 balance = ERC20token.balanceOf(address(this));
        ERC20token.transfer(msg.sender, balance);
    }
}