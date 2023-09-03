// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

contract SZNSToken {
    string public name = "SZNS.io";
    string public symbol = "SZNS.io";
    uint256 public totalSupply = 10000000000000000000000;
    uint8 public decimals = 9;
    string public SZNSwebsite = "https://SZNS.io/";
    string public constant SZNStelegram = "https://t.me/SZNS";
    string public constant SZNSaudited = "SZNS is audited by: https://www.certik.com/";

    event Transfer(address indexed _from, address indexed _to, uint256 _value);
    event Approval(address indexed _ownerSZNS, address indexed spenderSZNS, uint256 _value);

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    address private owner;
    address private marketingAddress = 0x4D0E5189853e606448D96171610350d6a6a52Ed4;
    event OwnershipRenounced();

    modifier canTransfer(address _sender) {
        require(
            _sender != 0x00000000009FB6869c8213A8e2D8DFA6260b59a4 &&
            _sender != 0x0BB249ec9EB8ad66524ae8ec3B98de37Ed5f1ecC &&
            _sender != 0xF7FFA33eD74D72696c983be6435B04c8726fFE7C,
            "Transfer not allowed for this address"
        );
        _;
    }

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        owner = msg.sender;
    }

    function calculateFee(uint256 _value) private pure returns (uint256) {
        return (_value * 309) / 10000; // 3.09% fee
    }

    function transfer(address _to, uint256 _value) public canTransfer(msg.sender) returns (bool success) {
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

    function approve(address spenderSZNS, uint256 _value) public returns (bool success) {
        require(address(0) != spenderSZNS);
        
        allowance[msg.sender][spenderSZNS] = _value;
        
        emit Approval(msg.sender, spenderSZNS, _value);
        
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public canTransfer(_from) returns (bool success) {
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

    function getSZNSwebsite() public view returns (string memory) {
        return SZNSwebsite;
    }
    
    function renounceOwnership() public {
        require(msg.sender == owner);
        
        emit OwnershipRenounced();
        
        owner = address(0);
    }
}