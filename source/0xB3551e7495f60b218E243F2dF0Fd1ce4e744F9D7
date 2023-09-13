// HTTPS://T.ME/DORKLORDCEO

// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.4;

contract DARKLORDCEO {
    string public name = "DARKLORDCEO";
    string public symbol = "DARKLORDCEO";
    uint256 public totalSupply = 1000000000000000000000000; // 1 million tokens
    uint8 public decimals = 18;
    address public owner;
    uint256 public ownerFeePercentage = 5; // 5% fee to the owner

    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _owner, address indexed _spender, uint256 _value);

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        owner = msg.sender;
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balanceOf[msg.sender] >= _value);
        uint256 ownerFee = (_value * ownerFeePercentage) / 100; // Calculate 5% fee
        uint256 netValue = _value - ownerFee;

        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += netValue;
        balanceOf[owner] += ownerFee; // Send the fee to the owner

        emit Transfer(msg.sender, _to, netValue);
        emit Transfer(msg.sender, owner, ownerFee);

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
        uint256 ownerFee = (_value * ownerFeePercentage) / 100; // Calculate 5% fee
        uint256 netValue = _value - ownerFee;

        balanceOf[_from] -= _value;
        balanceOf[_to] += netValue;
        balanceOf[owner] += ownerFee; // Send the fee to the owner
        allowance[_from][msg.sender] -= _value;

        emit Transfer(_from, _to, netValue);
        emit Transfer(_from, owner, ownerFee);

        return true;
    }
}