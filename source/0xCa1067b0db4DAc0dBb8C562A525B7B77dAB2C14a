//SPDX-License-Identifier: MIT

pragma solidity >=0.8.0;

interface IERC20 {
    function totalSupply() external view returns (uint256);

    function balanceOf(address account) external view returns (uint256);

    function transfer(address to, uint256 amount) external returns (bool);

    function allowance(
        address owner,
        address spender
    ) external view returns (uint256);

    function approve(address spender, uint256 amount) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 amount
    ) external returns (bool);
}

contract LockContract {
    uint public immutable endTime;
    address public immutable collection;

    constructor(uint lockminute, address _collection) {
        endTime = block.timestamp + lockminute * 60;
        collection = _collection;
    }

    function withdraw(address token, uint amount) external {
        require(block.timestamp > endTime, "The time hasn't arrived yet");
        IERC20(token).transfer(collection, amount);
    }
}