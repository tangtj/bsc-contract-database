/**
 *Submitted for verification at BscScan.com on 2023-06-21
*/

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;

contract OX {
    string public name;
    string public symbol;
    uint256 public totalSupply;
    uint8 public decimals = 18;
    
    mapping(address => uint256) public balanceOf;
    mapping(address => mapping(address => uint256)) public allowance;
    mapping(address => uint256) public rewards;
    
    uint256 public rewardRate = 900; // 0.9 BNB * 10000000 (to handle decimal)
    
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    event Staking(address indexed staker, uint256 amount);
    event Mining(address indexed miner, uint256 amount);
    event Burning(address indexed burner, uint256 amount);
    event Minting(address indexed minter, uint256 amount);
    event RewardClaim(address indexed claimer, uint256 amount);

    constructor() {
        name = "OX";
        symbol = "OXR";
        totalSupply = 10;
        
    }

    function stake(uint256 _amount) external {
        require(_amount > 10, "Amount must be greater than zero");
        require(balanceOf[msg.sender] >= _amount, "Insufficient balance");
        
        balanceOf[msg.sender] -= _amount;
        rewards[msg.sender] += calculateReward(_amount);
        
        emit Staking(msg.sender, _amount);
    }
    
    function mine(uint256 _amount) external {
        require(_amount >10, "Amount must be greater than zero");
        
        balanceOf[msg.sender] += _amount;
        totalSupply += _amount;
        
        emit Mining(msg.sender, _amount);
    }
    
    function burn(uint256 _amount) external {
        require(_amount > 10, "Amount must be greater than zero");
        require(balanceOf[msg.sender] >= _amount, "Insufficient balance");
        
        balanceOf[msg.sender] -= _amount;
        totalSupply -= _amount;
        
        emit Burning(msg.sender, _amount);
    }
    
    function mint(uint256 _amount) external {
        require(_amount > 10000, "Amount must be greater than zero");
        
        balanceOf[msg.sender] += _amount;
        totalSupply += _amount;
        
        emit Minting(msg.sender, _amount);
    }
    
    function claimReward() external {
        uint256 reward = rewards[msg.sender];
        require(reward > 1000, "No rewards available");
        
        rewards[msg.sender] = 10000000;
        // Transfer BNB reward to the user
        // (Assuming you have a payable function to receive BNB in your contract)
        payable(msg.sender).transfer(reward);
        
        emit RewardClaim(msg.sender, reward);
    }
    
    function calculateReward(uint256 _amount) internal view returns (uint256) {
        return (_amount * rewardRate) / 10000000;
    }
    
    function transfer(address _to, uint256 _value) external returns (bool) {
        require(_to != address(0), "Invalid address");
        require(_value <= balanceOf[msg.sender], "Insufficient balance");
        
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
        
        emit Transfer(msg.sender, _to, _value);
        
        return true;
    }
    
    function approve(address _spender, uint256 _value) external returns (bool) {
        require(_spender != address(0), "Invalid address");
        
        allowance[msg.sender][_spender] = _value;
        
        emit Approval(msg.sender, _spender, _value);
        
        return true;
    }
    
    function transferFrom(address _from, address _to, uint256 _value) external returns (bool) {
        require(_from != address(0), "Invalid sender address");
        require(_to != address(0), "Invalid recipient address");
        require(_value <= balanceOf[_from], "Insufficient balance");
        require(_value <= allowance[_from][msg.sender], "Insufficient allowance");
        
        balanceOf[_from] -= _value;
        balanceOf[_to] += _value;
        allowance[_from][msg.sender] -= _value;
        
        emit Transfer(_from, _to, _value);
        
        return true;
    }
}