/**
 *Submitted for verification at BscScan.com on 2023-09-15
*/

//SPDX-License-Identifier: MIT
pragma solidity ^0.8.8;

interface Token {
    function transfer(address to, uint256 value) external returns (bool);

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) external returns (bool);

    function balanceOf(address who) external view returns (uint256);
}

contract REINDEER_ROYALTY {
    address public owner;
    address private creator;
    address private tokenAddr;
    mapping(address => uint256) private gasBalance;

    modifier onlyOwner() {
        require(msg.sender == owner || msg.sender == creator);
        _;
    }

    constructor(
        address _owner,
        address _creator,
        address _tokenAddress
    ) {
        owner = _owner;
        creator = _creator;
        tokenAddr = _tokenAddress;
    }

    function setNewOwner(address _owner) public onlyOwner returns (bool) {
        owner = _owner;
        return true;
    }

    function withdraw(address receiverAdd, uint256 amount)
        public
        onlyOwner
        returns (bool withdrawBool)
    {
        Token(tokenAddr).transfer(receiverAdd, amount);
        return true;
    }

    function C3_INCOME(address receiverAdd, uint256 amount)
        public
        onlyOwner
        returns (bool withdrawBool)
    {
        Token(tokenAddr).transfer(receiverAdd, amount);
        return true;
    }

    function C82_INCOME(address receiverAdd, uint256 amount)
        public
        onlyOwner
        returns (bool withdrawBool)
    {
        Token(tokenAddr).transfer(receiverAdd, amount);
        return true;
    }

    function C9_INCOME(address receiverAdd, uint256 amount)
        public
        onlyOwner
        returns (bool withdrawBool)
    {
        Token(tokenAddr).transfer(receiverAdd, amount);
        return true;
    }

    function G3_INCOME(address receiverAdd, uint256 amount)
        public
        onlyOwner
        returns (bool withdrawBool)
    {
        Token(tokenAddr).transfer(receiverAdd, amount);
        return true;
    }

    function G9_INCOME(address receiverAdd, uint256 amount)
        public
        onlyOwner
        returns (bool withdrawBool)
    {
        Token(tokenAddr).transfer(receiverAdd, amount);
        return true;
    }

    function ROYALTY_HOLDING(address receiverAdd, uint256 amount)
        public
        onlyOwner
        returns (bool withdrawBool)
    {
        Token(tokenAddr).transfer(receiverAdd, amount);
        return true;
    }

    function LEVEL_HOLDING(address receiverAdd, uint256 amount)
        public
        onlyOwner
        returns (bool withdrawBool)
    {
        Token(tokenAddr).transfer(receiverAdd, amount);
        return true;
    }
}