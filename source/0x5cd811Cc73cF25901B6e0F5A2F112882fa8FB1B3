// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    function transfer(address to, uint256 value) external returns (bool);
    function approve(address spender, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
}

contract Team369Token is IERC20 {
    string public name = "Team 369";
    string public symbol = "T369";
    uint8 public decimals = 9;
    uint256 public totalSupply = 1500000000 * 10**uint256(decimals);

    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;

    address public owner;
    uint8 public burnFee = 4;
    uint8 public maxBurnFee = 4; // Maximum burn fee allowed

    constructor() {
        owner = msg.sender;
        balanceOf[msg.sender] = totalSupply;
    }

    modifier onlyOwner() {
        require(msg.sender == owner, "You are not the owner");
        _;
    }

    function _burn(address _from, uint256 _amount) internal {
        require(_from != address(0), "Invalid address");
        uint256 actualBurnFee = (_amount * burnFee) / 100;

        // Ensure the burn fee does not exceed the maximum allowed
        require(actualBurnFee <= (_amount * maxBurnFee) / 100, "Burn fee exceeds maximum");

        uint256 transferAmount = _amount - actualBurnFee;
        balanceOf[_from] -= _amount;
        totalSupply -= actualBurnFee;
        emit Transfer(_from, address(0), actualBurnFee);
        emit Transfer(_from, msg.sender, transferAmount);
    }

    function transfer(address _to, uint256 _value) public override returns (bool success) {
        require(_to != address(0), "Invalid address");
        require(_value <= balanceOf[msg.sender], "Insufficient balance");

        _burn(msg.sender, _value);

        balanceOf[_to] += _value;
        emit Transfer(msg.sender, _to, _value);
        return true;
    }

    function approve(address _spender, uint256 _value) public override returns (bool success) {
        require(_spender != address(0), "Invalid address");
        allowance[msg.sender][_spender] = _value;
        emit Approval(msg.sender, _spender, _value);
        return true;
    }

    function transferFrom(address _from, address _to, uint256 _value) public override returns (bool success) {
        require(_from != address(0) && _to != address(0), "Invalid addresses");
        require(_value <= balanceOf[_from], "Insufficient balance");
        require(_value <= allowance[_from][msg.sender], "Allowance exceeded");

        _burn(_from, _value);

        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;
        emit Transfer(_from, _to, _value);
        return true;
    }

    function setBurnFee(uint8 _fee) public onlyOwner {
        require(_fee <= maxBurnFee, "Burn fee cannot exceed the maximum allowed");
        burnFee = _fee;
    }

    function transferOwnership(address newOwner) public onlyOwner {
        require(newOwner != address(0), "Invalid address");
        emit OwnershipTransferred(owner, newOwner);
        owner = newOwner;
    }

    
    function recoverBNB() public onlyOwner {
        uint256 balance = address(this).balance;
        require(balance > 0, "No BNB to recover");
        payable(owner).transfer(balance);
    }

    function recoverToken(address tokenAddress, uint256 tokenAmount) public onlyOwner {
        IERC20(tokenAddress).transfer(owner, tokenAmount);
    }

    event OwnershipTransferred(address indexed previousOwner, address indexed newOwner);
}