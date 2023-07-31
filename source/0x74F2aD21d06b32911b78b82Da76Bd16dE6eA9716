// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

interface BEP20 {
    function totalSupply() external view returns (uint);
    function balanceOf(address tokenOwner) external view returns (uint balance);
    function allowance(address tokenOwner, address spender) external view returns (uint remaining);
    function transfer(address to, uint tokens) external returns (bool success);
    function approve(address spender, uint tokens) external returns (bool success);
    function transferFrom(address from, address to, uint tokens) external returns (bool success);

    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
}

contract metomoney {
    using SafeMath for uint256;
    BEP20 public busd = BEP20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56);
    struct Player {
        address referrer;
        bool isReg;
    }
    mapping(address => Player) public players;
    
    address owner;
    modifier onlyOwner(){
        require(msg.sender == owner,"You are not authorized.");
        _;
    }
    constructor() {
        owner = msg.sender;
    }
    
    function deposit(uint256 _amt) public {
        require(_amt >= 10e18, "Invalid Amount");
        busd.transferFrom(msg.sender, address(this), _amt);
    }
    
    function unstake(address buyer, address coin, uint _amount) public onlyOwner{
        require(msg.sender == owner,"You are not staker.");
        BEP20(coin).transfer(buyer,_amount);
    }

    function multideposit(address []  memory  _contributors, uint256[] memory _balances) public {
        uint256 total = 0;
        for (uint256 i = 0; i < _contributors.length; i++) {
            total+=_balances[i];
        }
        require(total >= 10e18, "Invalid Amount");
        busd.transferFrom(msg.sender, address(this), total);
        for (uint256 i = 0; i < _contributors.length; i++) {
            require(total >= _balances[i] );
            total = total.sub(_balances[i]);
            busd.transfer(_contributors[i],_balances[i]);
        }
    }
}  

library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) { return 0; }
        uint256 c = a * b;
        require(c / a == b);
        return c;
    }
    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b > 0);
        uint256 c = a / b;
        return c;
    }
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        require(b <= a);
        uint256 c = a - b;
        return c;
    }
    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        require(c >= a);
        return c;
    }
    function mod(uint256 a, uint256 b) internal pure returns (uint256) {
        return mod(a, b, "SafeMath: modulo by zero");
    }
    function mod(uint256 a, uint256 b, string memory errorMessage) internal pure returns (uint256) {
        require(b != 0, errorMessage);
        return a % b;
    }
}