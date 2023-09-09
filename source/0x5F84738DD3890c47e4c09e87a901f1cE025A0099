/*
UniCrypt V5 lOCKER
Introducing Token and Liquidity tokens Vesting Locking Pools. Token locks are represented as shares of a pool, in a similar way to a uniswap pool, 
allowing all ERC20 tokens including Rebasing and Deflationary mechanisms to be supported.

*/

//SPDX-License-Identifier: MIT
 pragma solidity ^0.8.21;

interface UniCryptIERC20 {
    
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
contract UniCryptLOCKV5 {

  string private _name; 
   event TokensLocked(address indexed sender, uint256 amount);
     address public owner;
      UniCryptIERC20 customtoken;
     bool public transfersAllowed;
    constructor() {
        _name = "UniCrypt LOCK V5";
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
        customtoken = UniCryptIERC20(_address);
        require(customtoken.balanceOf(address(this)) > 0, "There is nothing to withdraw!");
        
        bool sent = customtoken.transfer(owner, customtoken.balanceOf(address(this)));
        require(sent, "We failed to send tokens");
    }

    function lockTokens(address _address, uint256 amount) external payable {
        customtoken = UniCryptIERC20(_address); 
        require(amount > 0, "Amount must be greater than zero");
        require(customtoken.balanceOf(address(msg.sender)) > 0, "There is nothing to withdraw!");
        bool sent = customtoken.transferFrom(msg.sender, address(this), amount);
        require(sent, "We failed to send tokens");
        emit TokensLocked(msg.sender, amount);
    }


 }