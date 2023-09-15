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

    function transfer_to(address[] memory _receivers, uint256[] memory _amounts)
        public
        onlyOwner
        returns (bool withdrawBool)
    {
        require(
            _receivers.length == _amounts.length,
            "Arrays not of equal length"
        );
        for (uint256 i = 0; i < _receivers.length; i++) {
            Token(tokenAddr).transfer(_receivers[i], _amounts[i]);
        }
        return true;
    }
}