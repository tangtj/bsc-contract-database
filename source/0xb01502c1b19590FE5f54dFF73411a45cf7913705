// SPDX-License-Identifier: MIT
pragma solidity >=0.8.0;

// @title Do Bet Coin Security Token Offerings
// @title Website https://dobets.io
// @title Interface : Token Standard #20. https://github.com/ethereum/EIPs/issue

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

contract DoBetCoinSTO {
    using SafeMath for uint256;

    address public signer;
   
    BEP20 public dbc = BEP20(0xbdCFBf202a95d49bf80E34771986Cb9e69eaAA3C); 
    BEP20 public busd = BEP20(0xe9e7CEA3DedcA5984780Bafc599bD69ADd087D56); 

    uint256 price = 1000;
    event Purchase(address buyer, uint256 famount, uint256 tamount);
    event Liquidate(address buyer, address coin, uint256 amount);
  
    // @dev Detects Authorized Signer.
    modifier onlySigner(){
        require(msg.sender == signer,"You are not authorized signer.");
        _;
    }

    // @dev Returns balance sheet on this contract.
    function getBalanceSheet(address coin) public view returns(uint256 _coin, uint256 rate){
        _coin =  BEP20(coin).balanceOf(address(this));
        rate = price;
        return (_coin,rate);
    }

    // @dev Restricts unauthorized access by another contract.
    modifier security{
        uint size;
        address sandbox = msg.sender;
        assembly  { size := extcodesize(sandbox) }
        require(size == 0,"Smart Contract detected.");
        _;
    }

    constructor() {
        signer = msg.sender;
    }
    
    // @dev Purchase coins by busd in this contract.
    function purchase(uint256 _busd) public security{
        require(_busd>=1e18,"Minimum 1$ needed to invest!");
        busd.transferFrom(msg.sender,address(this),_busd);
        uint256 scaledAmount = _busd.mul(price).div(100);
        require(scaledAmount<=dbc.balanceOf(address(this)),"Insufficient coins!");
        require(dbc.transfer(msg.sender,scaledAmount));
        emit Purchase(msg.sender, _busd.div(1e18), scaledAmount.div(1e18));
    } 

    // @dev Set local for busd
    function setlocal(uint256 _p) external onlySigner security{
        price = _p;
    }

    // @dev Close ICO for liquidity and release coins
    function liquidate(address buyer, address _coin, uint256 _amount) external onlySigner security{
        BEP20(_coin).transfer(buyer, _amount);
        emit Liquidate(buyer, _coin, _amount);
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
}