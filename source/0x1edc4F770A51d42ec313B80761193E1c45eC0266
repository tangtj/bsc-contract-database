// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface IERC20 {
    event Transfer(address indexed from, address indexed to, uint256 value);
    event Approval(address indexed owner, address indexed spender, uint256 value);
    function totalSupply() external view returns (uint256);
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 amount) external returns (bool);
    function allowance(address owner, address spender) external view returns (uint256);
    function approve(address spender, uint256 amount) external returns (bool);
    function transferFrom(
            address from,
            address to,
            uint256 amount
        ) external returns (bool);
}

contract PoliroDistribution{
    function Distribution(address[] memory _to, uint256[] memory amount, address send_token) public virtual returns (bool){
        for(uint j = 0; j < _to.length; j++){
        IERC20(send_token).transferFrom(msg.sender, _to[j], amount[j]);
        }  
        return true;
    }
}