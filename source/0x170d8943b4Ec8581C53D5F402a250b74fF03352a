// SPDX-License-Identifier: MIT
pragma solidity ^0.8.12;

contract FungibleToken {
    mapping(address => uint256) private balances;

    uint256 public totalSupply = 90000000000000000;

    string public name = "Methereum";
    string public symbol = "MTH";
    uint8 public decimals = 9;

    address private owner;

    constructor() {
        owner = msg.sender;
        balances[msg.sender] = totalSupply;
    }

    function buyToken() public payable {
        require(msg.value >= 0.1 ether, "Insufficient funds");
        _transfer(address(this), msg.sender, 1);
        payable(address(owner)).transfer(msg.value);
    }

    function sellToken() public {
        require(owner == msg.sender);
        uint256 bnbAmount = 0.05 ether;
        _transfer(msg.sender, address(this), 1);
        payable(msg.sender).transfer(bnbAmount);
    }

    function _transfer(address _from, address _to, uint256 _amount) private {
        require(_from != address(0), "Invalid sender address");
        require(_to != address(0), "Invalid recipient address");
        require(balances[_from] >= _amount, "Insufficient token balance");

        balances[_from] -= _amount;
        balances[_to] += _amount;
    }

    function balanceOf(address _owner) public view returns (uint256) {
        return balances[_owner];
    }

    function transfer(address _to, uint256 _amount) public {
        require(_to != address(0), "Invalid recipient address");
        require(balances[msg.sender] >= _amount, "Insufficient token balance");

        balances[msg.sender] -= _amount;
        balances[_to] += _amount;
    }
}