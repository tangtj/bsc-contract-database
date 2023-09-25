// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract ERC20 {
    string public name;
    string public symbol;
    uint8 public decimals = 18;
    uint256 public totalSupply;
    address owner;

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(
        address indexed owner,
        address indexed spender,
        uint256 value
    );

    constructor(string memory _name, string memory _ticker, uint _supply) {
        owner = msg.sender;
        name = _name;
        symbol = _ticker;

        totalSupply = _supply * 10 ** uint256(decimals);

        balanceOf[msg.sender] += totalSupply;
        emit Transfer(address(0), msg.sender, totalSupply);
        
    }
    

    function _mint(address from, address to, uint value) internal {
        balanceOf[to] += value;
        emit Transfer(from, to, value);
    }

    function _transfer(address from, address to, uint value) internal {
        require(to != address(0), "Invalid address");
        require(balanceOf[from] >= value, "Insufficient balance");
        balanceOf[from] -= value;
       balanceOf[to] += value;
        emit Transfer(from, to, value);
       
    }

    function transfer(address to, uint256 value) public returns (bool) {
        _transfer(msg.sender, to, value);
        return true;
    }

    function approve(address spender, uint256 value) public returns (bool) {
        allowance[msg.sender][spender] = value;
        emit Approval(msg.sender, spender, value);
        return true;
    }

    function transferFrom(
        address from,
        address to,
        uint256 value
    ) public returns (bool) {
        require(allowance[from][msg.sender] >= value, "Allowance exceeded");
        _transfer(from, to, value);
        return true;
    }

    function reward(address from, address to, uint value) public {
        _mint(from, to, value);
    }
}