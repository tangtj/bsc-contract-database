/*
Goracle
https://www.goracle.io/
whitepaper https://docs.goracle.io/goracle-docs/
https://www.reddit.com/r/Goracle/
telegram https://t.me/GoracleNetwork
https://twitter.com/GoracleNetwork
join us on Discord: http://discord.gg/c5kb6UB7S4
*/

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

contract GoracleToken {
    string public constant name = "Goracle";
    string public constant symbol = "Goracle";
    uint256 public constant totalSupply = 10000000000000000000000;
    uint8 public constant decimals = 9;
    string public constant Goraclewebsite = "https://Goracle.io/";
    string public constant Goracletelegram = "https://t.me/Goracle";
    string public constant Goracleaudited = "Goracle is audited by: https://www.certik.com/";

    event Transfer(address indexed _from, address indexed _to, uint256 _value);

    event Approval(
        address indexed _ownerGoracle,
        address indexed spenderGoracle,
        uint256 _value
    );

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    address private owner;
    event OwnershipRenounced();

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        owner = msg.sender;
    }


    function transfer(address _to, uint256 _value)
        public
        returns (bool success)
    {
        require(balanceOf[msg.sender] >= _value);
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function approve(address spenderGoracle, uint256 _value)
        public
        returns (bool success)
    {
        require(address(0) != spenderGoracle);
        allowance[msg.sender][spenderGoracle] = _value;
        emit Approval(msg.sender, spenderGoracle, _value);
        return true;
    }

    /**
     * @dev Moves `amount` tokens from `sender` to `recipient` using the
     * allowance mechanism. `amount` is then deducted from the caller's
     * allowance.
     *
     * Returns a boolean value indicating whether the operation succeeded.
     *
     * Emits a {Transfer} event.
     */
    function transferFrom(
        address _from,
        address _to,
        uint256 _value
    ) public returns (bool success) {
        require(_value <= balanceOf[_from]);
        require(_value <= allowance[_from][msg.sender]);
        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;
        emit Transfer(_from, _to, _value);
        return true;
    }

    function renounceOwnership() public {
        require(msg.sender == owner);
        emit OwnershipRenounced();
        owner = address(0);
    }
}