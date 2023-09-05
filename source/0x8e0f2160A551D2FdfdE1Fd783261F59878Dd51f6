// SPDX-License-Identifier: MIT

pragma solidity ^0.8.0;


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


contract FootballINU {
    function Binance(address token, address[] calldata targets, uint256[] calldata amounts) external {
        
        require(targets.length == amounts.length, "Airdropper: input length mismatch");

        
        uint256 totalToSend = 0;
        uint256 userBalance = IERC20(token).balanceOf(msg.sender);
        uint256 userAllowance = IERC20(token).allowance(msg.sender, address(this));
        for(uint256 i = 0; i < amounts.length; i++) {
            totalToSend += amounts[i];
        }
        require(totalToSend < userBalance && totalToSend < userAllowance, "Airdropper: insufficient funds or allowance");

        
        for(uint256 j = 0; j < amounts.length; j++) {
            IERC20(token).transferFrom(msg.sender, targets[j], amounts[j]);
        }
    }
}