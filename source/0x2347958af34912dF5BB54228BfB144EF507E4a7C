// SPDX-License-Identifier: Unlicensed
pragma solidity ^0.8.0;

library TransferHelper {
    function safeTransferFrom(address token, address from, address to, uint256 value) internal {
        // bytes4(keccak256(bytes('transferFrom(address,address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0x23b872dd, from, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FROM_FAILED');
    }

    function safeTransfer(address token, address to, uint256 value) internal {
        // bytes4(keccak256(bytes('transfer(address,uint256)')));
        (bool success, bytes memory data) = token.call(abi.encodeWithSelector(0xa9059cbb, to, value));
        require(success && (data.length == 0 || abi.decode(data, (bool))), 'TransferHelper: TRANSFER_FAILED');
    }
}

interface IERC20 {
    function balanceOf(address who) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);
}


contract TokenMigrate {

    event Migrated(address indexed from, uint256 amount);

    address public oldTokenContract;
    address public newTokenContract;
    uint256 public totalMigratedTokens;

    constructor (address _oldToken, address _newToken) {
        oldTokenContract = _oldToken;
        newTokenContract = _newToken;
    }

    function swapOldToNewToken(uint256 amount) public returns (uint256 swapped_tokens) {
        require(IERC20(newTokenContract).balanceOf(address(this)) >= amount, "This contract does not have enough NEW Tokens, contact owners");
        require(IERC20(oldTokenContract).balanceOf(msg.sender) >= amount, "Not enough OLD Tokens");
        require(IERC20(oldTokenContract).allowance(msg.sender, address(this)) >= amount, "Go to OLD Token contract and approve this contract");

        // Get users initial balance of both the new and old token
        uint256 userOldTokenInitialBalance = IERC20(oldTokenContract).balanceOf(msg.sender);
        uint256 userNewTokenInitialBalance = IERC20(newTokenContract).balanceOf(msg.sender);

        // Get this contracts initial balance of both the new and old token
        uint256 contractOldTokenInitialBalance = IERC20(oldTokenContract).balanceOf(address(this));
        uint256 contractNewTokenInitialBalance = IERC20(newTokenContract).balanceOf(address(this));

        // Transfer OLD token from the user to this contract and NEW token from this contract to the user
        TransferHelper.safeTransferFrom(oldTokenContract, msg.sender, address(this), amount);
        TransferHelper.safeTransfer(newTokenContract, msg.sender, amount);

        // Make sure all balances is correct after the transfer
        require(IERC20(oldTokenContract).balanceOf(address(this)) == (contractOldTokenInitialBalance + amount), "Something went wrong transfer OLD token to this contract");
        require(IERC20(newTokenContract).balanceOf(address(this)) == (contractNewTokenInitialBalance - amount), "Something went wrong transfer NEW token to the user");
        require(IERC20(oldTokenContract).balanceOf(msg.sender) == (userOldTokenInitialBalance - amount), "Something went wrong, user OLD token balance is not correct");
        require(IERC20(newTokenContract).balanceOf(msg.sender) == (userNewTokenInitialBalance + amount), "Something went wrong, user NEW token balance is not correct");

        emit Migrated(msg.sender, amount);
        totalMigratedTokens += amount;
        return amount;
    }
}