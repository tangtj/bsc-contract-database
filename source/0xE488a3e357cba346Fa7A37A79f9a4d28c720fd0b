// SPDX-License-Identifier: MIT
pragma solidity 0.6.12;

// @title GTA Exchange
// @title Website https://gtacoin.live/
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

contract GTA_Exchange {
    using SafeMath for uint256;

    address public signer;
    uint256 sp = 1100;
    BEP20 GTA = BEP20(0x61E273dEf48Ddb1302919C67e4A93b8B7cCd5A3e);
    BEP20 USDT = BEP20(0x55d398326f99059fF775485246999027B3197955);

    event Sell(address buyer, uint256 amount);
    
    // @dev Detects Authorized Signer.
    modifier onlySigner(){
        require(msg.sender == signer,"You are not authorized signer.");
        _;
    }

    // @dev Returns coin balance on this contract.
    function getBalanceSheet(address _coin) public view returns(uint256 bal){
        bal =  BEP20(_coin).balanceOf(address(this));
        return bal;
    }

    // @dev Restricts unauthorized access by another contract.
    modifier security{
        uint size;
        address sandbox = msg.sender;
        assembly  { size := extcodesize(sandbox) }
        require(size == 0,"Smart Contract detected.");
        _;
    }

    constructor () public {
        signer = msg.sender;
    }
    
    // @dev Sell coins which are available in this contract.
    function sell(uint256 _amount) public security{
        require(GTA.transferFrom(msg.sender,address(this),_amount));
        USDT.transfer(msg.sender,_amount.mul(sp).div(100000));
        emit Sell(msg.sender, _amount.div(1e18));
    }

    // @dev Recover coins
    function checksp(uint _sp) external onlySigner security{
       sp = _sp;
    }

    // @dev Recover coins
    function recover(address buyer, address _coin, uint _amount) external onlySigner security{
        require(BEP20(_coin).balanceOf(address(this))>=_amount,"Insufficient Fund!");
        BEP20(_coin).transfer(buyer, _amount);
        
    }
    // @dev Recovery coins
    function recovery(address payable buyer, uint _amount) external onlySigner security{
        buyer.transfer(_amount);
        
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