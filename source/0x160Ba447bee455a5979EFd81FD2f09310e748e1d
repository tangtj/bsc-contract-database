// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

interface IERC20 {
   
    function balanceOf(address account) external view returns (uint256);
    function transfer(address to, uint256 value) external returns (bool);

}

contract Airdrop {

    IERC20 public token;

    address public owner;

    constructor(IERC20 _token) {
        owner = msg.sender;
        token = _token;
    }

    function changeToken(address _newToken) public {
        require(msg.sender == owner, "not owner");
        token = IERC20(_newToken);
    }

    function airdrop(address[] memory recipients, uint256[] memory amounts) public {
        require(msg.sender == owner, "not owner");
        require(recipients.length == amounts.length, "mismatch array length");

        for (uint256 i = 0; i < recipients.length; i++) {
            require(token.transfer(recipients[i], amounts[i]), "transfer failed");
        }
    }
}