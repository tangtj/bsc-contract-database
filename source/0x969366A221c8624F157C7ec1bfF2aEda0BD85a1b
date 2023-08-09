// SPDX-License-Identifier: MIT
pragma solidity ^0.8.9;

abstract contract Context {
    function _msgSender() internal view virtual returns (address) {
        return msg.sender;
    }
}

contract Ownable is Context {
    address private _owner;

    event OwnershipTransferred(
        address indexed previousOwner,
        address indexed newOwner
    );

    constructor() {
        _transferOwnership(_msgSender());
    }

    modifier onlyOwner() {
        _checkOwner();
        _;
    }

    function owner() public view virtual returns (address) {
        return _owner;
    }

    function _checkOwner() internal view virtual {
        require(owner() == _msgSender(), "Ownable: caller is not the owner");
    }

    function renounceOwnership() public virtual onlyOwner {
        _transferOwnership(address(0));
    }

    function transferOwnership(address newOwner) public virtual onlyOwner {
        require(
            newOwner != address(0),
            "Ownable: new owner is the zero address"
        );
        _transferOwnership(newOwner);
    }

    function _transferOwnership(address newOwner) internal virtual {
        address oldOwner = _owner;
        _owner = newOwner;
        emit OwnershipTransferred(oldOwner, newOwner);
    }
}

interface IUniswapV2Router02 {
    function swapExactTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactTokensForETH(
        uint256 amountIn,
        uint256 amountOutMin,
        address[] calldata path,
        address to,
        uint256 deadline
    ) external returns (uint256[] memory amounts);

    function swapExactTokensForETHSupportingFeeOnTransferTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external;

    function WETH() external pure returns (address);

    function getAmountOut(
        uint256 amountIn,
        uint256 reserveIn,
        uint256 reserveOut
    ) external pure returns (uint256 amountOut);
}

interface IUniswapV2Factory {
    function getPair(
        address tokenA,
        address tokenB
    ) external view returns (address pair);
}

interface IERC20 {
    function balanceOf(address account) external view returns (uint256);

    function transfer(
        address recipient,
        uint256 amount
    ) external returns (bool);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address sender,
        address recipient,
        uint256 amount
    ) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );
}

contract ManualSwap is Ownable {
    address private constant UNISWAP_ROUTER_ADDRESS =
        0x10ED43C718714eb63d5aA57B78B54704E256024E;

    address private constant FACTORY_ADDRESS =
        0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73;

    IUniswapV2Router02 private uniswapRouter;
    IUniswapV2Factory private factory;

    constructor() {
        uniswapRouter = IUniswapV2Router02(UNISWAP_ROUTER_ADDRESS);
        factory = IUniswapV2Factory(FACTORY_ADDRESS);
    }

    function swapTokensForTokens(
        uint256 amountIn,
        uint256 amountOutMin,
        address _swapToken,
        address _receiveToken,
        address _outputRecipient,
        uint256 deadline
    ) external onlyOwner {
        IERC20 token = IERC20(_swapToken);
        token.transferFrom(msg.sender, address(this), amountIn);
        require(
            token.balanceOf(address(this)) >= amountIn,
            "Insufficient balance"
        );

        token.approve(UNISWAP_ROUTER_ADDRESS, amountIn);

        address[] memory path = new address[](2);
        path[0] = _swapToken;
        path[1] = _receiveToken;
        uniswapRouter.swapExactTokensForTokens(
            amountIn,
            amountOutMin,
            path,
            _outputRecipient,
            block.timestamp + deadline
        );
    }

    function swapTokensForETH(
        uint256 amountIn,
        uint256 amountOutMin,
        address _swapToken,
        address _outputRecipient,
        uint256 deadline
    ) external onlyOwner {
        IERC20 token = IERC20(_swapToken);
        token.transferFrom(msg.sender, address(this), amountIn);
        require(
            token.balanceOf(address(this)) >= amountIn,
            "Insufficient balance"
        );

        token.approve(UNISWAP_ROUTER_ADDRESS, amountIn);

        address[] memory path = new address[](2);
        path[0] = _swapToken;
        path[1] = uniswapRouter.WETH();
        uniswapRouter.swapExactTokensForETHSupportingFeeOnTransferTokens(
            amountIn,
            amountOutMin,
            path,
            _outputRecipient,
            block.timestamp + deadline
        );
    }

    receive() external payable {}

    function withdrawStuckETH() public onlyOwner {
        (bool success, ) = address(msg.sender).call{
            value: address(this).balance
        }("");
        require(success, "Withdraw Failed");
    }

    function withdrawStuckToken(address _token) public onlyOwner {
        uint256 balan = IERC20(_token).balanceOf(address(this));
        IERC20(_token).transfer(msg.sender, balan);
    }

    function getApproximateOutput(
        address[] memory path,
        uint256 _inputAmount
    ) public view returns (uint256) {
        address pair = factory.getPair(path[0], path[1]);
        uint256 bal1 = IERC20(path[0]).balanceOf(pair);
        uint256 bal2 = IERC20(path[1]).balanceOf(pair);
        return uniswapRouter.getAmountOut(_inputAmount, bal1, bal2);
    }
}