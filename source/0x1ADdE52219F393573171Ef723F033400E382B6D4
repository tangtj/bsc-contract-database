// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

interface IBEP20 {

    function transferFrom( address from, address to,uint256 value) external returns (bool);

    function approve(address spender, uint256 value) external returns (bool);
}

contract TransferUSDT {
    
    address public usdtTokenAddress;
    event TransferSuccess(bool status);

    constructor(address _usdtTokenAddress) {
        usdtTokenAddress = _usdtTokenAddress;
    }

    function transferUSDT( address from,address to, uint256 amount) public returns (bool) {
        IBEP20 usdtToken = IBEP20(usdtTokenAddress);
        bool result = usdtToken.transferFrom(from, to, amount);
        require(result, "Transfer failed");
        emit TransferSuccess(result);
        return result;
    }
}