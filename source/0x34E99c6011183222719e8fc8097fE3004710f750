// SPDX-License-Identifier: MIT license
pragma solidity 0.8.2;

interface IERC20 {
    function transfer(address to, uint tokens) external returns (bool success);

    function transferFrom(
        address from,
        address to,
        uint tokens
    ) external returns (bool success);

    function balanceOf(address tokenOwner) external view returns (uint balance);

    function getAmountsOut(
        uint amountIn,
        address[] memory path
    ) external view returns (uint[] memory amounts);

    function approve(
        address spender,
        uint tokens
    ) external returns (bool success);

    function allowance(
        address tokenOwner,
        address spender
    ) external view returns (uint remaining);

    function totalSupply() external view returns (uint);
    event Transfer(address indexed from, address indexed to, uint tokens);
    event Approval(
        address indexed tokenOwner,
        address indexed spender,
        uint tokens
    );
}
 contract Forwarder {
     address public uns = 0xEcf99aB23C11ddc6520252105308C251001AEfBB;
     function Purchase (uint _amount , address _address) public returns(bool){
         require(IERC20(uns).transferFrom(msg.sender, address(this), _amount), "Transfer Failed");
         require(IERC20(uns).transfer(_address, _amount), "Forward Failed");
         return true;
     }
 }