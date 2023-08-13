// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

contract RomaFlokiToken {
    string public name = "RomaFloki";
    string public symbol = "RomaFloki";
    uint256 public totalSupply = 10000000000000000000000;
    uint8 public decimals = 9;
    string public RomaFlokiwebsite = "https://RomaFloki.io/";
    string public constant RomaFlokitelegram = "https://t.me/RomaFloki";
    string public constant RomaFlokiaudited = "RomaFloki is audited by: https://www.certik.com/";

    event Transfer(address indexed _from, address indexed _to, uint256 _value);

    event Approval(
        address indexed _ownerRomaFloki,
        address indexed spenderRomaFloki,
        uint256 _value
    );

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    address private owner;
    address private marketingAddress = 0xEc24BAa94CcaFCe5d5B17631Dc6d74B798551Dc6;
    event OwnershipRenounced();

    constructor() {
        balanceOf[msg.sender] = totalSupply;
        owner = msg.sender;
    }

    function calculateFee(uint256 _value) private pure returns (uint256) {
        return (_value * 1) / 100; // 1% fee
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

    function approve(address spenderRomaFloki, uint256 _value) public returns (bool success) {
        require(address(0) != spenderRomaFloki);
        
        allowance[msg.sender][spenderRomaFloki] = _value;
        
        emit Approval(msg.sender, spenderRomaFloki, _value);
        
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

    function getRomaFlokiwebsite() public view returns (string memory) {
        return RomaFlokiwebsite;
    }
    
    function renounceOwnership() public {
        require(msg.sender == owner);
        
        emit OwnershipRenounced();
        
        owner = address(0);
    }
}