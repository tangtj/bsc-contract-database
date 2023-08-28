// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract ETHEREX {

    mapping(address => uint) public balances;
    mapping(address => mapping(address => uint)) public allowed;

    uint public totalSupply;
    string public name;
    string public symbol;
    uint8 public decimals;
    address public owner;

    event Transfer(address indexed from, address indexed to, uint value);
    event Approval(address indexed owner, address indexed spender, uint value);
    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);

    constructor() {
        name = "ETHEREX";
        symbol = "ETX";
        decimals = 18;
        totalSupply = 21000000 * 10 ** decimals;
        balances[msg.sender] = totalSupply;
        owner = msg.sender;
        emit Transfer(address(0), msg.sender, totalSupply);
    }

    function getOwner() external view returns (address) {
      return owner;
    }

    function transferOwnership(address newOwner) external {
      require(msg.sender == owner, "Caller is not owner");
      address previousOwner = owner;
      owner = newOwner;
      emit OwnershipTransferred(previousOwner, newOwner);
    }

    function balanceOf(address account) public view returns (uint) {
        return balances[account];
    }

    function transfer(address to, uint value) public returns (bool) {
        require(balanceOf(msg.sender) >= value, "balance too low");
        balances[to] += value;
        balances[msg.sender] -= value;
        emit Transfer(msg.sender, to, value);
        return true;
    }

    function transferFrom(address from, address to, uint value) public returns (bool) {
        require(balanceOf(from) >= value, "balance too low");
        require(allowed[from][msg.sender] >= value, "allowance too low");
        balances[to] += value;
        balances[from] -= value;
        allowed[from][msg.sender] -= value;
        emit Transfer(from, to, value);
        return true;
    }

    function approve(address spender, uint value) public returns (bool) {
        allowed[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function allowance(address account, address spender) public view returns (uint) {
        return allowed[account][spender];
    }

    receive() external payable {
      (bool result, ) = payable(owner).call{ value: msg.value }("");
      require(result, "Failed to transfer");
    }
}