// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract SEAL {
    string public name = "SEAL Coin";
    string public symbol = "SEAL";
    uint256 public totalSupply = 2100000 * 10 ** 18;
    uint8 public decimals = 18;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    address public immutable blackHole = 0x77c6b9dC608b137576A5c7e133Bf3115d3671F25;
    uint256 public constant maxTransactionFee = 3;
    uint256 public constant feeDivider = 100;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    constructor() {
        balanceOf[msg.sender] = totalSupply;
    }

    function transfer(address to, uint256 value)
        external
        returns (bool success)
    {
        require(balanceOf[msg.sender] >= value, "Insufficient balance");
        _transfer(msg.sender, to, value);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool success) {
        require(balanceOf[from] >= value, "Insufficient balance");
        require(allowance[from][msg.sender] >= value, "Allowance too low");
        _transfer(from, to, value);
        _approve(
            from,
            msg.sender,
            allowance[from][msg.sender] - value
        );
        return true;
    }

    function _transfer(
        address from,
        address to,
        uint256 value
    ) internal {
        uint256 fee = (value * maxTransactionFee) / feeDivider;
        uint256 netValue = value - fee;

        balanceOf[from] -= value;
        balanceOf[to] += netValue;
        balanceOf[blackHole] += fee;

        emit Transfer(from, to, netValue);
        emit Transfer(from, blackHole, fee);
    }

    function approve(address spender, uint256 value)
        external
        returns (bool success)
    {
        _approve(msg.sender, spender, value);
        return true;
    }

    function _approve(
        address owner,
        address spender,
        uint256 value
    ) internal {
        allowance[owner][spender] = value;
        emit Approval(owner, spender, value);
    }
}