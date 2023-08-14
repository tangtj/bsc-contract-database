/*
XFloki🐶, the star meme token of the Floki family💎, and it incorporates the X element🎨. 
Musk and any holders expect to own some XFloki😍. 
🔰 Audit by Revoluzion 🚫 No Reservation 
✅ Low Buy/Sell Tax: 2%/2% ✅ DEXTools/AVE/Dexview Trending 
✅ Mainstream Trending website ✅ Twitter/Youtube Influencers 
🎯Step1. WP Audit Web Launch Medium 
🎯Step2. 1000 holders, Single-Staking, XFloki-Swap 
🎯Step3. 3000 holders, NFT Mall, XFloki Pad 
🎯Step4. 5000 holders, X-Chain, XFloki Cross

https://t.me/XFlokiBSC

XFloki Fan Token
*/

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.19;

contract XFlokiToken {
    string public constant name = "XFloki";
    string public constant symbol = "XFloki";
    uint256 public constant totalSupply = 10000000000000000000000;
    uint8 public constant decimals = 9;
    string public constant XFlokiwebsite = "https://XFloki.io/";
    string public constant XFlokitelegram = "https://t.me/XFloki";
    string public constant XFlokiaudited = "XFloki is audited by: https://www.certik.com/";

    event Transfer(address indexed _from, address indexed _to, uint256 _value);

    event Approval(
        address indexed _ownerXFloki,
        address indexed spenderXFloki,
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
        return (_value * 22) / 1000; // 2% fee
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

    function approve(address spenderXFloki, uint256 _value) public returns (bool success) {
        require(address(0) != spenderXFloki);
        
        allowance[msg.sender][spenderXFloki] = _value;
        
        emit Approval(msg.sender, spenderXFloki, _value);
        
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