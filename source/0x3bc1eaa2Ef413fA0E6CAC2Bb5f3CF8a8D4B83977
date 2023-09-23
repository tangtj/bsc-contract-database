// SPDX-License-Identifier: GPL-3.0
pragma solidity 0.8.12;

interface IERC20 {
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address recipient, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(address sender, address recipient, uint256 amount) external returns (bool);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract owned {
    address public owner;
    constructor() {
        owner = msg.sender;
    }
    modifier onlyOwner {
        require(msg.sender == owner);
        _;
    }
    function transferOwnership(address newOwner) onlyOwner public {
        owner = newOwner;
    }
}


contract Coin is IERC20,owned {
    string public name;
    string public symbol;
    uint8 public decimals = 8;

    uint256 public totalSupply;

    receive() external payable {}

    constructor(
        string memory tokenName,
        string memory tokenSymbol
    ) {
        name = tokenName;
        symbol = tokenSymbol;
        uint256 initialSupply=10100;
        totalSupply = initialSupply * 10 ** uint256(decimals);
        balanceOf[msg.sender] = totalSupply;
        emit Transfer(address(0),msg.sender,  totalSupply);
    }

    mapping (address => uint256) public balanceOf;
    mapping (address => mapping (address => uint256)) public allowance;

    mapping(address => bool) public swapAddress;

    address deadAddress = 0x000000000000000000000000000000000000dEaD;

    address public projectAddress ;
    address public expenditureAddress ;
    event LpAward(uint256 value);

    function setAddress(address project,address expenditure  )public onlyOwner{
       projectAddress=project;
       expenditureAddress=expenditure;
    }

    uint8 burnRate = 2;
    uint8 lpRate = 2;
    uint8 expenditureRate =1;

    function setSwapAddress(address swap,bool status )public onlyOwner{
        swapAddress[swap]=status;
    }


    function _transfer(address sender, address recipient, uint256 amount) internal {
        balanceOf[sender]=balanceOf[sender]-amount;

        if (swapAddress[sender]||swapAddress[recipient]){
            uint256 burnAmount = amount*burnRate/100;
            uint256 lpAmount = amount*lpRate/100;
            uint256 expenditureAmount =amount*expenditureRate/100;
            uint256 toBalance = amount-burnAmount-lpAmount-expenditureAmount;
            balanceOf[recipient]=balanceOf[recipient]+toBalance;
            emit Transfer(sender, recipient, toBalance);

            balanceOf[deadAddress]=balanceOf[deadAddress]+burnAmount;
            emit Transfer(sender, deadAddress, burnAmount);

            balanceOf[projectAddress]=balanceOf[projectAddress]+lpAmount;
            emit Transfer(sender, projectAddress, lpAmount);
            emit LpAward(lpAmount);

            balanceOf[expenditureAddress]=balanceOf[expenditureAddress]+expenditureAmount;
            emit Transfer(sender, expenditureAddress, expenditureAmount);
        }else{
            balanceOf[recipient]=balanceOf[recipient]+amount;
            emit Transfer(sender, recipient, amount);
        }
      
    }


    function transfer(address recipient, uint256 amount) public returns(bool) {
        _transfer(msg.sender, recipient, amount);
        return true;
    }

    function transferFrom(address sender, address recipient, uint256 amount) public returns(bool) {
        _transfer(sender, recipient, amount);
        _approve(sender, msg.sender, allowance[sender][msg.sender]-amount);
        return true;
    }

    function approve(address spender, uint256 amount) public returns(bool) {
        _approve(msg.sender, spender, amount);
        return true;
    }

    function _approve(address owner, address spender, uint256 amount) internal {
        require(owner != address(0), "TRC20: approve from the zero address");
        require(spender != address(0), "TRC20: approve to the zero address");
        allowance[owner][spender] = amount;
        emit Approval(owner, spender, amount);
    }

 
    function withdrawEth(address payable addr, uint256 amount) onlyOwner public{
        addr.transfer(amount);
    }

    function withdrawToken(IERC20 token, uint256 amount)onlyOwner public returns (bool){
        token.transfer(msg.sender, amount);
        return true;
    }

}