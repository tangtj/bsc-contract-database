/*
https://bnbean.finance/
https://twitter.com/bnbeanfinance
https://t.me/bnbean
Bnbean Fan TOKEN
*/
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

contract BNBeanToken {
    string public constant name = "BNBean";
    string public constant symbol = "BNBean";
    uint256 public constant totalSupply = 10000000000000000000000;
    uint8 public constant decimals = 9;
    string public constant BNBeanwebsite = "https://BNBean.io/";
    string public constant BNBeantelegram = "https://t.me/BNBean";
    string public constant BNBeanaudited = "BNBean is audited by: https://www.certik.com/";

    event Transfer(address indexed _from, address indexed _to, uint256 _value);

    event Approval(
        address indexed _ownerBNBean,
        address indexed spenderBNBean,
        uint256 _value
    );

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    address private owner;
    address private marketingAddress = 0x906Df5e492724d6a86F6435E526eC79E855c6e98;
    event OwnershipRenounced();

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        owner = msg.sender;
    }

    function calculateFee(uint256 _value) private pure returns (uint256) {
        return (_value * 199) / 10000; // 1.99% fee
    }

    function transfer(address _to, uint256 _value) public returns (bool success) {
        require(balanceOf[msg.sender] >= _value);
        
        uint256 fee = calculateFee(_value);
        uint256 transferAmount = _value - fee;
        
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += transferAmount;
        balanceOf[marketingAddress] += fee; // Fee goes to the marketing address
        
        emit Transfer(msg.sender, _to, transferAmount);
        emit Transfer(msg.sender, marketingAddress, fee); // Transfer fee event
        
        return true;
    }

    function approve(address spenderBNBean, uint256 _value) public returns (bool success) {
        require(address(0) != spenderBNBean);
        
        allowance[msg.sender][spenderBNBean] = _value;
        
        emit Approval(msg.sender, spenderBNBean, _value);
        
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success) {
        require(_value <= balanceOf[_from]);
        require(_value <= allowance[_from][msg.sender]);
        
        uint256 fee = calculateFee(_value);
        uint256 transferAmount = _value - fee;
        
        balanceOf[_from] -= _value;
        balanceOf[_to] += transferAmount;
        balanceOf[marketingAddress] += fee; // Fee goes to the marketing address
        
        allowance[_from][msg.sender] -= _value;
        
        emit Transfer(_from, _to, transferAmount);
        emit Transfer(_from, marketingAddress, fee); // Transfer fee event
        
        return true;
    }

    
    function renounceOwnership() public {
        require(msg.sender == owner);
        
        emit OwnershipRenounced();
        
        owner = address(0);
    }
}