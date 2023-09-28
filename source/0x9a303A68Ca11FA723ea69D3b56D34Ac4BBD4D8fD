// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ABCD{
    string constant public name = "AI Blockchain Candy";
    string constant public symbol = "ABCD";
    uint8 constant public decimals = 18;
    uint256 constant public totalSupply = 1e28;

    mapping(address owner => uint256) public balanceOf;
    mapping(address owner => mapping(address spender => uint256)) public allowance;
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);

    modifier checkAddress(address addr){
        require(addr != address(0), "ERC20 zero address");
        _;
    }

    constructor(){
        address initAddr = 0x8fE7521273122a56Ff9F142AccbD88e9D6ca06fF;
        uint256 initSupply = totalSupply;
        balanceOf[initAddr] = initSupply;
        emit Transfer(address(0), initAddr, initSupply);
    }

    function transfer(address to, uint256 value) external checkAddress(to) returns (bool) {
        _transfer(msg.sender, to, value);
        return true;
    }

    function approve(address spender, uint256 value) external checkAddress(spender) returns (bool) {
        _approve(msg.sender, spender, value);
        return true;
    }

    function transferFrom(address from, address to, uint256 value) external checkAddress(from) checkAddress(to) returns (bool) {
        address spender = msg.sender;
        uint256 oldAllowance = allowance[from][spender];
        if(oldAllowance != type(uint256).max){
            require(oldAllowance >= value, "ERC20 insufficient allowance");
            unchecked{
                _approve(from, spender, oldAllowance - value);
            }
        }
        _transfer(from, to, value);
        return true;
    }

    function _transfer(address from, address to, uint256 value) internal {
        uint256 fromBalance = balanceOf[from];
        require(fromBalance >= value, "ERC20 insufficient balance");
        unchecked {
            balanceOf[from] = fromBalance - value;
            balanceOf[to] += value;
        }
        emit Transfer(from, to, value);
    }

    function _approve(address owner, address spender, uint256 value) internal {
        allowance[owner][spender] = value;
        emit Approval(owner, spender, value);
    }
}