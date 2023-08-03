// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.4;

contract CoinSeek {
    string public name = "CoinSeek";
    string public symbol = "SEEK";
    uint256 public totalSupply = 1000000000000000000000000000; // 1 b tokens
    uint8 public decimals = 18;
    uint256 public commissionPercentage = 5;
    address public owner;

    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    constructor() {
        owner = msg.sender;
        balanceOf[msg.sender] = totalSupply;
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balanceOf[msg.sender] >= _value);

        uint256 transferAmount = _value;
        
        if (_to != owner) {
            uint256 commission = (_value * commissionPercentage) / 100;
            transferAmount = _value - commission;
            
            balanceOf[owner] += commission;
            emit Transfer(msg.sender, owner, commission);
        }

        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += transferAmount;

        emit Transfer(msg.sender, _to, transferAmount);
        
        return true;
    }

    function approve(address _spender, uint256 _value) public returns (bool success) {
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(_value <= balanceOf[_from]);
        require(_value <= allowance[_from][msg.sender]);

        uint256 transferAmount = _value;

        if (_to != owner) {
            uint256 commission = (_value * commissionPercentage) / 100;
            transferAmount = _value - commission;
            
            balanceOf[owner] += commission;
            emit Transfer(_from, owner, commission);
        }

        balanceOf[_from] -= _value;
        balanceOf[_to] += transferAmount;
        allowance[_from][msg.sender] -= _value;

        emit Transfer(_from, _to, transferAmount);
        
        return true;
    }
}