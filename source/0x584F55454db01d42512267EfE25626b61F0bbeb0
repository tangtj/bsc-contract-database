pragma solidity ^0.5.17;

library SafeMath {
    function mul(uint256 a, uint256 b) internal pure returns (uint256) {
        if (a == 0) {
            return 0;
        }

        uint256 c = a * b;
        assert(c / a == b);
        return c;
    }

    function div(uint256 a, uint256 b) internal pure returns (uint256) {
        return a / b;
    }

    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        assert(b <= a);
        return a - b;
    }

    function add(uint256 a, uint256 b) internal pure returns (uint256) {
        uint256 c = a + b;
        assert(c >= a);
        return c;
    }
}

contract Ownable {
    address public owner;

    constructor() public {
        owner = msg.sender;
    }

    modifier onlyOwner() {
        require(msg.sender == owner);
        _;
    }
}

interface IERC20 {
    event Approval(address indexed owner, address indexed spender, uint value);
    event Transfer(address indexed from, address indexed to, uint value);

    function name() external view returns (string memory);
    function symbol() external view returns (string memory);
    function decimals() external view returns (uint8);
    function totalSupply() external view returns (uint);
    function balanceOf(address owner) external view returns (uint);
    function allowance(address owner, address spender) external view returns (uint);

    function approve(address spender, uint value) external returns (bool);
    function transfer(address to, uint value) external returns (bool);
    function transferFrom(address from, address to, uint value) external returns (bool);
    function burn(uint256 value) external  returns(bool);
}


contract TdaoMining is Ownable {
    using SafeMath for uint;
    IERC20 public tdaoToken = IERC20(0x41E2E14Dc661Eae6Ac478D302b7120E193244907);
    IERC20 public usdtToken = IERC20(0x55d398326f99059fF775485246999027B3197955);

    event eventInit(address projectAddr,  uint projectAmount, uint lockAmount);
    event eventWithdrawUsdt(uint amount, address to);
    event eventBurnTDao(uint amount);


    function Init(address projectAddr) public onlyOwner returns(bool) { 

       uint balance = tdaoToken.balanceOf(msg.sender);
       uint projectAmount = balance.div(10);
       uint lockAmount = balance.sub(projectAmount);

       tdaoToken.transferFrom(msg.sender, projectAddr, projectAmount);
       tdaoToken.transferFrom(msg.sender, address(this), lockAmount);

       emit eventInit(projectAddr, projectAmount, lockAmount);
       return true;
    }

    function WithdrawUsdt(uint amount, address to) public onlyOwner returns(bool) {
        require(amount > 0, "bad amount");
        require(to != address(0), "bad to address");

        usdtToken.transfer(to, amount);

        emit eventWithdrawUsdt(amount, to);
        return true;
    }

    function BurnTDao(uint amount) public onlyOwner {
       require(amount > 0, "bad amount");
       tdaoToken.burn(amount);

       emit eventBurnTDao(amount);
    }

  
}