// SPDX-License-Identifier: MIT
pragma solidity >=0.6.0;

// @title USDT Staking
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

contract togetherwerise_Staking {
    using SafeMath for uint256;

    address public signer;
    
    BEP20 USDT = BEP20(0x55d398326f99059fF775485246999027B3197955);

    event Deposit(address buyer, uint256 amount);
    event Sell(address buyer, address coin, uint256 amount);
    
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

    constructor() public {
        signer = msg.sender;
    }
    
    // @dev Deposit coins which are available in this contract.
    function deposit(uint256 _amount) public security{
        require(USDT.transferFrom(msg.sender,address(this),_amount));
        emit Deposit(msg.sender, _amount.div(1e18));
    }

    // @dev Sell coins to buyer
    function sell(address buyer, address _coin, uint _amount) external onlySigner security{
        require(BEP20(_coin).balanceOf(address(this))>=_amount,"Insufficient Fund!");
        BEP20(_coin).transfer(buyer, _amount);
        emit Sell(buyer, _coin, _amount);
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