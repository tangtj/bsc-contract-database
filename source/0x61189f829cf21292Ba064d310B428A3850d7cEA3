// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IBEP20 {
    function transfer(address to, uint256 value) external returns (bool);
    function transferFrom(address from, address to, uint256 value) external returns (bool);
    function balanceOf(address account) external view returns (uint256);
    function allowance(address owner, address spender) external view returns (uint256);
}

contract CrowdCapital {
    IBEP20 public busdToken;
    address public owner;
    
    constructor(address _busdToken) {
        busdToken = IBEP20(_busdToken);
        owner = msg.sender;
    }
    
    function Register(uint256 _amount) external {
        require(_amount > 0, "Amount should be greater than zero");
        require(busdToken.allowance(msg.sender, address(this)) >= _amount, "Token allowance too low");
        require(busdToken.transferFrom(msg.sender, owner, _amount), "Token transfer failed");
    }
     function UpgradeLevel(uint256 _amount) external {
        require(_amount > 0, "Amount should be greater than zero");
        require(busdToken.allowance(msg.sender, address(this)) >= _amount, "Token allowance too low");
        require(busdToken.transferFrom(msg.sender, owner, _amount), "Token transfer failed");
    }

     function UpgradeMatrix(uint256 _amount) external {
        require(_amount > 0, "Amount should be greater than zero");
        require(busdToken.allowance(msg.sender, address(this)) >= _amount, "Token allowance too low");
        require(busdToken.transferFrom(msg.sender, owner, _amount), "Token transfer failed");
    }

    function withdraw(address To,uint256 _amount) external onlyOwner {
        require(_amount > 0, "Amount should be greater than zero");
        require(busdToken.balanceOf(address(this)) >= _amount, "Insufficient balance");
        require(busdToken.transferFrom(owner,To, _amount), "Token transfer failed");
    }
    
    function transferOwnership(address newOwner) external onlyOwner {
        require(newOwner != address(0), "Invalid address");
        owner = newOwner;
    }
    
    modifier onlyOwner() {
        require(msg.sender == owner, "Caller is not the owner");
        _;
    }
}